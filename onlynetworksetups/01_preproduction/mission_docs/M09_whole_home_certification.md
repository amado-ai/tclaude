# Mission 09: Whole-Home Certification

**Mission ID:** M09
**Mission Name:** Whole-Home Certification
**Version Target:** V0.7.0
**Wave:** 3
**Owner:** Lead Software Architect + Senior Game Developer + Creative Director & VFX
**Status:** Approved — pending V0.7.0 implementation (depends on FT-006)
**Folder Destination:** `02_production/levels/mission_09_whole_home_certification/`

---

## Mission Purpose

The capstone mission. The player sets up the complete network — modem, primary router,
and mesh nodes — from scratch in a full home environment, then uses the certification
terminal to audit every room's coverage and confirm the network meets whole-home standards.
This mission synthesizes every skill the game has taught across Missions 1–8.

---

## Learning Objectives

- Execute the full network setup sequence from modem through mesh, independently.
- Understand per-room coverage requirements for a certified whole-home network.
- Use the certification terminal to identify any room that fails the coverage threshold.
- Resolve any coverage gaps through node repositioning or addition before certification passes.
- Demonstrate mastery of the full physical-layer setup workflow.

---

## Environment

The largest environment in the game. Full two-story home floor plan:
- Ground floor: living room, kitchen, dining area, hallway, garage.
- Upper floor: master bedroom, two secondary bedrooms, bathroom hallway.
- Staircase connecting floors.

Scene starts empty (no cables connected, no nodes placed). The player must complete everything.

---

## Player Tasks

1. **Phase 1 — Modem Setup:** Connect coax cable (wall → modem). Connect modem power. Verify modem Online state.
2. **Phase 2 — Router Setup:** Connect Ethernet (modem → router WAN). Connect router power. Verify router WifiBroadcasting state.
3. **Phase 3 — Mesh Node Placement:** Three mesh nodes are provided. Place, power, and pair all three to achieve coverage on both floors.
4. **Phase 4 — Certification Audit:** Approach the certification terminal. Initiate the audit. The terminal runs a per-room coverage check.
5. **Phase 5 — Gap Resolution (if needed):** If any room fails the audit, the terminal highlights the room. Player repositions the relevant node until the room passes.
6. **Phase 6 — Certification Pass:** All rooms meet >= 70% coverage threshold. `CertificationPassedEvent` fires. Final completion screen.

---

## Fail States

- Certification audit initiated before all physical connections are complete → terminal shows "Network not fully initialized. Complete hardware setup first."
- Audit fails one or more rooms → terminal displays failing rooms; player must resolve before re-running audit.
- Node placed so far from backhaul chain that it cannot pair → backhaul warning; repositioning required.

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Player idle at scene start 60 s | "Start with the modem. Connect the coaxial cable from the wall to the modem's Coax-IN port." |
| Audit initiated with incomplete physical setup | "The terminal shows which systems are offline. Complete the hardware setup before running the certification." |
| Audit fails one room | "The [room name] is below the coverage threshold. Try moving the nearest mesh node to a more central position in that area." |
| All nodes placed but coverage still insufficient in one room | "You have a third node available. Adding it between the router and the weak room may fill the gap." |

---

## Success Condition

`CertificationPassedEvent` fires. All rooms >= 70% coverage. Certification terminal displays
"CERTIFIED: Whole-Home Network" with a green badge. Final completion screen and credits roll.

---

## Real-World Takeaway Card

**What you just did:** Designed and deployed a certified whole-home mesh network from scratch.

**In real life:** A properly installed home network follows exactly this sequence: ISP signal → modem → router → wired or wireless distribution. Mesh nodes fill coverage gaps that a single router cannot reach. Real installers use signal meters and coverage maps identical to what you used in this game. You now know how to plan, install, and verify a whole-home network.

**Key terms:** Whole-home mesh · Certification audit · Coverage threshold · Modem → Router → Mesh chain · Physical layer mastery

---

## Required Assets

- Two-story home level geometry (largest level in the game).
- Three mesh node props + boxes (unpacking at scene start).
- Certification terminal 3D prop (wall-mounted console with world-space canvas screen).
- Certification UI: per-room coverage percentage grid with pass/fail indicators.
- Green certification badge VFX on terminal screen on pass.
- End-screen / credits sequence.

---

## Required Systems

- FT-001 Core Interaction Prototype (complete).
- FT-002 Mission Framework.
- FT-003 Diagnostics Tablet (all panels).
- FT-004 Router Placement System.
- FT-005 Signal Heat Map (multi-node, always-on during Phase 3+).
- FT-006 Mesh Node System (including certification terminal).
- Hint Manager.

---

## QA Checks

- [ ] Scene starts fully empty; all connections must be made by the player.
- [ ] All Mission 1–8 skills are required in sequence with no shortcuts.
- [ ] Certification terminal correctly evaluates per-room coverage.
- [ ] Terminal shows failing rooms clearly with room names and percentage scores.
- [ ] Re-running audit after repositioning reflects updated coverage.
- [ ] `CertificationPassedEvent` fires exactly once.
- [ ] End screen and takeaway card display correctly.
- [ ] Mission completable without developer intervention.
- [ ] 60 FPS with full mesh (router + 3 nodes) and heat map active.
- [ ] Accessibility options functional throughout the mission.

---

## Done Definition

Player completes the full setup sequence independently, achieves whole-home certification
on the terminal, and reaches the end screen. No developer intervention required. All
systems from FT-001 through FT-006 are exercised. The player has demonstrated mastery of
the complete game skill arc.
