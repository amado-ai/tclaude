# onlynetworksetups — Full Production Roadmap
**by Amado A.I.**

| | |
|---|---|
| **Project Goal** | Ship a focused educational simulation with strong real-world skill transfer. |
| **Core Loop** | Inspect → connect → power → test → troubleshoot → learn. |
| **Version 1.0 Scope** | 9 missions, guided learning, diagnostics, and final mesh certification. |

This document consolidates the roadmap, version plan, folder workflow, and operating templates used to keep development on track.

---

## 1. Executive Summary

This roadmap organizes onlynetworksetups as a studio-style production effort instead of a loose concept file. It locks the project around one promise: the player should leave the game understanding real modem, router, coax, Ethernet, Wi-Fi placement, and mesh setup concepts well enough to apply them in the real world.

- **Primary outcome:** practical learning transfer, not abstract tech flavor.
- **Primary delivery strategy:** build in phases, validate early, and move completed work into the correct folder and version.
- **Primary control mechanism:** one source of truth across the roadmap, GDD, feature tracker, changelog, and production folders.

---

## 2. Production Principles

- Nothing is "done" until it is implemented, reviewed, documented, tested, and logged in a changelog.
- No major feature enters production without a version target and final folder destination.
- Every milestone has explicit exit criteria. The project does not move forward on vibes alone.
- Version 1.0 stays focused. Anything that does not strengthen the core loop or learning promise goes to backlog or post-launch.

---

## 3. Full Development Model

| Phase | Build | Purpose | Exit Condition |
|---|---|---|---|
| Concept foundation | ONS_v0.1.0_concept | Lock vision, audience, scope, folders, and roadmap. | Team can describe exactly what the game is and what 1.0 includes. |
| Prototype | ONS_v0.2.0_prototype | Prove the basic setup loop works. | A player can bring a simple modem-router chain online. |
| Vertical slice | ONS_v0.3.0_vertical_slice | Polish one mission to near-final quality. | One mission is presentable and educationally clear. |
| Systems expansion | ONS_v0.4.0_systems_expansion | Build reusable systems for all missions. | Core systems support scaling without reinvention. |
| Pre-alpha | ONS_v0.5.0_prealpha | Complete the first five missions. | Mission flow and progression work through the first half. |
| Troubleshooting | ONS_v0.6.0_troubleshooting | Add intermittent-fault and dead-zone logic. | Players can diagnose unstable setups and coverage limits. |
| Alpha | ONS_v0.7.0_alpha | Make the full game playable end to end. | All missions are playable and critical systems exist. |
| Internal QA | ONS_v0.8.0_internal_qa | Clean up blockers and improve clarity. | Internal testers can complete the game reliably. |
| Beta | ONS_v0.9.0_beta | Feature-complete external test build. | External testers can play without developer hand-holding. |
| Release candidate | ONS_v0.9.5_rc1 | Prepare to ship. | No known critical blockers remain. |
| Launch | ONS_v1.0.0_launch | Public release and support readiness. | Game is live and post-launch support is active. |

---

## 4. Production Folder Structure

