# Docs Home

This page is the top-level navigation hub for the local Xenonauts 2 modding knowledge base.

## Core Principle

Always begin with the official mod wiki:

- [GoldhawkInteractive/X2-Modding Wiki](https://github.com/GoldhawkInteractive/X2-Modding/wiki)

Use this repository to capture tested local evidence, practical implementation detail, and hard-won failure lessons.

## Quick Navigation

### Foundations

- [Official Sources And Working Rules](01-foundations/official-sources-and-rules.md)

### Setup And Packaging

- [Manual Mod Layout And Detection](02-setup/manual-mod-layout.md)
- [Build And Deploy Workflow](02-setup/build-and-deploy.md)

### UI And Systems

- [UI Injection Journey](03-ui/ui-injection-journey.md)
- [Passive Skill System](04-systems/passive-skill-system.md)
- [Save Persistence Findings](04-systems/save-persistence.md)

### Reusable Patterns

- [Kill-Count Driven Sync Pattern](05-patterns/kill-count-driven-sync.md)

### Safety

- [Startup Safety And Recovery](06-safety/startup-and-recovery.md)

### Implementation History

- [UI Component Probe Implementation History](07-implementations/ui-component-probe.md)

### Research Backlog

- [Open Questions And Next Tests](08-research/open-questions-and-next-tests.md)

## Most Important Confirmed Breakthroughs

### 1. UI Injection Is Real

Confirmed:

- Harmony patches work in Xenonauts 2 code mods
- overlay UI can be rendered
- overlay UI can receive input
- page-specific UI can be attached to soldier-related screens

### 2. New Gameplay Layers Are Possible

Confirmed:

- a custom passive-skill panel can be injected onto the soldier page
- clicking custom toggles can change soldier state
- those toggles can apply live stat deltas to the selected soldier

This is more significant than a balance tweak. It demonstrates that an entirely new system layer can be added on top of existing game flows.

### 3. Save / Load Behavior Is More Promising Than Expected

Observed in the latest test:

- a previously enabled passive remained `ON` after loading the game

Because the panel UI is driven by custom passive state, this strongly suggests that the tested passive-state data path survived save / load. That is a major milestone, although edge cases still need systematic validation.

### 4. Lifecycle Cleanup Matters

The passive panel originally stayed visible after leaving the soldier page because it was created on a persistent overlay canvas.

Confirmed fix:

- explicit cleanup hooks on `Hide`, `OnTeardown`, and `Destroy`
- soldier-overview cleanup
- main-menu reset cleanup

## How To Read This Wiki

If you are new to the project:

1. Read the foundations page.
2. Read setup and deployment.
3. Read the UI injection journey.
4. Read the passive-skill system page.
5. Read startup safety before changing any live mod package.
