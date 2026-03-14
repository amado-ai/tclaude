# FT-004: Router Placement System

**Feature ID:** FT-004
**Feature Name:** Router Placement System
**Status:** approved
**Version Target:** V0.5.0
**Owner:** Senior Game Developer + Lead Software Architect
**Date Created:** 2026-03-14
**Last Updated:** 2026-03-14

---

## Purpose

Teach the player that router physical placement affects Wi-Fi coverage. The player must pick
up, carry, and place the router in the environment to maximize signal coverage. Wrong placement
(inside a cabinet, against an exterior wall, in a corner) produces visible degraded signal in
the signal heat map. Correct placement achieves the mission coverage threshold.

---

## Core Behavior

- Player picks up the router using the existing `ObjectHolder` system.
- A valid placement zone system: designated surfaces (shelves, tables, TV-stand tops) accept the router when the player releases it nearby. Invalid surfaces reject placement with error feedback.
- On placement, `RouterPlacedEvent` fires with the placement position.
- `SignalCoverageCalculator` computes a per-room coverage score (0–100) based on router position relative to room geometry using a lightweight grid raycast (not physics-expensive).
- Coverage score drives both the `SignalHeatMap` visualization (FT-005) and the mission objective state.
- Mission objective: "Achieve 80% room coverage" is met when `SignalCoverageCalculator.Score >= 80`.
- Bad placement zones (e.g. inside the TV cabinet): `CoveragePenaltyVolume` components reduce the score multiplicatively.

---

## Dependencies

- FT-001 Core Interaction Prototype (`ObjectHolder`, `IInteractable`, `EventBus`).
- FT-002 Mission Framework (objective evaluation hooks).
- FT-005 Signal Heat Map (visualization layer — developed in parallel).
- Mission 5 level geometry (`02_production/levels/mission_05_signal_wars/`).

---

## Assets Needed

- Placement zone indicator: a subtle floor/surface highlight that appears when carrying the router.
- Placement confirm VFX: small pulse when router is set down on a valid surface.
- `CoveragePenaltyVolume` gizmo for level design use (editor-only visualization).

---

## UI/UX Needs

- While carrying the router: a small signal preview indicator (ghost heat map overlay, low opacity) shows predicted coverage from the current held position.
- On placement: heat map fades in to full opacity over 1 s.
- If coverage is below threshold: amber indicator with "Try a more central location" hint text (hint manager integration).
- If coverage meets threshold: green indicator with objective-complete chime.

---

## Test Cases

- [ ] Router can be picked up, carried, and placed on any valid surface.
- [ ] Placement on an invalid surface shows error feedback and does not latch.
- [ ] `SignalCoverageCalculator` returns a higher score for a central open placement than a corner placement.
- [ ] `CoveragePenaltyVolume` reduces score when router is placed inside it.
- [ ] Coverage objective completes when score >= threshold (configurable per mission).
- [ ] Ghost heat map preview updates at least 10 times per second while carrying.
- [ ] 60 FPS maintained during coverage calculation on target mobile device.

---

## Done Definition

Player can pick up and place a router in a multi-room (or large single-room) environment.
Signal coverage score updates live. Correct placement in an open central area achieves the
mission threshold. Poor placement in a penalty zone visibly fails to meet it. Mission
objective completes on threshold breach.

---

## Final Folder Destination

| Artifact | Location |
|---|---|
| `RouterPlacementController.cs` | `02_production/game_build/networking_logic/` |
| `SignalCoverageCalculator.cs` | `02_production/game_build/networking_logic/` |
| `CoveragePenaltyVolume.cs` | `02_production/game_build/networking_logic/` |
| `RouterPlacedEvent.cs` | `02_production/game_build/mission_system/` |
| Placement zone assets | `02_production/levels/mission_05_signal_wars/` |

---

## Changelog Version

V0.5.0

---

## Open Risks

| Risk | Mitigation |
|---|---|
| Grid raycast too expensive on mobile | Limit grid resolution to 0.5 m cells; run calculation on placement event, not per-frame |
| Preview heat map causes frame drops while carrying | Run preview at 10 Hz on a background thread; blit result to texture |
| Valid surface detection feels finicky | Use generous snap radius (0.4 m) and a clear visual indicator before release |

---

## Decision History

| Date | Decision | Reason |
|---|---|---|
| 2026-03-14 | Grid-based coverage, not full physics raycasting | Fast enough for mobile; accurate enough for educational feedback |
| 2026-03-14 | `CoveragePenaltyVolume` as a scene volume, not baked data | Level designers can place and tune penalty zones without code |
| 2026-03-14 | Ghost preview while carrying | Players need to see the consequence before committing; reduces frustration |
