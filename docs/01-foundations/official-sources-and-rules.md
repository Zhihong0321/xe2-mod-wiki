# Official Sources And Working Rules

## First Rule

Before guessing how to mod Xenonauts 2, check the official wiki first:

- [GoldhawkInteractive/X2-Modding Wiki](https://github.com/GoldhawkInteractive/X2-Modding/wiki)

Treat it as the source of truth for:

- mod structure
- manifest format
- load behavior
- code-mod workflow
- GameAPI and event guidance

## Local Documentation Rule

This repository is for tested local findings and implementation knowledge.

Use it to document:

- what worked
- what failed
- how it was built
- what remains uncertain

Do not let local notes silently replace official upstream documentation.

## Evidence Labels

All future documentation should prefer these labels:

### Confirmed

Directly tested and observed in-game, in a build, or in logs.

### Observed

Seen in a user test, screenshot, save/load result, or runtime behavior, but not yet broad enough to generalize everywhere.

### Research

Suggested by assembly inspection, code reading, or official docs, but not yet proven by a live end-to-end test.

### Warning

A known way to break startup, the mod registry, UI behavior, or the live install.

## Development Rules That Have Already Paid Off

### Keep Source And Live Install Separate

Use `codex_mods/` as the editable source area.

Treat the live AppData mod folder as a deploy target only.

This reduces confusion about which files are safe to experiment with and which files can poison startup.

### Change One Variable At A Time

When troubleshooting, avoid changing several of these at once:

- manifest structure
- code
- content data
- mod UID
- live `mods.json`

Single-variable changes make failures traceable.

### Update Visible Metadata On Every Live Test

For active test mods:

- bump `asset.Version`
- update `Description`
- keep the root manifest schema version unchanged unless the schema itself changes

This makes it obvious which build the game actually loaded.

### Never Guess About Startup-Sensitive Manifest Fields

The local project already proved that one wrong manifest change can break the boot path before the main menu.

## Interpretation Rule

When a new technique appears to work, do not immediately generalize it to "the engine supports everything."

Instead separate the claim:

- exact thing proven
- nearby thing strongly suggested
- future thing still untested

This prevents overclaiming and keeps the wiki credible.
