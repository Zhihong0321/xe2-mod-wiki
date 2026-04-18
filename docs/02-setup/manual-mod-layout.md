# Manual Mod Layout And Detection

## Confirmed Working Mod Location

Xenonauts 2 reliably detected local mods only after they were placed in the game-created manual mod folder:

```text
C:\Users\zhiho\AppData\LocalLow\Goldhawk Interactive\Xenonauts 2\My Games\Xenonauts 2\Mods\<mod-uid>\
```

This is the safest place to treat as the real live install target for manual mods.

## Required Root Files

At minimum, the mod root should contain:

- `manifest.json`
- any game-generated preview asset if applicable
- content folders such as `template/`
- code folders such as `assembly/common/` for DLL mods

The key rule is that `manifest.json` sits directly at the mod root.

## Manifest Shape That Worked

The project used the game-generated manual-mod shape as the baseline.

Important findings:

- the root schema field `version` is not the same thing as `asset.Version`
- live local mod updates should bump `asset.Version`
- ordinary iteration should not change the root schema `version`

## Practical Manual-Mod Rule

The safest workflow is:

1. Create an empty manual mod from inside the game.
2. Use the generated folder as the template.
3. Preserve its manifest style.
4. Add `template/` or `assembly/` content under that folder.

## Folder Layout Patterns

### Content-Mod Example

```text
<mod-root>/
├─ manifest.json
├─ preview.png
└─ template/
   ├─ groundcombat/
   └─ strategy/
```

### Code-Mod Example

```text
<mod-root>/
├─ manifest.json
└─ assembly/
   └─ common/
      └─ YourMod.dll
```

## What Did Not Work Reliably

Confirmed non-working or misleading paths:

- putting the mod only in Steam Workshop content folders
- putting the mod inside the game install folder and expecting it to behave like a manual mod
- inventing a manifest layout instead of following the generated schema

## Tactical Module Content-Mod Example

One early successful content-mod test was the `Improved Tactical Module` concept:

- new research: `IMPROVED TACTICAL MODULE`
- result: upgrade `TACTICAL MODULE`
- effect: `+7 Accuracy`
- effect: `10 weight`

The important lesson from that work was not just the item itself. The lesson was that detection became reliable only after the package lived inside the proper manual-mod folder.

## Starter-Template Reminder

Another later content-mod lesson is that a correct profile edit does not always guarantee that a fresh campaign uses the changed value immediately.

For example, the starting Skyhawk dropship capacity test only fully worked after patching the starter-template paths directly, not just the shared aircraft profile.

See:

- [Dropship Capacity And Starting Pools](../05-patterns/dropship-capacity-and-starting-pools.md)
