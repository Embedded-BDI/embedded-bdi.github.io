---
title: Build & Run
sidebar: mydoc_sidebar
permalink: build_run.html
folder: mydoc
---

<br>

## Build

A Makefile is available with the necessary targets to build the agent. More information about the each target is available below.

### translate

The `translate` target converts the AgentSpeak code file to the C++ header. This pre-compilation process is done through the Jason interpreter, so [OpenJDK](https://openjdk.java.net/) (or [Java SDK](https://www.oracle.com/java/technologies/javase-downloads.html)) must be installed.

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseTranslate" aria-expanded="false" aria-controls="collapseTranslate">
    Example: make translate
  </button>
</p>
<div class="collapse" id="collapseTranslate">
  <div class="card card-body">
    <pre><code>javac -cp lib/parser/lib/jason-2.6.jar                                    \
            lib/parser/src/translator/As2Json.java                          \
            lib/parser/src/translator/PlanSkeleton.java                     \
            lib/parser/src/translator/BodyInstruction.java                  \
            lib/parser/src/translator/EventOperatorType.java                \
            lib/parser/src/translator/HeaderCreator.java
java -cp lib/parser/lib/jason-2.6.jar:lib/parser/src                      \
           translator.As2Json                                               \
           data/agentspeak.asl                                              \
           data/functions.h                                                 \
           src/config/configuration.h                                       \
           5 5 5


Starting parsing to individual structures...

---

BELIEF:
Name:
        happy
Value:
        false

---

BELIEF:
Name:
        money
Value:
        false

---

BELIEF:
Name:
        sunny
Value:
        true

---

PLAN:
Operator:
        BELIEF_ADDITION
Statement:
        sunny
Context:
Body:
        [BELIEF, happy, BELIEF_ADDITION]

---

PLAN:
Operator:
        BELIEF_ADDITION
Statement:
        happy
Context:
        sunny
        money
Body:
        [ACTION, buy_ice_cream, null]</code></pre>
  </div>
</div>


### agent

Invokes the `translate` target, and subsequently compiles the agent, taking the `agent.config`, `data/functions.h`, `src/config/configuration.h`, and the Embedded-BDI library as input. The agent executable will be available at `build/agent.out`.

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseAgent" aria-expanded="false" aria-controls="collapseAgent">
    Example: make agent
  </button>
</p>
<div class="collapse" id="collapseAgent">
  <div class="card card-body">
    <pre><code>javac -cp lib/parser/lib/jason-2.6.jar                                    \
            lib/parser/src/translator/As2Json.java                          \
            lib/parser/src/translator/PlanSkeleton.java                     \
            lib/parser/src/translator/BodyInstruction.java                  \
            lib/parser/src/translator/EventOperatorType.java                \
            lib/parser/src/translator/HeaderCreator.java
java -cp lib/parser/lib/jason-2.6.jar:lib/parser/src                      \
           translator.As2Json                                               \
           data/agentspeak.asl                                              \
           data/functions.h                                                 \
           src/config/configuration.h                                       \
           5 5 5


Starting parsing to individual structures...

---

BELIEF:
Name:
        happy
Value:
        false

---

BELIEF:
Name:
        money
Value:
        false

---

BELIEF:
Name:
        sunny
Value:
        true

---

PLAN:
Operator:
        BELIEF_ADDITION
Statement:
        sunny
Context:
Body:
        [BELIEF, happy, BELIEF_ADDITION]

---

PLAN:
Operator:
        BELIEF_ADDITION
Statement:
        happy
Context:
        sunny
        money
Body:
        [ACTION, buy_ice_cream, null]
touch ./src/agent_loop.cpp
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/instantiated_plan.cpp -o build/./lib/bdi/instantiated_plan.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/belief_base.cpp -o build/./lib/bdi/belief_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/belief.cpp -o build/./lib/bdi/belief.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/plan_base.cpp -o build/./lib/bdi/plan_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/intention_base.cpp -o build/./lib/bdi/intention_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/intention.cpp -o build/./lib/bdi/intention.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/event.cpp -o build/./lib/bdi/event.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/event_base.cpp -o build/./lib/bdi/event_base.cpp.o
mkdir -p build/./lib/agent/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/agent/agent.cpp -o build/./lib/agent/agent.cpp.o
mkdir -p build/./lib/lib/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/lib/event_id.cpp -o build/./lib/lib/event_id.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body_return.cpp -o build/./lib/syntax/body_return.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/plan.cpp -o build/./lib/syntax/plan.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/context_condition.cpp -o build/./lib/syntax/context_condition.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/statement.cpp -o build/./lib/syntax/statement.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/context.cpp -o build/./lib/syntax/context.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body_instruction.cpp -o build/./lib/syntax/body_instruction.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body.cpp -o build/./lib/syntax/body.cpp.o
mkdir -p build/./src/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c src/agent_loop.cpp -o build/./src/agent_loop.cpp.o
g++ ./build/./lib/bdi/instantiated_plan.cpp.o ./build/./lib/bdi/belief_base.cpp.o ./build/./lib/bdi/belief.cpp.o ./build/./lib/bdi/plan_base.cpp.o ./build/./lib/bdi/intention_base.cpp.o ./build/./lib/bdi/intention.cpp.o ./build/./lib/bdi/event.cpp.o ./build/./lib/bdi/event_base.cpp.o ./build/./lib/agent/agent.cpp.o ./build/./lib/lib/event_id.cpp.o ./build/./lib/syntax/body_return.cpp.o ./build/./lib/syntax/plan.cpp.o ./build/./lib/syntax/context_condition.cpp.o ./build/./lib/syntax/statement.cpp.o ./build/./lib/syntax/context.cpp.o ./build/./lib/syntax/body_instruction.cpp.o ./build/./lib/syntax/body.cpp.o ./build/./src/agent_loop.cpp.o -o build/agent.out</code></pre>
  </div>
</div>

### clean

Deletes all existing builds generated by the Makefile.

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseClean" aria-expanded="false" aria-controls="collapseClean">
    Example: make clean
  </button>
</p>
<div class="collapse" id="collapseClean">
  <div class="card card-body">
    <pre><code>rm -f -r ./build
rm -f -r ./docs/html</code></pre>
  </div>
</div>

### tests

Compiles unit test suite for Embedded-BDI library. [GoogleTest](https://github.com/google/googletest) is used for testing. The executable will be available at `build/unitest.out`.

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseTests" aria-expanded="false" aria-controls="collapseTests">
    Example: make tests
  </button>
</p>
<div class="collapse" id="collapseTests">
  <div class="card card-body">
    <pre><code>mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/instantiated_plan.cpp -o build/./lib/bdi/instantiated_plan.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/belief_base.cpp -o build/./lib/bdi/belief_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/belief.cpp -o build/./lib/bdi/belief.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/plan_base.cpp -o build/./lib/bdi/plan_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/intention_base.cpp -o build/./lib/bdi/intention_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/intention.cpp -o build/./lib/bdi/intention.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/event.cpp -o build/./lib/bdi/event.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/event_base.cpp -o build/./lib/bdi/event_base.cpp.o
mkdir -p build/./lib/agent/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/agent/agent.cpp -o build/./lib/agent/agent.cpp.o
mkdir -p build/./lib/lib/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/lib/event_id.cpp -o build/./lib/lib/event_id.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body_return.cpp -o build/./lib/syntax/body_return.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/plan.cpp -o build/./lib/syntax/plan.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/context_condition.cpp -o build/./lib/syntax/context_condition.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/statement.cpp -o build/./lib/syntax/statement.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/context.cpp -o build/./lib/syntax/context.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body_instruction.cpp -o build/./lib/syntax/body_instruction.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body.cpp -o build/./lib/syntax/body.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/belief_base_test.cpp -o build/./test/utest/bdi/belief_base_test.cpp.o
mkdir -p build/./test/utest/bdi/intention_base_test/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/intention_base_test/intention_base_test.cpp -o build/./test/utest/bdi/intention_base_test/intention_base_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/instantiated_plan_test.cpp -o build/./test/utest/bdi/instantiated_plan_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/plan_base_test.cpp -o build/./test/utest/bdi/plan_base_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/belief_test.cpp -o build/./test/utest/bdi/belief_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/event_test.cpp -o build/./test/utest/bdi/event_test.cpp.o
mkdir -p build/./test/utest/bdi/intention_test/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/intention_test/intention_test.cpp -o build/./test/utest/bdi/intention_test/intention_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/event_base_test.cpp -o build/./test/utest/bdi/event_base_test.cpp.o
mkdir -p build/./test/utest/agent/nb_subgoal_full_intention_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/nb_subgoal_full_intention_base/nb_subgoal_full_intention_base.cpp -o build/./test/utest/agent/nb_subgoal_full_intention_base/nb_subgoal_full_intention_base.cpp.o
mkdir -p build/./test/utest/agent/empty_event_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/empty_event_base/empty_event_base_test.cpp -o build/./test/utest/agent/empty_event_base/empty_event_base_test.cpp.o
mkdir -p build/./test/utest/agent/overflow_intention_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/overflow_intention_base/overflow_intention_base_test.cpp -o build/./test/utest/agent/overflow_intention_base/overflow_intention_base_test.cpp.o
mkdir -p build/./test/utest/agent/overflow_stacked_intention/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/overflow_stacked_intention/overflow_stacked_intention.cpp -o build/./test/utest/agent/overflow_stacked_intention/overflow_stacked_intention.cpp.o
mkdir -p build/./test/utest/agent/simple_intention/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/simple_intention/simple_intention_test.cpp -o build/./test/utest/agent/simple_intention/simple_intention_test.cpp.o
mkdir -p build/./test/utest/agent/failed_intention/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/failed_intention/failed_intention_test.cpp -o build/./test/utest/agent/failed_intention/failed_intention_test.cpp.o
mkdir -p build/./test/utest/agent/nb_subgoal_failed_plan/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/nb_subgoal_failed_plan/nb_subgoal_failed_plan.cpp -o build/./test/utest/agent/nb_subgoal_failed_plan/nb_subgoal_failed_plan.cpp.o
mkdir -p build/./test/utest/agent/stacked_intention/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/stacked_intention/stacked_intention.cpp -o build/./test/utest/agent/stacked_intention/stacked_intention.cpp.o
mkdir -p build/./test/utest/agent/nb_subgoal_empty_intention_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/nb_subgoal_empty_intention_base/nb_subgoal_empty_intention_base.cpp -o build/./test/utest/agent/nb_subgoal_empty_intention_base/nb_subgoal_empty_intention_base.cpp.o
mkdir -p build/./test/utest/agent/fill_event_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/fill_event_base/fill_event_base_test.cpp -o build/./test/utest/agent/fill_event_base/fill_event_base_test.cpp.o
mkdir -p build/./test/utest/agent/multiple_intentions/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/multiple_intentions/multiple_intentions_test.cpp -o build/./test/utest/agent/multiple_intentions/multiple_intentions_test.cpp.o
mkdir -p build/./test/utest/lib/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/lib/event_id_test.cpp -o build/./test/utest/lib/event_id_test.cpp.o
mkdir -p build/./test/utest/lib/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/lib/vector_queue_test.cpp -o build/./test/utest/lib/vector_queue_test.cpp.o
mkdir -p build/./test/utest/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/AllTests.cpp -o build/./test/utest/AllTests.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/context_condition_test.cpp -o build/./test/utest/syntax/context_condition_test.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/statement_test.cpp -o build/./test/utest/syntax/statement_test.cpp.o
mkdir -p build/./test/utest/syntax/body_test/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/body_test/body_test.cpp -o build/./test/utest/syntax/body_test/body_test.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/body_instruction_test.cpp -o build/./test/utest/syntax/body_instruction_test.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/context_test.cpp -o build/./test/utest/syntax/context_test.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/body_return_test.cpp -o build/./test/utest/syntax/body_return_test.cpp.o
mkdir -p build/./test/utest/syntax/plan_test/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/plan_test/plan_test.cpp -o build/./test/utest/syntax/plan_test/plan_test.cpp.o
mkdir -p build/./test/external/gtest/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/external/gtest/gtest-all.cc -o build/./test/external/gtest/gtest-all.cc.o
g++ ./build/./lib/bdi/instantiated_plan.cpp.o ./build/./lib/bdi/belief_base.cpp.o ./build/./lib/bdi/belief.cpp.o ./build/./lib/bdi/plan_base.cpp.o ./build/./lib/bdi/intention_base.cpp.o ./build/./lib/bdi/intention.cpp.o ./build/./lib/bdi/event.cpp.o ./build/./lib/bdi/event_base.cpp.o ./build/./lib/agent/agent.cpp.o ./build/./lib/lib/event_id.cpp.o ./build/./lib/syntax/body_return.cpp.o ./build/./lib/syntax/plan.cpp.o ./build/./lib/syntax/context_condition.cpp.o ./build/./lib/syntax/statement.cpp.o ./build/./lib/syntax/context.cpp.o ./build/./lib/syntax/body_instruction.cpp.o ./build/./lib/syntax/body.cpp.o ./build/./test/utest/bdi/belief_base_test.cpp.o ./build/./test/utest/bdi/intention_base_test/intention_base_test.cpp.o ./build/./test/utest/bdi/instantiated_plan_test.cpp.o ./build/./test/utest/bdi/plan_base_test.cpp.o ./build/./test/utest/bdi/belief_test.cpp.o ./build/./test/utest/bdi/event_test.cpp.o ./build/./test/utest/bdi/intention_test/intention_test.cpp.o ./build/./test/utest/bdi/event_base_test.cpp.o ./build/./test/utest/agent/nb_subgoal_full_intention_base/nb_subgoal_full_intention_base.cpp.o ./build/./test/utest/agent/empty_event_base/empty_event_base_test.cpp.o ./build/./test/utest/agent/overflow_intention_base/overflow_intention_base_test.cpp.o ./build/./test/utest/agent/overflow_stacked_intention/overflow_stacked_intention.cpp.o ./build/./test/utest/agent/simple_intention/simple_intention_test.cpp.o ./build/./test/utest/agent/failed_intention/failed_intention_test.cpp.o ./build/./test/utest/agent/nb_subgoal_failed_plan/nb_subgoal_failed_plan.cpp.o ./build/./test/utest/agent/stacked_intention/stacked_intention.cpp.o ./build/./test/utest/agent/nb_subgoal_empty_intention_base/nb_subgoal_empty_intention_base.cpp.o ./build/./test/utest/agent/fill_event_base/fill_event_base_test.cpp.o ./build/./test/utest/agent/multiple_intentions/multiple_intentions_test.cpp.o ./build/./test/utest/lib/event_id_test.cpp.o ./build/./test/utest/lib/vector_queue_test.cpp.o ./build/./test/utest/AllTests.cpp.o ./build/./test/utest/syntax/context_condition_test.cpp.o ./build/./test/utest/syntax/statement_test.cpp.o ./build/./test/utest/syntax/body_test/body_test.cpp.o ./build/./test/utest/syntax/body_instruction_test.cpp.o ./build/./test/utest/syntax/context_test.cpp.o ./build/./test/utest/syntax/body_return_test.cpp.o ./build/./test/utest/syntax/plan_test/plan_test.cpp.o ./build/./test/external/gtest/gtest-all.cc.o -o build/unittest.out</code></pre>
  </div>
</div>

### valgrind

Checks code for memory leaks. Requires [valgrind](https://valgrind.org/). More information is available [here](/memory_leaks.html).

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseValgrind" aria-expanded="false" aria-controls="collapseValgrind">
    Example: make valgrind
  </button>
</p>
<div class="collapse" id="collapseValgrind">
  <div class="card card-body">
    <pre><code>mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/instantiated_plan.cpp -o build/./lib/bdi/instantiated_plan.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/belief_base.cpp -o build/./lib/bdi/belief_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/belief.cpp -o build/./lib/bdi/belief.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/plan_base.cpp -o build/./lib/bdi/plan_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/intention_base.cpp -o build/./lib/bdi/intention_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/intention.cpp -o build/./lib/bdi/intention.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/event.cpp -o build/./lib/bdi/event.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/event_base.cpp -o build/./lib/bdi/event_base.cpp.o
mkdir -p build/./lib/agent/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/agent/agent.cpp -o build/./lib/agent/agent.cpp.o
mkdir -p build/./lib/lib/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/lib/event_id.cpp -o build/./lib/lib/event_id.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body_return.cpp -o build/./lib/syntax/body_return.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/plan.cpp -o build/./lib/syntax/plan.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/context_condition.cpp -o build/./lib/syntax/context_condition.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/statement.cpp -o build/./lib/syntax/statement.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/context.cpp -o build/./lib/syntax/context.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body_instruction.cpp -o build/./lib/syntax/body_instruction.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body.cpp -o build/./lib/syntax/body.cpp.o
mkdir -p build/./valgrind/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c valgrind/valgrind.cpp -o build/./valgrind/valgrind.cpp.o
g++ ./build/./lib/bdi/instantiated_plan.cpp.o ./build/./lib/bdi/belief_base.cpp.o ./build/./lib/bdi/belief.cpp.o ./build/./lib/bdi/plan_base.cpp.o ./build/./lib/bdi/intention_base.cpp.o ./build/./lib/bdi/intention.cpp.o ./build/./lib/bdi/event.cpp.o ./build/./lib/bdi/event_base.cpp.o ./build/./lib/agent/agent.cpp.o ./build/./lib/lib/event_id.cpp.o ./build/./lib/syntax/body_return.cpp.o ./build/./lib/syntax/plan.cpp.o ./build/./lib/syntax/context_condition.cpp.o ./build/./lib/syntax/statement.cpp.o ./build/./lib/syntax/context.cpp.o ./build/./lib/syntax/body_instruction.cpp.o ./build/./lib/syntax/body.cpp.o ./build/./valgrind/valgrind.cpp.o -o build/valgrind.out</code></pre>
  </div>
</div>

### docs

Generates documentation for Embedded-BDI API. Requires [Doxygen](https://www.doxygen.nl/index.html) and [Graphviz](https://graphviz.org/). For more details, visit [API](/api.html).

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseDocs" aria-expanded="false" aria-controls="collapseDocs">
    Example: make docs
  </button>
</p>
<div class="collapse" id="collapseDocs">
  <div class="card card-body">
    <pre><code>if [ -d "/Applications/Doxygen.app/Contents/Resources" ];                 \
  then                                                                      \
    cd ./docs;                                                         \
    /Applications/Doxygen.app/Contents/Resources/doxygen Doxygen.doxyfile;  \
  else                                                                      \
    cd ./docs;                                                         \
    doxygen Doxygen.doxyfile;                                               \
  fi
warning: ignoring unsupported tag 'PYTHON_DOCSTRING' at line 236, file Doxygen.doxyfile
warning: ignoring unsupported tag 'NUM_PROC_THREADS' at line 471, file Doxygen.doxyfile
warning: ignoring unsupported tag 'HTML_FORMULA_FORMAT' at line 1599, file Doxygen.doxyfile
warning: the dot tool could not be found at /usr/local/bin/dot
Searching for include files...
Searching for example files...
Searching for images...
Searching for dot files...
Searching for msc files...
Searching for dia files...
Searching for files to exclude
Searching for files in directory /root/project/lib/parser
Searching for files in directory /root/project/lib/parser/lib
Searching for files in directory /root/project/lib/parser/src
Searching for files in directory /root/project/lib/parser/src/translator
Searching INPUT for files to process...
Searching for files in directory /root/project/lib
Searching for files in directory /root/project/lib/agent
Searching for files in directory /root/project/lib/bdi
Searching for files in directory /root/project/lib/lib
Searching for files in directory /root/project/lib/parser
Searching for files in directory /root/project/lib/parser/lib
Searching for files in directory /root/project/lib/parser/src
Searching for files in directory /root/project/lib/parser/src/translator
Searching for files in directory /root/project/lib/syntax
Reading and parsing tag files
Parsing files
Preprocessing /root/project/lib/agent/agent.cpp...
Parsing file /root/project/lib/agent/agent.cpp...
Preprocessing /root/project/lib/agent/agent.h...
Parsing file /root/project/lib/agent/agent.h...
Preprocessing /root/project/lib/bdi/belief.cpp...
Parsing file /root/project/lib/bdi/belief.cpp...
Preprocessing /root/project/lib/bdi/belief.h...
Parsing file /root/project/lib/bdi/belief.h...
Preprocessing /root/project/lib/bdi/belief_base.cpp...
Parsing file /root/project/lib/bdi/belief_base.cpp...
Preprocessing /root/project/lib/bdi/belief_base.h...
Parsing file /root/project/lib/bdi/belief_base.h...
Preprocessing /root/project/lib/bdi/event.cpp...
Parsing file /root/project/lib/bdi/event.cpp...
Preprocessing /root/project/lib/bdi/event.h...
Parsing file /root/project/lib/bdi/event.h...
Preprocessing /root/project/lib/bdi/event_base.cpp...
Parsing file /root/project/lib/bdi/event_base.cpp...
Preprocessing /root/project/lib/bdi/event_base.h...
Parsing file /root/project/lib/bdi/event_base.h...
Preprocessing /root/project/lib/bdi/instantiated_plan.cpp...
Parsing file /root/project/lib/bdi/instantiated_plan.cpp...
Preprocessing /root/project/lib/bdi/instantiated_plan.h...
Parsing file /root/project/lib/bdi/instantiated_plan.h...
Preprocessing /root/project/lib/bdi/intention.cpp...
Parsing file /root/project/lib/bdi/intention.cpp...
Preprocessing /root/project/lib/bdi/intention.h...
Parsing file /root/project/lib/bdi/intention.h...
Preprocessing /root/project/lib/bdi/intention_base.cpp...
Parsing file /root/project/lib/bdi/intention_base.cpp...
Preprocessing /root/project/lib/bdi/intention_base.h...
Parsing file /root/project/lib/bdi/intention_base.h...
Preprocessing /root/project/lib/bdi/plan_base.cpp...
Parsing file /root/project/lib/bdi/plan_base.cpp...
Preprocessing /root/project/lib/bdi/plan_base.h...
Parsing file /root/project/lib/bdi/plan_base.h...
Preprocessing /root/project/lib/lib/event_id.cpp...
Parsing file /root/project/lib/lib/event_id.cpp...
Preprocessing /root/project/lib/lib/event_id.h...
Parsing file /root/project/lib/lib/event_id.h...
Preprocessing /root/project/lib/lib/vector_queue.h...
Parsing file /root/project/lib/lib/vector_queue.h...
Preprocessing /root/project/lib/syntax/body.cpp...
Parsing file /root/project/lib/syntax/body.cpp...
Preprocessing /root/project/lib/syntax/body.h...
Parsing file /root/project/lib/syntax/body.h...
Preprocessing /root/project/lib/syntax/body_instruction.cpp...
Parsing file /root/project/lib/syntax/body_instruction.cpp...
Preprocessing /root/project/lib/syntax/body_instruction.h...
Parsing file /root/project/lib/syntax/body_instruction.h...
Preprocessing /root/project/lib/syntax/body_return.cpp...
Parsing file /root/project/lib/syntax/body_return.cpp...
Preprocessing /root/project/lib/syntax/body_return.h...
Parsing file /root/project/lib/syntax/body_return.h...
Preprocessing /root/project/lib/syntax/body_type.h...
Parsing file /root/project/lib/syntax/body_type.h...
Preprocessing /root/project/lib/syntax/context.cpp...
Parsing file /root/project/lib/syntax/context.cpp...
Preprocessing /root/project/lib/syntax/context.h...
Parsing file /root/project/lib/syntax/context.h...
Preprocessing /root/project/lib/syntax/context_condition.cpp...
Parsing file /root/project/lib/syntax/context_condition.cpp...
Preprocessing /root/project/lib/syntax/context_condition.h...
Parsing file /root/project/lib/syntax/context_condition.h...
Preprocessing /root/project/lib/syntax/event_operator.h...
Parsing file /root/project/lib/syntax/event_operator.h...
Preprocessing /root/project/lib/syntax/plan.cpp...
Parsing file /root/project/lib/syntax/plan.cpp...
Preprocessing /root/project/lib/syntax/plan.h...
Parsing file /root/project/lib/syntax/plan.h...
Preprocessing /root/project/lib/syntax/statement.cpp...
Parsing file /root/project/lib/syntax/statement.cpp...
Preprocessing /root/project/lib/syntax/statement.h...
Parsing file /root/project/lib/syntax/statement.h...
Reading /root/project/README.md...
Preprocessing /root/project/src/agent_loop.cpp...
Parsing file /root/project/src/agent_loop.cpp...
Building group list...
Building directory list...
Building namespace list...
Building file list...
Building class list...
Computing nesting relations for classes...
Associating documentation with classes...
Building example list...
Searching for enumerations...
Searching for documented typedefs...
Searching for members imported via using declarations...
Searching for included using directives...
Searching for documented variables...
Building interface member list...
Building member list...
Searching for friends...
Searching for documented defines...
Computing class inheritance relations...
Computing class usage relations...
Flushing cached template relations that have become invalid...
Computing class relations...
Add enum values to enums...
Searching for member function documentation...
Creating members for template instances...
Building page list...
Search for main page...
Computing page relations...
Determining the scope of groups...
Sorting lists...
Determining which enums are documented
Computing member relations...
Building full member lists recursively...
Adding members to member groups.
Computing member references...
Inheriting documentation...
Generating disk names...
Adding source references...
Adding xrefitems...
Sorting member lists...
Setting anonymous enum type...
Computing dependencies between directories...
Generating citations page...
Counting members...
Counting data structures...
Resolving user defined references...
Finding anchors and sections in the documentation...
Transferring function references...
Combining using relations...
Adding members to index pages...
Correcting members for VHDL...
Generating style sheet...
Generating search indices...
Generating example documentation...
Generating file sources...
Generating file documentation...
Generating docs for file agent.cpp...
Generating docs for file agent.h...
Generating docs for file agent_loop.cpp...
Generating docs for file belief.cpp...
Generating docs for file belief.h...
Generating docs for file belief_base.cpp...
Generating docs for file belief_base.h...
Generating docs for file body.cpp...
Generating docs for file body.h...
Generating docs for file body_instruction.cpp...
Generating docs for file body_instruction.h...
Generating docs for file body_return.cpp...
Generating docs for file body_return.h...
Generating docs for file body_type.h...
Generating docs for file context.cpp...
Generating docs for file context.h...
Generating docs for file context_condition.cpp...
Generating docs for file context_condition.h...
Generating docs for file event.cpp...
Generating docs for file event.h...
Generating docs for file event_base.cpp...
Generating docs for file event_base.h...
Generating docs for file event_id.cpp...
Generating docs for file event_id.h...
Generating docs for file event_operator.h...
Generating docs for file instantiated_plan.cpp...
Generating docs for file instantiated_plan.h...
Generating docs for file intention.cpp...
Generating docs for file intention.h...
Generating docs for file intention_base.cpp...
Generating docs for file intention_base.h...
Generating docs for file plan.cpp...
Generating docs for file plan.h...
Generating docs for file plan_base.cpp...
Generating docs for file plan_base.h...
Generating docs for file README.md...
Generating docs for file statement.cpp...
Generating docs for file statement.h...
Generating docs for file vector_queue.h...
Generating page documentation...
Generating group documentation...
Generating class documentation...
Generating docs for compound Agent...
Generating docs for compound Belief...
Generating docs for compound BeliefBase...
Generating docs for compound Body...
Generating docs for compound BodyInstruction...
Generating docs for compound BodyReturn...
Generating docs for compound Context...
Generating docs for compound ContextCondition...
Generating docs for compound Event...
Generating docs for compound EventBase...
Generating docs for compound EventID...
Generating docs for compound InstantiatedPlan...
Generating docs for compound Intention...
/root/project/lib/bdi/intention.h:37: warning: The following parameter of Intention::suspend(EventID *event_id, EventBase *events) is not documented:
  parameter 'events'
Generating docs for compound IntentionBase...
Generating docs for compound Plan...
Generating docs for compound PlanBase...
Generating docs for compound Statement...
Generating docs for compound VectorQueue...
Generating namespace index...
Generating graph info page...
Generating directory documentation...
Generating dependency graph for directory agent
Generating dependency graph for directory bdi
Generating dependency graph for directory lib
Generating dependency graph for directory src
Generating dependency graph for directory syntax
Generating index page...
Generating page index...
Generating module index...
Generating namespace index...
Generating namespace member index...
Generating annotated compound index...
Generating alphabetical compound index...
Generating hierarchical class index...
Generating graphical class hierarchy...
Generating member index...
Generating file index...
Generating file member index...
Generating example index...
finalizing index lists...
writing tag file...
Running plantuml with JAVA...
Running dot...
Generating dot graphs using 37 parallel threads...
Running dot for graph 1/81
Running dot for graph 2/81
Running dot for graph 3/81
Running dot for graph 4/81
Running dot for graph 5/81
Running dot for graph 6/81
Running dot for graph 7/81
Running dot for graph 8/81
Running dot for graph 9/81
Running dot for graph 10/81
Running dot for graph 11/81
Running dot for graph 12/81
Running dot for graph 13/81
Running dot for graph 14/81
Running dot for graph 15/81
Running dot for graph 16/81
Running dot for graph 17/81
Running dot for graph 18/81
Running dot for graph 19/81
Running dot for graph 20/81
Running dot for graph 21/81
Running dot for graph 22/81
Running dot for graph 23/81
Running dot for graph 24/81
Running dot for graph 25/81
Running dot for graph 26/81
Running dot for graph 27/81
Running dot for graph 28/81
Running dot for graph 29/81
Running dot for graph 30/81
Running dot for graph 31/81
Running dot for graph 32/81
Running dot for graph 33/81
Running dot for graph 34/81
Running dot for graph 35/81
Running dot for graph 36/81
Running dot for graph 37/81
Running dot for graph 38/81
Running dot for graph 39/81
Running dot for graph 40/81
Running dot for graph 41/81
Running dot for graph 42/81
Running dot for graph 43/81
Running dot for graph 44/81
Running dot for graph 45/81
Running dot for graph 46/81
Running dot for graph 47/81
Running dot for graph 48/81
Running dot for graph 49/81
Running dot for graph 50/81
Running dot for graph 51/81
Running dot for graph 52/81
Running dot for graph 53/81
Running dot for graph 54/81
Running dot for graph 55/81
Running dot for graph 56/81
Running dot for graph 57/81
Running dot for graph 58/81
Running dot for graph 59/81
Running dot for graph 60/81
Running dot for graph 61/81
Running dot for graph 62/81
Running dot for graph 63/81
Running dot for graph 64/81
Running dot for graph 65/81
Running dot for graph 66/81
Running dot for graph 67/81
Running dot for graph 68/81
Running dot for graph 69/81
Running dot for graph 70/81
Running dot for graph 71/81
Running dot for graph 72/81
Running dot for graph 73/81
Running dot for graph 74/81
Running dot for graph 75/81
Running dot for graph 76/81
Running dot for graph 77/81
Running dot for graph 78/81
Running dot for graph 79/81
Running dot for graph 80/81
Running dot for graph 81/81
Patching output file 1/142
Patching output file 2/142
Patching output file 3/142
Patching output file 4/142
Patching output file 5/142
Patching output file 6/142
Patching output file 7/142
Patching output file 8/142
Patching output file 9/142
Patching output file 10/142
Patching output file 11/142
Patching output file 12/142
Patching output file 13/142
Patching output file 14/142
Patching output file 15/142
Patching output file 16/142
Patching output file 17/142
Patching output file 18/142
Patching output file 19/142
Patching output file 20/142
Patching output file 21/142
Patching output file 22/142
Patching output file 23/142
Patching output file 24/142
Patching output file 25/142
Patching output file 26/142
Patching output file 27/142
Patching output file 28/142
Patching output file 29/142
Patching output file 30/142
Patching output file 31/142
Patching output file 32/142
Patching output file 33/142
Patching output file 34/142
Patching output file 35/142
Patching output file 36/142
Patching output file 37/142
Patching output file 38/142
Patching output file 39/142
Patching output file 40/142
Patching output file 41/142
Patching output file 42/142
Patching output file 43/142
Patching output file 44/142
Patching output file 45/142
Patching output file 46/142
Patching output file 47/142
Patching output file 48/142
Patching output file 49/142
Patching output file 50/142
Patching output file 51/142
Patching output file 52/142
Patching output file 53/142
Patching output file 54/142
Patching output file 55/142
Patching output file 56/142
Patching output file 57/142
Patching output file 58/142
Patching output file 59/142
Patching output file 60/142
Patching output file 61/142
Patching output file 62/142
Patching output file 63/142
Patching output file 64/142
Patching output file 65/142
Patching output file 66/142
Patching output file 67/142
Patching output file 68/142
Patching output file 69/142
Patching output file 70/142
Patching output file 71/142
Patching output file 72/142
Patching output file 73/142
Patching output file 74/142
Patching output file 75/142
Patching output file 76/142
Patching output file 77/142
Patching output file 78/142
Patching output file 79/142
Patching output file 80/142
Patching output file 81/142
Patching output file 82/142
Patching output file 83/142
Patching output file 84/142
Patching output file 85/142
Patching output file 86/142
Patching output file 87/142
Patching output file 88/142
Patching output file 89/142
Patching output file 90/142
Patching output file 91/142
Patching output file 92/142
Patching output file 93/142
Patching output file 94/142
Patching output file 95/142
Patching output file 96/142
Patching output file 97/142
Patching output file 98/142
Patching output file 99/142
Patching output file 100/142
Patching output file 101/142
Patching output file 102/142
Patching output file 103/142
Patching output file 104/142
Patching output file 105/142
Patching output file 106/142
Patching output file 107/142
Patching output file 108/142
Patching output file 109/142
Patching output file 110/142
Patching output file 111/142
Patching output file 112/142
Patching output file 113/142
Patching output file 114/142
Patching output file 115/142
Patching output file 116/142
Patching output file 117/142
Patching output file 118/142
Patching output file 119/142
Patching output file 120/142
Patching output file 121/142
Patching output file 122/142
Patching output file 123/142
Patching output file 124/142
Patching output file 125/142
Patching output file 126/142
Patching output file 127/142
Patching output file 128/142
Patching output file 129/142
Patching output file 130/142
Patching output file 131/142
Patching output file 132/142
Patching output file 133/142
Patching output file 134/142
Patching output file 135/142
Patching output file 136/142
Patching output file 137/142
Patching output file 138/142
Patching output file 139/142
Patching output file 140/142
Patching output file 141/142
Patching output file 142/142
lookup cache used 259/65536 hits=1060 misses=279
finished...</code></pre>
  </div>
</div>

### all

Invokes the targets `tests`, `valgrind`, `agent`, and `docs`.

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseAll" aria-expanded="false" aria-controls="collapseAll">
    Example: make all
  </button>
</p>
<div class="collapse" id="collapseAll">
  <div class="card card-body">
    <pre><code>mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/instantiated_plan.cpp -o build/./lib/bdi/instantiated_plan.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/belief_base.cpp -o build/./lib/bdi/belief_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/belief.cpp -o build/./lib/bdi/belief.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/plan_base.cpp -o build/./lib/bdi/plan_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/intention_base.cpp -o build/./lib/bdi/intention_base.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/intention.cpp -o build/./lib/bdi/intention.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/event.cpp -o build/./lib/bdi/event.cpp.o
mkdir -p build/./lib/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/bdi/event_base.cpp -o build/./lib/bdi/event_base.cpp.o
mkdir -p build/./lib/agent/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/agent/agent.cpp -o build/./lib/agent/agent.cpp.o
mkdir -p build/./lib/lib/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/lib/event_id.cpp -o build/./lib/lib/event_id.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body_return.cpp -o build/./lib/syntax/body_return.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/plan.cpp -o build/./lib/syntax/plan.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/context_condition.cpp -o build/./lib/syntax/context_condition.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/statement.cpp -o build/./lib/syntax/statement.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/context.cpp -o build/./lib/syntax/context.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body_instruction.cpp -o build/./lib/syntax/body_instruction.cpp.o
mkdir -p build/./lib/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c lib/syntax/body.cpp -o build/./lib/syntax/body.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/belief_base_test.cpp -o build/./test/utest/bdi/belief_base_test.cpp.o
mkdir -p build/./test/utest/bdi/intention_base_test/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/intention_base_test/intention_base_test.cpp -o build/./test/utest/bdi/intention_base_test/intention_base_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/instantiated_plan_test.cpp -o build/./test/utest/bdi/instantiated_plan_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/plan_base_test.cpp -o build/./test/utest/bdi/plan_base_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/belief_test.cpp -o build/./test/utest/bdi/belief_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/event_test.cpp -o build/./test/utest/bdi/event_test.cpp.o
mkdir -p build/./test/utest/bdi/intention_test/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/intention_test/intention_test.cpp -o build/./test/utest/bdi/intention_test/intention_test.cpp.o
mkdir -p build/./test/utest/bdi/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/bdi/event_base_test.cpp -o build/./test/utest/bdi/event_base_test.cpp.o
mkdir -p build/./test/utest/agent/nb_subgoal_full_intention_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/nb_subgoal_full_intention_base/nb_subgoal_full_intention_base.cpp -o build/./test/utest/agent/nb_subgoal_full_intention_base/nb_subgoal_full_intention_base.cpp.o
mkdir -p build/./test/utest/agent/empty_event_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/empty_event_base/empty_event_base_test.cpp -o build/./test/utest/agent/empty_event_base/empty_event_base_test.cpp.o
mkdir -p build/./test/utest/agent/overflow_intention_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/overflow_intention_base/overflow_intention_base_test.cpp -o build/./test/utest/agent/overflow_intention_base/overflow_intention_base_test.cpp.o
mkdir -p build/./test/utest/agent/overflow_stacked_intention/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/overflow_stacked_intention/overflow_stacked_intention.cpp -o build/./test/utest/agent/overflow_stacked_intention/overflow_stacked_intention.cpp.o
mkdir -p build/./test/utest/agent/simple_intention/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/simple_intention/simple_intention_test.cpp -o build/./test/utest/agent/simple_intention/simple_intention_test.cpp.o
mkdir -p build/./test/utest/agent/failed_intention/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/failed_intention/failed_intention_test.cpp -o build/./test/utest/agent/failed_intention/failed_intention_test.cpp.o
mkdir -p build/./test/utest/agent/nb_subgoal_failed_plan/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/nb_subgoal_failed_plan/nb_subgoal_failed_plan.cpp -o build/./test/utest/agent/nb_subgoal_failed_plan/nb_subgoal_failed_plan.cpp.o
mkdir -p build/./test/utest/agent/stacked_intention/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/stacked_intention/stacked_intention.cpp -o build/./test/utest/agent/stacked_intention/stacked_intention.cpp.o
mkdir -p build/./test/utest/agent/nb_subgoal_empty_intention_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/nb_subgoal_empty_intention_base/nb_subgoal_empty_intention_base.cpp -o build/./test/utest/agent/nb_subgoal_empty_intention_base/nb_subgoal_empty_intention_base.cpp.o
mkdir -p build/./test/utest/agent/fill_event_base/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/fill_event_base/fill_event_base_test.cpp -o build/./test/utest/agent/fill_event_base/fill_event_base_test.cpp.o
mkdir -p build/./test/utest/agent/multiple_intentions/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/agent/multiple_intentions/multiple_intentions_test.cpp -o build/./test/utest/agent/multiple_intentions/multiple_intentions_test.cpp.o
mkdir -p build/./test/utest/lib/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/lib/event_id_test.cpp -o build/./test/utest/lib/event_id_test.cpp.o
mkdir -p build/./test/utest/lib/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/lib/vector_queue_test.cpp -o build/./test/utest/lib/vector_queue_test.cpp.o
mkdir -p build/./test/utest/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/AllTests.cpp -o build/./test/utest/AllTests.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/context_condition_test.cpp -o build/./test/utest/syntax/context_condition_test.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/statement_test.cpp -o build/./test/utest/syntax/statement_test.cpp.o
mkdir -p build/./test/utest/syntax/body_test/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/body_test/body_test.cpp -o build/./test/utest/syntax/body_test/body_test.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/body_instruction_test.cpp -o build/./test/utest/syntax/body_instruction_test.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/context_test.cpp -o build/./test/utest/syntax/context_test.cpp.o
mkdir -p build/./test/utest/syntax/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/body_return_test.cpp -o build/./test/utest/syntax/body_return_test.cpp.o
mkdir -p build/./test/utest/syntax/plan_test/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/utest/syntax/plan_test/plan_test.cpp -o build/./test/utest/syntax/plan_test/plan_test.cpp.o
mkdir -p build/./test/external/gtest/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c test/external/gtest/gtest-all.cc -o build/./test/external/gtest/gtest-all.cc.o
g++ ./build/./lib/bdi/instantiated_plan.cpp.o ./build/./lib/bdi/belief_base.cpp.o ./build/./lib/bdi/belief.cpp.o ./build/./lib/bdi/plan_base.cpp.o ./build/./lib/bdi/intention_base.cpp.o ./build/./lib/bdi/intention.cpp.o ./build/./lib/bdi/event.cpp.o ./build/./lib/bdi/event_base.cpp.o ./build/./lib/agent/agent.cpp.o ./build/./lib/lib/event_id.cpp.o ./build/./lib/syntax/body_return.cpp.o ./build/./lib/syntax/plan.cpp.o ./build/./lib/syntax/context_condition.cpp.o ./build/./lib/syntax/statement.cpp.o ./build/./lib/syntax/context.cpp.o ./build/./lib/syntax/body_instruction.cpp.o ./build/./lib/syntax/body.cpp.o ./build/./test/utest/bdi/belief_base_test.cpp.o ./build/./test/utest/bdi/intention_base_test/intention_base_test.cpp.o ./build/./test/utest/bdi/instantiated_plan_test.cpp.o ./build/./test/utest/bdi/plan_base_test.cpp.o ./build/./test/utest/bdi/belief_test.cpp.o ./build/./test/utest/bdi/event_test.cpp.o ./build/./test/utest/bdi/intention_test/intention_test.cpp.o ./build/./test/utest/bdi/event_base_test.cpp.o ./build/./test/utest/agent/nb_subgoal_full_intention_base/nb_subgoal_full_intention_base.cpp.o ./build/./test/utest/agent/empty_event_base/empty_event_base_test.cpp.o ./build/./test/utest/agent/overflow_intention_base/overflow_intention_base_test.cpp.o ./build/./test/utest/agent/overflow_stacked_intention/overflow_stacked_intention.cpp.o ./build/./test/utest/agent/simple_intention/simple_intention_test.cpp.o ./build/./test/utest/agent/failed_intention/failed_intention_test.cpp.o ./build/./test/utest/agent/nb_subgoal_failed_plan/nb_subgoal_failed_plan.cpp.o ./build/./test/utest/agent/stacked_intention/stacked_intention.cpp.o ./build/./test/utest/agent/nb_subgoal_empty_intention_base/nb_subgoal_empty_intention_base.cpp.o ./build/./test/utest/agent/fill_event_base/fill_event_base_test.cpp.o ./build/./test/utest/agent/multiple_intentions/multiple_intentions_test.cpp.o ./build/./test/utest/lib/event_id_test.cpp.o ./build/./test/utest/lib/vector_queue_test.cpp.o ./build/./test/utest/AllTests.cpp.o ./build/./test/utest/syntax/context_condition_test.cpp.o ./build/./test/utest/syntax/statement_test.cpp.o ./build/./test/utest/syntax/body_test/body_test.cpp.o ./build/./test/utest/syntax/body_instruction_test.cpp.o ./build/./test/utest/syntax/context_test.cpp.o ./build/./test/utest/syntax/body_return_test.cpp.o ./build/./test/utest/syntax/plan_test/plan_test.cpp.o ./build/./test/external/gtest/gtest-all.cc.o -o build/unittest.out 
mkdir -p build/./valgrind/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c valgrind/valgrind.cpp -o build/./valgrind/valgrind.cpp.o
g++ ./build/./lib/bdi/instantiated_plan.cpp.o ./build/./lib/bdi/belief_base.cpp.o ./build/./lib/bdi/belief.cpp.o ./build/./lib/bdi/plan_base.cpp.o ./build/./lib/bdi/intention_base.cpp.o ./build/./lib/bdi/intention.cpp.o ./build/./lib/bdi/event.cpp.o ./build/./lib/bdi/event_base.cpp.o ./build/./lib/agent/agent.cpp.o ./build/./lib/lib/event_id.cpp.o ./build/./lib/syntax/body_return.cpp.o ./build/./lib/syntax/plan.cpp.o ./build/./lib/syntax/context_condition.cpp.o ./build/./lib/syntax/statement.cpp.o ./build/./lib/syntax/context.cpp.o ./build/./lib/syntax/body_instruction.cpp.o ./build/./lib/syntax/body.cpp.o ./build/./valgrind/valgrind.cpp.o -o build/valgrind.out 
javac -cp lib/parser/lib/jason-2.6.jar                                    \
            lib/parser/src/translator/As2Json.java                          \
            lib/parser/src/translator/PlanSkeleton.java                     \
            lib/parser/src/translator/BodyInstruction.java                  \
            lib/parser/src/translator/EventOperatorType.java                \
            lib/parser/src/translator/HeaderCreator.java
java -cp lib/parser/lib/jason-2.6.jar:lib/parser/src                      \
           translator.As2Json                                               \
           data/agentspeak.asl                                              \
           data/functions.h                                                 \
           src/config/configuration.h                                       \
           5 5 5


Starting parsing to individual structures...

---

BELIEF:
Name:
        happy
Value:
        false

---

BELIEF:
Name:
        money
Value:
        false

---

BELIEF:
Name:
        sunny
Value:
        true

---

PLAN:
Operator:
        BELIEF_ADDITION
Statement:
        sunny
Context:
Body:
        [BELIEF, happy, BELIEF_ADDITION]

---

PLAN:
Operator:
        BELIEF_ADDITION
Statement:
        happy
Context:
        sunny
        money
Body:
        [ACTION, buy_ice_cream, null]
touch ./src/agent_loop.cpp
mkdir -p build/./src/
g++ -std=c++11 -Os -Wall -DGTEST_HAS_PTHREAD=0   -I./lib -I./lib/bdi -I./lib/agent -I./lib/parser -I./lib/parser/lib -I./lib/parser/src -I./lib/parser/src/translator -I./lib/lib -I./lib/syntax -I./test -I./test/utest -I./test/utest/bdi -I./test/utest/bdi/intention_base_test -I./test/utest/bdi/intention_test -I./test/utest/agent -I./test/utest/agent/nb_subgoal_full_intention_base -I./test/utest/agent/empty_event_base -I./test/utest/agent/overflow_intention_base -I./test/utest/agent/overflow_stacked_intention -I./test/utest/agent/simple_intention -I./test/utest/agent/failed_intention -I./test/utest/agent/nb_subgoal_failed_plan -I./test/utest/agent/stacked_intention -I./test/utest/agent/nb_subgoal_empty_intention_base -I./test/utest/agent/fill_event_base -I./test/utest/agent/multiple_intentions -I./test/utest/lib -I./test/utest/syntax -I./test/utest/syntax/body_test -I./test/utest/syntax/plan_test -I./test/test_lib -I./test/external -I./test/external/gtest  -c src/agent_loop.cpp -o build/./src/agent_loop.cpp.o
g++ ./build/./lib/bdi/instantiated_plan.cpp.o ./build/./lib/bdi/belief_base.cpp.o ./build/./lib/bdi/belief.cpp.o ./build/./lib/bdi/plan_base.cpp.o ./build/./lib/bdi/intention_base.cpp.o ./build/./lib/bdi/intention.cpp.o ./build/./lib/bdi/event.cpp.o ./build/./lib/bdi/event_base.cpp.o ./build/./lib/agent/agent.cpp.o ./build/./lib/lib/event_id.cpp.o ./build/./lib/syntax/body_return.cpp.o ./build/./lib/syntax/plan.cpp.o ./build/./lib/syntax/context_condition.cpp.o ./build/./lib/syntax/statement.cpp.o ./build/./lib/syntax/context.cpp.o ./build/./lib/syntax/body_instruction.cpp.o ./build/./lib/syntax/body.cpp.o ./build/./src/agent_loop.cpp.o -o build/agent.out
if [ -d "/Applications/Doxygen.app/Contents/Resources" ];                 \
  then                                                                      \
    cd ./docs;                                                         \
    /Applications/Doxygen.app/Contents/Resources/doxygen Doxygen.doxyfile;  \
  else                                                                      \
    cd ./docs;                                                         \
    doxygen Doxygen.doxyfile;                                               \
  fi
warning: ignoring unsupported tag 'PYTHON_DOCSTRING' at line 236, file Doxygen.doxyfile
warning: ignoring unsupported tag 'NUM_PROC_THREADS' at line 471, file Doxygen.doxyfile
warning: ignoring unsupported tag 'HTML_FORMULA_FORMAT' at line 1599, file Doxygen.doxyfile
warning: the dot tool could not be found at /usr/local/bin/dot
Searching for include files...
Searching for example files...
Searching for images...
Searching for dot files...
Searching for msc files...
Searching for dia files...
Searching for files to exclude
Searching for files in directory /root/project/lib/parser
Searching for files in directory /root/project/lib/parser/lib
Searching for files in directory /root/project/lib/parser/src
Searching for files in directory /root/project/lib/parser/src/translator
Searching INPUT for files to process...
Searching for files in directory /root/project/lib
Searching for files in directory /root/project/lib/agent
Searching for files in directory /root/project/lib/bdi
Searching for files in directory /root/project/lib/lib
Searching for files in directory /root/project/lib/parser
Searching for files in directory /root/project/lib/parser/lib
Searching for files in directory /root/project/lib/parser/src
Searching for files in directory /root/project/lib/parser/src/translator
Searching for files in directory /root/project/lib/syntax
Reading and parsing tag files
Parsing files
Preprocessing /root/project/lib/agent/agent.cpp...
Parsing file /root/project/lib/agent/agent.cpp...
Preprocessing /root/project/lib/agent/agent.h...
Parsing file /root/project/lib/agent/agent.h...
Preprocessing /root/project/lib/bdi/belief.cpp...
Parsing file /root/project/lib/bdi/belief.cpp...
Preprocessing /root/project/lib/bdi/belief.h...
Parsing file /root/project/lib/bdi/belief.h...
Preprocessing /root/project/lib/bdi/belief_base.cpp...
Parsing file /root/project/lib/bdi/belief_base.cpp...
Preprocessing /root/project/lib/bdi/belief_base.h...
Parsing file /root/project/lib/bdi/belief_base.h...
Preprocessing /root/project/lib/bdi/event.cpp...
Parsing file /root/project/lib/bdi/event.cpp...
Preprocessing /root/project/lib/bdi/event.h...
Parsing file /root/project/lib/bdi/event.h...
Preprocessing /root/project/lib/bdi/event_base.cpp...
Parsing file /root/project/lib/bdi/event_base.cpp...
Preprocessing /root/project/lib/bdi/event_base.h...
Parsing file /root/project/lib/bdi/event_base.h...
Preprocessing /root/project/lib/bdi/instantiated_plan.cpp...
Parsing file /root/project/lib/bdi/instantiated_plan.cpp...
Preprocessing /root/project/lib/bdi/instantiated_plan.h...
Parsing file /root/project/lib/bdi/instantiated_plan.h...
Preprocessing /root/project/lib/bdi/intention.cpp...
Parsing file /root/project/lib/bdi/intention.cpp...
Preprocessing /root/project/lib/bdi/intention.h...
Parsing file /root/project/lib/bdi/intention.h...
Preprocessing /root/project/lib/bdi/intention_base.cpp...
Parsing file /root/project/lib/bdi/intention_base.cpp...
Preprocessing /root/project/lib/bdi/intention_base.h...
Parsing file /root/project/lib/bdi/intention_base.h...
Preprocessing /root/project/lib/bdi/plan_base.cpp...
Parsing file /root/project/lib/bdi/plan_base.cpp...
Preprocessing /root/project/lib/bdi/plan_base.h...
Parsing file /root/project/lib/bdi/plan_base.h...
Preprocessing /root/project/lib/lib/event_id.cpp...
Parsing file /root/project/lib/lib/event_id.cpp...
Preprocessing /root/project/lib/lib/event_id.h...
Parsing file /root/project/lib/lib/event_id.h...
Preprocessing /root/project/lib/lib/vector_queue.h...
Parsing file /root/project/lib/lib/vector_queue.h...
Preprocessing /root/project/lib/syntax/body.cpp...
Parsing file /root/project/lib/syntax/body.cpp...
Preprocessing /root/project/lib/syntax/body.h...
Parsing file /root/project/lib/syntax/body.h...
Preprocessing /root/project/lib/syntax/body_instruction.cpp...
Parsing file /root/project/lib/syntax/body_instruction.cpp...
Preprocessing /root/project/lib/syntax/body_instruction.h...
Parsing file /root/project/lib/syntax/body_instruction.h...
Preprocessing /root/project/lib/syntax/body_return.cpp...
Parsing file /root/project/lib/syntax/body_return.cpp...
Preprocessing /root/project/lib/syntax/body_return.h...
Parsing file /root/project/lib/syntax/body_return.h...
Preprocessing /root/project/lib/syntax/body_type.h...
Parsing file /root/project/lib/syntax/body_type.h...
Preprocessing /root/project/lib/syntax/context.cpp...
Parsing file /root/project/lib/syntax/context.cpp...
Preprocessing /root/project/lib/syntax/context.h...
Parsing file /root/project/lib/syntax/context.h...
Preprocessing /root/project/lib/syntax/context_condition.cpp...
Parsing file /root/project/lib/syntax/context_condition.cpp...
Preprocessing /root/project/lib/syntax/context_condition.h...
Parsing file /root/project/lib/syntax/context_condition.h...
Preprocessing /root/project/lib/syntax/event_operator.h...
Parsing file /root/project/lib/syntax/event_operator.h...
Preprocessing /root/project/lib/syntax/plan.cpp...
Parsing file /root/project/lib/syntax/plan.cpp...
Preprocessing /root/project/lib/syntax/plan.h...
Parsing file /root/project/lib/syntax/plan.h...
Preprocessing /root/project/lib/syntax/statement.cpp...
Parsing file /root/project/lib/syntax/statement.cpp...
Preprocessing /root/project/lib/syntax/statement.h...
Parsing file /root/project/lib/syntax/statement.h...
Reading /root/project/README.md...
Preprocessing /root/project/src/agent_loop.cpp...
Parsing file /root/project/src/agent_loop.cpp...
Building group list...
Building directory list...
Building namespace list...
Building file list...
Building class list...
Computing nesting relations for classes...
Associating documentation with classes...
Building example list...
Searching for enumerations...
Searching for documented typedefs...
Searching for members imported via using declarations...
Searching for included using directives...
Searching for documented variables...
Building interface member list...
Building member list...
Searching for friends...
Searching for documented defines...
Computing class inheritance relations...
Computing class usage relations...
Flushing cached template relations that have become invalid...
Computing class relations...
Add enum values to enums...
Searching for member function documentation...
Creating members for template instances...
Building page list...
Search for main page...
Computing page relations...
Determining the scope of groups...
Sorting lists...
Determining which enums are documented
Computing member relations...
Building full member lists recursively...
Adding members to member groups.
Computing member references...
Inheriting documentation...
Generating disk names...
Adding source references...
Adding xrefitems...
Sorting member lists...
Setting anonymous enum type...
Computing dependencies between directories...
Generating citations page...
Counting members...
Counting data structures...
Resolving user defined references...
Finding anchors and sections in the documentation...
Transferring function references...
Combining using relations...
Adding members to index pages...
Correcting members for VHDL...
Generating style sheet...
Generating search indices...
Generating example documentation...
Generating file sources...
Generating file documentation...
Generating docs for file agent.cpp...
Generating docs for file agent.h...
Generating docs for file agent_loop.cpp...
Generating docs for file belief.cpp...
Generating docs for file belief.h...
Generating docs for file belief_base.cpp...
Generating docs for file belief_base.h...
Generating docs for file body.cpp...
Generating docs for file body.h...
Generating docs for file body_instruction.cpp...
Generating docs for file body_instruction.h...
Generating docs for file body_return.cpp...
Generating docs for file body_return.h...
Generating docs for file body_type.h...
Generating docs for file context.cpp...
Generating docs for file context.h...
Generating docs for file context_condition.cpp...
Generating docs for file context_condition.h...
Generating docs for file event.cpp...
Generating docs for file event.h...
Generating docs for file event_base.cpp...
Generating docs for file event_base.h...
Generating docs for file event_id.cpp...
Generating docs for file event_id.h...
Generating docs for file event_operator.h...
Generating docs for file instantiated_plan.cpp...
Generating docs for file instantiated_plan.h...
Generating docs for file intention.cpp...
Generating docs for file intention.h...
Generating docs for file intention_base.cpp...
Generating docs for file intention_base.h...
Generating docs for file plan.cpp...
Generating docs for file plan.h...
Generating docs for file plan_base.cpp...
Generating docs for file plan_base.h...
Generating docs for file README.md...
Generating docs for file statement.cpp...
Generating docs for file statement.h...
Generating docs for file vector_queue.h...
Generating page documentation...
Generating group documentation...
Generating class documentation...
Generating docs for compound Agent...
Generating docs for compound Belief...
Generating docs for compound BeliefBase...
Generating docs for compound Body...
Generating docs for compound BodyInstruction...
Generating docs for compound BodyReturn...
Generating docs for compound Context...
Generating docs for compound ContextCondition...
Generating docs for compound Event...
Generating docs for compound EventBase...
Generating docs for compound EventID...
Generating docs for compound InstantiatedPlan...
Generating docs for compound Intention...
/root/project/lib/bdi/intention.h:37: warning: The following parameter of Intention::suspend(EventID *event_id, EventBase *events) is not documented:
  parameter 'events'
Generating docs for compound IntentionBase...
Generating docs for compound Plan...
Generating docs for compound PlanBase...
Generating docs for compound Statement...
Generating docs for compound VectorQueue...
Generating namespace index...
Generating graph info page...
Generating directory documentation...
Generating dependency graph for directory agent
Generating dependency graph for directory bdi
Generating dependency graph for directory lib
Generating dependency graph for directory src
Generating dependency graph for directory syntax
Generating index page...
Generating page index...
Generating module index...
Generating namespace index...
Generating namespace member index...
Generating annotated compound index...
Generating alphabetical compound index...
Generating hierarchical class index...
Generating graphical class hierarchy...
Generating member index...
Generating file index...
Generating file member index...
Generating example index...
finalizing index lists...
writing tag file...
Running plantuml with JAVA...
Running dot...
Generating dot graphs using 37 parallel threads...
Running dot for graph 1/81
Running dot for graph 2/81
Running dot for graph 3/81
Running dot for graph 4/81
Running dot for graph 5/81
Running dot for graph 6/81
Running dot for graph 7/81
Running dot for graph 8/81
Running dot for graph 9/81
Running dot for graph 10/81
Running dot for graph 11/81
Running dot for graph 12/81
Running dot for graph 13/81
Running dot for graph 14/81
Running dot for graph 15/81
Running dot for graph 16/81
Running dot for graph 17/81
Running dot for graph 18/81
Running dot for graph 19/81
Running dot for graph 20/81
Running dot for graph 21/81
Running dot for graph 22/81
Running dot for graph 23/81
Running dot for graph 24/81
Running dot for graph 25/81
Running dot for graph 26/81
Running dot for graph 27/81
Running dot for graph 28/81
Running dot for graph 29/81
Running dot for graph 30/81
Running dot for graph 31/81
Running dot for graph 32/81
Running dot for graph 33/81
Running dot for graph 34/81
Running dot for graph 35/81
Running dot for graph 36/81
Running dot for graph 37/81
Running dot for graph 38/81
Running dot for graph 39/81
Running dot for graph 40/81
Running dot for graph 41/81
Running dot for graph 42/81
Running dot for graph 43/81
Running dot for graph 44/81
Running dot for graph 45/81
Running dot for graph 46/81
Running dot for graph 47/81
Running dot for graph 48/81
Running dot for graph 49/81
Running dot for graph 50/81
Running dot for graph 51/81
Running dot for graph 52/81
Running dot for graph 53/81
Running dot for graph 54/81
Running dot for graph 55/81
Running dot for graph 56/81
Running dot for graph 57/81
Running dot for graph 58/81
Running dot for graph 59/81
Running dot for graph 60/81
Running dot for graph 61/81
Running dot for graph 62/81
Running dot for graph 63/81
Running dot for graph 64/81
Running dot for graph 65/81
Running dot for graph 66/81
Running dot for graph 67/81
Running dot for graph 68/81
Running dot for graph 69/81
Running dot for graph 70/81
Running dot for graph 71/81
Running dot for graph 72/81
Running dot for graph 73/81
Running dot for graph 74/81
Running dot for graph 75/81
Running dot for graph 76/81
Running dot for graph 77/81
Running dot for graph 78/81
Running dot for graph 79/81
Running dot for graph 80/81
Running dot for graph 81/81
Patching output file 1/142
Patching output file 2/142
Patching output file 3/142
Patching output file 4/142
Patching output file 5/142
Patching output file 6/142
Patching output file 7/142
Patching output file 8/142
Patching output file 9/142
Patching output file 10/142
Patching output file 11/142
Patching output file 12/142
Patching output file 13/142
Patching output file 14/142
Patching output file 15/142
Patching output file 16/142
Patching output file 17/142
Patching output file 18/142
Patching output file 19/142
Patching output file 20/142
Patching output file 21/142
Patching output file 22/142
Patching output file 23/142
Patching output file 24/142
Patching output file 25/142
Patching output file 26/142
Patching output file 27/142
Patching output file 28/142
Patching output file 29/142
Patching output file 30/142
Patching output file 31/142
Patching output file 32/142
Patching output file 33/142
Patching output file 34/142
Patching output file 35/142
Patching output file 36/142
Patching output file 37/142
Patching output file 38/142
Patching output file 39/142
Patching output file 40/142
Patching output file 41/142
Patching output file 42/142
Patching output file 43/142
Patching output file 44/142
Patching output file 45/142
Patching output file 46/142
Patching output file 47/142
Patching output file 48/142
Patching output file 49/142
Patching output file 50/142
Patching output file 51/142
Patching output file 52/142
Patching output file 53/142
Patching output file 54/142
Patching output file 55/142
Patching output file 56/142
Patching output file 57/142
Patching output file 58/142
Patching output file 59/142
Patching output file 60/142
Patching output file 61/142
Patching output file 62/142
Patching output file 63/142
Patching output file 64/142
Patching output file 65/142
Patching output file 66/142
Patching output file 67/142
Patching output file 68/142
Patching output file 69/142
Patching output file 70/142
Patching output file 71/142
Patching output file 72/142
Patching output file 73/142
Patching output file 74/142
Patching output file 75/142
Patching output file 76/142
Patching output file 77/142
Patching output file 78/142
Patching output file 79/142
Patching output file 80/142
Patching output file 81/142
Patching output file 82/142
Patching output file 83/142
Patching output file 84/142
Patching output file 85/142
Patching output file 86/142
Patching output file 87/142
Patching output file 88/142
Patching output file 89/142
Patching output file 90/142
Patching output file 91/142
Patching output file 92/142
Patching output file 93/142
Patching output file 94/142
Patching output file 95/142
Patching output file 96/142
Patching output file 97/142
Patching output file 98/142
Patching output file 99/142
Patching output file 100/142
Patching output file 101/142
Patching output file 102/142
Patching output file 103/142
Patching output file 104/142
Patching output file 105/142
Patching output file 106/142
Patching output file 107/142
Patching output file 108/142
Patching output file 109/142
Patching output file 110/142
Patching output file 111/142
Patching output file 112/142
Patching output file 113/142
Patching output file 114/142
Patching output file 115/142
Patching output file 116/142
Patching output file 117/142
Patching output file 118/142
Patching output file 119/142
Patching output file 120/142
Patching output file 121/142
Patching output file 122/142
Patching output file 123/142
Patching output file 124/142
Patching output file 125/142
Patching output file 126/142
Patching output file 127/142
Patching output file 128/142
Patching output file 129/142
Patching output file 130/142
Patching output file 131/142
Patching output file 132/142
Patching output file 133/142
Patching output file 134/142
Patching output file 135/142
Patching output file 136/142
Patching output file 137/142
Patching output file 138/142
Patching output file 139/142
Patching output file 140/142
Patching output file 141/142
Patching output file 142/142
lookup cache used 259/65536 hits=1060 misses=279
finished...</code></pre>
  </div>
</div>

## Run

The agent executable is located at `build/agent.out`. For Unix-like systems, simply run `./agent.out` to start the agent.

Executables for the valgrind memory test and unit tests are also available at `/build/valgrind.out` and `/build/unittest.out`, respectively.