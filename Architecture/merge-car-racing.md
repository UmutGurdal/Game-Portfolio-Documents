# Merge Car Racing — Architecture

## Composition
- `InitScene` establishes persistent services, then routes to `MergeScene` or `RaceScene`
- `GameManager` owns shared `GameData`, consent, save/cloud, ads init, scene transitions
- Mode façades (`MergeUI`, `RaceUI`, currency, race, GPGS) resolve as scene children

## Data
- ScriptableObjects: game state/config, merge catalogue, race car stats, ad rewards, bonuses
- Merge progression is the meta source of truth; race consumes unlocks/stats from it

## Bootstrap order
Update check → local load → UMP consent → optional cloud restore → ads init → Merge or Race

## Why this shape
One product, two modes, one save graph — avoids divergent currencies and keeps unlocks meaningful in both loops.
