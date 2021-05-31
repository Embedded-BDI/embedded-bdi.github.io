---
title: CI
sidebar: mydoc_sidebar
permalink: ci.html
folder: mydoc
---

<br>

[GitHub Actions](https://github.com/Embedded-BDI/embedded-bdi/blob/master/.github/workflows/unit-tests.yml) is used for Continuous Integration, and every commit starts a pipeline with the following steps:

* Starts `ubuntu-20.04` runner
* Deploys custom docker image for project CI, available at https://github.com/orgs/Embedded-BDI/packages/container/package/embeddedbdi-cicd
* Checkout repository
* Build agent and unit tests
* Check code for memory leaks using Valgrind
* Runs unit tests
* Builds documentation

The pipeline configuration is available at `.circleci/config.yml`.