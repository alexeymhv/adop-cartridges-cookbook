---
layout: docs
title: Archiving an artefact to Nexus
permalink: /docs/recipes/archiving-artefact-to-nexus/
---

Let's say you want to archive an artefact to Nexus.

# Method

There are two methods how you can upload your artefacts from Jenkins to Nexus:

1. Making a call to Nexus API using cURL (or _[Nexus Artifact Uploader](https://wiki.jenkins-ci.org/display/JENKINS/Nexus+Artifact+Uploader)_).

2. Uploading artifact as a part of Maven build lifecycle.

### cURL
