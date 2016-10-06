---
layout: docs
title: Running tests against the Selenium Grid
permalink: /docs/recipes/selenium-tests/
---

Let's say you want to run tests against Selenium.

The method described in this recipe runs Selenium tests as a part of the Maven build lifecycle.

# Method

1. First of all you need a source repository where you have a Maven _pom_ file and all your Selenium tests stored. It should be a basic java project. As an example see the [adop-cartridge-java-regression-tests](https://github.com/Accenture/adop-cartridge-java-regression-tests) repository. You should put the link to it in the _/src/urls.txt_ file in your cartridge directory.






