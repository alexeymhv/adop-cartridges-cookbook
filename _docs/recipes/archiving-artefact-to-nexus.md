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
Nexus API requires user credentials to be provided.
We advice you to pass your nexus credentials to a jenkins job as environment variables.

Let's assume your _ENV_ variables have the following names _USERNAME_ and _PASSWORD_.

The following snippet should go inside Jenkins job definition:

```
    steps {
        shell('''
            |cd target
            |curl -v -F r=<REPOSITORY_NAME> -F hasPom=false -F e=war -F g=com.spring.test -F a=petclinic -F v=1.0 -F p=war -F file=@petclinic.war -u $USERNAME:$PASSWORD http://nexus:8081/nexus/service/local/artifact/maven/content
              '''.stripMargin()
        )
    }
```

#### Parameters

- r - repository

- e - extension

- g - group id

- a - artifact id

- v - version

- p - packaging

- c - classifier (optional)

### Maven
