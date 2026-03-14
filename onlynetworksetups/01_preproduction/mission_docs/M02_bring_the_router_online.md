# Mission 02: Bring the Router Online

**Mission ID:** M02
**Mission Name:** Bring the Router Online
**Version Target:** V0.3.0 (vertical slice) — V0.5.0 (full framework integration)
**Wave:** 1
**Owner:** Senior Game Developer + Creative Director & VFX
**Status:** Vertical slice target (V0.3.0)
**Folder Destination:** `02_production/levels/mission_02_bring_the_router_online/`

---

## Mission Purpose

Extend Mission 1 by adding a router to the chain. The player connects the modem to the
router via an Ethernet cable, powers both devices, and brings the full modem-router chain
online. This is the vertical slice mission — it must demonstrate near-final quality.

---

## Learning Objectives

- Identify an Ethernet cable (RJ-45) and distinguish it from a coax cable.
- Identify the WAN (Wide Area Network) port on a router — the port that connects to the modem, not to client devices.
- Understand the boot sequence dependency: the router needs the modem online before its WAN link activates.
- Recognize router LED states: Power, WAN, Wi-Fi 2.4 GHz, Wi-Fi 5 GHz.

---

## Environment

Same room as Mission 1, extended. Adds a router on a second shelf. Contains:
- Wall coax outlet (fixed).
- Cable modem (fixed).
- Wi-Fi router (fixed — placement is not the learning objective of this mission).
- Coax cable (pickupable).
- Ethernet cable (pickupable).
- Modem power adapter (pickupable).
- Router power adapter (pickupable).
- Power strip (fixed).

---

## Player Tasks

1. Connect coax cable: wall outlet → modem Coax-IN.
2. Connect Ethernet cable: modem Ethernet-OUT → router WAN-IN.
3. Connect modem power adapter: power strip → modem Power-IN.
4. Connect router power adapter: power strip → router Power-IN.
5. Observe modem boot → Online.
6. Observe router WAN link activate → Wi-Fi broadcasting.

---

## Fail States

- Ethernet cable connected to router LAN port instead of WAN port → error feedback. Hint triggers on 2nd incorrect attempt.
- Router powered before modem reaches Online → router stays in Booting/WAN-inactive state. Hint triggers after 20 s of no progress.
- Coax cable connected after modem is already powered → modem detects coax and re-evaluates sync (intended behavior, not a fail state).

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Player attempts Ethernet into LAN port 2x | "The WAN port is usually a different color — it connects the router to the modem, not to your devices." |
| Router stuck in Booting 20 s after power | "The router needs the modem to be fully online before its WAN link will activate. Check the modem's Online LED first." |
| Player idle 45 s with modem online but router not connected | "Connect an Ethernet cable from the modem's LAN/Ethernet port to the router's WAN port." |

---

## Success Condition

Router reaches `WifiBroadcasting` state. All modem and router LEDs solid green.

---

## Real-World Takeaway Card

**What you just did:** Built a complete modem-to-router connection and brought a home Wi-Fi network online.

**In real life:** The WAN port is the gateway between your router and the internet. Plugging into a LAN port instead creates a second local network segment — the router gets no internet signal. The boot order matters because the router checks for a live WAN connection during startup.

**Key terms:** WAN port · LAN port · RJ-45 / Ethernet · Router boot sequence · Wi-Fi broadcast

---

## Required Assets

- All Mission 1 assets.
- Wi-Fi router mesh + correct port colliders (WAN, LAN x4, Power-IN).
- Ethernet cable mesh (flexible, two RJ-45 CableEnd components).
- Router power adapter mesh.
- Port labels (3D world-space text overlays): "WAN", "LAN 1–4", "Coax-IN", "Ethernet-OUT", "Power".

---

## Required Systems

- FT-001 Core Interaction Prototype (complete).
- FT-002 Mission Framework (V0.3.0 checklist UI integration).
- Hint Manager (V0.3.0 basic version).
- LEDController full polish pass.

---

## QA Checks

- [ ] Ethernet into LAN port → error; into WAN port → latches correctly.
- [ ] Modem must be Online before router WAN activates.
- [ ] All port labels are visible and readable at normal play distance.
- [ ] LED sequences for both modem and router complete correctly.
- [ ] Hint fires on 2nd LAN-port mistake and after 20 s router-stuck scenario.
- [ ] Checklist updates in real time as each objective completes.
- [ ] Takeaway card displays on mission complete.
- [ ] Mission completable without developer intervention.

---

## Done Definition

Mission 2 is the vertical slice: every system (interaction, LEDs, labels, hints, checklist,
takeaway card, success screen) is at near-final quality. The mission is presentable to
publishers, testers, or partners without apology.
