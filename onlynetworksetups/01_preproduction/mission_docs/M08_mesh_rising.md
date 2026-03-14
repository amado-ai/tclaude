# Mission 08: Mesh Rising

**Mission ID:** M08
**Mission Name:** Mesh Rising
**Version Target:** V0.7.0
**Wave:** 3
**Owner:** Senior Game Developer + Lead Software Architect
**Status:** Approved — pending V0.7.0 implementation (depends on FT-006)
**Folder Destination:** `02_production/levels/mission_08_mesh_rising/`

---

## Mission Purpose

Introduce mesh networking as the solution to the dead zones identified in Mission 7.
The player places, powers, and pairs mesh nodes in the same large floor plan, watching
the heat map fill in as each node is added. The player learns that node placement matters
and that nodes must maintain backhaul (upstream connection) to the primary router or
another paired node.

---

## Learning Objectives

- Understand what a mesh node is and how it extends the primary router's coverage.
- Place mesh nodes to eliminate dead zones identified in Mission 7.
- Pair a mesh node to the network (hands-on pairing sequence).
- Understand backhaul: a node must be within signal range of the router or another node.
- Recognize the backhaul-weak warning and correct it by repositioning the node.

---

## Environment

Same large floor plan as Mission 7. Primary router is fixed in its living room position.
Two mesh nodes are unpacked (in boxes) in the living room. Player must place them in the
far bedroom and garage/basement areas to achieve whole-home coverage.

---

## Player Tasks

1. Pick up the first mesh node from its box.
2. Carry it toward the far bedroom — range ring overlay appears showing coverage radius and backhaul status.
3. Place the node in a valid location that is within backhaul range of the primary router.
4. Power the node (connect power adapter).
5. Initiate pairing (hold button near node while pairing indicator fills over 4 s).
6. Observe the node's LEDs transition: Off → Booting → Pairing → Paired.
7. Observe heat map fill in the far bedroom area.
8. Repeat for the second mesh node in the garage/basement area.
9. For the second node: place it within range of the first node (node-to-node backhaul), not the primary router — the primary router signal does not reach the garage.

---

## Fail States

- Node placed out of backhaul range → `Backhaul-Weak` warning appears, LEDs go amber, coverage does not register. Hint fires.
- Player tries to pair before powering → pairing button is inactive; error feedback.
- Second node placed out of range of both primary router and first node → goes offline. Player must reposition.

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Node placed out of range | "The node can't reach the router from here. Move it closer to the living room, then back toward the bedroom — it needs to stay in range." |
| Player tries to pair unpowered node | "Connect the power adapter first. The node needs power before it can pair." |
| Second node out of range of both router and first node | "The second node is too far from the first. It needs to connect back through the first node, not directly to the router." |

---

## Success Condition

Both mesh nodes are placed, powered, and paired. Heat map shows >= 80% coverage in all
rooms including the previously dead zones. `MissionCompleteEvent` fires.

---

## Real-World Takeaway Card

**What you just did:** Built a mesh network that extends Wi-Fi coverage to every room in a large home.

**In real life:** Mesh nodes work by creating a chain of wireless backhaul connections back to the primary router. Each node must be within range of the next. Place them halfway between the router and the dead zone — not at the edge of coverage. The range ring preview you saw is how real installers think about node placement.

**Key terms:** Mesh node · Backhaul · Node pairing · Coverage chain · Wi-Fi 6 mesh

---

## Required Assets

- Mesh node 3D model (distinct from router — smaller, satellite-style).
- Node box prop (unpacking interaction at scene start).
- Pairing VFX: pulse arc between node and nearest paired device during pairing sequence.
- Backhaul-weak amber LED on node.
- Range ring overlay (FT-006).
- Updated heat map with multi-node blending (FT-005 + FT-006).

---

## Required Systems

- FT-001 Core Interaction Prototype.
- FT-002 Mission Framework.
- FT-003 Diagnostics Tablet (mesh topology panel).
- FT-004 Router Placement (placement zone system reused for node placement).
- FT-005 Signal Heat Map (multi-node blending).
- FT-006 Mesh Node System.
- Hint Manager.

---

## QA Checks

- [ ] Mesh node is pickupable and placeable on valid surfaces.
- [ ] Range ring overlay shows backhaul status (green/amber/red) in real time while carrying.
- [ ] Node pairing sequence completes in 4 s when in range.
- [ ] Backhaul-weak state triggers when node is out of range.
- [ ] Moving node back into range re-establishes connection automatically.
- [ ] Heat map correctly blends coverage from primary router + both nodes.
- [ ] Node-to-node backhaul (second node chains through first) works correctly.
- [ ] Mission completable without developer intervention.
- [ ] 60 FPS with both nodes active and heat map visible.

---

## Done Definition

Player places and pairs two mesh nodes, eliminates all dead zones from Mission 7, and
achieves whole-home coverage. Node-to-node backhaul is demonstrated. Mission completable
without hints.
