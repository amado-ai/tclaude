# FT-006: Mesh Node System

**Feature ID:** FT-006
**Feature Name:** Mesh Node System
**Status:** approved
**Version Target:** V0.7.0
**Owner:** Lead Software Architect + Senior Game Developer
**Date Created:** 2026-03-14
**Last Updated:** 2026-03-14

---

## Purpose

Implement the final learning tier of the game: whole-home mesh networking. The player must
place, power, and pair multiple mesh nodes to extend coverage across a multi-room home.
This system is the subject of Missions 8 and 9 and is the capstone of the game's
educational arc. A player who finishes Mission 9 understands why one router is not always
enough and how mesh networks fill the gaps.

---

## Core Behavior

- `MeshNode` component extends `DeviceStateMachine` with additional states: `Pairing`, `Paired`, `Backhaul`.
- Pairing sequence: node is powered on → player initiates pair (button press near the primary router or a paired node) → 4 s pairing animation → `Paired` state.
- A paired node must be within signal range of the primary router or another paired node to maintain `Paired` state. Moving it out of range degrades it to `Backhaul-Weak` and eventually `Offline`.
- `MeshNetworkManager` singleton: maintains the graph of paired nodes, evaluates connectivity, and publishes `MeshTopologyChangedEvent`.
- `SignalCoverageCalculator` (FT-004) is extended to accept multiple coverage sources (primary router + all paired nodes) for heat map blending.
- Node quality states: `Paired` (full green LEDs), `Backhaul-Weak` (amber LED on backhaul indicator), `Offline` (all off).
- Certification terminal in Mission 9: a fixed in-world console that runs a coverage audit. Passes when all required rooms reach >= 70% coverage. Fires `CertificationPassedEvent` on success.

---

## Dependencies

- FT-001 Core Interaction Prototype (all base interaction and DeviceStateMachine infrastructure).
- FT-002 Mission Framework (objective evaluation for pairing and certification objectives).
- FT-004 Router Placement System (`SignalCoverageCalculator` multi-source extension).
- FT-005 Signal Heat Map (multi-node blending).
- Mission 8 and 9 level geometry (multi-room environments).

---

## Assets Needed

- Mesh node 3D model (distinct from router — smaller, satellite-style puck or tower).
- Pairing animation VFX (pulse between node and primary router during pair sequence).
- Backhaul strength indicator LED (separate LED on node face showing upstream link quality).
- Certification terminal 3D prop + UI screen (world-space canvas).
- Node placement zone indicators (same system as FT-004 but with range-ring overlay showing node's effective coverage radius).

---

## UI/UX Needs

- Range ring overlay: when carrying a mesh node, a translucent circle on the floor shows the node's coverage radius and whether it is within backhaul range of the nearest paired device (green ring = in range, amber = borderline, red = out of range).
- Pairing progress indicator: animated arc that fills during the 4 s pairing sequence.
- `MeshNetworkManager` status panel on the diagnostics tablet: shows the network graph (primary router + nodes), their states, and backhaul link quality.
- Certification terminal UI: room-by-room coverage percentage readout with pass/fail per room.

---

## Test Cases

- [ ] Node powers on, enters Booting, then waits for pairing.
- [ ] Pairing succeeds when node is within range of primary router or a paired node.
- [ ] Pairing fails (error feedback) when node is out of range.
- [ ] Moving a paired node out of range transitions it to Backhaul-Weak, then Offline.
- [ ] Moving it back into range re-establishes the connection automatically.
- [ ] Heat map correctly blends coverage from primary router + all paired nodes.
- [ ] Certification terminal correctly evaluates per-room coverage and passes only when all rooms meet the threshold.
- [ ] `CertificationPassedEvent` fires exactly once.
- [ ] System supports up to 3 mesh nodes simultaneously without frame rate drop on target mobile device.

---

## Done Definition

Player can place, power, and pair up to 3 mesh nodes. The heat map shows blended coverage
across the full home. Moving a node out of backhaul range degrades it visibly. The
certification terminal correctly audits all rooms and passes the mission when the threshold
is met. Missions 8 and 9 complete end to end using this system.

---

## Final Folder Destination

| Artifact | Location |
|---|---|
| `MeshNode.cs` | `02_production/game_build/networking_logic/` |
| `MeshNetworkManager.cs` | `02_production/game_build/networking_logic/` |
| `CertificationTerminal.cs` | `02_production/game_build/networking_logic/` |
| `MeshTopologyChangedEvent.cs` | `02_production/game_build/mission_system/` |
| `CertificationPassedEvent.cs` | `02_production/game_build/mission_system/` |
| Mesh node prefab | `02_production/levels/mission_08_mesh_rising/` |
| Certification terminal prefab | `02_production/levels/mission_09_whole_home_certification/` |
| Node VFX assets | `02_production/assets/vfx/` |
| Mesh status tablet panel | `02_production/game_build/ui/` |

---

## Changelog Version

V0.7.0

---

## Open Risks

| Risk | Mitigation |
|---|---|
| Graph connectivity evaluation is expensive with many nodes | Maximum 3 nodes in V1.0; BFS over a 4-node graph is negligible cost |
| Backhaul range transitions feel unfair to players | Show amber warning before nodes drop offline; give a 2 s grace period before Offline transition |
| Certification terminal pass criteria too strict/lenient | Make threshold configurable per mission in the MissionDefinition ScriptableObject |
| Players confused by backhaul concept | Hint manager triggers "nodes need to talk to each other" explanation on first Backhaul-Weak event |

---

## Decision History

| Date | Decision | Reason |
|---|---|---|
| 2026-03-14 | Extend `DeviceStateMachine` rather than create a separate node class | Reuse all existing state machine infrastructure; nodes are devices |
| 2026-03-14 | Graph-based connectivity, not simple proximity check | Accurately models real mesh behavior (node → node → router chain) |
| 2026-03-14 | Certification terminal as a diegetic prop | Consistent with diagnostics tablet design language; keeps immersion |
| 2026-03-14 | Cap at 3 nodes for V1.0 | Sufficient for educational arc; performance is safe; more nodes are post-launch territory |
