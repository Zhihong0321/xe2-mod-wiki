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
- [Direct Campaign Resource Deltas](04-systems/direct-campaign-resource-deltas.md)
- [Direct Personnel Backend Command](04-systems/direct-personnel-backend-command.md)
- [Research Project Overrides](04-systems/research-project-overrides.md)
- [Save Persistence Findings](04-systems/save-persistence.md)

### Reusable Patterns

- [Kill-Count Driven Sync Pattern](05-patterns/kill-count-driven-sync.md)
- [Dropship Capacity And Starting Pools](05-patterns/dropship-capacity-and-starting-pools.md)

### Safety

- [Startup Safety And Recovery](06-safety/startup-and-recovery.md)
- [Failure Patterns And Reset Rules](06-safety/failure-patterns-and-reset-rules.md)

### Implementation History

- [UI Component Probe Implementation History](07-implementations/ui-component-probe.md)
- [Class System Probe](07-implementations/class-system-probe.md)

### Research Backlog

- [Open Questions And Next Tests](08-research/open-questions-and-next-tests.md)
- [Session Handoff: Personnel Count Reset](08-research/session-handoff-personnel-count-reset.md)

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
- a custom class panel can assign a one-way soldier class
- that class choice can persist through save, quit to main menu, and reload

This is more significant than a balance tweak. It demonstrates that an entirely new system layer can be added on top of existing game flows.

### 3. Save / Load Behavior Is More Promising Than Expected

Observed and confirmed in the latest tests:

- a previously enabled passive remained `ON` after loading the game
- a soldier class assigned through the class probe remained set after save, quit to main menu, and load

Because both the passive panel and the class probe are driven by custom soldier-bound state, this now provides stronger evidence that the tested custom-component path survived save / load. That is a major milestone, although edge cases still need systematic validation.

### 4. Lifecycle Cleanup Matters

The passive panel originally stayed visible after leaving the soldier page because it was created on a persistent overlay canvas.

Confirmed fix:

- explicit cleanup hooks on `Hide`, `OnTeardown`, and `Destroy`
- soldier-overview cleanup
- main-menu reset cleanup

### 5. Research Project Overrides Are Reliable

Confirmed:

- direct `ProgressPoints` overrides work for fast research testing
- `research_duration0` is not instant
- custom research text can require explicit `LocalizableGUID` plus locale CSV rows

### 6. Campaign Resources Are Directly Mutable

Confirmed:

- funds can be changed directly through backend entity mutation
- operation points can be changed directly through backend entity mutation
- doomsday can be changed directly through backend global-variable mutation
- the visible geoscape UI reacts to those changes in live play

## How To Read This Wiki

If you are new to the project:

1. Read the foundations page.
2. Read setup and deployment.
3. Read the UI injection journey.
4. Read the passive-skill system page.
5. Read startup safety before changing any live mod package.
