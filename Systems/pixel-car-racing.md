# Pixel Car Racing — Systems

Gameplay and platform systems owned on this title.

## Vehicle physics & gearbox
- Wheel locomotion via continuous `Rigidbody2D` torque (`AddTorque`), not transform cheating
- Custom RPM normalization between authored min/max engine limits
- Dynamic gear ratios, shift thresholds from speed/RPM, torque curves per car personality
- Raycast ground checks change drag airborne vs grounded (jumps/bridges)

## Data-driven car roster
- `CarData` ScriptableObjects: speed, acceleration, gear count, RPM limits, upgrade multipliers, visuals
- Calculated tuned stats from upgrade levels without per-car controller forks
- Scaled to **20+** distinct vehicles on one driving brain

## Telemetry UI & audio feel
- Dashboard / battery UI consume simulation outputs only (no physics in UI)
- Engine audio crossfades Idle / Low / Med / High from normalized RPM
- Pitch/volume modulated by load and gear speed ratios

## Progression & meta
- Showroom, tuning, unlock, test-ride, and race end flows as dedicated modules
- Currency and ownership tied to saved car progress
- Lifetime statistics (distance, flips, airtime, pumps, spend, etc.)

## Persistence & platform
- JSON local save with legacy schema migration fallback
- Google Play Saved Games cloud sync
- UMP consent → rewarded ads
- Remote update checks, localization, DOTween UI/camera polish

## Architecture notes
- `MonoSingleton` services (`GameManager`, `PlayManager`, `AudioManager`, `SaveManager`, …)
- Physics in `FixedUpdate`; presentation in `Update`
