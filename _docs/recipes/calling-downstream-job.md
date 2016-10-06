---
layout: docs
title: Calling a downstream job with parameters
permalink: /docs/recipes/calling-downstream-job/
---

Use a publishers{} method to define a post-build action. 

You can triiger the next job as a post-build action using a downstreamParameterized{} method.

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

* trigger(String project)
  
  Triggers the job specified inside the parentheses.
  
* parameters{}
  
  Encloses the parameter definitions for the job to trigger.
  
* predefinedProp(String key, String value)
  
  Adds a parameter.
