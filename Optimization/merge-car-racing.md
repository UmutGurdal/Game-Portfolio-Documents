# Merge Car Racing — Optimization

Runtime performance work specific to this title.

## Endless road budget
- Fixed rolling window of recycled road segments (not unbounded instantiation)
- Segments release/reuse as the player advances the highway

## Object pooling
- Generic prefab pools for traffic, VFX, and other high-churn race objects
- Prewarm + grow-on-demand; release returns instances to queues

## Traffic density control
- Spawn checks use overlap volumes before placing NPCs
- Weighted vehicle mix without packing impossible local density

## Economy / UI cadence
- Race income accumulates continuously but pays on a **1 second** cadence to cut UI/event spam
- Leaderboard submits throttled (about once per minute unless forced)
- Merge-board auto-generation pauses while the player is interacting

## Dual-scene cost awareness
- Merge and Race scenes load as modes rather than keeping both full simulations hot
- Shared save/data hub avoids duplicating heavy runtime state graphs

## Outcome
Endless highway racing and idle track production remain allocation-stable on mid-tier Android while still supporting near-miss FX, NPC AI, and merge-board interactivity.
