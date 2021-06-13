---
title: Software
sidebar: mydoc_sidebar
permalink: nodemcuv3_software.html
folder: mydoc

---

<br>

## Download the repository

Recursively clone the repository:

```
git clone --recurse-submodules https://github.com/Embedded-BDI/nodemcuv3-embedded-bdi
```

> <small>Note: Older versions of Git (>2.12) use the flag `--recursive` instead of `--recurse-submodules`</small>

The recursive clone will automatically download the entire project with the Embedded-BDI library and the ESP8266 SDK to flash the code to the NodeMCUv3 (ESP8266 is the microcontroller of the NodeMCUv3 board).

### Repository structure

* `ESP8266_RTOS_SDK`: the SDK that allows C and C++ code to be implemented in the ESP8266 microcontroller of the board.
* `embedded-bdi`: Embedded-BDI library.
* `examples`: contains the example codes for multiple versions of the vacuum cleaner.
* `main/src`: stores the main code file, `agent_loop.cpp`, and the translated AgentSpeak code to C++, `main/src/config/configuration.h`.
* `main/data`: has the `agentspeak.asl`, `functions.h`, and `functions.cpp` files.
* `main/CMakeLists.txt`: links the necessary BDI libraries for compilation.
* `Makefile`: Makefile with multiple targets for agent compilation and flash to the board.
* `agent.config`: agent configuration.
* `sdkconfig`: SDK configuration.

<center>{% include image.html file="nodeMCUv3_folder.png" height="200" caption="nodemcuv3-embedded-bdi repository" %}</center>

## Install the SDK

Install the SDK by following the steps outlined on the [SDK documentation](https://docs.espressif.com/projects/esp8266-rtos-sdk/en/latest/get-started/index.html#get-esp8266-rtos-sdk), also available in <a href="./pages/files/docs_esp8266_sdk.pdf" target="_blank">pdf</a> format (the pdf file may be outdated).

It is highly recommended to build and flash an example project to ensure that the SDK was correctly installed and that code can be flashed to the board successfully.

## Set up the project

Connect the board to the computer and check the port used by the OS to communicate with the board device. Change the `interface` field in the Makefile at the root of the *nodemcuv3-embedded-bdi* repository to include the correct interface for your setup.

Run `make menuconfig` and load the `sdkconfig` available at the root of the repository - it contains custom configuration, disabling RTOS and unnecessary libraries.

## Compile, flash, and monitor one of the examples

The vacuum cleaner examples are available at `nodemcuv3-embedded-bdi/examples/vacuum`. Several targets to run the vacuum cleaner examples are available in the `Makefile` at the root of the repository:

* `v1-traditional`
* `v1-reactive`
* `v1-proactive`
* `v2-traditional`
* `v2-reactive`
* `v2-proactive`
* `v3-traditional`
* `v3-reactive`
* `v3-proactive`

Each of those targets will automatically copy the source files of the chosen example to the appropriate location and compile, flash, and monitor the corresponding agent code to the NodeMCUv3 board. The naming convention is intuitive: the first part of the name refers to the vacuum cleaner version, and the second to the implementation approach applied.

Note that the targets above require the board to be connected to run successfully, since flashing the agent in the board is part of the make process.

<!-- ## Results -->