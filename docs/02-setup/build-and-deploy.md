# Build And Deploy Workflow

## Confirmed Code-Mod Placement

The official modding guidance and local tests agree on the safe DLL location:

```text
<mod-root>/assembly/common/YourMod.dll
```

This is the path that allowed the `UI Component Probe` assembly to load and execute Harmony patches successfully.

## Local Build Workflow

The local test machine successfully built the code mod with PowerShell `Add-Type`, without requiring a full external `.NET SDK` workflow.

Referenced assemblies included:

- `Assembly-CSharp.dll`
- `Assembly-CSharp-firstpass.dll`
- `0Harmony.dll`
- `netstandard.dll`
- `UnityEngine.dll`
- `UnityEngine.CoreModule.dll`
- `UnityEngine.UI.dll`
- `UnityEngine.UIModule.dll`
- `UnityEngine.TextRenderingModule.dll`
- `UnityEngine.InputLegacyModule.dll`

## Practical Build Pattern

The working pattern was:

1. keep source in `codex_mods/<mod-name>/src/`
2. compile directly against the game-managed assemblies
3. output the DLL straight into the live mod folder under `assembly/common/`

This is fast for iteration, but it increases the importance of versioning and safe recovery rules because the live package changes immediately.

## Recommended Deployment Discipline

When a build changes behavior:

- bump `asset.Version`
- update `Description`
- rebuild the DLL
- test the game with that exact visible version

## Current Live Passive-System Package

The current passive-system work lives in the `UI Component Probe` mod family.

Important behavior it has now demonstrated:

- code-mod load succeeds
- Harmony-patched UI hooks fire
- custom soldier-page UI can be created
- custom buttons can change soldier state
- the latest build adds cleanup for page exit and screen transitions

## Warning About Direct Live Output

Compiling directly to the live mod folder is convenient, but it means a bad build can immediately affect:

- startup
- menu flow
- UI interaction
- save testing confidence

Use the safety rules in the startup page before and after risky changes.
