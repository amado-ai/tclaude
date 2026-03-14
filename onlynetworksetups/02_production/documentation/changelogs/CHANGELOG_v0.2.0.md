# Changelog — V0.2.0 Core Interaction Prototype

**Version:** V0.2.0
**Phase:** Core Interaction Prototype
**Build Name:** prototype-room-v1
**Date Started:** 2026-03-14
**Status:** IN PROGRESS

---

## Summary

V0.2.0 proves the fundamental game loop works: pick up hardware, connect cables in correct
order, watch devices boot, reach success screen. Single room. No story. No extra systems.
One clean path from start to success state.

---

## Systems Added

- [ ] `InteractionRaycaster` — camera-forward raycast, 20/s idle, per-frame when holding.
- [ ] `ObjectHolder` — pick up, inspect (free-rotate), release.
- [ ] `CableConnector` + `CableEnd` — cable state machine (Idle → Held → Connected).
- [ ] `PortReceiver` — typed port with occupation guard.
- [ ] `DeviceStateMachine` — modem (Off/Booting/DsUsSync/Online) and router (Off/Booting/WanActive/WifiBroadcasting).
- [ ] `LEDController` + `LEDStateConfig` — per-state LED color and blink config.
- [ ] `EventBus` — static pub/sub for cross-layer communication.
- [ ] `MissionValidator` — success detection (router reaches WifiBroadcasting state).
- [ ] `FeedbackManager` — snap, error, boot audio + haptics.
- [ ] `HapticProvider` — iOS / Android / NoOp abstraction.

---

## Missions Added or Updated

- [ ] Prototype room scene in `02_production/levels/mission_01_first_signal/`.

---

## UI / UX Changes

- [ ] Minimal success screen (text overlay): "Modem and router online. Mission complete."
- [ ] Hover prompt (object name label) when raycaster hits an interactable.
- [ ] Port connection highlight (shader emission pulse on port collider mesh).

---

## Audio / Visual Changes

- [ ] Snap SFX on successful port connection.
- [ ] Error SFX on type-mismatch connection attempt.
- [ ] Boot beep SFX on device power-on.
- [ ] LED emission animations per device/state.

---

## Bug Fixes

- N/A (new prototype).

---

## Docs Updated

- `02_production/documentation/technical_design/v0.2.0_code_architecture.md` — complete.
- `01_preproduction/feature_tracker/in_progress/FT-001_core_interaction_prototype.md` — active.

---

## Known Issues

- None yet (implementation not started).

---

## Testing Status

- QA plan pending. Will be created in `03_testing/qa_plans/` when implementation begins.

---

## Next Milestone Target

V0.3.0 — Vertical Slice
Exit condition: Mission 2 polished for demonstration with checklist UI, port labels, LEDs, basic audio, and success screen.
