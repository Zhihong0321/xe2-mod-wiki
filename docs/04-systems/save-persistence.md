# Save Persistence Findings

## Why This Page Matters

A custom UI button is useful.

A custom gameplay system that survives save / load is far more important.

The passive-skill prototype has now produced the first meaningful evidence that this is possible.

## Latest Observed Result

Observed in user testing:

- a passive that was previously toggled `ON` remained `ON` after loading the game
- a soldier that was assigned a class in `Class System Probe` kept that class after saving, quitting to the main menu, and loading the save

Because both systems reflect custom soldier-bound state, this strongly suggests that the tested custom-component path survived save / load in both cases.

## What This Likely Means

The current implementations store custom state in soldier-bound custom components:

- `PassiveSkillStateComponent`
- `ActiveSkillIds`
- `SoldierClassStateComponent`
- `SelectedClassId`

Given the observed behavior, the most likely interpretation is:

- the custom soldier-bound passive state persisted through the save / load path in the tested scenario
- the custom soldier-bound class state persisted through save, quit to main menu, and load in the tested scenario

That is a major result.

## Why This Is A Big Breakthrough

If confirmed more broadly, this means future mods can potentially implement persistent custom soldier systems such as:

- passive skill trees
- implant slots
- traits granted by events
- training unlocks
- officer roles
- custom perk assignments

This is not just a menu trick. It is a route toward durable new gameplay structures.

## What Is Confirmed Vs Not Yet Confirmed

### Confirmed / Observed

- a previously enabled passive remained visible as enabled after loading
- a previously assigned class remained set after save, quit to main menu, and load

### Not Yet Fully Characterized

- whether all save entry points behave the same way
- whether combat transitions preserve the same state
- whether save compatibility remains stable across future code changes
- whether removed or renamed skill IDs behave safely on old saves

## Recommended Validation Matrix

The next persistence tests should be systematic:

1. enable one passive and save on the strategy layer
2. reload and confirm the passive state, stat state, and UI state
3. disable it, save again, and confirm the off-state persists
4. enable all three passives and confirm each survives reload
5. assign one class and confirm the class state and stat state survive reload
6. verify soldier switching does not leak state between soldiers
7. verify a new campaign starts clean
8. verify injured, transferred, or otherwise changed soldiers still reload cleanly

## Documentation Rule

Future documentation should avoid claiming "custom components always serialize" until the matrix above is tested more broadly.

The correct current statement is narrower and stronger:

- the passive-state implementation survived save / load in the tested scenario
- the class-state implementation survived save, quit to main menu, and load in the tested scenario
