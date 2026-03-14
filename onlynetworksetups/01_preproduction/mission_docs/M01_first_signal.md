# Mission 01: First Signal

**Mission ID:** M01
**Mission Name:** First Signal
**Version Target:** V0.5.0 (playable) — V0.3.0 prototype room basis
**Wave:** 1
**Owner:** Senior Game Developer
**Status:** In progress (prototype room from V0.2.0)
**Folder Destination:** `02_production/levels/mission_01_first_signal/`

---

## Mission Purpose

Teach the player the very first step of home network setup: getting the modem online via
a coaxial cable connection from the wall. No router. No Wi-Fi. Just a modem, a coax cable,
a power cable, and one clear goal — get the modem's Online LED lit.

---

## Learning Objectives

- Identify a coaxial cable and its F-connector plug.
- Identify the Coax-IN port on a modem.
- Understand that the modem needs both the coax signal and power before it can sync.
- Recognize modem LED states: Power, Downstream/Upstream sync, Online.

---

## Environment

Single room. A small living room or utility closet analog. Contains:
- Wall coax outlet (fixed).
- Cable modem on a shelf (fixed position, must be connected — no placement required).
- Coax cable (pickupable).
- Power strip (fixed).
- Modem power adapter (pickupable).

---

## Player Tasks

1. Pick up the coax cable.
2. Connect one end to the wall coax outlet.
3. Connect the other end to the modem Coax-IN port.
4. Pick up the modem power adapter.
5. Connect it to the power strip and the modem Power-IN port.
6. Observe modem boot sequence: Power → DS/US Sync → Online.

---

## Fail States

- Attempting to connect the coax cable to the wrong port → error feedback, no latch.
- Powering the modem before coax is connected → modem enters Booting but stays in DS/US Sync indefinitely (hint triggers after 15 s of no progress).

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Player has not picked up anything after 30 s | "The coaxial cable connects the wall outlet to the modem. Look for the round silver connector." |
| Modem is powered but coax is not connected, 15 s elapsed | "The modem needs the coaxial cable connected to receive the internet signal." |
| Player attempts wrong port 2x | "The Coax-IN port is the round threaded socket on the back of the modem." |

---

## Success Condition

Modem reaches `Online` state (all four LEDs solid green). `MissionCompleteEvent` fires.

---

## Real-World Takeaway Card

**What you just did:** Connected a cable modem to a live coaxial cable line.

**In real life:** The coaxial cable carries the internet signal from your ISP into your home. The modem decodes that signal. Without a solid coax connection, the modem cannot sync — no matter how many times you power-cycle it.

**Key terms:** Coaxial cable · F-connector · Downstream/Upstream sync · Modem Online LED

---

## Required Assets

- Wall coax outlet (fixed mesh, non-interactable).
- Cable modem mesh + correct port colliders.
- Coax cable mesh (flexible, two CableEnd components).
- Power strip mesh (fixed).
- Modem power adapter mesh + barrel plug CableEnd.
- Room environment (basic living room / utility space).

---

## Required Systems

- FT-001 Core Interaction Prototype (all interaction and DeviceStateMachine).
- FT-002 Mission Framework (objective checklist for V0.5.0 integration).
- Hint Manager (V0.4.0+).

---

## QA Checks

- [ ] Coax cable connects correctly to wall outlet and modem Coax-IN.
- [ ] Connecting coax to wrong port gives error feedback and does not latch.
- [ ] Modem LED sequence (Off → Booting → DS/US Sync → Online) plays correctly.
- [ ] Powering modem before coax → modem stays in DS/US Sync; hint fires after 15 s.
- [ ] Success screen appears exactly once on modem reaching Online.
- [ ] Mission completable without developer intervention.

---

## Done Definition

A player with no context can enter the room, connect the coax cable and power adapter,
and watch the modem come online without needing a hint. Takeaway card is shown on completion.
