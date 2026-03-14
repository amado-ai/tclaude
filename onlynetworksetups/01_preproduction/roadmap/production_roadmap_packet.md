# onlynetworksetups — Production Roadmap Packet

**By:** Amado A.I.
**Document Type:** Production planning packet
**Active Version:** V0.2.0 (in progress)
**Last Updated:** 2026-03-14

---

## Phase Roadmap

| Version | Phase | Status |
|---|---|---|
| V0.1.0 | Concept Foundation | COMPLETE |
| V0.2.0 | Core Interaction Prototype | IN PROGRESS |
| V0.3.0 | Vertical Slice | Pending |
| V0.4.0 | Systems Expansion | Pending |
| V0.5.0 | Pre-Alpha Content Framework | Pending |
| V0.6.0 | Troubleshooting Layer | Pending |
| V0.7.0 | Alpha | Pending |
| V0.8.0 | Internal QA | Pending |
| V0.9.0 | Beta | Pending |
| V0.9.5 | Release Candidate | Pending |
| V1.0.0 | Launch | Pending |

---

## Milestone Exit Criteria

| Milestone | Required Exit Condition |
|---|---|
| V0.2.0 | Player completes modem-router setup loop in one room without developer intervention. |
| V0.3.0 | Slice stable for demonstration: visible labels, clear pass/fail, takeaway card present. |
| V0.5.0 | Missions 1–5 playable in sequence with working progression, no missing critical systems. |
| V0.7.0 | All nine missions playable; mesh certification works end to end. |
| V0.9.0 | External testers understand game without explanation; usability issues (not blocking confusion) reported. |
| V0.9.5 | No critical blockers; release assets ready. |

---

## Production Operating Rules

1. **One source of truth** — Roadmap, GDD, mission docs, feature tracker, changelogs, and build folders must agree.
2. **Approved work only** — Any new feature must support the core learning promise or core loop.
3. **Definition of done** — Implementation + review + documentation + QA + folder placement all complete.
4. **Version discipline** — Every significant change belongs to a target version with a changelog entry.
5. **Archive instead of clutter** — Deprecated ideas and rejected features go to `99_archive/`.

---

## Current Active Feature Trackers

| ID | Feature | Status | Target |
|---|---|---|---|
| FT-001 | Core Interaction Prototype | in_progress | V0.2.0 |

---

## Immediate Next Actions (V0.2.0)

1. Scaffold Unity project (or target engine) scene in `02_production/levels/mission_01_first_signal/`.
2. Implement `EventBus` and `IInteractable` interface first (zero dependencies, unblocks all other work).
3. Implement `CableConnector` + `PortReceiver` with test objects (two cubes, one trigger zone).
4. Implement `DeviceStateMachine` (modem only first, validate LED transitions in isolation).
5. Wire router state machine to subscribe to `DeviceStateChangedEvent` from modem.
6. Implement `MissionValidator` and success screen stub.
7. Dress prototype room with placeholder modem/router meshes and correct port collider positions.
8. QA pass against all criteria in FT-001.
9. Move FT-001 to `feature_tracker/complete/`, update V0.2.0 changelog, begin V0.3.0 planning.
