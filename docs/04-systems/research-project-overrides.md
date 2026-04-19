# Research Project Overrides

## Why This Page Exists

The `Improved Tactical Module` content mod provided a clean live test for two research-project behaviors that were previously only partially understood:

- how to make research effectively instant for testing
- how to force custom research name / description text to display correctly

These are now confirmed from a working local mod build.

## Confirmed

### 1. `research_duration0.json` Is Not Instant

Using the parent template:

- `ST-::-masters/projects/research/research_duration0.json`

did **not** produce a near-instant project in live play.

Observed result:

- the project still showed about `3 Days, 15 Hrs`

This matches the official modding wiki guidance that `research_duration0` is only the shortest preset, not zero-time.

Practical takeaway:

- do not use `research_duration0` as a shortcut for instant research tests

## Confirmed

### 2. Direct `ProgressPoints` Override Works

Research speed became effectively instant only after directly adding a `ProgressPoints` component to each research project with a max value of `1`.

Working component shape:

```json
{
  "_min": 0.0,
  "_val": 0.0,
  "_max": 1.0,
  "$type": "Xenonauts.Strategy.Components.ProgressPoints"
}
```

Practical takeaway:

- for fast test passes, override `ProgressPoints` directly
- use a very small `_max` such as `1.0`

## Confirmed

### 3. Raw `Name` / `Description` Alone May Not Control The UI Text

The Tactical Module research initially showed the correct title but an unrelated vanilla description:

- `Interrogation of The General may help explain why he betrayed our organisation.`

This happened even though the mod JSON already contained a custom `Description` component.

Practical takeaway:

- a research project may still resolve UI text from localization data instead of the raw component text you expect

## Confirmed

### 4. `LocalizableGUID` Plus Mod Locale CSV Fixes Research Text

The incorrect description was fixed by adding both:

- a `LocalizableGUID` component in each research JSON
- matching rows in mod-local locale CSV files

Working locale paths:

- `data/common/localization/locales/en-US.csv`
- `data/common/localization/locales/en-GB.csv`

Working `LocalizableGUID` pattern:

```json
{
  "$content": [
    {
      "GUID": "your-guid-for-name",
      "TargetComponent": "Common.Components.NameComponent",
      "Request": "Translate"
    },
    {
      "GUID": "your-guid-for-description",
      "TargetComponent": "Common.Components.DescriptionComponent",
      "Request": "Translate"
    }
  ],
  "$t": "LocalizableGUID"
}
```

Working CSV row pattern:

```csv
guid-here,Xenonauts.GameScreens.Strategy-::-projects/research/your_project.json#Name,Display Name,Display Name
guid-here-2,Xenonauts.GameScreens.Strategy-::-projects/research/your_project.json#Description,Display Description,Display Description
```

Practical takeaway:

- if a custom research project shows the wrong text, add explicit localization instead of relying only on inline `Name` / `Description`
- include both `en-US` and `en-GB` if you want the common English installs covered cleanly

## Working Test Case

Confirmed live with:

- mod: `Improved Tactical Module`
- version: `1.2.1`

Verified behaviors:

- all Tactical Module research projects loaded
- incorrect vanilla description was replaced by the intended custom text
- all research projects became effectively instant for testing after the `ProgressPoints` override

## Safe Rule Of Thumb

When creating or debugging custom research projects:

1. start with the official project structure from the Goldhawk wiki
2. use duration presets only for rough balance tiers
3. use direct `ProgressPoints` overrides for test-speed experiments
4. add explicit `LocalizableGUID` and locale CSV entries if project text appears wrong in the UI

## Source Notes

Official upstream reference:

- [GoldhawkInteractive/X2-Modding Wiki](https://github.com/GoldhawkInteractive/X2-Modding/wiki)

Useful official guidance confirmed by this test:

- duration presets are only presets, not guaranteed instant behavior
- research text can be localized through `LocalizableGUID`
