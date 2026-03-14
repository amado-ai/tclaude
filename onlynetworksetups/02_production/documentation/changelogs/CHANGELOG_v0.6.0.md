# Changelog — V0.6.0 Troubleshooting Layer

**Build Version:** ONS_v0.6.0_troubleshooting
**Date:** TBD
**Status:** Not Started
**Author:** Omni-Studio

---

## Summary

V0.6.0 adds the troubleshooting identity that makes onlynetworksetups more than a setup
tutorial. Missions 6 and 7 introduce randomized faults, timed stability validation, and
dead-zone detection. The player must now diagnose problems, not just follow a setup checklist.

---

## Systems Added

- [ ] Randomized fault pool — per-session random selection from a fault library (loose cable, wrong port, modem not fully synced, power cycling device).
- [ ] Timed stability validation — player must hold a correct connection state for N seconds before mission registers success.
- [ ] Dead-zone detection system — heat map shows uncovered areas; player uses diagnostics tablet to identify root cause.
- [ ] Extended diagnostics tablet — fault history panel added.
- [ ] Fault injection system (editor tool) — allows designers to author new faults without code.

---

## Content Added

- [ ] Mission 6: Intermittent Failure — playable.
- [ ] Mission 7: The Dead Zone — playable.

---

## UI / UX Changes

- [ ] Fault history panel on diagnostics tablet.
- [ ] Timed stability progress bar (shown during validation window).
- [ ] Heat map always-on mode for Mission 7.

---

## Audio / Visual Changes

- [ ] Device fault audio cue (intermittent disconnect sound).
- [ ] Stability validation countdown visual.
- [ ] Dead-zone red pulse on heat map cells with zero coverage.

---

## Bugs Fixed

- TBD at implementation time.

---

## Docs Updated

- [ ] Mission production docs for Missions 6–7 updated.
- [ ] `02_production/documentation/system_docs/fault_system.md` created.

---

## Known Issues

- TBD at implementation time.

---

## Testing Status

- QA plan to be created at `03_testing/qa_plans/v0.6.0_qa_plan.md` when implementation begins.

---

## Next Milestone Target

V0.7.0 — Alpha
Exit condition: All missions playable end to end; no critical systems missing.
