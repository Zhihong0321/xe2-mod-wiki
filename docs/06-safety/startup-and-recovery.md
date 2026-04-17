# Startup Safety And Recovery

## Why This Page Exists

The project has already hit multiple startup-breaking states.

Those failures were expensive, but they produced important rules that should now be treated as hard safety policy.

## Critical Manifest Rule

Confirmed startup-breaking mistake:

- changing the root manifest schema field `version` away from `0.1.0`

Observed consequence:

- broken manifest uplift
- failed startup
- skipped or frozen boot path before normal menu flow

## Safe Versioning Rule

For ordinary mod updates:

- safe: change `asset.Version`
- safe: update `Description`
- unsafe: casually change the root manifest schema `version`

## Analyzer Failure Lesson

The `AI-Assisted Battle Analyzer` / `AI Assisted Battle Kit` line of work became startup-poisoning.

Important lessons from that failure:

- reusing one live mod UID for multiple very different package shapes is risky
- repeatedly mutating the same live package during recovery is risky
- hand-editing `mods.json` can leave invalid state behind
- a broken disabled mod can still affect startup if Xenonauts scans its manifest path

## Safe Recovery Procedure

If startup breaks:

1. remove the bad mod from the live `Mods` folder
2. remove its entry from live `mods.json`
3. validate `mods.json`
4. launch the game cleanly with the mod absent
5. only then rebuild or restore anything

## Safe Development Rules

### Treat Live Mods As Deploy Artifacts

Do not treat the AppData mod folder as your experimental scratch space.

### Use New UIDs For Risky Structural Changes

If a package has already become unstable, do not keep reshaping the same live UID.

### Avoid Multi-Variable Recovery

When the game is already unstable, do not simultaneously change:

- package shape
- manifest
- code
- content
- registry state

## UI Safety Rule

A UI mod is not safe just because it renders and clicks.

It also needs:

- exit cleanup
- screen transition cleanup
- reliable targeting
- safe refresh logic

The passive-skill panel issue proved that forgotten cleanup can block unrelated UI even without crashing the game.
