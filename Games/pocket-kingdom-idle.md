# Pocket Kingdom Idle — Engineering Case Study

> [!NOTE]
> Portfolio documentation only. Proprietary source code is not included. Class and system names are referenced to describe architecture ownership. The deepest engineering investment on this title was the **custom large-number (`BigInteger`) economy layer**.

## Snapshot

| Field | Detail |
|--------|--------|
| **Genre** | Idle / incremental combat |
| **Role** | Lead Developer / Software Engineer |
| **Engine** | Unity 6000.3 · C# |
| **Custom code** | ~150 scripts · ~14 managers |
| **Highlights** | Custom scientific large numbers, 1–1,800 stage curve, prestige meta, pooled combat, offline time integrity, GPGS + LevelPlay |

**Tech stack:** ScriptableObjects, custom `Muchwood.BigInteger`, object pooling, Google Play Games, Google UMP, Unity LevelPlay / AdMob mediation, DOTween, 8-language localization

---

## 1. The Context

Ship a long-run idle combat game where gold, damage, health, upgrade costs, income, offline earnings, and prestige tickets all grow for hundreds of stages. Players expect readable numbers, fair AFK rewards, smooth combat, and cloud-safe progress on Google Play.

## 2. The Problem

**Primary (highest effort): numeric scale**

- Standard `float` / `double` overflow or lose meaning deep into progression (designed stage range **1–1,800**).
- `System.Numerics.BigInteger` is exact but allocation-heavy and awkward for Unity serialization, UI formatting, and per-hit combat math on mobile.
- Every subsystem — combat hits, income ticks, upgrade affordability, offline payout, prestige tickets — must speak one numeric language.

**Secondary production problems**

- Continuous mob/projectile/VFX churn causes GC spikes without pooling.
- Offline rewards without trusted time become clock-cheat exploits.
- Prestige must reset runs without feeling punitive.
- Ads require UMP consent before init; cloud restore must not wipe stronger progress.

## 3. The Approach

| Option | Decision |
|--------|----------|
| Cap numbers to stay in `double` | Rejected — fights the idle fantasy |
| Use `System.Numerics.BigInteger` everywhere | Rejected for hot-path cost + poor Unity JSON story |
| String-based “big number” hacks | Rejected — fragile math |
| Custom mantissa × 10^exponent type | **Chosen** |

Chosen direction: a serializable scientific `BigInteger` (`Value`, `Power`) with normalized arithmetic, idle-tuned add/sub early-outs, K/M/B/T → scientific formatting, and scratch reuse on the main thread — then wire it through the entire economy spine.

## 4. The Solution

### Custom large-number system (flagship work)
- Serializable class: mantissa `Value` + base-10 `Power`.
- Ops: multiply, divide, add, subtract, compare, floor, assign.
- Normalization keeps mantissa in a usable range and rounds for stable compares/UI.
- Add/subtract ignore negligible terms when exponent gap is large (typical late-game dominance) to save work without visible error.
- Display: plain → K/M/B/T → scientific (`e` notation) by magnitude.
- **Max affordable / bulk buy** uses exponent-aware approximation so “Buy Max” works when money and costs differ by many orders of magnitude.
- Persisted as `{ Value, Power }` in local JSON and Google Play Saved Games.

### Where it plugged in
- Combat health/damage and gold rewards
- Currency earn/spend and building income aggregation
- Offline earnings (`income × time × multipliers` with caps)
- Upgrade quadratic costs, bonus stacks, artifact/ticket prices
- Prestige ticket and skill-point facing values
- Save/load of money, levels, and related progression fields

### Broader architecture
- `GameManager` bootstrap: remote update → local save → UMP consent → agreement → idle scene.
- `IdleManager` gameplay composition: currency, stage, crit, ads, heroes, tutorial, language.
- ScriptableObject balance: upgrades, buildings, skills, artifacts, spells, ads, mob database, language packs.
- Heroes: tap/swipe player + autonomous Archer/Mage; five spells; crit manager.
- Prestige at stage 50+: collections → tickets; stage-above-max → skill points; soft-start after reset.
- Pooling for mobs, projectiles, health bars, popups, spell VFX; concurrent mob budget; 60 FPS target.
- Offline: Android auto-time or HTTP date; capped away time; stage simulation from expected HP vs damage; gated UI payout.
- Platform: LevelPlay rewarded dragons, GPGS achievements/leaderboard/cloud, in-app review, 8 languages.

## 5. The Result

- Full stage curve remains playable without float overflow; UI stays human-readable at extreme magnitudes.
- One numeric model shared by combat, economy, meta, offline, and saves — reducing “special case” currency bugs.
- Hot-path math stays mobile-viable via normalization rules, dominance early-outs, and reusable scratch values.
- Offline + prestige loops support idle retention without open-ended clock abuse or dead post-prestige starts.
- Store-ready pipeline: consent-aware ads, cloud continuity, social features, localization.

## Skills Demonstrated

- Designing production numeric types for games (not just using a library)
- Idle economy architecture end-to-end
- Serialization-aware API design for Unity
- Performance-conscious math on combat ticks
- Meta progression (prestige / skills / artifacts)
- Mobile live-ops and compliance integrations

## Interview Angle (recommended lead story)

> “On Pocket Kingdom Idle the hardest problem wasn’t a feature — it was representing an economy that grows for 1,800 stages. I built a custom scientific large-number type, tuned arithmetic for idle dominance cases, solved max-buy across exponents, and pushed that type through combat, upgrades, offline rewards, prestige, and cloud saves.”

## Related Docs

- [Architecture](../Architecture/pocket-kingdom-idle.md)
- [Systems](../Systems/pocket-kingdom-idle.md) ← includes BigInteger deep dive
- [Optimization](../Optimization/pocket-kingdom-idle.md)
