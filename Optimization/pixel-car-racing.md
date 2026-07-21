# Pixel Car Racing — Optimization

Runtime performance work specific to this title.

## Object pooling
- Pooled clouds, stars, parallax pieces, and flip/combo UI popups
- Generic get/release path avoids `Instantiate`/`Destroy` during long runs
- Keeps endless side-scroll decoration from generating GC spikes

## Physics / frame budget
- Simulation forces in `FixedUpdate` for stable vehicle behavior
- Visual interpolation and non-physics presentation in `Update`
- Target feel: stable **~60 FPS** on mid-to-high Android devices

## Content efficiency
- Vehicle stats and upgrades live in ScriptableObjects (lightweight assets) instead of unique heavy prefab logic per car
- One shared driving controller + data variants reduces code/runtime duplication across **20+** cars

## UI / audio cost control
- Dashboard reads precomputed telemetry; does not recalculate gearbox logic in UI scripts
- Engine layer crossfades driven by normalized RPM (bounded clip set) rather than spawning new audio logic paths per frame

## Outcome
Racing sessions stay smooth during prop-heavy stretches and flip-combo feedback without hitching from allocation spikes.
