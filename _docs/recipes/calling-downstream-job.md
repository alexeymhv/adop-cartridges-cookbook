---
layout: docs
title: Calling a downstream job with parameters
permalink: /docs/recipes/calling-downstream-job/
---

This section describes how to call a downstream job with parameters.

# Method

Use a publishers{} method to define a post-build action. 

You can trigger the next job as a post-build action using a downstreamParameterized{} method.

The following snippet should be placed inside the job definition.

```
    publishers {
        archiveArtifacts("**/*")
        downstreamParameterized {
            trigger(projectFolderName + "/Reference_Application_Unit_Tests") {
                condition("UNSTABLE_OR_BETTER")
                parameters {
                    predefinedProp("B", '${BUILD_NUMBER}')
                    predefinedProp("PARENT_BUILD", '${JOB_NAME}')
                }
            }
        }
    }
```

# Properties

* trigger(String project)
  
  Triggers the job specified inside the parentheses.
  
* parameters{}
  
  Encloses the parameter definitions for the job to trigger.
  
* predefinedProp(String key, String value)
  
  Adds a parameter.

---

More details here:

- [Jenkins Job DSL API / downstreamParameterized](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.helpers.publisher.PublisherContext.downstreamParameterized)
