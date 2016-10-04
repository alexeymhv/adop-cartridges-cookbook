---
layout: docs
title: Welcome
permalink: /docs/welcome/
---
This is the home of all ADOP Cartridge recipes.

All the recipes, as an example take the _[Java](https://github.com/Accenture/adop-cartridge-java)_ cartridge.

When developing a cartridge, it is recommended that Job DSL be used in favor of XML to write your Jenkins jobs. If your cartridge only uses Job DSL a cartridge can be loaded multiple times without having to create a new project. However, if your cartridge loads XML then the jobs will need to be removed in order to reload the cartridge.

More information about the Job DSL plugin API can be found here - [Jenkins Job DSL Plugin API](https://jenkinsci.github.io/job-dsl-plugin/#)
