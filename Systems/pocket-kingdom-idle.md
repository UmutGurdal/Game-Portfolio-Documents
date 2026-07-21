# Pocket Kingdom Idle — Systems

Gameplay and platform systems owned on this title. Flagship system: **custom large-number economy**.

## Custom large-number economy (highest effort)
- Serializable scientific type: mantissa `Value` × 10^`Power` (not `System.Numerics.BigInteger`)
- Ops: multiply, divide, add, subtract, compare, floor, assign + normalization
- Idle-tuned add/sub: ignore negligible terms when exponent gap is large
- UI formatting: plain → K/M/B/T → scientific
- Exponent-aware **Buy Max / bulk buy** when wallet and cost differ by many orders
- Wired through combat HP/damage, gold, income, upgrades, offline payouts, prestige tickets, JSON/cloud saves

## Idle combat loop
- Stage progression **1–1,800** with rarity-weighted mob spawns
- Tap/swipe player + autonomous Archer / Mage; crit manager; five spells
- Collect gold/mana → building income tick → stage advance
- Difficulty classification from expected health vs strongest damage source

## Meta progression
- Prestige from stage 50+: collections → tickets (artifacts); stage-above-max → skill points
- Reset run state while keeping tickets, artifacts, skills
- Soft-start after prestige (income seed + first building level) to avoid dead starts
- Upgrade pages: Armory, Buildings, Souls, Skills, Artifacts, Prestige

## Offline progression
- Trusted time (Android automatic time or HTTP `Date`)
- Capped away entitlement + income multipliers
- Simulated stage clears from expected HP vs damage throughput
- Payout gated when time validation fails

## Data-driven balance
- ScriptableObjects: upgrades, buildings, skills, artifacts, spells, ads, mob database, languages
- Precomputed stage health/gold curves for the full stage range
- 8-language localization assets + tutorial/cutscene onboarding

## Persistence & platform
- `GameManager` bootstrap: remote update → local save → UMP → agreement → idle scene
- JSON local save; GPGS cloud restore on first install when available
- LevelPlay rewarded “dragon” rewards (stage-gated, data-driven actions)
- Achievements, stage leaderboard, in-app review, vibration/audio settings

## Architecture notes
- App root (`GameManager`) + gameplay composition root (`IdleManager`)
- ~14 managers; event-driven UI refresh from currency/stage/prestige/ads
