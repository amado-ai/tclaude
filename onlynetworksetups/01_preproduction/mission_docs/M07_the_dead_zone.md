# Mission 07: The Dead Zone

**Mission ID:** M07
**Mission Name:** The Dead Zone
**Version Target:** V0.6.0
**Wave:** 2
**Owner:** Senior Game Developer + Lead Software Architect
**Status:** Approved — pending V0.6.0 implementation
**Folder Destination:** `02_production/levels/mission_07_the_dead_zone/`

---

## Mission Purpose

Teach the player to recognize when a single router is physically incapable of covering a
space and to identify which rooms have zero coverage. This mission ends without a hardware
fix — the player's job is diagnosis and documentation, not resolution. The resolution
(mesh networking) comes in Missions 8 and 9. This mission creates the problem that
mesh solves.

---

## Learning Objectives

- Use the heat map to identify dead zones (zero-coverage areas).
- Understand that distance and building materials cause dead zones.
- Recognize that optimal router placement cannot always solve a coverage problem.
- Understand that the solution to a dead zone is a signal extender or mesh node.

---

## Environment

Larger, multi-room floor plan: living room + kitchen + dining area + two bedrooms + garage
or basement. Router is fixed in an optimal position for the living room area, but the
far bedroom and garage/basement have zero coverage due to distance and wall material
(concrete or brick penalty volumes).

---

## Player Tasks

1. Toggle the heat map on (always-on mode — map cannot be turned off in this mission).
2. Walk through the entire floor plan.
3. Identify all red (zero-coverage) zones on the heat map.
4. Pick up the diagnostics tablet and use the Signal Meter panel to confirm signal strength in each room (reads 0 in dead zones).
5. Log the dead zones on the in-world certification clipboard (a diegetic notepad prop).
6. Mission complete when all dead zones are identified and logged.

---

## Fail States

- Player tries to log a room that has adequate coverage (>20%) → clipboard shows "Signal detected — this is not a dead zone."
- Player completes certification clipboard without identifying all dead zones → incomplete status, hint fires.

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Player stays in the living room for 90 s | "The heat map shows the whole floor plan. Walk through every room and look for the dark zones." |
| Player misses one dead zone and tries to complete | "There are still unchecked areas. The garage and far bedroom may need attention." |
| Player tries to move the router | "The router is mounted — placement is not the lesson here. The problem is distance and wall material." |

---

## Success Condition

All dead zones (rooms with coverage < 10%) identified and logged on the certification clipboard.
`MissionCompleteEvent` fires.

---

## Real-World Takeaway Card

**What you just did:** Identified Wi-Fi dead zones in a home that a single router cannot cover.

**In real life:** Thick walls, long distances, and metal surfaces create dead zones that no amount of router repositioning can fix. The solution is a Wi-Fi extender or, better, a mesh networking system that places additional nodes throughout the home. That's what you'll build in the next two missions.

**Key terms:** Dead zone · Signal attenuation · Coverage map · Mesh network · Wi-Fi extender

---

## Required Assets

- Large multi-room level geometry.
- `CoveragePenaltyVolume` zones for concrete walls and building material barriers.
- Certification clipboard prop (pickupable notepad with room checklist UI).
- Heat map always-on mode configuration.
- Room boundary markers for dead zone evaluation (invisible trigger volumes per room).

---

## Required Systems

- FT-001 Core Interaction Prototype.
- FT-002 Mission Framework.
- FT-003 Diagnostics Tablet (Signal Meter panel — per-room readings).
- FT-005 Signal Heat Map (always-on mode).
- Hint Manager.
- Room-boundary evaluation system (per-room coverage score).

---

## QA Checks

- [ ] Heat map correctly shows zero-coverage rooms with the router in its fixed position.
- [ ] Signal Meter on diagnostics tablet reads 0 in confirmed dead zones.
- [ ] Certification clipboard accepts dead-zone rooms and rejects covered rooms.
- [ ] All dead zones in the level are correctly identified by the system.
- [ ] Mission completes when all dead zones are logged.
- [ ] Hint fires if player tries to complete with unchecked dead zones.
- [ ] Mission completable without developer intervention.

---

## Done Definition

Player walks the full floor plan, uses the heat map and Signal Meter to identify all dead
zones, logs them on the clipboard, and completes the mission. The lesson — one router is
not always enough — is communicated clearly through the visual evidence.
