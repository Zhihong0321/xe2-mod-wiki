# Failure Patterns And Reset Rules

## Why This Page Exists

One of the most expensive project failures was not a crash.

It was a thinking failure:

- staying inside the same bad frame for hours
- adding more hooks, more diagnostics, and more theory
- not resetting to the simplest user-visible target
- burning large amounts of time and tokens without increasing signal fast enough

This page is here to make that pattern visible and unacceptable.

## Confirmed Failure Pattern

The failed pattern looked like this:

- the user wanted a simple visible result: change scientist / engineer count in game
- the work drifted into indirect solutions: derived production bonuses, funding hooks, multiple UI hooks, backend probes
- when those guesses failed, the response was not to simplify
- instead, the response was to dig deeper into the same frame and add more instrumentation

That is not effective research.

That is tunnel vision.

## Core Lesson

When the user-facing target is simple, the first attempt must also be simple.

For example:

- if the goal is `Scientists at Base` increasing, try to modify the real stored scientist count first
- if the goal is `Engineers at Base` increasing, try to modify the real stored engineer count first
- if the goal is a visible label changing, patch the object that owns that label or its source value first

Do not begin with a wide indirect system unless the direct path has already been disproven.

## Anti-Pattern Checklist

If several of these are true, stop and reset:

- more than one subsystem is being patched for one simple visible goal
- new diagnostics are being added before the last diagnostics changed the plan
- the current patch does not create a new clear pass / fail clue
- the user has already run several tests with no change on the target screen
- the work is optimizing theory instead of touching the value the player can see

## Mandatory Reset Rule

When stuck on a simple target, force this reset:

1. restate the smallest visible success condition
2. identify the most direct owned value behind that condition
3. patch only that value or its immediate UI owner
4. add raw-value debug output for the exact value being changed
5. run one test that can clearly falsify the approach

If the patch does not satisfy step 2 or step 3, it is probably still too indirect.

## Token And Time Stop-Loss Rule

If two consecutive attempts do not move the target value or target UI:

- stop the current line of thought
- write down why it failed
- propose a simpler path
- do not keep widening the same approach

More effort inside a bad frame is not persistence.

It is waste.

## Modding Rule For Campaign Staff Work

For scientist / engineer / funding work, use this order:

1. confirm the real stored count or owned value
2. change that value directly
3. confirm the live UI reflects it
4. only then reason about derived systems like project speed, wages, reports, or capacity

## Handoff Rule

Any session that gets trapped in this pattern must record:

- the wrong frame that dominated the session
- the simpler frame that should have been tried first
- the exact point where the work should have reset
- the next session's narrower test target

Do not hide this in private memory.

Write it into the wiki so the failure becomes reusable knowledge.
