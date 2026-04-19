# UI Injection Journey

## Why This Page Exists

The project began by asking whether Xenonauts 2 UI could be extended at all.

That question is now answered: yes.

What followed was a progression from rough overlays to page-specific interactive controls and then to a working passive-skill system.

## Stage 1: Global Overlay Proof

Confirmed:

- a screen-space overlay canvas could be created from the mod
- visible debug banners appeared on the main menu and strategy screens
- overlay UI could sit above normal game UI

This proved rendering, but not clean native placement.

## Stage 2: Input Proof

Confirmed:

- a custom strategy-screen button received clicks
- a click counter updated in real time
- the overlay stayed interactive even while some in-game dialogs were open

This proved input handling, not just visual draw.

## Stage 3: Soldier Page Hook Proof

The next step targeted real soldier-related classes:

- `Strategy.UI.Elements.Soldiers.SoldiersOverviewElement`
- `Xenonauts.Strategy.UI.SoldierInfoPanelController`

Confirmed:

- those hooks fired
- page-specific custom UI could be shown during soldier-related flows

This was the first strong evidence that the mod could interact with a real gameplay-facing page rather than a generic overlay.

## Stage 4: Native-Root Attachment

Early attempts that climbed to a shared canvas behaved too much like a global overlay.

The next improvement was to attach closer to the page root itself.

Result:

- a compact soldier-page control became visible in the native soldier-page region
- input still worked
- the project moved from proof-of-concept overlay behavior toward usable page augmentation

## Stage 5: First Real Feature

The UI experiment evolved into a real system prototype:

- a `PASSIVE SKILLS` panel
- three clickable toggles
- selected-soldier labeling
- live stat summaries

The important transition here was conceptual:

- no longer just "can we draw UI?"
- now "can we add a new gameplay system through custom UI and code?"

## Stage 6: Crash Fix

The first toggle implementation crashed because refresh logic used reflection with ambiguous method lookup.

Confirmed root cause:

- `SoldierInfoPanelController` exposes overloaded methods
- name-only `GetMethod()` was unsafe

Confirmed fix:

- choose a compatible overload
- wrap the click path in `try/catch`

## Stage 7: Lifecycle Cleanup Fix

After toggles worked, a new issue appeared:

- the added UI did not disappear after leaving the soldier page
- it remained on screen and blocked other UI
- it could even survive the return to the main menu

Confirmed cause:

- the passive panel lived on a persistent overlay canvas
- the mod created the UI but did not fully tear it down when the page closed

Confirmed fix:

- cleanup on `Hide`
- cleanup on `OnTeardown`
- cleanup on `Destroy`
- cleanup when the soldiers overview re-enters
- cleanup when the main menu sets up

This is a critical lesson for all future custom UI systems:

Rendering and clicking are not enough. Lifecycle cleanup is part of a production-ready UI mod.

Additional note from later banner testing:

- a debug banner attached into the existing UI tree was unreliable
- a dedicated `ScreenSpaceOverlay` canvas with high sorting order rendered consistently

That makes a separate overlay canvas the safer option for temporary diagnostics, even when the long-term feature should live inside a native page tree.

## Current Conclusion

The project has now proven all of the following in one line of work:

- custom UI injection
- custom UI input
- soldier-page integration
- live stat mutation
- custom state management
- save/load promise
- lifecycle cleanup requirements

That is the foundation for many future mod ideas, including systems that do not exist in the base game UI.
