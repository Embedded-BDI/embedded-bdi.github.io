---
title: Vacuum Cleaner Example
sidebar: mydoc_sidebar
permalink: vacuum_cleaner_example.html
folder: mydoc
toc: false
---

<br>

> Note: To install the requirements and set up Embedded-BDI, see the [Get Started](/get_started.html) page.

<font size="+1"><b>This example is based on the <a href="http://jason.sourceforge.net/mini-tutorial/getting-started/#_exercise" target="_blank">Jason tutorial</a> and the source code is available <a href="./pages/files/simple_example.zip" target="_blank">here</a></b>.</font>

### Specification

Imagine a very simple environment formed by 4 locations (identified by 1, 2, 3, and 4) as in the figure below:

<center>{% include image.html file="example_application_env.png" alt="Jekyll" caption="" %}</center>

A vacuum-cleaner robot should be programmed in AgentSpeak to maintain the environment clean. The available actions for the robot are:

* `suck`: remove dirt at the robot’s position;
* `left`: move the left;
* `right`: move to right;
* `up`: move up;
* `down`: move down.

To help the robot decide what action to take, the following percepts are given:

* `dirty`: the robot is in a dirty location;
* `clean`: the robot is in a clean location;
* `pos_1`, `pos_2`, `pos_3`, `pos_4`: the location of the robot for positions 1, 2, 3, and 4.

### Programming the Agent Behavior

A possible solution for this problem, using AgentSpeak, is the code below:

<pre><code>// File: agentspeak.asl<br>
+pos_1 : dirty <- suck; right.
+pos_2 : dirty <- suck; down.
+pos_3 : dirty <- suck; up.
+pos_4 : dirty <- suck; left.<br>
+pos_1 : clean <- right.
+pos_2 : clean <- down.
+pos_3 : clean <- up.
+pos_4 : clean <- left.</code></pre>

In the code above, the agent has plans to check whether the position is clean or dirty. In case the position is dirty, the agent sucks the dirt and moves to the next position. However, if the position is clean, the vacuum simply moves to the next position.

> <small>The code above is a simple reactive solution to the problem, where the agent simply perceives the environment, takes action, if necessary, and then moves to the next position. Other solutions, much more sofisticated, are available [here](http://jason.sourceforge.net/mini-tutorial/getting-started/exercise-answers.txt). Note that, in the alternate solutions, the predicates must be replaced by the corresponding propositions from the current specification: `pos_1`, `pos_2`, `pos_3`, and `pos_4`).*</small>

### Programming the Agent Perception and Action in the Environment

Although the AgentSpeak code describes the behavior of the agent, it does not describe how the agent perceives and acts in the environment. For that, we must provide functions so the agent knows how to a) update its beliefs and b) act in the environment. The following C++ header code has the declaration of the functions needed:

<pre><code><div style="height:400px;overflow:auto;padding:3%">// File: functions.h<br>
#ifndef FUNCTIONS_H_
#define FUNCTIONS_H_<br>
#include &lt;cstdlib&gt;
#include &lt;ctime&gt;
#include &lt;iostream&gt;
#include &lt;unistd.h&gt;<br>
#define CYCLE_DELAY 1000000
#define SUCK_DELAY 50000<br>
// moves right (P1->P2)
bool action_right();<br>
// moves down (P2->P4)
bool action_down();<br>
// moves up (P3->P1)
bool action_up();<br>
// moves left (P4->P3)
bool action_left();<br>
// sucks dirty space
bool action_suck();<br>
// updates 'dirty' belief
bool update_dirty(bool var);<br>
// updates 'clean' belief
bool update_clean(bool var);<br>
// updates 'pos_1' belief
bool update_pos_1(bool var);<br>
// updates 'pos_2' belief
bool update_pos_2(bool var);<br>
// updates 'pos_3' belief
bool update_pos_3(bool var);<br>
// updates 'pos_4' belief
bool update_pos_4(bool var);<br>
#endif /* FUNCTIONS_H_ */</div></code></pre>

The definition of the functions is available below:

<pre><code><div style="height:400px;overflow:auto;padding:3%">// File: functions.cpp<br>
#include "functions.h"

// vacuum position
int pos = 1;
// dirty space
bool dirty = true;
// if program is running for the first time, initialize srand
bool first_run = true;

/*---------------------------------------------------------------------------*/

void randomize_dirty()
{
  if (first_run)
  {
    srand(time(NULL));
    first_run = false;
  }
  dirty = rand()%2;
}

