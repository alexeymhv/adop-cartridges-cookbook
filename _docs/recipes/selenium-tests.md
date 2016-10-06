---
layout: docs
title: Running tests against the Selenium Grid
permalink: /docs/recipes/selenium-tests/
---

Let's say you want to run tests against Selenium.

The method described in this recipe runs Selenium tests as a part of the Maven build lifecycle.

# Method


First of all you need a source repository where you have a Maven _pom_ file and all your Selenium tests stored. It should be a basic java project. As an example see the [adop-cartridge-java-regression-tests](https://github.com/Accenture/adop-cartridge-java-regression-tests) repository. 
  
The link to the repository should be put in the _[urls.txt](https://github.com/Accenture/adop-cartridge-java/blob/master/src/urls.txt)_ file in the _/src/_ folder of your cartridge.
  
We assume that the reader knows how to write Selenium tests and pom files. Thus, we won't cover this topic in this particular recipe. You can take this [Selenium test](https://github.com/Accenture/adop-cartridge-java-regression-tests/blob/master/src/test/java/springpetclinic_selenium/selenium/OwnerTest.java) and a [pom.xml](https://github.com/Accenture/adop-cartridge-java-regression-tests/blob/master/pom.xml) file as an example.

The following snippet should go inside the job definition.

```
steps {
      ...
      
      environmentVariables {
          propertiesFile('env.properties')
      }
      maven {
          goals('clean -B test -DPETCLINIC_URL=${APP_URL} -DZAP_IP=${ZAP_IP} -DZAP_PORT=${ZAP_PORT} -DZAP_ENABLED=${ZAP_ENABLED}')
          mavenInstallation("ADOP Maven")
      }
      
      ...
}
```

The idea is to get the necessary environment variables and pass them as the properties of the Maven goal. 

Maven will trigger the Selenium tests.
