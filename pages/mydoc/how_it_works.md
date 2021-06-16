---
title: How It Works
sidebar: mydoc_sidebar
permalink: how_it_works.html
folder: mydoc
---

A basic explanation of how the framework works is provided below. For a complete description of the design decisions when implementing the framework, see [Implementation Details](./implementation_details.html).

### Programming the Agent

Embedded-BDI implements the agent reasoning cycle and necessary libraries to support AgentSpeak syntax. Therefore, users can focus on programming the agent behavior. Three files are required to program and compile the agent:

* `data/agentspeak.asl`: defines the agent behavior, coded in AgentSpeak;
* `data/functions.h`: includes the belief update and action functions;
* `agent.config`: specifies the size of the following internal data structures of the agent: event base, intention base, and intention stack.

#### Agent Behavior

Agent behavior can be described in the `data/agentspeak.asl` file using AgentSpeak syntax. Be aware of the [limitations](./unsupported_features.html) of the framework, notably the lack of support for predicates. Details about the supported syntax for this file and agent features can be checked on the [Supported Features](./supported_features.html) page.

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseAgentBehavior" aria-expanded="false" aria-controls="collapseAgentBehavior">
    Example: agentspeak.asl
  </button>
</p>
<div class="collapse" id="collapseAgentBehavior">
  <div class="card card-body">
    <pre><code>!start.
+!start <- +happy.
+happy <- !!hello.
+!hello <- say_hello.</code></pre>
  </div>
</div>


#### Belief update and action functions

Because hardware used in embedded systems is heterogeneous and I/O interfaces can vary, users must provide the necessary functions to update beliefs and act in the environment. These functions should be provided in the `data/functions.h` file, and must follow specific syntax: the `update_` prefix must be used to associate the update function with its corresponding belief, and the `action_` prefix shall be used to link the function with its corresponding body instruction. Note that:
  * Both function types must respect its return type (`bool`) and arguments received (`bool var`, for `update_` functions);
  * While update functions are optional for beliefs, all actions should have a corresponding function.

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseFunctions" aria-expanded="false" aria-controls="collapseFunctions">
    Example: functions.h
  </button>
</p>
<div class="collapse" id="collapseFunctions">
  <div class="card card-body">
    <pre><code>#ifndef FUNCTIONS_H_
#define FUNCTIONS_H_<br>
#include &lt;iostream&gt;<br>
bool action_say_hello()
{
  std::cout << "Hello world!" << std::endl;
  std::cout << "I am an agent and I will keep running until " <<
               "I am terminated" << std::endl;
  return true;
}<br><br>
bool update_sunny(bool var)
{
  return true;
}<br>
#endif /* FUNCTIONS_H_ */</code></pre>
  </div>
</div>

#### Agent Configuration

Lastly, because of the limited resources available in embedded systems, memory usage must be pre-allocated and strictly managed for the internal data structures used by the agent. Hence, it is necessary to configure the size of the event and intention queues and the size of the intention stack. This can be done through the `agent.config` file.

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseConfig" aria-expanded="false" aria-controls="collapseConfig">
    Example: agent.config
  </button>
</p>
<div class="collapse" id="collapseConfig">
  <div class="card card-body">
    <pre><code>EVENT_BASE_SIZE=5
INTENTION_BASE_SIZE=5
INTENTION_STACK_SIZE=5</code></pre>
  </div>
</div>

### Compile agent executable

Once the three files are placed on their corresponding locations (`data/functions.h`, `data/agentspeak.asl`, and `agent.config`), the agent executable can be compiled. The build process is split into two phases:

1. AgentSpeak translation: first, the `data/agentspeak.asl` file is translated from the AgentSpeak language to a C++ header file (`src/config/configuration.h`).
2. Agent compilation: once the agent code is translated to a corresponding C++ header file, the agent can be compiled using the Embedded-BDI library and the files provided by the user.

This process is illustrated by the image below:

<center>{% include image.html file="compilation.png" caption="Steps to build agent" %}</center>

Instructions to build and run the agent are available [here](./build_run.html).