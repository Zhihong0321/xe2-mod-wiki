# Open Questions And Next Tests

## Why This Page Exists

The project now has enough proven ground that the next mistakes are likely to come from overconfidence rather than lack of capability.

This page keeps future work disciplined.

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
- soldier switching validation
- fresh-campaign validation

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

If passive IDs, component shapes, or serialization expectations change over time, older saves may become a risk.

That needs careful versioning once the prototype becomes a long-lived feature mod.

## Good Future Candidates

The current breakthroughs make these ideas much more realistic:

- soldier perk trees
- implant systems
- role-based passive loadouts
- officer leadership systems
- event-earned traits
- custom progression panels
- aura or adjacency bonuses
- campaign-layer soldier management extensions

## Documentation Rule For Future Expansion

When a new idea appears possible, create a dedicated page only after one of these is true:

- it has a live implementation
- it has a tested prototype
- it has enough assembly / official-doc evidence to deserve a real research page
