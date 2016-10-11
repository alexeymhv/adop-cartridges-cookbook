---
layout: docs
title: Running a SonarQube analysis
permalink: /docs/recipes/running-sq-analysis/
---

This section describes how to run a SonarQube analysis.

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

### Sonar parameters

- sonar.projectKey - The project key that is unique for each project
- sonar.projectName - Name of the project that will be displayed on the web interface
- sonar.projectVersion - The project version
- sonar.sources - Comma-separated paths to directories containing source files
- sonar.language - Set the language of the source code to analyze
- sonar.sourceEncoding - Set the source file encoding
- sonar.scm.enabled - Enable or disable SCM activity plugin

# Example

```
// Folders
def workspaceFolderName = "${WORKSPACE_NAME}"
def projectFolderName = "${PROJECT_NAME}"

// Variables
def projectNameKey = projectFolderName.toLowerCase().replace("/", "-")

def codeAnalysisJob = freeStyleJob(projectFolderName + "/Reference_Application_Code_Analysis")

codeAnalysisJob.with {
    description("This job runs code quality analysis for Java reference application using SonarQube.")
    
    ...
    
    environmentVariables {
        env('WORKSPACE_NAME', workspaceFolderName)
        env('PROJECT_NAME', projectFolderName)
        env('PROJECT_NAME_KEY', projectNameKey)
    }
    
    ...
    
    steps {
        copyArtifacts('Reference_Application_Unit_Tests') {
            buildSelector {
                buildNumber('${B}')
            }
        }
    }
    
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
    
    ...
}
```

---

More details here:

- [Jenkins Job DSL API / configure](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.jobs.WorkflowJob.configure)
- [adop-cardridge-java](https://github.com/Accenture/adop-cartridge-java/blob/master/jenkins/jobs/dsl/java_reference_application_jobs.groovy)
- [Sonar parameters](http://docs.sonarqube.org/display/SONAR/Analysis+Parameters)



