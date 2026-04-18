# Xenonauts 2 Modding Wiki

This repository is a structured knowledge base for the Xenonauts 2 modding work carried out in the local test environment.

The goal is not just to keep raw notes. The goal is to preserve:

- confirmed findings
- implementation patterns
- failure cases
- safe workflows
- reusable system designs

## Why This Matters

The biggest breakthrough so far is that Xenonauts 2 can be extended beyond simple stat edits or JSON tweaks.

The current evidence shows that we can:

- inject custom UI into live game screens
- add an entirely new gameplay-facing system layer on top of the base game
- attach new state to soldiers through a custom component
- toggle that state from custom UI
- apply live stat changes from that state
- persist the observed passive-state behavior through save / load in the tested scenario

That moves the project from "modding values" into "building systems."

## Source Of Truth

Before making assumptions, always check the official Xenonauts 2 modding wiki first:

- [GoldhawkInteractive/X2-Modding Wiki](https://github.com/GoldhawkInteractive/X2-Modding/wiki)

This repo records local implementation evidence and working patterns. The official wiki remains the upstream reference for mod structure, manifest behavior, code-mod workflow, and game-facing conventions.

## Documentation Map

- [Docs Home](docs/index.md)
- [Official Sources And Working Rules](docs/01-foundations/official-sources-and-rules.md)
- [Manual Mod Layout And Detection](docs/02-setup/manual-mod-layout.md)
- [Build And Deploy Workflow](docs/02-setup/build-and-deploy.md)
- [UI Injection Journey](docs/03-ui/ui-injection-journey.md)
- [Passive Skill System](docs/04-systems/passive-skill-system.md)
- [Save Persistence Findings](docs/04-systems/save-persistence.md)
- [Kill-Count Driven Sync Pattern](docs/05-patterns/kill-count-driven-sync.md)
- [Dropship Capacity And Starting Pools](docs/05-patterns/dropship-capacity-and-starting-pools.md)
- [Startup Safety And Recovery](docs/06-safety/startup-and-recovery.md)
- [UI Component Probe Implementation History](docs/07-implementations/ui-component-probe.md)
- [Open Questions And Next Tests](docs/08-research/open-questions-and-next-tests.md)

## Documentation Standards

Each page should clearly separate:

- `Confirmed`: verified by direct test or shipped local implementation
- `Observed`: behavior seen in a run, screenshot, save, or user test
- `Research`: plausible or code-backed, but not yet fully tested
- `Warning`: known ways to break startup, UI, or live installs

## Current Status Snapshot

As of the latest documented progress:

- startup is stable with the current `UI Component Probe` package
- passive-skill toggles work
- passive-skill bonuses apply live
- the injected soldier-page UI now has explicit cleanup hooks
- passive state remained `ON` after loading a save in the tested scenario

## Scope

This wiki currently covers:

- manual mod packaging
- content-mod detection
- code-mod assembly placement
- UI injection
- custom soldier systems
- save-persistence observations
- safe iteration practices
- reusable patterns for future mods
