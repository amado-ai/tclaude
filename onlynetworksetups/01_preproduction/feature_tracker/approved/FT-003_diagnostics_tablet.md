# FT-003: Diagnostics Tablet

**Feature ID:** FT-003
**Feature Name:** Diagnostics Tablet
**Status:** approved
**Version Target:** V0.4.0
**Owner:** Senior Game Developer + Creative Director & VFX
**Date Created:** 2026-03-14
**Last Updated:** 2026-03-14

---

## Purpose

Give the player a diegetic tool for reading device state, signal levels, and connection
status. The tablet is the in-world equivalent of a laptop running a network diagnostics
app. It grounds the game's learning in a real-world analogue and provides a secondary
feedback surface beyond LED strips.

---

## Core Behavior

- Physical prop: a tablet device the player can pick up and inspect (uses FT-001 interaction system).
- Tablet UI renders as a world-space canvas on the tablet screen when in inspect mode.
- **Device Status Panel:** lists all powered devices in the room and their current state (Off / Booting / Online / Error).
- **Connection Map Panel:** shows a diagram of which ports are connected to which devices with cable type labels.
- **Signal Meter Panel:** displays a numerical signal strength value (0–100) for the active WAN connection. Updates live.
- **Event Log Panel:** a scrolling feed of the last 10 connection/state events (e.g. "Modem reached Online state", "Ethernet cable disconnected from Router WAN").
- Tablet data is driven by `EventBus` subscriptions — no direct references to scene objects.
- Tablet is available in every mission from V0.4.0 onward.

---

## Dependencies

- FT-001 Core Interaction Prototype (IInteractable, ObjectHolder, EventBus, DeviceStateMachine).
- FT-002 Mission Framework (EventBus event vocabulary must be stable).
- World-space UI canvas support in the target engine.

---

## Assets Needed

- Tablet 3D model + screen texture (world-space canvas blit).
- Tablet UI stylesheet (dark theme, monospaced readout font, green/amber/red status colors).
- Connection diagram icons (modem, router, cable, port).

---

## UI/UX Needs

- Tab navigation between the four panels (Device Status / Connection Map / Signal Meter / Event Log).
- Status colors: green = healthy, amber = degraded/booting, red = offline/error.
- Signal meter animates smoothly (lerp, not jump) when the value changes.
- Event log auto-scrolls to latest entry; maximum 50 entries retained.
- Tablet screen dims when the player is not in inspect mode (battery-save visual metaphor).

---

## Test Cases

- [ ] Device Status panel updates within one frame of `DeviceStateChangedEvent`.
- [ ] Connection Map reflects every connected cable correctly, including partial connections.
- [ ] Signal Meter shows 0 when WAN is offline and a non-zero value when router is in WifiBroadcasting state.
- [ ] Event Log receives and displays entries for all relevant EventBus events.
- [ ] Tablet can be picked up, inspected, and released without affecting game state.
- [ ] Tablet works correctly in all nine missions without scene-specific code.

---

## Done Definition

Player can pick up the tablet in any mission, read accurate device state and connection
data, and use that information to diagnose problems without needing to look at LEDs alone.
All four panels display live data sourced purely from EventBus events.

---

## Final Folder Destination

| Artifact | Location |
|---|---|
| `DiagnosticsTablet.cs` | `02_production/game_build/networking_logic/` |
| `TabletUIController.cs` | `02_production/game_build/ui/` |
| `TabletEventLog.cs` | `02_production/game_build/ui/` |
| Tablet prefab (prop + canvas) | `02_production/tools/diagnostics_tablet/` |
| Tablet UI assets | `02_production/assets/icons/` |

---

## Changelog Version

V0.4.0

---

## Open Risks

| Risk | Mitigation |
|---|---|
| World-space canvas performance on mobile | Use a render texture blit only when tablet is in inspect mode; disable when holstered |
| Event log growing unbounded | Cap at 50 entries with ring-buffer; discard oldest |
| Tablet UI readability at arm's length in-world | Test at real device FOV; minimum 18pt equivalent for all diagnostic text |

---

## Decision History

| Date | Decision | Reason |
|---|---|---|
| 2026-03-14 | Diegetic tablet prop, not a heads-up menu | Keeps immersion; real techs use laptops/phones for diagnostics |
| 2026-03-14 | Data sourced from EventBus, not direct scene queries | Tablet works in any scene with no per-mission wiring |
