# FT-005: Signal Heat Map Visualization

**Feature ID:** FT-005
**Feature Name:** Signal Heat Map Visualization
**Status:** approved
**Version Target:** V0.5.0
**Owner:** Creative Director & VFX + Senior Game Developer
**Date Created:** 2026-03-14
**Last Updated:** 2026-03-14

---

## Purpose

Make Wi-Fi signal strength visible. The heat map overlays a color-coded coverage grid on
the floor of the environment so the player can immediately see which areas have strong
signal (green), weak signal (amber), and no signal (red/dark). This is the primary visual
feedback for FT-004 Router Placement and Mission 5, and returns in later missions for
dead-zone diagnosis (Mission 7) and mesh verification (Mission 9).

---

## Core Behavior

- `SignalHeatMap` reads coverage score data from `SignalCoverageCalculator` (FT-004).
- Renders as a grid overlay on a flat mesh plane matched to the room floor.
- Each grid cell is colored by signal strength: 80–100 = green, 50–79 = yellow-green, 20–49 = amber, 0–19 = red.
- Heat map is toggled by a player action (e.g. pressing the tablet's Signal Meter panel, or a dedicated quick-toggle button).
- In Mission 7 (The Dead Zone): heat map is always-on; player must identify the red zones and diagnose the cause.
- In Mission 9 (Whole-Home Certification): heat map shows per-node coverage with color blending between mesh node ranges.
- Grid resolution: 0.5 m cells. Maximum map size: 15 m × 15 m (30 × 30 grid = 900 cells, trivial GPU cost).
- Heat map mesh is rendered with a custom unlit shader using vertex colors. Zero texture fetches.

---

## Dependencies

- FT-004 Router Placement System (`SignalCoverageCalculator` provides the data).
- FT-003 Diagnostics Tablet (Signal Meter panel triggers heat map toggle).
- FT-001 EventBus (`RouterPlacedEvent`, `DeviceStateChangedEvent` trigger recalculation).
- Mission 7 level geometry (always-on mode).
- Mission 9 mesh node positions (multi-node blending).

---

## Assets Needed

- Heat map shader (unlit, vertex color, alpha blend, no Z-write so it layers over floor).
- Heat map floor mesh (generated at runtime from room bounds, not a fixed asset).
- Toggle button icon for the diagnostics tablet UI.

---

## UI/UX Needs

- Heat map fades in over 0.8 s when toggled on; fades out over 0.5 s when toggled off.
- Cell colors lerp smoothly when signal score changes (not instant jump).
- A legend strip in the corner of the screen while the heat map is active: green=strong / amber=weak / red=none.
- Heat map does not render through walls (cells behind solid geometry are hidden via stencil mask).

---

## Test Cases

- [ ] Heat map correctly shows high coverage near the router and low coverage at room edges.
- [ ] Moving the router updates the heat map within 0.2 s of placement.
- [ ] `CoveragePenaltyVolume` zones show as red/amber even when near the router.
- [ ] Toggle on/off works from both the tablet panel and the quick-toggle.
- [ ] Heat map does not visually appear through walls.
- [ ] Multi-node blending in Mission 9 correctly shows coverage overlap zones as brighter green.
- [ ] 60 FPS maintained with heat map active on target mobile device.

---

## Done Definition

Player can toggle the signal heat map on and off. The map accurately reflects router
placement quality. Color zones update within 200 ms of any coverage-affecting event.
Legend is always visible when map is active. System works in Missions 5, 7, and 9 without
scene-specific code changes.

---

## Final Folder Destination

| Artifact | Location |
|---|---|
| `SignalHeatMap.cs` | `02_production/game_build/networking_logic/` |
| `HeatMapRenderer.cs` | `02_production/game_build/networking_logic/` |
| Heat map shader | `02_production/assets/materials/` |
| Heat map toggle UI | `02_production/game_build/ui/` |
| Tool reference prefab | `02_production/tools/signal_visualizer/` |

---

## Changelog Version

V0.5.0

---

## Open Risks

| Risk | Mitigation |
|---|---|
| Heat map mesh bleeds visually through thin walls | Use stencil masking; test at all room geometry configurations |
| Color accessibility (red/green colorblind) | Add a colorblind-safe palette option (blue/yellow) in accessibility settings |
| Multi-node blending math in Mission 9 is non-trivial | Use additive coverage accumulation per cell, capped at 100 |

---

## Decision History

| Date | Decision | Reason |
|---|---|---|
| 2026-03-14 | Runtime-generated mesh, not a pre-baked texture | Room sizes and router positions are dynamic; baked textures would not update |
| 2026-03-14 | Vertex color shader, not a texture fetch | Minimal GPU cost; works at any resolution without aliasing |
| 2026-03-14 | Colorblind palette as accessibility setting | Identified as a production risk early; cheaper to plan for now than retrofit |
