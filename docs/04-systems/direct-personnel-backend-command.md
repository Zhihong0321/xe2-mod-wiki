# Direct Personnel Backend Command

## Why This Page Exists

This page records the first locally proven method for changing engineer or scientist count in live strategy play without using the native hire screen.

The important result is not just "a button worked."

The important result is:

- a direct backend command path can change displayed personnel count
- the path is reusable for future mods
- the path is suitable for campaign-layer passive or progression systems

## Proven Result

Confirmed in the live experiment mod:

- `+1 ENGINEER` changed the visible engineer count from `5` to `6`
- engineer wage cost updated in the base UI
- the command was issued from custom code, not by clicking the vanilla `HIRE ENGINEER` button

This means engineer count can be manipulated through strategy backend systems directly.

## Proven Working Path

The working flow is:

1. get the active strategy `World`
2. resolve the Xenonaut player entity
3. resolve a valid owned geobase
4. get `HiringPoolSystem`
5. get the correct hiring pool for the target division
6. create `HirePersonnelCommand.FromPoolGeneric(...)`
7. send the command with `world.HandleEvent(...)`

## Working Components And Systems

The confirmed reusable pieces are:

- `Xenonauts.Strategy.Systems.PersonnelManagementSystem`
- `Xenonauts.Strategy.Systems.HiringPoolSystem`
- `Xenonauts.Strategy.Systems.HirePersonnelCommand`
- `Xenonauts.Strategy.Components.DivisionTypeComponent.Type.Engineer`
- `Xenonauts.Strategy.Components.DivisionTypeComponent.Type.Scientist`

## Reusable Pattern

Practical implementation pattern:

```csharp
Artitas.World world = /* active strategy world */;
Artitas.Entity playerEntity = /* Xenonaut player entity */;
Artitas.Entity geoBase = /* resolved owned geobase */;

var hiringPoolSystem =
    world.GetSystem(typeof(Xenonauts.Strategy.Systems.HiringPoolSystem))
    as Xenonauts.Strategy.Systems.HiringPoolSystem;

Artitas.Entity hiringPool = hiringPoolSystem.GetHiringPool(
    playerEntity,
    Xenonauts.Strategy.Components.DivisionTypeComponent.Type.Engineer);

var command = Xenonauts.Strategy.Systems.HirePersonnelCommand.FromPoolGeneric(
    geoBase,
    hiringPool,
    1,
    true,
    true,
    false);

world.HandleEvent(command);
```

To add a scientist instead:

- keep the same pattern
- change the division type to `Scientist`

## What Must Be Resolved First

This path only works if these three inputs are valid:

- `world`
- `playerEntity`
- `geoBase`

The experiment session confirmed all three can fail if resolved too early or through the wrong context.

### World

The active strategy `World` must already exist.

Do not assume it is valid during every early screen update.

### Player Entity

The player entity can be read from `PersonnelManagementSystem`, but the property getter can throw before the faction layer is fully ready.

Safe rule:

- treat player lookup as nullable
- make reflection or property reads non-throwing
- do not let debug UI crash the game if the player is temporarily unavailable

### GeoBase

The geobase should preferably come from the active strategy screen context.

Good resolution order:

1. remembered recruit-page geobase
2. active `StrategyScreen` target / parameters
3. owned geobase scan from live entities

## Important Negative Result

The same experiment also proved something equally important:

- displayed engineer count is **not** explained only by `EngineeringSystem.GetOccupiedWorkshopSlots(...)`

Observed result after the successful `+1 ENGINEER` test:

- visible engineer count increased
- wage cost changed
- `engOccupied`, `engTransit`, and `engAll` snapshot values did **not** change

Practical takeaway:

- workshop-slot families are not the full source of truth for displayed engineer count
- do not assume `occupied workshop slots == current engineers at base`

This matters for future passive systems because a campaign bonus may need to target the same backend path that the hire command uses, not only slot collections.

## Best Current Reuse Guidance

For future mods that grant engineers or scientists directly:

- prefer backend command creation over UI simulation
- prefer backend command creation over editing visible text
- do not start by mutating workshop or laboratory slot counts directly

Use this method for:

- passive skills that grant personnel
- campaign events that grant personnel
- debug tools
- progression rewards
- temporary experiment consoles

## What Is Proven Versus Still Open

Proven:

- engineer count can be increased directly through backend command flow
- the same pattern is valid in principle for scientists
- custom in-game test UI can safely trigger the backend path once `world`, `player`, and `base` are resolved

Still open:

- which exact component or entity field drives the final displayed personnel count after hire
- whether direct fire / remove flows should use the mirrored backend command path
- whether scientist count follows the same internal split between display count and slot-family count

## Recommendation For Future Work

Treat this as the default personnel-grant pattern for serious mod development.

If a future system needs:

- `+1 engineer`
- `+1 scientist`
- `grant personnel on unlock`
- `grant personnel from passive progression`

start from this backend command path first.

Do **not** reopen the old approach of guessing UI-only hooks or treating workshop-slot families as the only count source.
