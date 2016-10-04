---
layout: docs
title: Creating a freestyle Jenkins job
permalink: /docs/recipes/creating-a-freestyle-job/
---

This section describes how to create a _freestyle_ Jenkins job.

# Method

At first, you'll need to define several variables:

* Project folder name should be set to the folder where you want the job to be created.

    ```
    def projectFolderName = "${PROJECT_NAME}"
    ```
    
    **Note:** The _projectFolderName_ variable should be always set to _${PROJECT_NAME}_ and nothing else, since this is parameterised by the Cartridge Loader job.
    
* The freestyle job:

    Put your job name in place of the placeholder \<JOB_NAME\>.

    ```
    def buildAppJob = freeStyleJob(projectFolderName + "/<JOB_NAME>")
    ```

Once you've defined the necessary variables, you can start setting up your job.

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
def projectFolderName = "${PROJECT_NAME}"
def buildAppJob = freeStyleJob(projectFolderName + "/<JOB_NAME>")
buildAppJob.with {
    description("This job builds Java Spring reference application")
    wrappers {
      preBuildCleanup()
    }
    
  <JOB_CONFIGURATIONS_GO_HERE>
}
```

---

More details here:

- [Jenkins Job DSL API - freeStyleJob](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.DslFactory.freeStyleJob)



