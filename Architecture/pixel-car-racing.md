# Pixel Car Racing — Architecture

## Composition
- Generic `MonoSingleton<T>` services for game, play, audio, save, money, region, effects
- Persistent app services vs race/play session modules
- UI (`CarDashboard`, battery, menus) reads telemetry; does not own physics

## Data
- `CarData` / balance ScriptableObjects as the extension point for new vehicles and upgrades
- Saved progress models map ownership and tuning into JSON for local/cloud

## Bootstrap & scenes
- Main menu / showroom / race flows composed as dedicated managers
- Platform gates (UMP, updates, cloud) handled before or around play entry

## Why this shape
Fast Unity iteration for a roster-heavy racer: add cars as data, keep one physics brain, keep UI and simulation separated.
