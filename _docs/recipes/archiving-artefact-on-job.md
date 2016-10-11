---
layout: docs
title: Archiving an artefact on the job
permalink: /docs/recipes/archiving-artefact-on-job/
---

This section describes how to archive an artefact on the job.

# Method

The artifacts should be archived as a part of a post build action.

The snippet bellow should be placed inside the job definition.

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

More details here:

- [Jenkins Job DSL Plugin Documentation / archiveArtifacts](https://jenkinsci.github.io/job-dsl-plugin/#method/javaposse.jobdsl.dsl.helpers.publisher.PublisherContext.archiveArtifacts)






