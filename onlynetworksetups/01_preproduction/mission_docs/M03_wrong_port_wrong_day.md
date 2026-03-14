# Mission 03: Wrong Port, Wrong Day

**Mission ID:** M03
**Mission Name:** Wrong Port, Wrong Day
**Version Target:** V0.5.0
**Wave:** 1
**Owner:** Senior Game Developer
**Status:** Approved — pending V0.5.0 implementation
**Folder Destination:** `02_production/levels/mission_03_wrong_port_wrong_day/`

---

## Mission Purpose

Test and reinforce port identification under realistic pressure. The player is presented
with a modem and router that have already been incorrectly cabled by a fictional previous
technician. The player must identify what is wrong, disconnect the incorrect cables, and
reconnect them properly. This mission teaches diagnosis, not just initial setup.

---

## Learning Objectives

- Identify an incorrectly connected cable by reading LED state and diagnostics tablet.
- Distinguish WAN from LAN ports on a router under time-neutral conditions.
- Practice disconnecting a cable and reconnecting it to the correct port.
- Understand how incorrect port connections produce predictable failure states.

---

## Environment

Same room footprint as Mission 2. Scene loads with cables already connected — incorrectly.
Pre-wired wrong connections (randomized from a pool each session):
- Ethernet connected to router LAN port instead of WAN.
- OR: Coax connected to modem Ethernet-OUT instead of Coax-IN.
- OR: Both power adapters swapped (modem adapter into router, router adapter into modem — different barrel sizes give error feedback).

---

## Player Tasks

1. Read device LEDs and/or diagnostics tablet to identify the fault.
2. Disconnect the incorrect cable.
3. Reconnect it to the correct port.
4. Confirm all devices reach their correct Online/WifiBroadcasting states.

---

## Fail States

- Disconnecting a correct cable → no latching issue; player must reconnect it. Hint fires if correct cable stays disconnected for 30 s.
- Reconnecting the wrong cable to the same wrong port → error feedback (cable does not latch in this version of the wrong port — uses port type guard).

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Player idle 30 s at scene start | "Check the modem and router LEDs. Something is not right. The diagnostics tablet can help identify the problem." |
| Player disconnects correct cable and leaves it disconnected 30 s | "That cable was correct. Reconnect it and keep looking for the actual problem." |
| Player has found the fault but reconnects to wrong port again | "Remember: the WAN port connects the router to the modem. The LAN ports connect devices to the router." |

---

## Success Condition

All devices reach correct terminal states (modem Online, router WifiBroadcasting) after the
fault is identified and corrected.

---

## Real-World Takeaway Card

**What you just did:** Diagnosed and corrected an incorrect cable connection.

**In real life:** Most home networking problems are physical: wrong port, loose connection, or swapped cable. Before calling your ISP, check that every cable is in the right port and fully seated. The modem's and router's LEDs tell you exactly where the chain is broken.

**Key terms:** Fault diagnosis · Port identification · WAN vs LAN · LED status reading

---

## Required Assets

- Pre-wired wrong-connection scene state (scene loads with cables already snapped into wrong ports).
- Fault pool configuration (ScriptableObject or scene variant per fault type).
- All Mission 2 assets (same environment).

---

## Required Systems

- FT-001 Core Interaction Prototype (cable disconnect action must be implemented).
- FT-002 Mission Framework.
- FT-003 Diagnostics Tablet (first mission where tablet is primary diagnostic tool).
- Hint Manager.
- Fault injection system (basic — scene-loaded wrong states, not runtime randomization yet).

---

## QA Checks

- [ ] Scene loads with cables visibly in wrong ports.
- [ ] Diagnostics tablet correctly reflects the fault state.
- [ ] Player can disconnect and reconnect cables correctly.
- [ ] All fault pool variants load and are completable.
- [ ] Hints fire at correct trigger conditions.
- [ ] Mission completable without developer intervention.

---

## Done Definition

Player can load the scene, identify the fault using LEDs and/or tablet, correct it, and
bring the network online. All fault pool variants are playable. Tablet provides accurate
diagnostic information. Mission completable without hints.
