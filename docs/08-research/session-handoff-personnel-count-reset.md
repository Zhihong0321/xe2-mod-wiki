# Session Handoff: Personnel Count Reset

## Situation Summary

This session spent too long trying to make soldier passive skills affect:

- scientist count
- engineer count
- research speed
- engineering speed
- monthly funding

The user-visible target should have been much narrower:

- make the actual scientist / engineer count change in game first

That did not happen.

## Main Failure

The dominant failure was not lack of effort.

It was ineffective thinking mode.

The work became trapped in a bad loop:

- assume the problem should be solved through derived bonuses and hook coverage
- patch multiple systems
- get no visible result
- add more probes
- stay in the same frame even longer

When stuck, the approach did not simplify.

It dug deeper.

That burned time and tokens without creating enough new decision-making value.

## Why This Was The Wrong Frame

The problem statement was simple:

- if a soldier passive grants `+1 scientist`, the game should show a higher scientist count

So the smartest first question was:

- where is the real scientist count stored, and can that value be changed directly?

Instead, the work drifted into:

- research production overrides
- engineering production overrides
- funding report injection
- UI hook guesses
- backend hook guesses
- broad diagnostics

Those may become useful later, but they were the wrong first move.

## Confirmed Evidence Gathered

Useful things were still learned:

- the soldier passive toggle UI works
- passive state persists through save / load in the tested soldier systems
- the campaign bonus cache can be rebuilt and logged in some runs
- the research screen entry hook is real
- the research screen dump showed `research_element(Clone)` and the `HIRE SCIENTISTS` navigation path
- multiple guessed hire-screen hooks did not fire

Important negative evidence:

- user-facing scientist count did not change
- user-facing engineer count did not change
- hiring page counts stayed unchanged in tests
- the session never proved the true owned scientist / engineer count path

## What The Next Session Must Not Do

Do not start by patching:

- research speed
- engineering speed
- funding calculations
- general backend formulas
- more broad speculative hooks

Do not add diagnostics unless they directly reveal the owned personnel count or the immediate UI source for that count.

## Required Reset For Next Session

The next session should use this exact order:

1. identify the real stored scientist count used by the hire screen
2. identify the real stored engineer count used by the hire screen
3. patch one direct test change to one of those counts
4. expose the raw before / after value in debug UI or log
5. verify the hire screen changes visibly

Only after that succeeds should any derived campaign bonus logic resume.

## Hard Warning For Future AI Work

This session proved a specific AI failure pattern:

- poor outside-the-box thinking
- over-commitment to the first technical frame
- inability to reset quickly after no-result tests
- deeper and deeper analysis inside the wrong approach

Future work must actively defend against this pattern.

If two attempts fail without moving the visible target, the correct action is to reset the frame, not intensify it.

## Current Recommendation

Pause all indirect campaign-bonus work.

Reopen the problem as:

- `direct personnel count mutation research`

That is the cleanest restart point.
