---
layout: docs
title: Running Maven
permalink: /docs/recipes/run-maven/
---

Let’s say you want your pipeline to invoke a Maven build.

# Method

The Maven definition should be set up as a part of a build step

```
  step{
    maven{
      <MAVEN_PARAMETERS>
    }
  }
```

The following parameters should be defined:

* goals(String goals)

  _Specifies the goals to execute including other command line options_
  
* mavenInstallation(String name)

  _Specifies the Maven installation for executing this step_
  
For more details about Maven parameters please refer to the official _[Jenkins Job DSL Plugin Documentation](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.helpers.step.StepContext.maven)_

# Example

_The snippet should go inside the job definition. Please refer to the documentation - [Creating a Jenkins job](https://mibzzz.github.io/adop-cartridges-cookbook/docs/recipes/creating-a-job/)_

```
    steps {
        maven {
            goals('clean install -DskipTests')
            mavenInstallation("ADOP Maven")
        }
    }
```