```
onlynetworksetups/
├── 01_preproduction/
│   ├── concept/        ├── vision/      ├── gdd/         ├── roadmap/
│   ├── mission_docs/   ├── ui_ux_specs/ ├── feature_tracker/
│   ├── art_direction/  └── references/
├── 02_production/
│   ├── game_build/
│   │   ├── core_systems/   ├── interaction/      ├── networking_logic/
│   │   ├── mission_system/ ├── ui/               ├── audio/
│   │   ├── save_system/    └── accessibility/
│   ├── levels/
│   │   ├── mission_01_first_signal/
│   │   ├── mission_02_bring_the_router_online/
│   │   ├── mission_03_wrong_port_wrong_day/
│   │   ├── mission_04_name_the_network/
│   │   ├── mission_05_signal_wars/
│   │   ├── mission_06_intermittent_failure/
│   │   ├── mission_07_the_dead_zone/
│   │   ├── mission_08_mesh_rising/
│   │   └── mission_09_whole_home_certification/
│   ├── assets/
│   │   ├── models/  ├── textures/  ├── materials/  ├── animations/
│   │   ├── vfx/     ├── sfx/       ├── voice/      └── icons/
│   ├── documentation/
│   │   ├── implementation_notes/  ├── technical_design/
│   │   ├── system_docs/           └── changelogs/
│   └── tools/
│       ├── diagnostics_tablet/  ├── signal_visualizer/
│       ├── mission_editor/      └── debug_tools/
├── 03_testing/
│   ├── qa_plans/          ├── bug_reports/       ├── usability_tests/
│   ├── playtest_feedback/ ├── accessibility_tests/
│   └── certification_checklists/
├── 04_release/
│   ├── build_exports/ ├── screenshots/ ├── trailers/
│   ├── store_assets/  ├── press_kit/   └── release_notes/
├── 05_postlaunch/
│   ├── patches/       ├── analytics/      ├── player_feedback/
│   ├── dlc_planning/  └── roadmap_updates/
└── 99_archive/
    ├── old_builds/  ├── deprecated_docs/
    ├── rejected_features/  └── retired_assets/
```

---

## 5. Version-by-Version Roadmap

### V0.1.0 — Concept Foundation
**Goal:** Lock project identity and planning.

Key deliverables:
- Finalize title, vision, target audience, pillars, learning outcomes, mission map, and folder structure.
- Create initial GDD, roadmap, and feature backlog.

**Exit condition:** The team can explain what the game is, who it serves, and what version 1.0 includes.

---

### V0.2.0 — Core Interaction Prototype
**Goal:** Prove the core loop works.

Key deliverables:
- Build first-person movement, object interaction, coax/Ethernet/power connections, modem/router boot logic, and one client test.
- Use a single room with a wall coax jack, modem, router, and test device.

**Exit condition:** A player can complete the basic connection chain from wall coax to working internet.

---

### V0.3.0 — Vertical Slice
**Goal:** Create one polished mission at near-final quality.

Key deliverables:
- Use Mission 2: Bring the Router Online.
- Polish UI, labels, LEDs, cable snapping, sound, guidance, completion flow, and real-world takeaway card.

**Exit condition:** The mission is presentable and communicates final product quality.

---

### V0.4.0 — Systems Expansion
**Goal:** Build reusable systems for scaling.

Key deliverables:
- Add mission objective manager, hint manager, glossary/help system, diagnostics tablet, skill cards, scoring framework, save/load framework, and reusable device state machines.

**Exit condition:** Future missions can be built without reinventing the same plumbing.

---

### V0.5.0 — Pre-Alpha
**Goal:** Complete the first half of the game.

Key deliverables:
- Ship Missions 1–5 with progression, WAN vs LAN troubleshooting, SSID/password setup, signal strength checks, and router placement gameplay.

**Exit condition:** Players can complete the first five missions in order with working progression.

---

### V0.6.0 — Troubleshooting Expansion
**Goal:** Build the troubleshooting identity.

Key deliverables:
- Ship Mission 6: Intermittent Failure and Mission 7: The Dead Zone.
- Add timed validation, randomized fault pools, extended diagnostics, and dead-zone detection.

**Exit condition:** Players can diagnose unstable internet and identify when one-router coverage is not enough.

---

### V0.7.0 — Alpha
**Goal:** Make the full game playable end to end.

Key deliverables:
- Ship Missions 8–9 and add mesh node pairing, node quality states, heat maps, final certification terminal, and baseline accessibility options.

**Exit condition:** All missions are playable and no critical core systems are missing.

---

### V0.8.0 — Internal QA and Polish
**Goal:** Remove major cracks before public testing.

Key deliverables:
- Focus on bug fixing, onboarding clarity, hint timing, UI cleanup, sound/lighting pass, glossary cleanup, and save/load validation.

**Exit condition:** Internal testers can complete the game reliably without heavy developer intervention.

---

### V0.9.0 — Beta
**Goal:** Run external playtests on a feature-complete build.

