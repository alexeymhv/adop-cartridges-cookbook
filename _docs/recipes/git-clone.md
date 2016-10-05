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
2. The job.
Put your job name in place of the placeholder(<JOB_NAME>).
```groovy
def buildAppJob = freeStyleJob(projectFolderName + "/<JOB_NAME>")
```

