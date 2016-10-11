---
layout: docs
title: Running Maven
permalink: /docs/recipes/running-maven/
---

This section describes how to invoke a Maven build.

# Method

The Maven definition should be set up as a part of a build step:

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
  By default, we have the _ADOP Maven_ installation available to use.

# Example

The following snippet should go inside the job definition: 

```
    steps {
        maven {
            goals('clean install -DskipTests')
            mavenInstallation("ADOP Maven")
        }
    }
```

---

More details here:

- [Jenkins Job DSL Plugin Documentation - maven](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.helpers.step.StepContext.maven)
- [Creating a Jenkins job](https://mibzzz.github.io/adop-cartridges-cookbook/docs/recipes/creating-a-job/)




