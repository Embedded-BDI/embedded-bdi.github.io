---
title: Supported Features
sidebar: mydoc_sidebar
permalink: supported_features.html
folder: mydoc
---

<br>

The features supported by Embedded-BDI are:

* Propositions for representing beliefs, plans, and actions;
* Automated pre-allocation of belief base: beliefs are mapped during translation process and memory for belief base is statically allocated;
* Configure sizes of event base, intention base, and intention stack;
* Context unification using the logical *and* operator (`&`);
* Event handling for sub-goals: belief updates and sub-goal invocation generate corresponding events, to be queued and processed during the following reasoning cycles;
* Handling of full intention base: if intention base is full, new intentions are discarded;
* Handling of full event base: if event base is full, new events are discarded (if event was generated during a plan execution, the plan fails);
* Handling of plan failures;
* Handling of sub-goal triggers: `!` and `!!`.