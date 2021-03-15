---
title: CI
sidebar: mydoc_sidebar
permalink: ci.html
folder: mydoc
---

<br>

[CircleCI](https://circleci.com) is used for Continuous Integration, and every commit starts a pipeline with the following steps:

* Spin up Ubuntu environment;
* Code checkout;
* Install [requirements](/requirements.html);
* Check for memory leaks using Valgrind;
* Build and run Unit tests;
* Install OpenJDK;
* Translate AgentSpeak code and build agent executable file;
* Build documentation.

The pipeline configuration is available at `.circleci/config.yml`.