Key deliverables:
- Test learning outcomes, frustration points, mission clarity, mesh comprehension, and accessibility baseline.

**Exit condition:** External testers can play and give useful feedback without being coached through the build.

---

### V0.9.5 — Release Candidate
**Goal:** Prepare to ship.

Key deliverables:
- Final bug triage, performance optimization, text pass, store assets, screenshots, trailer support, press kit, release notes, and packaging.

**Exit condition:** No known critical blockers remain and the build is stable enough to ship.

---

### V1.0.0 — Launch Build
**Goal:** Release the game and activate support.

Key deliverables:
- Ship the final build, launch notes, support workflow, patch backlog, and first post-launch review process.

**Exit condition:** The game is live and the post-launch patch process is ready.

---

## 6. Mission Production Order

Build content in waves to keep systems, testing, and onboarding manageable.

| Wave 1 | Wave 2 | Wave 3 |
|---|---|---|
| Mission 1 — First Signal | Mission 6 — Intermittent Failure | Mission 8 — Mesh Rising |
| Mission 2 — Bring the Router Online | Mission 7 — The Dead Zone | Mission 9 — Whole Home Certification |
| Mission 3 — Wrong Port, Wrong Day | | |
| Mission 4 — Name the Network | | |
| Mission 5 — Signal Wars | | |

---

## 7. Feature Tracker Workflow

Every feature file moves through four states. That movement is the discipline layer that prevents chaos.

| State | Meaning | Required Fields | Destination |
|---|---|---|---|
| Backlog | Idea exists but is not approved. | Name, purpose, rough fit, author, notes. | `01_preproduction/feature_tracker/backlog/` |
| Approved | Feature is accepted for future work. | Version target, dependencies, owner, done definition. | `01_preproduction/feature_tracker/approved/` |
| In progress | Feature is actively being built. | Implementation notes, active branch/task link, blocker notes. | `01_preproduction/feature_tracker/in_progress/` |
| Complete | Feature is shipped in a milestone. | Test verification, changelog entry, final folder placement. | `01_preproduction/feature_tracker/complete/` |

---

## 8. Move-to-Proper-Folder SOP

- New idea starts in `backlog/` with a short feature file.
- Once approved, the file moves to `approved/` and gets a version target.
- When implementation begins, the file moves to `in_progress/` and code/assets are created under `02_production/`.
- When completed, the feature file moves to `complete/`, implementation notes are saved, test verification is logged, and the milestone changelog is updated.
- If a feature is cut or replaced, move it to `99_archive/rejected_features/` or `99_archive/deprecated_docs/`. Do not leave orphaned files behind.

**Example path flow for FT-012_router_placement_system:**
```
01_preproduction/feature_tracker/backlog/FT-012_router_placement_system.md
→ 01_preproduction/feature_tracker/approved/FT-012_router_placement_system.md
→ 01_preproduction/feature_tracker/in_progress/FT-012_router_placement_system.md
→ implementation in 02_production/game_build/networking_logic/
→ mission integration in 02_production/levels/mission_05_signal_wars/
→ 01_preproduction/feature_tracker/complete/FT-012_router_placement_system.md
→ QA evidence in 03_testing/qa_plans/
→ changelog note in 02_production/documentation/changelogs/CHANGELOG_v0.5.0.md
```

---

## 9. Milestone Exit Criteria

- **Prototype:** the player can complete a basic setup chain without developer intervention.
- **Vertical slice:** the selected mission is polished, stable, readable, and educationally coherent.
- **Pre-alpha:** the first five missions work in sequence and the core teaching loop is intact.
- **Alpha:** the full game is playable start to finish and all critical systems exist.
- **Beta:** external testers can play independently and clearly explain the core concepts they learned.
- **Release candidate:** no known critical blockers remain and the build is stable enough to ship.

---

## 10. Biggest Production Risks

