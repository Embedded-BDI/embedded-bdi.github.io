---
title: CI
sidebar: mydoc_sidebar
permalink: ci.html
folder: mydoc
---

<br>

[GitHub Actions](https://github.com/Embedded-BDI/embedded-bdi/blob/master/.github/workflows/unit-tests.yml) is used for Continuous Integration, and every commit is checked for memory leaks, unit tests, and agent build. The CI process looks as follows:

* Start `ubuntu-20.04` runner
* Deploy [custom container image](https://github.com/orgs/Embedded-BDI/packages/container/package/embeddedbdi-cicd) with necessary requirements already installed
* Checkout repository to container
* Build agent and unit tests
* Check code for memory leaks using Valgrind
* Run unit tests
* Build documentation
* Publish Agent API to GitHub pages

CI configuration is available in the files `.github/workflows/unit-tests.yml` and `.github/workflows/gh-pages.yml`.