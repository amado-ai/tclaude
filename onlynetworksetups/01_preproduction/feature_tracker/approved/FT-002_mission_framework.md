# FT-002: Mission Framework and Objective Manager

**Feature ID:** FT-002
**Feature Name:** Mission Framework and Objective Manager
**Status:** approved
**Version Target:** V0.4.0
**Owner:** Lead Software Architect
**Date Created:** 2026-03-14
**Last Updated:** 2026-03-14

---

## Purpose

Build the reusable mission scaffolding that all nine missions run on. Without this system
every mission is a custom hack. With it, missions are data-driven and consistent. This is
the production multiplier that makes V0.5.0 through V0.7.0 content buildable at pace.

---

## Core Behavior

- `MissionDefinition` ScriptableObject: declares mission ID, name, wave, objectives list, hint config, success conditions, and takeaway card content.
- `MissionManager` singleton: loads the active mission definition, tracks objective state, fires `ObjectiveCompleteEvent` and `MissionCompleteEvent`.
- `ObjectiveManager`: maintains a list of `Objective` objects each with ID, description, required state, and completion status. Displays as a checklist in UI.
- `MissionLoader`: scene bootstrap — reads the active mission slot from save data and initializes `MissionManager` with the correct definition.
- Pass/fail evaluation is driven by state events from `DeviceStateMachine` and `CableConnector`, not hardcoded per mission.

---

## Dependencies

- FT-001 Core Interaction Prototype (EventBus, DeviceStateMachine) — must be complete.
- UI system (checklist panel) — developed in parallel during V0.4.0.
- Save/load system — stub required before full save integration.

---

## Assets Needed

- `MissionDefinition` ScriptableObject schema and editor tooling.
- Checklist UI prefab (reused across all missions).

---

## UI/UX Needs

- On-screen objective checklist: items check off as each objective is completed.
- Objective added / completed animations (subtle, non-intrusive).
- Mission title card shown at scene load (3 s, dismissible).

---

## Test Cases

- [ ] Objectives complete in correct order and checklist updates immediately.
- [ ] Completing all objectives triggers `MissionCompleteEvent` exactly once.
- [ ] Replaying a mission resets all objective states cleanly.
- [ ] Mission with 1 objective and mission with 6 objectives both function correctly.
- [ ] Objective completion does not fire if prerequisite objective is incomplete (where ordering is required).

---

## Done Definition

Any mission can be defined entirely in a ScriptableObject. Adding a new mission requires
zero new C# code for the core framework path. Checklist UI reflects live objective state.
`MissionCompleteEvent` fires exactly once on success.

---

## Final Folder Destination

| Artifact | Location |
|---|---|
| `MissionDefinition.cs` (ScriptableObject) | `02_production/game_build/mission_system/` |
| `MissionManager.cs` | `02_production/game_build/mission_system/` |
| `ObjectiveManager.cs` | `02_production/game_build/mission_system/` |
| `MissionLoader.cs` | `02_production/game_build/mission_system/` |
| Checklist UI prefab | `02_production/game_build/ui/` |
| Mission definition assets (.asset) | `02_production/levels/mission_0X_.../` (per mission) |

---

## Changelog Version

V0.4.0

---

## Open Risks

| Risk | Mitigation |
|---|---|
| Objective ordering constraints make data schema complex | Default to unordered; add `requires` field only where sequencing is strictly needed |
| Checklist UI diverges across missions | Lock prefab early and use data binding, not scene-level overrides |

---

## Decision History

| Date | Decision | Reason |
|---|---|---|
| 2026-03-14 | ScriptableObject-driven mission definitions | Keeps designers in the editor, not in code |
| 2026-03-14 | `MissionManager` listens to `EventBus`, not direct device references | Maintains layer separation from V0.2.0 architecture |
