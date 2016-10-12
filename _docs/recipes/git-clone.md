---
layout: docs
title: Cloning from Gerrit
permalink: /docs/recipes/git-clone/
---

This section describes how to make your pipeline clone a repository from Gerrit.

# Method

At first you'll need to define certain variables:

* The name of the repository you want to clone:

    ```
    def referenceAppgitRepo = "spring-petclinic"
    ```
    
    **Note:** The name of the repository in the variable should be the same as the name you have specified in _[urls.txt](https://github.com/Accenture/adop-cartridge-java/blob/master/src/urls.txt)_ file. 
    For example, if the url is https://github.com/Accenture/spring-petclinic.git, then the _referenceAppgitRepo_ variable should be set to _spring_petclinic_.

* The URL of your gerrit project:

    ```
    def referenceAppGitUrl = "ssh://jenkins@gerrit:29418/${PROJECT_NAME}/" + referenceAppgitRepo
    ```

Once you've defined the necessary variables, you can start setting up your job.

You will need to set several wrappers in order to provide the required credentials that will be used when cloning the repository.

```
    wrappers {
        preBuildCleanup()
        injectPasswords()
        maskPasswords()
        sshAgent("adop-jenkins-master")
    }
```

The _sshAgent()_ wrapper allows you to provide SSH credentials to a build. The _adop-jenkins-master_ credential is the id of the Jenkins credentials to be used in the job in order to clone from Gerrit.

An _scm_ provider allows the job to check out SCM sources.

In our case, we will clone the master branch of the _[spring-petclinic](https://github.com/Accenture/spring-petclinic)_ repository stored in Gerrit.

The _scm_ section should be placed inside a job definition.

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

---

More details here:

- [Jenkins Job DSL API - scm](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.jobs.FreeStyleJob.scm)
- [Creating a Jenkins job](https://accenture.github.io/adop-cartridges-cookbook/docs/recipes/creating-a-job/)
