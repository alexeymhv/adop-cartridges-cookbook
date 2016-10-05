---
layout: docs
title: How do I clone from Gerrit?
permalink: /docs/recipes/git-clone/
---

Let's say you want your pipeline to clone a repository from Gerrit.

At first you'll need to define several variables:

* Project folder name.

    ```
    def projectFolderName = "${PROJECT_NAME}"
    ```

* The freestyle job.

    Put your job name in place of the placeholder \<JOB_NAME\>.

    ```
    def buildAppJob = freeStyleJob(projectFolderName + "/<JOB_NAME>")
    ```

* The name of the repository you want to clone.

    ```
    def referenceAppgitRepo = "spring-petclinic"
    ```

* The URL of your gerrit project.

    ```
    def referenceAppGitUrl = "ssh://jenkins@gerrit:29418/${PROJECT_NAME}/" + referenceAppgitRepo
    ```

Once you've defined the necessary variable you can start setting up your job.

An _scm_ provider allows the job to check out SCM sources.

In our case we will clone the master branch of the _spring-petclinic_ repository stored in Gerrit.

_Don't forget to use the necessary **build wrappers** in order to clean the Jenkins workspace and to provide the required credentials that will be used when cloning the repository._

```
buildAppJob.with {
    description("This job builds Java Spring reference application")
    wrappers {
        preBuildCleanup()
        injectPasswords()
        maskPasswords()
        sshAgent("adop-jenkins-master")
    }
    scm {
        git {
            remote {
                url(referenceAppGitUrl)
                credentials("adop-jenkins-master")
            }
            branch("*/master")
        }
    }
```



