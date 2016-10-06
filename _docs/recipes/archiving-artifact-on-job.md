---
layout: docs
title: Archiving an artefact on the job
permalink: /docs/recipes/archiving-artifact-on-job/
---

Let's say you want to archive an artefact on the job.

# Method

The artifacts should be archived as a part of a post build action.

The snippet bellow should be places inside the job definition.

```
  publishers {
          archiveArtifacts("**/*")
          ...
  }
```

In the archiveArtifacts() method you should specify the pattern of file to archive.

# Properties

- archiveArtifacts(String glob, String excludeGlob = null)
  Archives artifacts with each build.

---
_More details on archiveArtifacts() parameters you can find in [Jenkins Job DSL Plugin Documentation / archiveArtifacts](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.helpers.publisher.PublisherContext.archiveArtifacts)_





