---
title: Unit Tests
sidebar: mydoc_sidebar
permalink: unit_tests.html
folder: mydoc
---

<br>

The supporting library used for unit testing is [GoogleTest](https://github.com/google/googletest). Note that the GoogleTest header and implementation files are already included in the Embedded-BDI repository at `test/external/gtest`, so no additional installation is needed.

All Embedded-BDI library classes have corresponding multiple unit tests, available in the `tests/` folder.

To compile the test executable, run `make tests`. The resulting executable file will be available at `build/unittest.out`. Alternatively, you can use the `UnitTests.launch` launcher file in Eclipse CDT to both build and run the testing routine. 

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseStatementUTest" aria-expanded="false" aria-controls="collapseStatementUTest">
    Example: Unit test file for Statement class
  </button>
</p>
<div class="collapse" id="collapseStatementUTest">
  <div class="card card-body">
    <pre><code>/*
 * proposition_test.cpp
 *
 *  Created on: Jun 28, 2020
 *      Author: Matuzalem Muller
 */

#include "gtest/gtest.h"
#include "syntax/proposition.h"

class TProposition : public ::testing::Test
{
protected:
  Proposition * prop;

public:
  TProposition()
  {
    this->prop = new Proposition(0);
  }

  ~TProposition()
  {
    delete this->prop;
  }
};

/*
 * Test return from get_name
 */
TEST_F(TProposition, get_name)
{
  EXPECT_EQ(0, prop->get_name());
}

/*
 * Test if is_equal_to compares propositions correctly
 */
TEST_F(TProposition, is_equal_to)
{
  Proposition equal(0);
  EXPECT_TRUE(prop->is_equal(equal));
  EXPECT_TRUE(prop->is_equal(&equal));
}</code></pre>
  </div>
</div>

<p>
  <button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseUnitTests" aria-expanded="false" aria-controls="collapseUnitTests">
    Example: Run unittest.out executable
  </button>
</p>
<div class="collapse" id="collapseUnitTests">
  <div class="card card-body">
    <pre><code>[==========] Running 71 tests from 28 test suites.
[----------] Global test environment set-up.
[----------] 1 test from TNBSubgoalFailedPlan
[ RUN      ] TNBSubgoalFailedPlan.fail_intention
[       OK ] TNBSubgoalFailedPlan.fail_intention (0 ms)
[----------] 1 test from TNBSubgoalFailedPlan (0 ms total)

[----------] 1 test from TStackedIntention
[ RUN      ] TStackedIntention.run_stacked_intention
[       OK ] TStackedIntention.run_stacked_intention (0 ms)
[----------] 1 test from TStackedIntention (0 ms total)

[----------] 1 test from TOverflowStackedIntention
[ RUN      ] TOverflowStackedIntention.run_overflow_stacked_intention
[       OK ] TOverflowStackedIntention.run_overflow_stacked_intention (0 ms)
[----------] 1 test from TOverflowStackedIntention (0 ms total)

[----------] 1 test from TEmptyEventBaseSettings
[ RUN      ] TEmptyEventBaseSettings.run_empty_event_base
[       OK ] TEmptyEventBaseSettings.run_empty_event_base (0 ms)
[----------] 1 test from TEmptyEventBaseSettings (0 ms total)

[----------] 1 test from TFillEventBaseSettings
[ RUN      ] TFillEventBaseSettings.run_fill_event_base
[       OK ] TFillEventBaseSettings.run_fill_event_base (0 ms)
[----------] 1 test from TFillEventBaseSettings (0 ms total)

[----------] 1 test from TFailedIntention
[ RUN      ] TFailedIntention.run_failed_intention
[       OK ] TFailedIntention.run_failed_intention (0 ms)
[----------] 1 test from TFailedIntention (0 ms total)

[----------] 1 test from TNBSubgoalFullIntentionBase
[ RUN      ] TNBSubgoalFullIntentionBase.drop_event
[       OK ] TNBSubgoalFullIntentionBase.drop_event (0 ms)
[----------] 1 test from TNBSubgoalFullIntentionBase (0 ms total)

[----------] 1 test from TMultipleIntentions
[ RUN      ] TMultipleIntentions.run_multiple_intentions
[       OK ] TMultipleIntentions.run_multiple_intentions (0 ms)
[----------] 1 test from TMultipleIntentions (0 ms total)

[----------] 1 test from TOverflowIntentionBase
[ RUN      ] TOverflowIntentionBase.overflow_intention_base
[       OK ] TOverflowIntentionBase.overflow_intention_base (0 ms)
[----------] 1 test from TOverflowIntentionBase (0 ms total)

[----------] 1 test from TSimpleIntention
[ RUN      ] TSimpleIntention.run_simple_intention
[       OK ] TSimpleIntention.run_simple_intention (0 ms)
[----------] 1 test from TSimpleIntention (0 ms total)

[----------] 1 test from TNBSubgoalEmptyIntentionBase
[ RUN      ] TNBSubgoalEmptyIntentionBase.run_intention
[       OK ] TNBSubgoalEmptyIntentionBase.run_intention (0 ms)
[----------] 1 test from TNBSubgoalEmptyIntentionBase (0 ms total)

[----------] 3 tests from TInstantiatedPlan
[ RUN      ] TInstantiatedPlan.run_plan_empty_event_base
[       OK ] TInstantiatedPlan.run_plan_empty_event_base (0 ms)
[ RUN      ] TInstantiatedPlan.run_plan_full_event_base
[       OK ] TInstantiatedPlan.run_plan_full_event_base (0 ms)
[ RUN      ] TInstantiatedPlan.is_finished
[       OK ] TInstantiatedPlan.is_finished (0 ms)
[----------] 3 tests from TInstantiatedPlan (0 ms total)

[----------] 3 tests from TBelief
[ RUN      ] TBelief.update_belief
[       OK ] TBelief.update_belief (0 ms)
[ RUN      ] TBelief.change_state
[       OK ] TBelief.change_state (0 ms)
[ RUN      ] TBelief.get_proposition
[       OK ] TBelief.get_proposition (0 ms)
[----------] 3 tests from TBelief (0 ms total)

[----------] 4 tests from TIntentionBase
[ RUN      ] TIntentionBase.add_intention
[       OK ] TIntentionBase.add_intention (0 ms)
[ RUN      ] TIntentionBase.run_intention_base
[       OK ] TIntentionBase.run_intention_base (0 ms)
[ RUN      ] TIntentionBase.is_empty
[       OK ] TIntentionBase.is_empty (0 ms)
[ RUN      ] TIntentionBase.is_full
[       OK ] TIntentionBase.is_full (0 ms)
[----------] 4 tests from TIntentionBase (0 ms total)

[----------] 5 tests from TBeliefBase
[ RUN      ] TBeliefBase.add_belief
[       OK ] TBeliefBase.add_belief (0 ms)
[ RUN      ] TBeliefBase.get_belief_state
[       OK ] TBeliefBase.get_belief_state (0 ms)
[ RUN      ] TBeliefBase.update
[       OK ] TBeliefBase.update (0 ms)
[ RUN      ] TBeliefBase.change_belief_state
[       OK ] TBeliefBase.change_belief_state (0 ms)
[ RUN      ] TBeliefBase.get_size
[       OK ] TBeliefBase.get_size (0 ms)
[----------] 5 tests from TBeliefBase (0 ms total)

[----------] 2 tests from TPlanBase
[ RUN      ] TPlanBase.add_plan
[       OK ] TPlanBase.add_plan (0 ms)
[ RUN      ] TPlanBase.revise
[       OK ] TPlanBase.revise (0 ms)
[----------] 2 tests from TPlanBase (0 ms total)

[----------] 7 tests from TIntention
[ RUN      ] TIntention.stack_plan
[       OK ] TIntention.stack_plan (0 ms)
[ RUN      ] TIntention.run_intention
[       OK ] TIntention.run_intention (0 ms)
[ RUN      ] TIntention.is_finished
[       OK ] TIntention.is_finished (0 ms)
[ RUN      ] TIntention.is_suspended_by
[       OK ] TIntention.is_suspended_by (0 ms)
[ RUN      ] TIntention.is_suspended
[       OK ] TIntention.is_suspended (0 ms)
[ RUN      ] TIntention.terminate
[       OK ] TIntention.terminate (0 ms)
[ RUN      ] TIntention.destructor
[       OK ] TIntention.destructor (0 ms)
[----------] 7 tests from TIntention (1 ms total)

[----------] 3 tests from TEvent
[ RUN      ] TEvent.get_operator
[       OK ] TEvent.get_operator (0 ms)
[ RUN      ] TEvent.get_proposition
[       OK ] TEvent.get_proposition (0 ms)
[ RUN      ] TEvent.get_event_id
[       OK ] TEvent.get_event_id (0 ms)
[----------] 3 tests from TEvent (0 ms total)

[----------] 6 tests from TEventBase
[ RUN      ] TEventBase.is_empty
[       OK ] TEventBase.is_empty (0 ms)
[ RUN      ] TEventBase.is_full
[       OK ] TEventBase.is_full (0 ms)
[ RUN      ] TEventBase.add_event
[       OK ] TEventBase.add_event (0 ms)
[ RUN      ] TEventBase.get_event
[       OK ] TEventBase.get_event (0 ms)
[ RUN      ] TEventBase.last_event
[       OK ] TEventBase.last_event (0 ms)
[ RUN      ] TEventBase.event_exists
[       OK ] TEventBase.event_exists (0 ms)
[----------] 6 tests from TEventBase (0 ms total)

[----------] 2 tests from TContext
[ RUN      ] TContext.add_belief
[       OK ] TContext.add_belief (0 ms)
[ RUN      ] TContext.is_valid
[       OK ] TContext.is_valid (0 ms)
[----------] 2 tests from TContext (0 ms total)

[----------] 1 test from TBodyInstruction
[ RUN      ] TBodyInstruction.run_instruction
[       OK ] TBodyInstruction.run_instruction (0 ms)
[----------] 1 test from TBodyInstruction (0 ms total)

[----------] 5 tests from TPlan
[ RUN      ] TPlan.get_operator
[       OK ] TPlan.get_operator (0 ms)
[ RUN      ] TPlan.get_proposition
[       OK ] TPlan.get_proposition (0 ms)
[ RUN      ] TPlan.get_context
[       OK ] TPlan.get_context (0 ms)
[ RUN      ] TPlan.get_body
[       OK ] TPlan.get_body (0 ms)
[ RUN      ] TPlan.run_body
[       OK ] TPlan.run_body (0 ms)
[----------] 5 tests from TPlan (0 ms total)

[----------] 3 tests from TBodyReturn
[ RUN      ] TBodyReturn.get_event
[       OK ] TBodyReturn.get_event (0 ms)
[ RUN      ] TBodyReturn.get_type
[       OK ] TBodyReturn.get_type (0 ms)
[ RUN      ] TBodyReturn.get_value
[       OK ] TBodyReturn.get_value (0 ms)
[----------] 3 tests from TBodyReturn (0 ms total)

[----------] 2 tests from TBody
[ RUN      ] TBody.add_instruction
[       OK ] TBody.add_instruction (0 ms)
[ RUN      ] TBody.run_body
[       OK ] TBody.run_body (0 ms)
[----------] 2 tests from TBody (0 ms total)

[----------] 1 test from TContextCondition
[ RUN      ] TContextCondition.get_proposition
[       OK ] TContextCondition.get_proposition (0 ms)
[----------] 1 test from TContextCondition (0 ms total)

[----------] 2 tests from TProposition
[ RUN      ] TProposition.get_name
[       OK ] TProposition.get_name (0 ms)
[ RUN      ] TProposition.is_equal_to
[       OK ] TProposition.is_equal_to (0 ms)
[----------] 2 tests from TProposition (0 ms total)

[----------] 2 tests from TEventID
[ RUN      ] TEventID.get_id
[       OK ] TEventID.get_id (0 ms)
[ RUN      ] TEventID.is_equal
[       OK ] TEventID.is_equal (0 ms)
[----------] 2 tests from TEventID (0 ms total)

[----------] 9 tests from TVectorQueue
[ RUN      ] TVectorQueue.push_front
[       OK ] TVectorQueue.push_front (0 ms)
[ RUN      ] TVectorQueue.push_back
[       OK ] TVectorQueue.push_back (0 ms)
[ RUN      ] TVectorQueue.pop_back
[       OK ] TVectorQueue.pop_back (0 ms)
[ RUN      ] TVectorQueue.item_at
[       OK ] TVectorQueue.item_at (0 ms)
[ RUN      ] TVectorQueue.erase
[       OK ] TVectorQueue.erase (0 ms)
[ RUN      ] TVectorQueue.rotate
[       OK ] TVectorQueue.rotate (0 ms)
[ RUN      ] TVectorQueue.size
[       OK ] TVectorQueue.size (0 ms)
[ RUN      ] TVectorQueue.is_empty
[       OK ] TVectorQueue.is_empty (0 ms)
[ RUN      ] TVectorQueue.is_full
[       OK ] TVectorQueue.is_full (0 ms)
[----------] 9 tests from TVectorQueue (0 ms total)

[----------] Global test environment tear-down
[==========] 71 tests from 28 test suites ran. (2 ms total)
[  PASSED  ] 71 tests.</code></pre>
  </div>
</div>