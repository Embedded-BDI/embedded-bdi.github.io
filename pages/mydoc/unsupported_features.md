---
title: Unsupported Features
sidebar: mydoc_sidebar
permalink: unsupported_features.html
folder: mydoc
---

<br>

Because this project is based on [Jason](http://jason.sourceforge.net/wp/), it is important to list some of the features available on Jason that have not yet been implemented in Embedded-BDI.

Some of the unsupported features are:

* Predicates;
* Annotations;
* Unification algorithm to handle operators such as the logical *or*, logical *not*, and parenthesis for logical precedence;
* Propositions are limited to 256 distinct values due to variables used internally (variable type is `uint8_t`, which uses 8 bits);
* Internal actions;
* Advanced _troubleshooting_ tools: Mind inspector, Console, Sniffer Agent.
* Dynamic belief base;
* Multi-agent support;
* Support for/integration with Artifacts and Organizations.