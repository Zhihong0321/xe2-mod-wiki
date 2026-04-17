# Kill-Count Driven Sync Pattern

## Status

Confirmed from the working `Soldier Kill Counter Hotkey` test mod.

## Core Discovery

The stable feature pattern was not "apply a bonus when the hotkey is pressed."

The stable pattern was:

- read kill count from soldier history
- derive the desired bonus from that history
- reconcile the difference during a periodic sync

## Reliable Source Of Truth

The tested source of truth was:

- `Xenonauts.Common.Util.ActorHistoryExtensions.REG_TOTAL_KILL_COUNT`

## Reliable Loop Pattern

The tested loop ran from a Harmony patch on `StrategyScreen.Update`, with a periodic sync around every `0.5s`.

The important design idea is:

- current game history is authoritative
- the mod computes a target effect from that history
- the mod applies only the missing delta

## Reconciliation Pattern

The reusable logic is:

1. read current kill count
2. compute target bonus from kill count
3. read how much the mod has already applied
4. subtract to get the missing delta
5. apply only that delta
6. store the newly applied amount

## Why This Pattern Is Safe

It avoids:

- infinite stacking
- dependence on one fragile event
- duplication when the sync loop runs repeatedly

## Why This Pattern Matters Beyond Kill Count

The same approach can drive:

- bravery from kills
- perk tiers from milestones
- one-time unlocks
- event counters
- accumulated training systems

## Practical Rule

When a feature can be derived from existing persistent game state, prefer:

- derive target value
- reconcile delta

over:

- blindly add effect again when a button or event fires
