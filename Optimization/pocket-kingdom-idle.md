# Pocket Kingdom Idle — Optimization

Runtime performance work specific to this title.

## Combat object pooling
- Pools for mobs, projectiles, health bars, damage popups, and spell VFX
- High-frequency hit feedback reuses health-bar display paths instead of per-hit instantiation

## Spawn budgets
- Concurrent mob cap (default around **40**)
- Initial population staggered across frames to avoid load hitch on enter
- Spell bursts (e.g. multi-arrow volleys) still draw from pools

## Large-number hot path
- Reusable scratch `BigInteger` instances for frequent earn/spend/damage ops
- Add/subtract early-out when exponent gap is large (dominant value wins) — less useless precision work late-game
- Avoids `System.Numerics.BigInteger` allocation patterns on every combat tick

## Frame target
- `Application.targetFrameRate = 60`
- Idle combat loop designed to stay interactive under continuous spawn/collect pressure

## Outcome
Long idle sessions with constant enemy turnover and spell FX remain closer to a stable 60 FPS target without GC thrash from spawn/despawn or oversized numeric objects.
