# Merge Car Racing — Systems

Gameplay and platform systems owned on this title.

## Hybrid progression loop
- **Merge mode:** drag/drop board, equal-level merges, containers by rarity, timed auto-spawns
- **Idle production:** deploy cars onto spline track; completed loops pay income (capacity-capped)
- **Race mode:** drive unlocked vehicles through traffic for skill payouts and XP
- Unlock manager maps merge depth → bodies, paints, and race eligibility so racing is not cosmetic

## Merge board state
- Slot snapshots for move/swap/merge/discard rules
- Persistent board + deployed-track assignments across sessions
- Container pricing scales with top merge level and purchase count

## Economy & progression
- Custom scientific large-number math for money/income at idle scale
- Upgrades: income, speed, AFK, XP, discounts, timed boosts
- AFK rewards with multi-hour caps and trusted-time checks (Android auto-time / network fallback)
- Player XP/level feeding packed leaderboard scores (level + XP progress + avatar)

## Racing systems
- Player: throttle/steer, multi-gear RPM, layered engine audio, braking, collision recovery, haptics
- Input schemes: joystick, buttons, pedals, tilt, auto-gas preference
- NPC traffic: proximity sensing, lane changes, weighted vehicle mix
- Near-miss rewards, finish payouts, rewarded second-chance recovery

## Content scale
- Hundreds of merge catalogue entries (archetypes × levels)
- Double-digit race vehicle models, unlockable materials, avatars
- Data-driven ad reward actions (boosts, containers, AFK, second chance)

## Persistence & platform
- Init bootstrap: update check → local save → UMP → optional cloud restore → ads → Merge/Race
- JSON autosave (~61s) + debounced event saves
- GPGS auth, achievements, leaderboards, Saved Games (`UseMostRecentlySaved`)
- LevelPlay / AdMob after consent; in-app review; remote version gate

## Architecture notes
- Scene composition: `InitScene` → `MergeScene` / `RaceScene`
- Shared `GameData` ScriptableObject as meta source of truth across modes
- Event-driven money/UI/ad/cloud refresh
