# Repository Layout and Conventions

This document explains where things live in the Sat Tracker repository and the rules for adding to it. Read it before adding files.

## Top-level folders

| Folder | Put here | Don't put here |
|---|---|---|
| `firmware/` | PlatformIO projects, one per unit (`ground-station/`, `rotator/`, `simple-variant/`) | Compiled binaries, personal `config.h` |
| `cad/` | Source CAD links or exports, STEP files, STL print files, grouped by assembly (`az-drive/`, `el-mount/`, `base-plate/`) | Rendered images (put those in `docs/`) |
| `drawings/` | Dimensioned PDF drawings for printing and review | Working CAD files |
| `electrical/` | Schematics, wiring tables, `bom.csv` | Firmware pin headers (keep those with the firmware) |
| `test/bench/` | Per-axis bench procedures and logs | Field results |
| `test/qc/` | Print tolerance checks, fit checks, sign-off records | Design notes |
| `test/field/` | On-sky pass logs and tracking results | Bench results |
| `docs/` | Build guide, calibration notes, learnings, this document | Anything that must stay in sync with code without a note on which version it matches |
| `tools/` | TLE fetch, pass prediction, calibration scripts | Firmware code |
| `archive/` | Superseded designs and firmware, kept with a short note on why they were replaced | Anything in active use |

## Naming

- **Versioned files:** `<part>_v<N>_<YYYY-MM>.<ext>`, for example `el-mount_v3_2026-10.step`. The version and date are what let a printed part be traced to a model.
- **Folders:** lowercase with hyphens (`az-drive`, not `AzDrive`).
- **Test records:** `<YYYY-MM-DD>_<unit>_<short-description>.md`, for example `2026-10-28_az-drive_backlash-check.md`.
- **Firmware versions:** tag releases in git (`v0.3.0`) and note the matching hardware revision in the release message.

## Rules

1. **Superseded means archived, not deleted.** Move the old file to `archive/` and add a line to its name or a sibling `README` saying what replaced it and why.
2. **Secrets stay out of git.** Commit `config.example.h`; keep `config.h` in `.gitignore`.
3. **Every printed part has a matching drawing version.** If a CAD file changes, update the drawing in `drawings/` or mark it stale.
4. **Test records are written when the test is run,** not reconstructed later.
5. **Units are stated.** Dimensions in mm unless noted; angles in degrees; frequencies in MHz.
6. **Measured values beat assumed values.** When you measure something (bolt pattern, tooth count, backlash), record the measurement in `docs/` and link it from the drawing it affects.

## Where to start

- New to the project: `README.md`, then `docs/`.
- Building the rotator: `electrical/`, then `cad/`, then `firmware/`, with `test/bench/` alongside.
- Checking whether a part fits: `drawings/` and `test/qc/`.
- Checking whether tracking works: `test/field/` and `docs/learnings.md`.
