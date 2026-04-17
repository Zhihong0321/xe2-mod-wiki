# UI Component Probe Implementation History

## Purpose

`UI Component Probe` started as a feasibility test and evolved into the first working custom soldier-page system prototype.

This page records the important milestones and what each stage proved.

## Early Probe Phase

Initial purpose:

- prove code-mod loading
- prove Harmony patch execution
- prove custom UI rendering

Confirmed outcomes:

- `Create()` fired
- overlay banners rendered
- strategy-screen update hooks fired

## Strategy Interaction Phase

Next purpose:

- prove that the custom UI could receive input

Confirmed outcomes:

- custom strategy-screen button clicks worked
- the click counter updated correctly

This established that the project was not limited to passive overlays.

## Soldier-Page Hook Phase

Next purpose:

- prove page-specific UI injection on soldier-related pages

Targeted classes:

- `Strategy.UI.Elements.Soldiers.SoldiersOverviewElement`
- `Xenonauts.Strategy.UI.SoldierInfoPanelController`

Confirmed outcomes:

- soldier-page hooks fired
- attached UI could appear in those flows

## Native Placement Iteration

Early soldier-page UI still behaved too much like a global overlay.

The project then refined:

- attachment parent choice
- probe size
- placement region

This led to a compact interactive control placed inside the soldier-page region instead of just hovering over the whole screen.

## Passive-System Phase

The probe then became the first real feature build:

- `PASSIVE SKILLS` panel
- three toggles
- selected soldier name
- live stat summaries

This changed the mod from "UI probe" to "system prototype."

## Crash-Fix Phase

Problem:

- clicking a passive toggle crashed the game

Confirmed root cause:

- reflection-based refresh logic used method-name lookup only
- overloaded controller methods caused `AmbiguousMatchException`

Fix:

- compatible overload selection
- safer invocation
- `try/catch` on the click handler

## Persistence Surprise

Observed result after further testing:

- passive state remained enabled after loading a save

This was one of the most important results in the entire project so far.

## Cleanup-Fix Phase

Problem:

- after leaving the soldier page, the added UI stayed alive and blocked other interface elements

Fix:

- cleanup on soldier controller `Hide`
- cleanup on `OnTeardown`
- cleanup on `Destroy`
- cleanup on soldiers-overview re-entry
- cleanup on main-menu setup

## Current Meaning Of This Implementation

`UI Component Probe` is no longer just a probe.

It is the working reference implementation for:

- custom soldier-page UI
- soldier-bound custom state
- live gameplay mutation
- lifecycle cleanup
- first-step persistence behavior

## Current Versioning Lesson

For this mod family:

- keep root manifest schema `version` unchanged unless the schema itself changes
- bump `asset.Version` every live test build
- describe the visible change in the manifest `Description`
