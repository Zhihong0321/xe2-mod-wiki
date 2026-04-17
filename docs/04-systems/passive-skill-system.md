# Passive Skill System

## Significance

This is the first locally proven example of adding a new soldier-facing gameplay system layer rather than just modifying existing data.

The test system introduces:

- a custom passive-state component
- a custom soldier-page panel
- clickable toggles
- live stat application

In practical terms, this means Xenonauts 2 can host new player-facing systems layered onto existing game flows.

## Current Test Skills

The prototype currently exposes three passive toggles:

- `Calibrated Vision`: `+2 Accuracy`
- `Trained Mind`: `+2 Bravery`
- `Bionic Implant`: `+4 Time Units`

## High-Level Architecture

### UI Layer

A custom `PASSIVE SKILLS` panel is created and shown on the soldier equipment / info page.

The panel displays:

- the selected soldier
- the active passive summary
- live stat readouts
- per-skill `ON` / `OFF` states

### State Layer

The system uses a custom component:

- `PassiveSkillStateComponent`

Its key state is:

- `ActiveSkillIds`

This list acts as the source of truth for which passives are currently enabled on that soldier.

### Effect Layer

When a toggle changes, the code applies stat deltas directly to the selected soldier entity:

- `DeltaAccuracy`
- `DeltaBravery`
- `DeltaTimeUnits`
- `DeltaUnmodifiedTimeUnits`

## Important Confirmed Implementation Detail

The selected soldier lookup was initially wrong.

Confirmed fix:

- `SoldierInfoPanelController.Target` needed to be read through tuple property `X`
- relying only on `Item1` did not produce the correct soldier in the working path

This was the key fix that made the passive panel show the correct target.

## Toggle Flow

The working toggle path is:

1. identify the selected soldier entity
2. ensure the soldier has `PassiveSkillStateComponent`
3. flip the selected skill in `ActiveSkillIds`
4. apply the matching stat delta
5. refresh the UI state
6. refresh the underlying soldier info panel

## What Is Confirmed Right Now

Confirmed:

- the passive panel appears
- the toggles can be clicked
- the game survives the click
- the soldier receives the expected bonus

This already moves beyond feasibility testing.

## Why This Is Bigger Than A UI Test

The passive system proves a reusable pattern:

- custom state
- custom UI
- custom player interaction
- direct gameplay effect

That pattern can be extended into many future systems, such as:

- traits
- perks
- implants
- role specializations
- training trees
- officer auras
- progression unlock panels
- custom soldier management overlays

## Known Limits

What is proven is still narrower than "full production system."

Still needing broader validation:

- new campaign behavior
- mission transition behavior
- roster transfer edge cases
- death / injury / dismissal edge cases
- stacking rules if future effects overlap
- UI placement polish for multiple resolutions

## Current Design Principle

Do not move straight into feature sprawl.

The passive prototype should continue to be treated as a reference implementation for:

- soldier-bound custom state
- soldier-page interaction
- live stat mutation
- persistence validation
