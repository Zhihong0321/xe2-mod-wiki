# Session Handoff: Test Console Expansion

## Outcome Update

Follow-up sessions have now directly proven the first three non-personnel targets from this handoff:

- money
- doomsday
- operation points

See:

- [Direct Campaign Resource Deltas](../04-systems/direct-campaign-resource-deltas.md)

## Situation Summary

The project now has a working in-game experiment console in the live `ui_component_probe` mod.

This is no longer a pure research guess.

A backend personnel grant has been proven in live play.

Confirmed result:

- clicking the experiment console `+1 ENGINEER` button increased the visible engineer count in the base UI
- engineer wage cost updated in the same screen
- the change happened through backend command code, not through the vanilla hire button

This is the current foundation for expanding the test mode into a broader player-state control console.

## Most Important Proven Method

The proven direct path is:

1. resolve the active strategy `World`
2. resolve the Xenonaut player entity
3. resolve the active / owned geobase
4. resolve `HiringPoolSystem`
5. get the correct division hiring pool
6. create `HirePersonnelCommand.FromPoolGeneric(...)`
7. call `world.HandleEvent(command)`

That path is now the default reference pattern for direct personnel grants.

See:

- [Direct Personnel Backend Command](../04-systems/direct-personnel-backend-command.md)

## Current Live Mod State

The active experiment console already supports:

- `REFRESH SNAPSHOT`
- `+1 ENGINEER`
- `+1 SCIENTIST`
- `CLEAR LOG`

The console now successfully resolves:

- strategy world
- player entity
- current base context

The console also logs:

- before / after backend snapshots
- last experiment action
- failure reasons

## Key Negative Result That Must Not Be Forgotten

The successful engineer test proved a second important fact:

- displayed engineer count is not fully explained by `EngineeringSystem` workshop-slot snapshot values

Observed live result:

- visible engineer count changed
- wage cost changed
- `engOccupied`, `engTransit`, and `engAll` did not change in the expected way

Practical meaning:

- direct backend hire works
- but the final displayed count likely depends on another component, value, or reconciliation path

So future research must not assume:

- workshop-slot families alone are the true count source

## What The Next Session Should Do

The user wants to expand the test mode into a reusable player-state control console.

Best next targets:

1. money
2. doomsday counter
3. operation points
4. scientist add / remove parity
5. engineer remove parity

Recommended working order:

1. keep the existing console as the test harness
2. add one new button per target system
3. log before / after values in the console
4. prove one direct backend path at a time
5. document each proven method immediately in the wiki

## Recommended Console Expansion Pattern

For each new player-state target:

1. identify the owning system or owner entity
2. identify a safe read path for the visible current value
3. add a test button to the console
4. apply a single small delta such as `+1`, `-1`, or `+$1000`
5. log before / after values
6. verify the visible UI changes
7. write a dedicated wiki page if the method is proven

Do not add broad batch edits before a single small delta is proven.

## Strong Next Research Targets

### 1. Money

Reason:

- very visible
- easy to verify from the top bar
- likely to become useful in many debug or progression mods

Best question:

- which backend system or player component owns current campaign funds?

### 2. Doomsday Counter

Reason:

- high-value campaign state
- useful for future event / passive / difficulty systems

Best question:

- which campaign entity or global system owns the live doom progression value shown to the player?

### 3. Operation Points

Reason:

- likely useful for advanced campaign systems
- a strong candidate for unlock, reward, or passive integration later

Best question:

- is this value owned by the player entity, a campaign entity, or a dedicated strategy system?

## What The Next Session Must Not Do

Do not throw away the current experiment console.

Do not return to:

- UI-only guessing
- broad speculative hook coverage
- derived formula patching before the owning value is identified

Do not start by trying to fully design the passive skill economy around unproven values.

First prove:

- one button
- one backend path
- one visible result

Then generalize.

## Good Immediate Implementation Idea

Turn the current console into a generic debug harness with these sections:

- `Personnel`
- `Resources`
- `Campaign`
- `Diagnostics`

Possible first buttons:

- `+1 Engineer`
- `+1 Scientist`
- `+1000 Money`
- `-1 Doom`
- `+1 Ops`
- `Snapshot`
- `Clear Log`

## Current Recommendation

The next session should treat the existing console as the official experiment platform for serious modding research.

The immediate goal is no longer:

- "can we touch player state at all?"

That has already been answered.

The immediate goal is now:

- "which other player-facing campaign values can be changed directly through stable backend methods?"
