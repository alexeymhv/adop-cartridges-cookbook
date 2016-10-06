---
layout: docs
title: Running a SonarQube analysis
permalink: /docs/recipes/running-sq-analysis/
---

Let's say you want to run a SonarQube analysis.

# Method

At first you should define several environment variables, that will be used by SonarQube anlysis.

```
/ Folders
def workspaceFolderName = "${WORKSPACE_NAME}"
def projectFolderName = "${PROJECT_NAME}"

// Variables
def projectNameKey = projectFolderName.toLowerCase().replace("/", "-")
```

The following part should go inside the job definition:

```
    environmentVariables {
        env('WORKSPACE_NAME', workspaceFolderName)
        env('PROJECT_NAME', projectFolderName)
        env('PROJECT_NAME_KEY', projectNameKey)
    }
```



