---
title: Vacuum Cleaner Application
sidebar: mydoc_sidebar
permalink: nodemcuv3_vacuum.html
folder: mydoc
---

<br>

The example for the [NodeMCUv3](https://docsv2.zerynth.com/latest/reference/boards/nodemcu3/docs/) expands the previous [Vacuum Cleaner example](vacuum_cleaner_example.html).

For the NodeMCUv3, three versions of the vacuum cleaner are implemented:

* Version 1: has the same **behavior** as the original vacuum, where the vacuum moves from one position to another, sucking the dust from each space, when dirty;
* Version 2: introduces the **start** perception, where the vacuum cycle should only start when the belief `button_start` is true;
* Version 3: adds the **full_deposit** perception and the **empty** action, where the agent should move back to the first position when the dust deposit is full (i.e. `full_deposit` is true) and empty the dust deposit (`empty` action). Once the deposit is empty, the vacuum should return to its original location and resume the dusting cycle.

For each version, three agents are implemented:

* `traditional`: the "traditional" versions are based on standard programming approaches for embedded systems, using the C programming language. It is important to note that, although these implementations aim to mirror common programming practices for embedded systems, functionalities such as interruptions are not taken into consideration, as Embedded-BDI does not use these. Therefore, these versions aim to implement simple code, based on loop control structures and functions similar to those used by the BDI agents to update beliefs and act in the environment.
* `reactive`: these versions use the Embedded-BDI library to implement reactive agents, sensible to changes in the environment.
* `proactive`: these versions use the Embedded-BDI library to implement proactive agents with goal-oriented behavior.

The specification above results in 9 versions of the vacuum cleaner. See [Hardware](./nodemcuv3_hardware.html) to see how the electronic circuit is connected to the NodeMCUv3 and [Software](./nodemcuv3_software.html) to check how the vacuum agents are implemented.