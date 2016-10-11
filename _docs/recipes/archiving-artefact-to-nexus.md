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

Frst you need to have a [Config File Provider Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Config+File+Provider+Plugin) installed on Jenkins. 

Once you have it installed, add a new Maven _settings.xml_ configuration file.

In the file you need to specify your Nexus credentials which will be used to upload artifacts.

Add the followin snippet to the _settings.xml_ configuration file:

```
<servers>
    ...
      <server>
            <id>nexus</id>
            <username>USERNAME</username>
            <password>PASSWORD</password>
      </server>
    ...
</servers>
```

In place of _USERNAME_ and _PASSWORD_ placeholders insert your real nexus credentials.

The artefact will be uploaded to Nexus as a part of Maven build lifecycle. Thus, the Nexus properties should be defined in Jenkins Job DSL _steps_ in _maven_ section.

```
steps {
        ...
        maven {
            goals('clean install -DskipTests')
            goals('deploy:deploy-file')
            property('groupId', 'com.spring.maventest')
            property('artifactId', 'petclinic')
            property('version', '1.0.0')
            property('generatePom', 'false')
            property('packaging', 'war')
            property('repositoryId', 'nexus')
            property('url', 'http://nexus:8081/nexus/content/repositories/<YOUR_REPO_NAME>/')
            property('file', 'target/petclinic.war')
            mavenInstallation("ADOP Maven")
            providedSettings('MySettings')
            
        }
        ...
}
```

### Parameters

- goals() - Maven goals to execute

- property() - Adds a property for the Maven build

    #### Nexus parameters
    
    - groupId - group id
    
    - artifactId - artifact id