| Risk | What It Looks Like | Control Mechanism |
|---|---|---|
| Scope creep | Adding smart home, fiber, enterprise networking, story systems before 1.0. | Keep a hard V1.0 scope and move extras to backlog/post-launch. |
| Overbuilding before validation | Spending too long on polish before proving the cable/setup loop works. | Prototype early and lock a vertical slice before broad content production. |
| Weak learning transfer | Players finish missions but cannot explain what they learned. | Post-mission takeaway cards and external learning-focused playtests. |
| Folder and version disorder | Files, builds, and tasks drift into the wrong places. | Use the folder-move SOP and update the changelog every time work is completed. |

---

## 11. Production Templates

### 11.1 Master Roadmap Sheet Template

```
PROJECT: onlynetworksetups by Amado A.I.
CURRENT TARGET BUILD:
PROJECT OWNER:
LAST UPDATED:

MILESTONE:
BUILD VERSION:
PHASE:
STATUS: Not Started / In Progress / At Risk / Blocked / Complete
OWNER:
START DATE:
TARGET DATE:

GOAL
-
KEY DELIVERABLES
-
EXIT CRITERIA
-
DEPENDENCIES
-
TOP RISKS
-
FOLDER OUTPUT
-
CHANGELOG FILE
-
NOTES
-
```

### 11.2 Feature Tracker Template

```
FEATURE ID:
FEATURE NAME:
STATUS: Backlog / Approved / In Progress / Complete / Archived
VERSION TARGET:
OWNER:
DATE CREATED:
LAST UPDATED:

PURPOSE
Why this feature exists and what player problem it solves.

CORE BEHAVIOR
-
DEPENDENCIES
-
ASSETS NEEDED
-
UI/UX NEEDS
-
TEST CASES
-
DONE DEFINITION
-
FINAL FOLDER DESTINATION
-
CHANGELOG VERSION
-
NOTES
-
```

### 11.3 Changelog Template

```
BUILD VERSION:
DATE:
STATUS: Internal / Test / Release Candidate / Launch
AUTHOR:

SYSTEMS ADDED
-
CONTENT ADDED
-
BUGS FIXED
-
DOCS UPDATED
-
KNOWN ISSUES
-
NEXT TARGET
-
```

### 11.4 Mission Production Template

```
MISSION ID:
MISSION NAME:
VERSION TARGET:
OWNER:
STATUS:

MISSION PURPOSE
What the mission teaches.

LEARNING OBJECTIVES
-
ENVIRONMENT
-
PLAYER TASKS
-
FAIL STATES
-
HINT TRIGGERS
-
SUCCESS CONDITION
-
REAL-WORLD TAKEAWAY
-
REQUIRED ASSETS
-
REQUIRED SYSTEMS
-
QA CHECKS
-
DONE DEFINITION
-
FOLDER DESTINATION
-
```

### 11.5 Folder Move SOP Template

```
ITEM NAME:
ITEM TYPE: Feature / Mission Doc / UI Spec / Asset / Build
CURRENT LOCATION:
TARGET LOCATION:
VERSION:
OWNER:
REASON FOR MOVE:

CHECKLIST
[ ] Implementation complete
[ ] Review complete
[ ] Documentation updated
[ ] QA evidence saved
[ ] Changelog updated
[ ] Archive or redirect old file if needed

MOVE DATE:
NOTES:
```

---

## 12. Immediate Next Actions

- [x] Create the actual repository and folder tree exactly as defined in Section 4.
- [x] Save this roadmap packet into `01_preproduction/roadmap/`.
- [x] Start the feature tracker with FT-001 Core Interaction Prototype (in_progress, V0.2.0).
- [ ] Create feature tracker files for all remaining approved systems: mission framework (FT-002), diagnostics tablet (FT-003), router placement (FT-004), signal heat map (FT-005), mesh node system (FT-006).
- [ ] Build ONS_v0.2.0_prototype first. Do not skip straight to broad content production.
- [ ] Select Mission 2 as the vertical slice and lock its success criteria before polishing anything else.
- [ ] Use the templates in Section 11 from the first development week onward.

---

*Prepared for project planning and internal production use.*
*— Amado A.I.*
