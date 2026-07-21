# Game Engineering Portfolio & Documentation

Proprietary game source code stays private. This repository publishes **engineering case studies** so readers can evaluate what I build and own.

## How to navigate

Docs are organized **by category, then by game** — not by abstract topic.

| Category | Pixel Car Racing | Merge Car Racing | Pocket Kingdom Idle |
|----------|------------------|-----------------|---------------------|
| **Games** (overview) | [case study](Games/pixel-car-racing.md) | [case study](Games/merge-car-racing.md) | [case study](Games/pocket-kingdom-idle.md) |
| **Architecture** | [doc](Architecture/pixel-car-racing.md) | [doc](Architecture/merge-car-racing.md) | [doc](Architecture/pocket-kingdom-idle.md) |
| **Systems** | [doc](Systems/pixel-car-racing.md) | [doc](Systems/merge-car-racing.md) | [doc](Systems/pocket-kingdom-idle.md) |
| **Optimization** | [doc](Optimization/pixel-car-racing.md) | [doc](Optimization/merge-car-racing.md) | [doc](Optimization/pocket-kingdom-idle.md) |

`Pipelines/` holds high-level release notes (not per-game source).

## Titles at a glance

| Game | Genre | Signature story |
|------|--------|-----------------|
| Pixel Car Racing | 2D physics racer | Torque wheels, RPM/gears, data-driven car roster |
| Merge Car Racing | Merge/idle + highway race | Dual-mode progression, persistent board, streamed traffic |
| Pocket Kingdom Idle | Idle combat | Custom large-number economy, prestige, stage scaling |

## Document format (Games overviews)

1. **The Context** — product/system goal  
2. **The Problem** — constraints and failure modes  
3. **The Approach** — options considered  
4. **The Solution** — architecture strategy (no proprietary source dumps)  
5. **The Result** — shipping / scale / performance outcomes  

## Core technologies

- **Engine:** Unity (incl. Unity 6 / 6000.x) · **Language:** C#
- **Patterns:** Manager composition, ScriptableObjects, events, object pooling
- **Platform:** Google Play Games, UMP, LevelPlay / AdMob, cloud saves, localization

## What this repo is not

- Not a source mirror of the games
- Not keystores, API secrets, or private dashboards

## Connect

Open an issue to discuss game architecture, mobile live-ops, idle economies, or vehicle gameplay systems.
