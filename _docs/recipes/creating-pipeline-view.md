---
layout: docs
title: Creating a pipeline view
permalink: /docs/recipes/creating-pipeline-view/
---

This section describes how to to build a pipeline.

# Method

At first you need to define a variable, which will represent your pipeline.

```
def pipelineView = buildPipelineView(projectFolderName + "/Java_Reference_Application")
```

Once you've defined a pipeline, you can start setting properties for it.

```
pipelineView.with {
   title('Reference Application Pipeline')
   displayedBuilds(5)
   selectedJob(projectFolderName + "/Reference_Application_Build")
   showPipelineParameters()
   showPipelineDefinitionHeader()
   refreshFrequency(5)
}
```

# Properties

* title(String title)

  Sets a title for the pipeline.

* displayedBuild(int displayedBuilds)
  
  Sets number of displayed builds.
  
* selectedJob(String selectedJob)
  
  Defines the first job in the pipeline. 

* showPipelineParameters()
  
  Use this method if you want to display the parameters used to run the first job in each pipeline's revision box.
  
* showPipelineDefinitionHeader()

  Use this method if you want to show the pipeline definition header in the pipeline view.
  
* refreshFrequency(int refreshFrequency)

  Frequency at which the Build Pipeline Plugin updates the build cards in seconds.
  
---

Please notice that each job in pipeline, except the last one, requires a post-build action, which triggers the next job in the line.

More details here:

- [Jenkins Job DSL API - buildPipelineView](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.DslFactory.buildPipelineView)
- [Calling a donwstream job with parameters](https://accenture.github.io/adop-cartridges-cookbook/docs/recipes/calling-downstream-job/)
