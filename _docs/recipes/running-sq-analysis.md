---
layout: docs
title: Running a SonarQube analysis
permalink: /docs/recipes/running-sq-analysis/
---

Let's say you want to run a SonarQube analysis.

# Method

At first you should define several environment variables, that will be used by SonarQube anlysis.

```
// Folders
def workspaceFolderName = "${WORKSPACE_NAME}"
def projectFolderName = "${PROJECT_NAME}"

// Variables
def projectNameKey = projectFolderName.toLowerCase().replace("/", "-")
```

The following snippets should go inside the job definition.

```
    environmentVariables {
        env('WORKSPACE_NAME', workspaceFolderName)
        env('PROJECT_NAME', projectFolderName)
        env('PROJECT_NAME_KEY', projectNameKey)
    }
```

Don't forget to get the build number from the artifacts of the previous job.

```
steps {
        copyArtifacts('Reference_Application_Unit_Tests') {
            buildSelector {
                buildNumber('${B}')
            }
        }
}
```

Once you have all the necessary variables you can start adjusting the SonarQube analysis properties.

```
configure { myProject ->
        myProject / builders << 'hudson.plugins.sonar.SonarRunnerBuilder'(plugin: "sonar@2.2.1") {
            project('sonar-project.properties')
            properties('''sonar.projectKey=${PROJECT_NAME_KEY}
                          sonar.projectName=${PROJECT_NAME}
                          sonar.projectVersion=1.0.${B}
                          sonar.sources=src/main/java
                          sonar.language=java
                          sonar.sourceEncoding=UTF-8
                         sonar.scm.enabled=false''')
            javaOpts()
            jdk('(Inherit From Job)')
            task()
        }
}
```




