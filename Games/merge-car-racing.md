# Merge Car Racing — Engineering Case Study

> [!NOTE]
> Portfolio documentation only. Proprietary source code is not included. Class and system names are referenced to describe architecture ownership.

## Snapshot

| Field | Detail |
|--------|--------|
| **Genre** | Hybrid idle merge + arcade highway racing |
| **Role** | Lead Developer / Software Engineer |
| **Engine** | Unity 6 · C# |
| **Custom code** | ~120 scripts |
| **Package ID** | `com.MuchwoodStudios.MergeCars` |
| **Highlights** | Merge board persistence, spline idle production, endless traffic racing, large-number economy, GPGS, consent-gated ads |

**Tech stack:** ScriptableObjects, Dreamteck Splines, custom large-number math, object pooling, Google Play Games (cloud / leaderboards / achievements), Google UMP, Unity LevelPlay + AdMob, in-app review, DOTween, Cinemachine

---

## 1. The Context

Build a dual-mode mobile product: a merge/idle loop that generates cars and income, plus a skill-based race mode where unlocked vehicles matter. Progression must feel continuous between modes — merges unlock race bodies/stats; racing feeds economy and XP.

## 2. The Problem

- **Two genres, one save:** Merge board slots, deployed track cars, upgrades, cosmetics, and race unlocks must stay consistent across scenes.
- **Idle scale:** Income and costs outgrow plain floats; offline rewards invite clock exploits.
- **Endless race cost:** Streaming roads and NPC traffic will GC-spike without budgets.
- **Meaningful bridge:** Race cars cannot be cosmetic skins; merge depth must change race stats and selection.
- **Store compliance:** Consent before ads, cloud conflict handling, leaderboards, and update gating on Android.

## 3. The Approach

| Option | Decision |
|--------|----------|
| Separate apps for merge vs race | Rejected — one meta progression is the product |
| Pure 2D merge only | Rejected — race mode is retention differentiator |
| Unbounded road instantiation | Rejected — use fixed rolling road window + pools |
| Float-only currency | Rejected — custom scientific large numbers |

Chosen direction: **Init → Merge / Race scene composition**, shared `GameData`, merge table as source of truth for unlocks, race mode as skill expression of that unlock graph.

## 4. The Solution

### Architecture
- `GameManager` bootstrap: consent (UMP), local save, optional cloud restore, ads init, remote update check, then Merge or Race.
- Singletons / façades for merge, race, UI, currency, ads, GPGS, tutorial, loading.
- ScriptableObjects: `GameData`, merge car catalogue, race car data, ad reward actions, bonuses.
- Event-driven money/UI/ad/cloud refresh to keep systems decoupled.

### Merge & idle production
- Drag/drop merge board (`MergeTablePage`): equal-level merge, swap/move rules, discard constraints, slot snapshots.
- Containers with rarity bands relative to top merge level; timed auto-spawn into empty slots.
- Deploy up to a fixed track capacity of cars onto spline followers; completed loops convert to income.
- Unlock manager maps merge progression → bodies, materials, and race eligibility.

### Economy & progression
- Custom mantissa/exponent large-number type for money and income composition.
- Per-car compounding income, upgrades (income, speed, AFK, XP, discounts, timed boosts).
- AFK rewards capped (configured up to multi-hour windows) with Android auto-time / network time validation.
- Player XP/level thresholds feeding leaderboard score packing (level + XP progress + avatar).

### Racing systems
- Player vehicle: throttle/steer, multi-gear RPM sim, layered engine audio, braking, collision recovery, haptics, dashboard.
- Configurable input: joystick, buttons, pedals, tilt, auto-gas preference.
- `RoadGenerator`: fixed rolling segment window (dozens of recycled pieces) instead of infinite instantiate.
- NPC traffic with proximity sensing, lane change logic, weighted vehicle mix; near-miss rewards; second-chance ad recovery.

### Content scale (authored)
- Hundreds of merge catalogue entries (archetypes × levels).
- Double-digit race vehicle models, unlockable paints, avatars.
- Data-driven rewarded ad actions (boosts, containers, AFK, second chance).

### Persistence & platform
- JSON local save (autosave ~61s, debounced event saves).
- GPGS cloud with most-recent-save conflict policy; achievements; player-centred + top leaderboard views.
- LevelPlay / AdMob after UMP consent; in-app review; remote version check.

## 5. The Result

- A coherent hybrid loop: merge depth → idle income + race unlocks → race rewards → stronger merge economy.
- Endless highway racing stays allocation-stable via pooling and a fixed road window.
- Large-number + validated AFK math keeps idle progression readable and harder to clock-cheat.
- Cloud/local parity preserves board state and cosmetics across devices.
- Consent-aware monetization and Play social features support a live Android release path.

## Skills Demonstrated

- Hybrid genre systems design (idle + action)
- Persistent interactive board state
- Spline-based production loops
- Traffic AI / endless runner streaming
- Large-number idle economies
- Full Google Play live-ops integration

## Related Docs

- [Architecture](../Architecture/merge-car-racing.md)
- [Systems](../Systems/merge-car-racing.md)
- [Optimization](../Optimization/merge-car-racing.md)
