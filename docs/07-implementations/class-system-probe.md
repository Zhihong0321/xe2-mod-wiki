# Class System Probe

## Purpose

`Class System Probe` exists to prove the class-system method before building the real mod.

The goal was not polish.

The goal was to answer four technical questions:

- can we add a dedicated class-selection UI to the soldier page?
- can a soldier be assigned a class only once?
- can that class apply immediate stat changes?
- can the extra class state survive save / load?

## Confirmed Result

The answer is now yes for the tested strategy-layer flow.

User verification confirmed all of the following:

- the custom soldier-page class UI appears
- class selection works
- picking a class changes the soldier stats immediately
- saving, quitting to the main menu, and loading the save keeps the class assignment
- the `Sniper` path can switch into a persistent `5x5` passive progression board
- board activation follows orthogonal adjacency from a seeded center node
- available board points can be derived from the soldier's total alien kill count
- activated board nodes remained enabled after save, quit to main menu, and load

That makes this the strongest current proof that a soldier-bound custom system can behave like a durable new gameplay layer.

## New Progression Milestone

The probe no longer stops at class locking.

For the tested `Sniper` path, the class modal now transitions into a progression board.

That progression board currently proves five more things:

- class-locked UI can branch into class-specific sub-UI
- a soldier-bound progression grid can persist generated node values
- progression unlock rules can enforce orthogonal adjacency
- progression points can be derived from existing soldier history instead of stored separately
- progression node activation can apply additional live stat bonuses

This is a major step forward because it turns the class probe from a one-time assignment test into a basic class progression system test.

## Probe Classes

The test mod currently exposes four one-way class assignments:

- `Sniper`: `+10 Accuracy`, `-5 HP`, `-5 Reflexes`
- `Ranger`: `+5 Time Units`, `+5 HP`, `-5 Accuracy`
- `Grenadier`: `+5 HP`, `+5 Reflexes`, `-5 Time Units`
- `Melee`: `+5 Time Units`, `+5 Reflexes`, `-10 Accuracy`

These values are probe values only.

They exist to prove the method, not final balance.

## Working Pattern

### UI Layer

A custom panel is injected onto the soldier info page.

It shows:

- the selected soldier
- a `CLASS PROGRESSION` launcher on the soldier page
- a half-screen class system window
- four class buttons before assignment
- class-specific progression content after assignment
- current lock state and live stat readouts

### State Layer

The probe stores class choice in a custom soldier component:

- `SoldierClassStateComponent`

Its key field is:

- `SelectedClassId`

That field acts as the source of truth for the soldier's chosen class.

The same component now also stores sniper-board state:

- generated node bonuses for each of the `25` board cells
- the set of activated node indexes

That makes the class component the current source of truth for both class lock and sniper progression state.

### Locking Rule

The probe enforces one-way assignment by treating any non-empty `SelectedClassId` as locked.

Confirmed behavior:

- before assignment, class buttons are available
- after assignment, the selected class is marked locked
- other class buttons become unavailable
- after `Sniper` is assigned, the class modal changes into the sniper progression board

## Sniper Progression Board

The current sniper board is a test implementation, not final content.

Confirmed structure:

- board size is `5x5`
- the center node is seeded automatically
- each node has a generated test `+1` to `+3 Accuracy` value
- only orthogonally adjacent nodes can be activated
- every total alien kill gives `1` available skill point
- spent points are derived from activated non-center nodes

This means the board uses soldier history as the progression currency source of truth, not a second independent counter.

## Stat Application Pattern

When a class is assigned, the probe applies direct deltas to the selected soldier entity.

Confirmed stat paths used:

- `DeltaAccuracy`
- `DeltaReflexes`
- `DeltaHitPoints`
- `DeltaUnmodifiedHitPoints`
- `DeltaTimeUnits`
- `DeltaUnmodifiedTimeUnits`

This is an important detail because it shows the class method can touch both the live and unmodified/base-side values where needed.

## Persistence Meaning

This probe is more important than a UI-only success.

The verified save test means a soldier-bound custom component plus direct stat mutation can survive this tested sequence:

1. assign class
2. observe stat change
3. save the game
4. quit to the main menu
5. load the save
6. confirm the class is still present

That is the current strongest local evidence for a future permanent soldier class system.

## What This Now Proves

The class-system method is now proven for the tested flow:

- custom class UI
- one-way class choice
- soldier-bound custom state
- immediate stat mutation
- save / quit / load persistence
- class-specific progression UI after class lock
- kill-count-driven progression points
- adjacency-gated progression activation
- persistent progression-node state
- additional live stat mutation from progression nodes

## What Is Still Not Solved

This does **not** yet mean the final class mod is complete.

Still needing later work:

- UI polish
- final balance values
- final naming and localization
- compatibility rules if class IDs or component shapes change later
- broader persistence tests across more edge cases
- campaign transition and combat transition validation
- progression support for non-sniper classes
- testing progression effects that target campaign-layer systems instead of only the soldier

## Recommended Documentation Rule

When moving from the probe to the production class mod:

- do not rename stored class IDs casually
- do not move component namespaces casually
- treat save compatibility as a design constraint from the start

The passive-skill work already suggested this risk.

The class probe now makes that warning more important.