/*---------------------------------------------------------------------------*/

bool action_right()
{
  std::cout << "Moving right...\t(P1->P2)" << std::endl;
  pos = 2;
  usleep(CYCLE_DELAY);
  randomize_dirty();
  return true;
}

bool action_down()
{
  std::cout << "Moving down...\t(P2->P4)" << std::endl;
  pos = 4;
  usleep(CYCLE_DELAY);
  randomize_dirty();
  return true;
}

bool action_up()
{
  std::cout << "Moving up...\t(P3->P1)" << std::endl;
  pos = 1;
  usleep(CYCLE_DELAY);
  randomize_dirty();
  return true;
}

bool action_left()
{
  std::cout << "Moving left...\t(P4->P3)" << std::endl;
  pos = 3;
  usleep(CYCLE_DELAY);
  randomize_dirty();
  return true;
}

bool action_suck()
{
  std::cout << "It's dirty!\tSucking..." << std::endl;
  usleep(CYCLE_DELAY);
  dirty = false;
  return true;
}

/*---------------------------------------------------------------------------*/

bool update_dirty(bool var)
{
  return dirty;
}

bool update_clean(bool var)
{
  return !dirty;
}

bool update_pos_1(bool var)
{
  if (pos == 1)
  {
    return true;
  }
  return false;
}

bool update_pos_2(bool var)
{
  
  if (pos == 2)
  {
    return true;
  }
  return false;
}

bool update_pos_3(bool var)
{
  if (pos == 3)
  {
    return true;
  }
  return false;
}

bool update_pos_4(bool var)
{
  if (pos == 4)
  {
    return true;
  }
  return false;
}</div></code></pre>

Note that the functions to act in the environment start with the `action_` prefix, while the functions that update beliefs start with `update_`. Also, all the functions follow the same return and argument types, as detailed in the [How it Works](./how_it_works.html) page.

### Configuring the Agent

We must set the size of the internal data structures of the agent: intention stack, event base, and intention base. Because no intentions have sub-goals or belief changes, the size of the intention stack can be set to 1, as only 1 plan will run for each intention. The size of the event base can be set to 4, since at most 4 events will exist in the event base at a time: two events for updating positions, one for seeing the position as dirty, and another seeing the position as not clean. Lastly, the size of the intention base can be set to 4, as at most 4 intentions will exist in the queue.

<pre><code>// File: agent.config<br>
EVENT_BASE_SIZE=4
INTENTION_BASE_SIZE=4
INTENTION_STACK_SIZE=1
</code></pre>

It is important to note that these values must be set by the programmer, so it is essential that we understand the behavior of the agent and configure the size of the data structures accordingly. This step is particularly sensitive for embedded systems, where computational resources are limited and memory must be managed wisely.

### Compiling the Agent

The complete source code, with all the files necessary to compile and run the agent, is available <a href="./pages/files/simple_example.zip" target="_blank">here</a>.

Now that we have the four files necessary to compile the agent, we can place them in the corresponding locations: `data/agentspeak.asl`, `data/functions.h`, `data/functions.cpp`, and `agent.config`.

To compile the agent, execute `make agent` at the root of the repository. To run the agent, run `./build/agent.out`. The output will show the agent moving from one location to another, sucking the dirt of each dirty location. Note that the dirt is randomly generated when the agent moves to a new location, so the output will not be the same as the one below.

<pre><code><div style="height:300px;overflow:auto;padding:3%">It's dirty!     Sucking...
Moving right... (P1->P2)
It's dirty!     Sucking...
Moving down...  (P2->P4)
Moving left...  (P4->P3)
It's dirty!     Sucking...
Moving up...    (P3->P1)
It's dirty!     Sucking...
Moving right... (P1->P2)
It's dirty!     Sucking...
Moving down...  (P2->P4)
Moving left...  (P4->P3)
It's dirty!     Sucking...
Moving up...    (P3->P1)
Moving right... (P1->P2)
It's dirty!     Sucking...
Moving down...  (P2->P4)
It's dirty!     Sucking...
Moving left...  (P4->P3)
It's dirty!     Sucking...
Moving up...    (P3->P1)
Moving right... (P1->P2)
It's dirty!     Sucking...
Moving down...  (P2->P4)
Moving left...  (P4->P3)
Moving up...    (P3->P1)
It's dirty!     Sucking...</div></code></pre>