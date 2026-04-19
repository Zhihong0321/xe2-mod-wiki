# Open Questions And Next Tests

## Why This Page Exists

The project now has enough proven ground that the next mistakes are likely to come from overconfidence rather than lack of capability.

This page keeps future work disciplined.

## Immediate Warning

Before resuming campaign-layer personnel work, read:

- [Session Handoff: Personnel Count Reset](session-handoff-personnel-count-reset.md)
- [Session Handoff: Test Console Expansion](session-handoff-test-console-expansion.md)
- [Failure Patterns And Reset Rules](../06-safety/failure-patterns-and-reset-rules.md)

That failed session established a hard rule:

- do not keep widening an indirect approach when the visible target is still a simple scientist / engineer count change

## Recently Resolved

These are no longer open questions:

- research testing speed can be forced by directly overriding `Xenonauts.Strategy.Components.ProgressPoints`
- `research_duration0` is confirmed to be only the shortest preset, not instant
- wrong custom research descriptions can be fixed with `LocalizableGUID` plus mod-local locale CSV files
- a one-way soldier class system can be prototyped with custom UI, custom state, live stat deltas, and save-backed persistence in the tested strategy-layer flow
- a sniper class can branch into a persistent `5x5` passive board with kill-count-derived points and orthogonal adjacency activation
- engineer count can be increased directly through backend hire-command flow without clicking the vanilla hire UI
- displayed engineer count is not fully explained by `EngineeringSystem` workshop-slot family snapshots alone
- campaign funds can be changed directly through backend entity mutation
- operation points can be changed directly through backend entity mutation
- doomsday can be changed directly through backend global-variable mutation
- partial-load global-variable reads can crash update-driven debug UI and must be treated as optional

See:

- [Direct Campaign Resource Deltas](../04-systems/direct-campaign-resource-deltas.md)
- [Research Project Overrides](../04-systems/research-project-overrides.md)
- [Direct Personnel Backend Command](../04-systems/direct-personnel-backend-command.md)

## Highest-Value Confirmed Direction

The biggest proven direction is:

- adding new gameplay systems through custom UI plus code-mod state

That direction should remain the priority because it unlocks the most design space.

## Best Immediate Next Tests

### Passive-System Persistence Matrix

Still needed:

- one passive on, save, reload
- one passive off, save, reload
- all three on, save, reload
- one class assigned, save, quit to main menu, load
- soldier switching validation
- fresh-campaign validation
- class board validation for non-sniper classes once implemented

### Progression Trigger Expansion

Next high-value research target:

- progression nodes that affect campaign-layer systems instead of only the soldier

Best first trigger candidates:

- monthly funding gain
- research speed
- engineering speed
- project cost reduction

Important question:

- which campaign-layer values are safe to derive and reconcile periodically from soldier-bound progression state?

Important newly-proven direction:

- personnel rewards should start from backend command issuance rather than UI simulation
- direct money / ops / doom rewards should start from backend entity mutation rather than UI simulation

### Lifecycle Regression Test

Still needed:

- leave soldier page
- enter a different page
- return to strategy screen
- return to main menu
- confirm no orphaned passive UI remains

### UI Placement Validation

Still needed:

- check different resolutions if available
- confirm the panel does not cover important controls
- confirm no clipping or masking regressions on different pages

## Research Questions

### Passive / Aura Expansion

Research strongly suggests that soldier passive abilities and aura-like behavior are plausible through:

- ability systems
- effects
- modifiers
- event-driven code mods

But the broad claim "aura systems are solved" is still too early.

### New Top-Level Screens

Adding panels and overlays is now strongly supported by evidence.

Adding a completely new top-level navigation page remains less certain and should still be treated as research until directly proven.

### Save Compatibility

If passive IDs, class IDs, component shapes, or serialization expectations change over time, older saves may become a risk.

That needs careful versioning once the prototype becomes a long-lived feature mod.

## Good Future Candidates

The current breakthroughs make these ideas much more realistic:

- soldier perk trees
- implant systems
- role-based passive loadouts
- permanent class systems
- officer leadership systems
- event-earned traits
- custom progression panels
- aura or adjacency bonuses
- campaign-layer soldier management extensions
- soldier-driven campaign economy modifiers
- soldier-driven research or engineering modifiers

## Documentation Rule For Future Expansion

When a new idea appears possible, create a dedicated page only after one of these is true:

- it has a live implementation
- it has a tested prototype
- it has enough assembly / official-doc evidence to deserve a real research page
