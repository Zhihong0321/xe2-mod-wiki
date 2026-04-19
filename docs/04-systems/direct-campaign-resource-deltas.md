# Direct Campaign Resource Deltas

## Why This Page Exists

This page records the first locally proven backend methods for changing visible campaign resources directly in live strategy play.

The important result is not just that a debug button changed a number.

The important result is:

- campaign funds can be changed directly through backend entity state
- operation points can be changed directly through backend entity state
- doomsday can be changed directly through a backend global-variable entity
- these paths are reusable for debug tools, rewards, passives, and progression systems

## Proven Result

Confirmed in the live experiment console after the startup-safety hardening pass:

- `+1000 MONEY` changed the visible `FUNDS AVAILABLE` value
- `+1 OPS` changed the visible operation points count on the geoscape
- `-1 DOOM` changed the visible doomsday value on the geoscape
- the changes were issued from custom backend code, not by simulating native UI buttons

This means all three values can be manipulated directly through strategy-layer backend state.

## Proven Working Paths

### Money

The working flow is:

1. get the active strategy `World`
2. resolve the Xenonaut player entity
3. use the player entity as the primary cash owner
4. confirm the target entity has `Cash`
5. call `DeltaCash(...)`

### Operation Points

The working flow is:

1. get the active strategy `World`
2. resolve the Xenonaut player entity
3. use the player entity as the primary operation-points owner
4. confirm the target entity has `OperationPoints`
5. call `DeltaOperationPoints(...)`

### Doomsday

The working flow is:

1. get the active strategy `World`
2. resolve `GlobalVariableSystem`
3. resolve the `DOOMSDAY_COUNTER` global-variable entity
4. confirm that entity has `DoomsdayCounter`
5. call `DeltaDoomsdayCounter(...)`

## Working Components And Systems

The confirmed reusable pieces are:

- `Artitas.Entity.DeltaCash(...)`
- `Artitas.Entity.DeltaOperationPoints(...)`
- `Artitas.Entity.DeltaDoomsdayCounter(...)`
- `Xenonauts.Common.GlobalVariables.GlobalVariableSystem`
- `Xenonauts.XenonautsConstants.GlobalVariables.DOOMSDAY_COUNTER`

Useful optional diagnostics:

- `Xenonauts.XenonautsConstants.GlobalVariables.OPERATION_POINTS_MODIFIER`
- `Xenonauts.XenonautsConstants.GlobalVariables.DOOMSDAY_COUNTER_MODIFIER`
- `Xenonauts.XenonautsConstants.GlobalVariables.DOOMSDAY_COUNTER_ACTIVE`

Those optional global variables are useful for readouts and debugging, but they are **not** required for the mutation path itself.

## Reusable Pattern

### Money

```csharp
Artitas.World world = /* active strategy world */;
Artitas.Entity playerEntity = /* Xenonaut player entity */;

if (playerEntity != null && playerEntity.HasCash())
{
    playerEntity.DeltaCash(1000f);
}
```

### Operation Points

```csharp
Artitas.World world = /* active strategy world */;
Artitas.Entity playerEntity = /* Xenonaut player entity */;

if (playerEntity != null && playerEntity.HasOperationPoints())
{
    playerEntity.DeltaOperationPoints(1f);
}
```

### Doomsday

```csharp
Artitas.World world = /* active strategy world */;

var globalVariableSystem =
    world.GetSystem(typeof(Xenonauts.Common.GlobalVariables.GlobalVariableSystem))
    as Xenonauts.Common.GlobalVariables.GlobalVariableSystem;

Artitas.Entity doomEntity =
    globalVariableSystem.Get(Xenonauts.XenonautsConstants.GlobalVariables.DOOMSDAY_COUNTER);

if (doomEntity != null && doomEntity.HasDoomsdayCounter())
{
    doomEntity.DeltaDoomsdayCounter(-1f);
}
```

## What Must Be Resolved First

These paths are only safe if strategy context is actually ready.

Hard rule:

- do not read or modify strategy state while `StrategyScreen.World` is still null
- do not assume the Xenonaut player entity already exists during early screen updates
- do not treat global-variable lookups as guaranteed during partial load

Safe readiness rule:

1. `StrategyScreen.World` is non-null
2. Xenonaut player entity resolves successfully
3. only then allow experiment UI to read or write live strategy values

## Important Safety Result

The resource experiments also proved an equally important failure pattern:

- a debug panel that polls strategy data during partial load can crash the game

Observed failure:

- reading missing global variables from `GlobalVariableSystem` during `StrategyScreen.Update`
- unhandled exception: `No Global Variable Found!`

Practical takeaway:

- treat optional global-variable reads as nullable
- wrap update-hook debug UI in its own exception guard
- disable the experimental panel for the rest of the run if it faults, rather than risking another crash

## Important Negative Result

The generic snapshot panel showed one misleading range detail during live testing:

- money and doomsday sometimes displayed a debug `max` value of `-2,147,483,648`

Current interpretation:

- the direct delta path is proven
- the generic range metadata readout is **not** yet proven to be semantically meaningful for design work

So future systems should trust:

- visible UI value changes
- direct backend delta calls

Do **not** yet trust:

- the generic `min` / `max` snapshot output as a gameplay rule by itself

## Best Current Reuse Guidance

For future mods that grant or spend campaign resources directly:

- prefer direct backend entity deltas over UI simulation
- prefer direct backend entity deltas over save-file edits
- use player-entity mutation first for money and operation points
- use global-variable entity mutation for doomsday

Use this method for:

- debug consoles
- campaign events
- passive rewards
- progression rewards
- economy experiments
- strategic-operation prototypes

## What Is Proven Versus Still Open

Proven:

- money can be changed directly through backend entity mutation
- operation points can be changed directly through backend entity mutation
- doomsday can be changed directly through backend global-variable mutation
- the visible geoscape UI reacts to those backend mutations in live play

Still open:

- what the money / doomsday range `Maximum` field actually means in runtime debug output
- whether there are cleaner higher-level command wrappers for some of these values
- which of these values should be changed through direct deltas versus a formal event path in long-lived production mods

## Recommendation For Future Work

Treat this as the default first-pass pattern for strategy resource experiments.

If a future system needs:

- `+money`
- `+ops`
- `-doom`
- campaign resource reward buttons
- strategic reward passives

start from these backend entity delta paths first.

Do **not** reopen the old pattern of guessing UI-only hooks before trying direct backend mutation.
