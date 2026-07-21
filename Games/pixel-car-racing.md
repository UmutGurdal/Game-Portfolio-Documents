# Pixel Car Racing — Engineering Case Study

> [!NOTE]
> Portfolio documentation only. Proprietary source code is not included. Class and system names are referenced to describe architecture ownership.

## Snapshot

| Field | Detail |
|--------|--------|
| **Genre** | Physics-based 2D side-scrolling racing |
| **Role** | Lead Developer / Software Engineer |
| **Engine** | Unity 6000.3 · C# |
| **Custom code** | ~150 scripts under gameplay systems |
| **Highlights** | Torque-driven vehicle physics, RPM/gear simulation, data-driven car roster, pooling, local + Google Play cloud save |

**Tech stack:** Unity 2D Physics, ScriptableObjects, JSON persistence, Google Play Games Services, Google UMP, rewarded ads, DOTween, localization

---

## 1. The Context

Ship a tactile mobile racer where cars feel mechanical — gears, RPM, torque, airborne drag — while remaining scalable for a large vehicle roster, stable on mid-tier Android devices, and safe for player progress across reinstalls and device changes.

## 2. The Problem

- **Physics authenticity vs mobile cost:** Translate/velocity hacks feel fake; full vehicle sim must stay cheap at 60 FPS.
- **Content scale:** Dozens of cars with upgrades cannot each hard-code physics and UI logic.
- **Runtime churn:** Endless road props, parallax, and combo popups allocate heavily if spawned with `Instantiate`/`Destroy`.
- **Progress reliability:** Tuning ownership, currency, and stats must survive schema changes and device switches.
- **Feel:** Engine audio and dashboard must track real RPM/gear state, not decorative loops.

## 3. The Approach

Evaluated alternatives:

| Option | Why rejected / deferred |
|--------|-------------------------|
| Pure transform-based movement | Cheap, but no terrain interaction or wheel feel |
| One physics preset for all cars | Fast to ship, kills roster identity and tuning depth |
| `System.Numerics` / heavy middleware for saves | Unnecessary complexity for this economy scale |
| Scene-per-car content | Inflates build size and maintenance |

Chosen direction: **torque-based `Rigidbody2D` wheels + ScriptableObject `CarData` + pooled world/UI + JSON/GPGS persistence**, with UI reading calculated telemetry rather than owning physics.

## 4. The Solution

### Architecture
- Generic `MonoSingleton` services: `GameManager`, `PlayManager`, `AudioManager`, `SaveManager`, money/region/effect managers.
- Clear split: physics simulation produces values; `CarDashboard` / battery UI consume them.
- Showroom, tuning, test-ride, race, and end-game flows composed as dedicated managers/UI modules.

### Data-driven vehicles
- `CarData` ScriptableObjects author max speed, acceleration, gear count, RPM limits, upgrade multipliers, and visuals.
- Calculated getters (e.g. tuned max speed / acceleration) keep balance in data, not scattered magic numbers.
- Enabled rapid addition of **20+** distinct vehicles without rewriting core driving code.

### Vehicle mechanics
- Continuous wheel torque via `AddTorque` for natural grip, slip, and terrain response.
- Custom gearbox: normalized RPM between min/max engine limits, dynamic gear ratios, shift thresholds from speed/RPM, torque curves for acceleration feel.
- Raycast ground checks adjust drag airborne vs grounded for jump/bridge readability.

### Performance
- Object pooling (`Spawner` + generic get/release) for clouds, stars, parallax pieces, and flip/combo popups.
- Physics in `FixedUpdate`; visual interpolation in `Update`.

### Persistence & platform
- `SaveManager` serializes `SavedGameData` / car progress with JSON; legacy fallback migration for older saves.
- Google Play Saved Games cloud sync; UMP consent; rewarded ads; remote update checks; multi-language support; lifetime statistics tracking.

### Audio polish
- RPM-normalized crossfade across Idle / Low / Med / High engine layers.
- Pitch/volume modulated by load and gear speed ratios; DOTween for UI/camera motion.

## 5. The Result

- Stable **~60 FPS** racing feel on mid-to-high mobile hardware through pooling and update-loop discipline.
- Vehicle roster scaled past **20 cars** via ScriptableObjects without architectural rewrites.
- Driving identity comes from coupled RPM, gears, torque, and layered engine audio — not cosmetic speedometers.
- Local + cloud saves reduced progress-loss risk across devices; legacy migration protected existing players.

## Skills Demonstrated

- 2D physics gameplay systems
- Data-oriented content architecture
- Mobile performance (pooling, FixedUpdate/Update split)
- Save schema design + cloud sync
- Game feel (audio tied to simulation state)
- Store-ready platform integration (consent, ads, updates, localization)

## Related Docs

- [Architecture](../Architecture/pixel-car-racing.md)
- [Systems](../Systems/pixel-car-racing.md)
- [Optimization](../Optimization/pixel-car-racing.md)
