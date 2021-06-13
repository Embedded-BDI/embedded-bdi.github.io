---
title: Implementation Details
sidebar: mydoc_sidebar
permalink: implementation_details.html
folder: mydoc
---

## Design

### Programming Language

For several reasons, the programming language chosen for implementation of the the Embedded-BDI framework is C++:

* C++ is commonly used in the implementation of embedded systems;
* The language allows efficient low-level memory management;
* The Object-Oriented Paradigm facilitates modularizing the code, allowing classes to be easily modified and new features to be implemented.

### Target Platform

Embedded-BDI is built with multi-platform support in mind. For this reason, the framework does not use proprietary/specific SDKs; instead, only standard C++ libraries are used in the implementation, allowing the framework to be used in multiple platforms that support the C++ programming language.

### Features Supported

Due to the challenge of porting the BDI architecture to the embedded system scenario, some features have been stripped from the framework. More information about the supported features is available in the [Supported Features](./supported_features.html) page, while the features not yet implemented can be checked on the [Unsupported Features](./unsupported_features.html) page.

#### Predicates

Support for predicates is one of the features not included in Embedded-BDI, due to the following reasons:

* The memory management of predicates is a big challenge, as predicates can assume multiple types of values: integers, chars, strings, etc. Statically storing variables of multiple types and sizes in memory is a big challenge, and goes in the opposite direction of software development for embedded systems;
* Because of the complexity of implementing predicates, it was decided that an initial version with support for propositions would provide:
  * Better overview of the challenges of implementing BDI agents in embedded systems;
  * More understanding of how much computing power is necessary to run simple agents in hardware commonly used in those platforms.

#### Operators

Due to the [large amount of processing required by unification algorithms](https://ieeexplore.ieee.org/document/7424005), it was decided to include only the logical and `&` operator for unification.

### Updating Beliefs and Acting in the Environment

The Jason interpreter provides a wide range of internal methods, each already implemented and, thanks to the Java Virtual Machine, available for multiple platforms.

However, the hardware used in embedded systems is usually heterogeneous, has different I/O interfaces and SDK implementations. For this reason, it was decided that no internal methods would be added to Embedded-BDI, and the programmer must provide the necessary functions to update beliefs and act in the environment.

### Interpreted vs Compiled Agent

Another difference between Embedded-BDI and Jason is that while Jason is an interpreter, Embedded-BDI translates the AgentSpeak code to a corresponding C++ header, to be compiled along with the framework. This approach aims to improve the performance of the embedded agent.

#### Translating the AgentSpeak code

The translation of the AgentSpeak code to a C++ header is achieved by parsing the AgentSpeak file using the parser available in Jason and generating the corresponding C++ header file via Java code.

## Implementation

### Code Modularization

Code is modularized to facilitate individual changes and improvements. For example, if one wishes to implement hash tables as the data structure for the intention base, all that is required is to modify the `intention_base` class to accommodate the new data structure.

### Data Structures

All internal data structures are statically allocated in memory, as usual in embedded systems. This means that the programmer must set the size of the intention stack, intention queue, and event queue.

Since the beliefs can only assume boolean values, the size of the belief base is automatically set by the framework by analyzing the AgentSpeak file, detecting how many beliefs there are, and allocating the belief base in memory.

#### Handling Overflow

Since the data structures have limited size, it is necessary to address the overflow. The following rules apply for overflow of data structures:

* `Event Base`: if the event queue is full and a new event needs to be added, the new event is discarded. If a plan generated the new event, the plan fails.
* `Intention Base`: if the intention base is full and a new intention needs to be added, the new intention is discarded.
* `Intention Stack`: if the intention stack is full and a new plan needs to be stacked, the intention execution fails.

#### Variables

`uint8_t` variables represent propositions to consume less memory and facilitate internal logic.

### Build

Compilation happens via a Makefile, which can be modified to include new compilation parameters or accommodate other compilers/SDKs. 