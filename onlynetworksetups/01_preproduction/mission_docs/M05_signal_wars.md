# Mission 05: Signal Wars

**Mission ID:** M05
**Mission Name:** Signal Wars
**Version Target:** V0.5.0
**Wave:** 1
**Owner:** Senior Game Developer + Lead Software Architect
**Status:** Approved — pending V0.5.0 implementation (depends on FT-004 + FT-005)
**Folder Destination:** `02_production/levels/mission_05_signal_wars/`

---

## Mission Purpose

Teach the player that router physical placement directly affects Wi-Fi coverage. The player
must pick up and carry the router to different positions in a multi-area environment, read
the signal heat map, and find a placement that achieves coverage threshold in all rooms.
Correct placement: open, central, elevated. Wrong placement: inside furniture, against
exterior walls, in corners.

---

## Learning Objectives

- Understand that physical placement of a router affects Wi-Fi coverage.
- Identify placement factors that degrade coverage (walls, cabinets, corners, distance).
- Use the signal heat map as a diagnostic tool to evaluate placement quality.
- Achieve a target coverage threshold through iterative placement.

---

## Environment

Multi-area floor plan: living room + kitchen + hallway + one bedroom. Router starts on the
floor in a corner of the living room (worst-case placement). Contains:
- Wall coax outlet (fixed, living room).
- Cable modem (fixed, near coax outlet).
- Wi-Fi router (pickupable — this mission's primary interactive object).
- Multiple valid placement surfaces across the floor plan.
- `CoveragePenaltyVolume` zones: TV cabinet interior, corner alcove, exterior wall shelf.
- Power strip (fixed, living room — player must carry router within power adapter cable range or use a longer cable prop).

---

## Player Tasks

1. Complete physical connections (coax, modem-router Ethernet, power).
2. Pick up the diagnostics tablet and toggle the signal heat map on.
3. Observe the heat map — router in corner shows heavy red zones in kitchen and bedroom.
4. Pick up the router and carry it to a more central location.
5. Place the router on a valid surface.
6. Observe heat map update.
7. Iterate until all rooms reach >= 80% coverage (mission threshold).

---

## Fail States

- Router placed inside the TV cabinet → coverage drops, amber warning hint fires.
- Router placed on exterior wall shelf → degraded coverage for rooms on the opposite side.
- Power adapter cable length exceeded (if applicable) → placement rejected with "cable too short" error.

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Router still in starting corner after 60 s | "Toggle the signal heat map on the diagnostics tablet. The red zones show where coverage is weakest." |
| Player places in a penalty zone | "Placing the router inside furniture or against an exterior wall blocks the signal. Try a more open, central location." |
| Coverage above 60% but below threshold after 2 placements | "The signal reaches farther when the router is higher up and away from thick walls." |

---

## Success Condition

`SignalCoverageCalculator.Score >= 80` for all required rooms.
`MissionCompleteEvent` fires.

---

## Real-World Takeaway Card

**What you just did:** Optimized router placement for whole-home Wi-Fi coverage.

**In real life:** The most common cause of weak Wi-Fi is a poorly placed router. Concrete walls, appliances, and distance all degrade the signal. Put your router in a central, open, elevated location — not inside a cabinet or behind the TV. If your home is large, one router may not be enough (see Mission 7).

**Key terms:** Wi-Fi coverage · Signal attenuation · Router placement · 2.4 GHz range · Dead zone

---

## Required Assets

- Multi-area level geometry (living room + kitchen + hallway + bedroom).
- Multiple valid placement surface zones (shelves, tables, TV-stand top).
- `CoveragePenaltyVolume` zones placed in level.
- Ghost heat map overlay (preview while carrying).
- All FT-004 and FT-005 assets.

---

## Required Systems

- FT-001 Core Interaction Prototype.
- FT-002 Mission Framework.
- FT-003 Diagnostics Tablet.
- FT-004 Router Placement System.
- FT-005 Signal Heat Map.
- Hint Manager.

---

## QA Checks

- [ ] Router is pickupable and can be carried to all valid surfaces.
- [ ] Heat map updates within 200 ms of router placement.
- [ ] Penalty zones correctly reduce coverage score.
- [ ] Ghost preview heat map visible while carrying.
- [ ] Coverage objective completes at >= 80% across all required rooms.
- [ ] Hints fire at correct trigger conditions.
- [ ] 60 FPS maintained during heat map update on target device.
- [ ] Mission completable without developer intervention.

---

## Done Definition

Player can pick up, carry, and place the router iteratively until the coverage threshold is
met. Heat map accurately reflects placement quality. All penalty zones correctly degrade
coverage. Mission completable without hints.
