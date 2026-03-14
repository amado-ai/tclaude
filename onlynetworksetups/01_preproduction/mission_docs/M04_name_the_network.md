# Mission 04: Name the Network

**Mission ID:** M04
**Mission Name:** Name the Network
**Version Target:** V0.5.0
**Wave:** 1
**Owner:** Senior Game Developer + Creative Director & VFX
**Status:** Approved — pending V0.5.0 implementation
**Folder Destination:** `02_production/levels/mission_04_name_the_network/`

---

## Mission Purpose

Teach the player about Wi-Fi network identity: SSID (the network name), password security,
and the difference between the 2.4 GHz and 5 GHz bands. The player completes a physical
setup (from Mission 2) and then accesses the router's admin interface via the diagnostics
tablet to configure the network name and password.

---

## Learning Objectives

- Understand what an SSID is and why it matters.
- Set a network name and password via a simulated router admin UI.
- Understand the difference between 2.4 GHz (range) and 5 GHz (speed) bands.
- Understand why the default router password should always be changed.

---

## Environment

Same room as Mission 2. Adds:
- Laptop or tablet client device on a table (a "test device" that will connect to Wi-Fi once SSID and password are configured).
- Diagnostics tablet now includes a "Router Admin" panel (new panel for this mission).

---

## Player Tasks

1. Complete the physical setup (coax, Ethernet, power) — same as Mission 2.
2. Pick up the diagnostics tablet.
3. Navigate to the Router Admin panel.
4. Set the SSID (network name) — player types a name using an on-screen keyboard.
5. Set the Wi-Fi password — player types a password (minimum 8 characters enforced).
6. Apply settings. Router LEDs blink briefly during apply, then return to solid.
7. Observe the client device connect to the network (connection animation on the laptop/test device).

---

## Fail States

- Password shorter than 8 characters → input rejected with error message ("Wi-Fi passwords must be at least 8 characters.").
- SSID left as default ("MyRouter" placeholder) → warning hint triggers, player can still proceed but receives a tip about default SSIDs.
- Settings applied before physical connections are complete → router admin panel shows "Offline — complete hardware setup first."

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Player idle in Router Admin panel 30 s without typing | "Give your network a name in the SSID field. This is what you will see when you search for Wi-Fi on your phone." |
| Password field rejected 2x | "Wi-Fi passwords must be at least 8 characters. Make it something you will remember but others cannot guess." |
| SSID left as default | "Using a default network name is a minor security risk — it tells anyone nearby what brand of router you have." |

---

## Success Condition

SSID and password set, settings applied, client device connects to network.
`MissionCompleteEvent` fires.

---

## Real-World Takeaway Card

**What you just did:** Configured your Wi-Fi network name and password through the router admin interface.

**In real life:** Every router ships with a default SSID and password. Changing both is a basic security step every home network owner should take. The 2.4 GHz band reaches farther through walls; the 5 GHz band is faster but shorter range. Many modern routers broadcast both simultaneously.

**Key terms:** SSID · Wi-Fi password · 2.4 GHz vs 5 GHz · Router admin interface · Band steering

---

## Required Assets

- Router admin UI panel (new panel on diagnostics tablet — dark theme, form-based).
- On-screen keyboard (mobile-friendly).
- Client device prop (laptop or tablet mesh, fixed on table).
- Client device connection animation (small Wi-Fi icon appears, green pulse).

---

## Required Systems

- FT-001 Core Interaction Prototype.
- FT-002 Mission Framework.
- FT-003 Diagnostics Tablet (Router Admin panel — new panel added in V0.5.0).
- Hint Manager.
- On-screen keyboard input system.

---

## QA Checks

- [ ] Router Admin panel inaccessible while physical setup is incomplete.
- [ ] SSID field accepts valid input; password field enforces 8-character minimum.
- [ ] Applying settings triggers router LED blink and then stabilizes.
- [ ] Client device shows connection animation after settings applied.
- [ ] Default SSID hint fires correctly.
- [ ] Mission completable without developer intervention.

---

## Done Definition

Player completes physical setup, navigates to Router Admin panel, sets a valid SSID and
password, applies settings, and sees the client device connect. Takeaway card displayed.
