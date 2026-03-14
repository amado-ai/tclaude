# Mission 06: Intermittent Failure

**Mission ID:** M06
**Mission Name:** Intermittent Failure
**Version Target:** V0.6.0
**Wave:** 2
**Owner:** Senior Game Developer
**Status:** Approved — pending V0.6.0 implementation
**Folder Destination:** `02_production/levels/mission_06_intermittent_failure/`

---

## Mission Purpose

Teach the player to diagnose unstable, intermittent network faults rather than clean
not-connected failures. The network appears to work, then drops, then returns. The player
must identify the root cause (a loose cable, a semi-seated connector, or a power-cycling
device) and permanently fix it. Introduces timed stability validation.

---

## Learning Objectives

- Recognize that intermittent behavior is often a physical layer problem.
- Use the diagnostics tablet event log to spot recurring disconnections.
- Identify a "loose" or partially connected cable as distinct from a fully disconnected one.
- Hold a stable connection for a validation window to confirm the fix.

---

## Environment

Same room topology as Mission 2. Scene loads with a "loose" fault active (randomized from pool):
- Coax cable partially seated (visually connected, but collider has a degraded-state variant that causes random disconnect events every 15–30 s).
- Ethernet cable at the modem-side partially seated.
- OR: Modem power adapter intermittently losing contact (power-cycle fault).

The network appears online at scene start — modem Online LED is lit — but the event log on the diagnostics tablet shows periodic "Connection lost" entries.

---

## Player Tasks

1. Open diagnostics tablet — observe the event log showing recurring disconnections.
2. Identify which connection is intermittent (event log timestamps point to the cable type; LED flickering provides a visual cue).
3. Disconnect the faulty cable fully.
4. Reconnect it firmly (confirm snap + audio cue is distinct from a loose connection).
5. Hold the connection stable for 30 s (timed stability validation window).
6. Validation bar fills — mission complete.

---

## Fail States

- Player disconnects and reconnects the wrong cable → fault remains active, event log continues showing drops. Hint fires.
- Player does not reconnect within 60 s → gentle reminder hint.
- Reconnected cable is still only partially seated (engine-side: collision not fully in confirm zone) → treated as loose, validation fails, error feedback.

---

## Hint Triggers

| Trigger | Hint Text |
|---|---|
| Player has not opened tablet after 45 s | "Check the event log on the diagnostics tablet. Something is dropping the connection repeatedly." |
| Player reconnects wrong cable | "The event log shows which type of connection is failing. Compare the timestamps." |
| Validation bar fails | "Make sure the cable is fully seated — listen for the click." |

---

## Success Condition

All connections stable for 30 s continuous. Timed stability validation bar reaches 100%.
`MissionCompleteEvent` fires.

---

## Real-World Takeaway Card

**What you just did:** Diagnosed and fixed an intermittent physical layer network fault.

**In real life:** Most "internet keeps dropping" problems are not ISP issues. A coax cable that is not fully threaded, an Ethernet cable with a worn clip, or a power adapter that vibrates loose are far more common causes. Before calling support, wiggle every cable. If the event log shows a pattern, the cause is physical.

**Key terms:** Intermittent fault · Physical layer · Cable seating · Stability validation

---

## Required Assets

- Loose-cable visual variant (cable slightly askew from port, distinct from fully-connected appearance).
- LED flicker animation for intermittent fault state.
- Timed stability validation bar UI (thin bar beneath the checklist or centered bottom).
- Fault pool ScriptableObject with loose-coax, loose-ethernet, and power-cycle variants.

---

## Required Systems

- FT-001 Core Interaction Prototype (loose-cable state = new intermediate CableState).
- FT-002 Mission Framework.
- FT-003 Diagnostics Tablet (event log must be functional and populated).
- Randomized fault pool system (V0.6.0).
- Timed stability validation system (V0.6.0).
- Hint Manager.

---

## QA Checks

- [ ] Scene loads with intermittent fault active.
- [ ] Event log on diagnostics tablet shows recurring disconnect entries.
- [ ] Loose cable is visually distinct from fully-connected cable.
- [ ] Correctly identified and firmly reconnected cable stops event log drops.
- [ ] Timed validation bar starts and fills over 30 s without interruption.
- [ ] Reconnecting wrong cable does not start the validation timer.
- [ ] All fault pool variants are completable.
- [ ] 60 FPS maintained throughout.

---

## Done Definition

Player can diagnose the intermittent fault using the event log, fix the correct cable,
and hold stability for the validation window. All fault pool variants are playable.
Mission completable without hints.
