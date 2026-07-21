# Pocket Kingdom Idle — Architecture

## Composition
- `GameManager`: app bootstrap (updates, UMP, local save, agreement, load idle scene)
- `IdleManager`: gameplay composition (currency, stage, crit, ads, heroes, tutorial, language)
- ~14 managers behind singleton access + C# events for UI refresh

## Data
- `GameData` runtime + balance hub
- Domain ScriptableObjects: upgrades, buildings, skills, artifacts, spells, ads, mobs, languages
- Custom `BigInteger` fields serialized through the same save model

## Bootstrap order
Remote update → local save → UMP consent → agreement → `IdleScene` init (cloud restore if first launch) → UI/stage/currency/ads → spawn heroes

## Why this shape
Idle games change balance constantly; systems stay formula-focused while content stays in assets. Numeric type is a first-class architectural dependency, not a utility afterthought.
