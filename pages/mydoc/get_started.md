---
title: Get Started
sidebar: mydoc_sidebar
permalink: get_started.html
folder: mydoc
---

## Before you start

It is recommended that users are familiar with the [AgentSpeak](https://en.wikipedia.org/wiki/AgentSpeak) programming language and/or the [Jason](http://jason.sourceforge.net/wp/) interpreter. If you are not familiar with either of these, please read the article [Getting Started with Jason](http://jason.sourceforge.net/mini-tutorial/getting-started/) to have a basic understanding of the programming language syntax and how agents operate.

## Clone the repository

Clone the [embedded-bdi](https://github.com/embedded-bdi/embedded-bdi) repository:

```sh
git clone https://github.com/embedded-bdi/embedded-bdi
```

## Install the requirements

* [OpenJDK](https://openjdk.java.net/) (or [Java SDK](https://www.oracle.com/java/technologies/javase-downloads.html)).
* C++11 compiler. [g++ 4.9 or later](https://gcc.gnu.org/gcc-4.9/changes.html) is recommended but other compilers should also be supported.
* (Recommended) [Doxygen](https://www.doxygen.nl/index.html) for documentation generation.
* (Recommended) [Graphviz](https://graphviz.org/) for UML/chart support in documentation.

<hr>

## For Developers:

### Install Development Tools

* Checking memory leaks: [Valgrind](https://valgrind.org/)
* Generating documentation: [Doxygen](https://www.doxygen.nl/index.html)
* Generating graphs in documentation: [Graphviz](https://graphviz.org/)

### (Optional) Install Eclipse CDT

[Eclipse CDT](https://www.eclipse.org/cdt/) can be used for development. To import the project into Eclipse, click on "File" -> "Import" -> "Existing Projects into Workspace". Make sure to select the root `embedded-bdi` directory so all projects are imported.
* Note: Launchers for the compiling and running the Agent and Unit Tests are already available in the project, so you should be good to go.
