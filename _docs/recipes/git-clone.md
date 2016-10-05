---
layout: docs
title: How do I clone from git?
permalink: /docs/recipes/git-clone/
---

Let's say you want your pipeline to clone a repository from Gerrit.
At first you need to define several variables:
1. Project folder name
```groovy
def projectFolderName = "${PROJECT_NAME}"
```
2. The freestyle job.
Put your job name in place of the placeholder(<JOB_NAME>).
```groovy
def buildAppJob = freeStyleJob(projectFolderName + "/<JOB_NAME>")
```

Once you've defined the necessary variable start setting your job.
An _scm_ provider allows the job to check out SCM sources.
Don't forget to use the necessary _build wrappers_ in order to clean the Jenkins workspace and to provide the required credentials.
```groovy
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



