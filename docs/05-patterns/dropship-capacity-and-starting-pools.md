# Dropship Capacity And Starting Pools

## Why This Page Exists

A live test with the `Now Everyone can Fly` mod produced a misleading result:

- the dropship profile JSON had already been changed from `8` crew slots to `10`
- the mod was enabled
- a brand-new campaign still showed the starting Skyhawk as `8/8`

The useful lesson was not just "edit the aircraft profile." The useful lesson was that starter aircraft can come from a different template path than the one you first expect.

## Confirmed

### Max Soldier Capacity Uses `InventorySlotSize`

For Xenonauts 2 dropships, the actual max soldier capacity is controlled by the crew inventory-slot component:

- `InventorySlotSize`

This is the value that determines whether the aircraft is effectively `8`, `10`, `12`, and so on.

`StrikeTeamSlotPreferredPositions` is related to how slots are laid out and presented, but it is not the core max-capacity value.

## Confirmed Starter-Campaign Trap

The starting Skyhawk for a fresh game did not reliably follow the intended balance change when only the shared aircraft profile was edited.

The working fix was to override the starter templates directly:

- `template/strategy/aircraft/starting_pool/dropship_starting1.json`
- `template/strategy/aircraft/tutorial_pool/dropship_tutorial1.json`

After those templates were patched with a `10`-slot crew definition, the fresh-campaign starter Skyhawk finally behaved correctly.

## Practical Rule

When a starting aircraft, soldier, or item ignores a balance change even though the base profile looks correct:

1. Verify the real stat field first.
2. Check whether the new-campaign instance is spawned from a `starting_pool` or `tutorial_pool` template.
3. Override that starter template directly if the fresh campaign still uses vanilla values.

## Worked Example

The proven fix path for the starting Skyhawk was:

1. increase the Skyhawk crew `InventorySlotSize` from `8` to `10`
2. keep the preferred slot list consistent with the larger crew size
3. patch both:
   - `starting_pool/dropship_starting1.json`
   - `tutorial_pool/dropship_tutorial1.json`
4. fully restart the game
5. start a brand-new campaign for the validation run

## Warning

Two easy mistakes came up during this test:

- confusing max capacity with slot-position layout
- assuming the shared profile is always the only template that matters for a fresh campaign

If the game still shows the old max value, do not assume the stat field is wrong. First confirm where that campaign object is actually instantiated from.

## Reusable Takeaway

This pattern is likely broader than dropships.

Starter content in Xenonauts 2 may be instantiated from dedicated pool templates that inherit from the normal profile chain, but still need direct attention during testing and mod validation.
