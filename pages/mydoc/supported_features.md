---
title: Supported Features
sidebar: mydoc_sidebar
permalink: supported_features.html
folder: mydoc
---

<br>

The features supported by Embedded-BDI are:

* Statements for representing beliefs, plans, and actions;
* Pre-allocation of belief base;
* Configure sizes of event base, intention base, and intention stack;
* Context unification using the logical *and* operator (`&`);
* Event handling for sub-goals: belief updates and sub-goal invocation generate corresponding events, to be queued and processed during the following reasoning cycles;
* Handling of overflow intentions and events;
* Handling of plan failures;
* Handling of sub-goal triggers: `!` and `!!`.