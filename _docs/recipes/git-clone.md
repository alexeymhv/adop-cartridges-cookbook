---
layout: docs
title: Cloning from Gerrit
permalink: /docs/recipes/git-clone/
---

Let's say you want your pipeline to clone a repository from Gerrit.

# Method

At first you'll need to define several variables:

* The name of the repository you want to clone.

    ```
    def referenceAppgitRepo = "spring-petclinic"
    ```

* The URL of your gerrit project.

    ```
    def referenceAppGitUrl = "ssh://jenkins@gerrit:29418/${PROJECT_NAME}/" + referenceAppgitRepo
    ```

Once you've defined the necessary variable you can start setting up your job.

You will need to set several wrappers in order to provide the required credentials that will be used when cloning the repository.

```
    wrappers {
        preBuildCleanup()
        injectPasswords()
        maskPasswords()
        sshAgent("adop-jenkins-master")
    }
```

An _scm_ provider allows the job to check out SCM sources.

In our case we will clone the master branch of the _spring-petclinic_ repository stored in Gerrit.

The _scm_ section should be placed inside a job definition (refer to [Creating a Jenkins job](https://mibzzz.github.io/adop-cartridges-cookbook/docs/recipes/creating-a-job/)).
```
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

# Example
```
def projectFolderName = "${PROJECT_NAME}"
def buildAppJob = freeStyleJob(projectFolderName + "/<JOB_NAME>")

def referenceAppgitRepo = "spring-petclinic"
def referenceAppGitUrl = "ssh://jenkins@gerrit:29418/${PROJECT_NAME}/" + referenceAppgitRepo

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
    ...
}
```
