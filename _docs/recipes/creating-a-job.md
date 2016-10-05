---
layout: docs
title: Creating a Jenkins job
permalink: /docs/recipes/creating-a-job/
---

Let's say you want to create a _freestyle_ Jenkins job.

# Method

At first you'll need to define several variables:

* Project folder name where you want the job to be created.

    ```
    def projectFolderName = "${PROJECT_NAME}"
    
    ```
* The freestyle job.

    Put your job name in place of the placeholder \<JOB_NAME\>.

    ```
    def buildAppJob = freeStyleJob(projectFolderName + "/<JOB_NAME>")
    ```

Once you've defined the variable you can start setting up your job.

The job skeleton should be as follows:

_Don't forget to use the preBuildCleanup() **build wrapper** which will clean the Jenkins workspace._

```
buildAppJob.with {
    description("This job builds Java Spring reference application")
    wrappers {
      preBuildCleanup()
    }
    
    <JOB_CONFIGURATION_GOES_HERE>
}
```

# Example

```
def buildAppJob = freeStyleJob(projectFolderName + "/<JOB_NAME>")
buildAppJob.with {
    description("This job builds Java Spring reference application")
    wrappers {
      preBuildCleanup()
    }
    
  <JOB_CONFIGURATIONS_GO_HERE>
}
```



