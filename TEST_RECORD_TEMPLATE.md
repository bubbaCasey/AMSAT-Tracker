# Test Record: <short title>

Copy this template for each test run. Save the copy as `YYYY-MM-DD_<unit>_<short-description>.md` in the matching folder: `test/bench/`, `test/qc/`, or `test/field/`.

## Identification

- **Date:** YYYY-MM-DD
- **Time:** HH:MM (local)
- **Tester:**
- **Test type:** bench / qc / field
- **Unit under test:** (for example: az-drive, el-mount, ground-station, rotator firmware)
- **Hardware revision:** (CAD version, for example `el-mount_v3_2026-10`)
- **Firmware version:** (git tag or commit hash)
- **Related files:** (drawings, schematics, or docs this test depends on)

## Purpose

What this test is meant to show, in one or two sentences.

## Setup

- Power supply and voltage:
- Motor driver settings (microstepping, current limit):
- Encoder and sensor status:
- Antenna attached: yes / no (if yes, which)
- Environment (temperature, indoor/outdoor, wind for field tests):
- Other equipment:

## Procedure

Numbered steps, written so someone else could repeat them.

1.
2.
3.

## Measurements

| Parameter | Expected | Measured | Units | Tolerance | Pass / Fail |
|---|---|---|---|---|---|
| | | | | | |

## Observations

Anything unexpected, including noise, vibration, slipping, heat, odd readings, or anything that happened and wasn't planned. Record these even if they don't affect the result.

## Result

**Overall:** PASS / FAIL / PARTIAL

One or two sentences summarizing the outcome.

## Issues and follow-up

| # | Issue | Severity | Owner | Action | Status |
|---|---|---|---|---|---|
| 1 | | low / med / high | | | open |

## Sign-off

- **Recorded by:**
- **Reviewed by (if applicable):**
- **Date:**

---

### Field test additions

Use these fields in `test/field/` records in place of the sections above where they don't apply.

- **Target satellite(s):**
- **Pass start / peak / end (UTC):**
- **Maximum elevation:**
- **TLE source and date:**
- **Predicted vs. actual azimuth/elevation tracking error:**
- **Signal result (for example: decoded telemetry, S-meter reading, no signal):**
- **Weather and visibility:**
