# AMSAT-Tracker
Build Repository for Sat Tracker CAD and 3D print files as well as Code
# Sat Tracker

A DIY two-axis (azimuth/elevation) motorized antenna rotator that autonomously points an Arrow II dual-band Yagi at low-Earth-orbit (LEO) amateur radio satellites during passes. Orbits come from Celestrak TLEs, are propagated with SGP4, and the rotator is driven over Hamlib `rotctld` (port 4533) so standard ham tracking software can also control it.

> **Status: work in progress.** Firmware has been through code review; the mechanical design is being revised around the components listed below. Verify anything marked "current" before building from it.

## Target satellites

SO-50, AO-91 (Fox-1B), AO-92, AO-27, FO-29, RS-44, AO-7, AO-73, ISS (NA1SS).

## System architecture

The project uses a split dual-ESP32 design:

| Unit | Role |
|---|---|
| **Ground station controller** (CrowPanel ESP32, 480×320 touch) | Touch UI, WiFi, satellite selection, TLE fetch, SGP4 pass prediction, Hamlib `rotctld` server |
| **Rotator controller** (ESP32-WROOM) | TMC2209 stepper drivers, AS5048A SPI encoders, NEO-6M GPS, MPU-6050 IMU, 100 Hz PID loop, Az/El LED displays |

The two units talk over UART using JSON packets. Motor control runs on Core 0 and UI/SGP4/WiFi on Core 1.

A simpler single-ESP32 variant (PCA9685 servo control, 4-line I²C LCD, rotary encoder) is kept as a more accessible build path.
<img width="749" height="571" alt="image" src="https://github.com/user-attachments/assets/e50c4b3c-6fa1-4067-b3b3-f5328f1fd057" />

## Hardware

**Electronics:** ESP32, Raspberry Pi Pico, Pi Zero, Arduino Uno; TMC2209 drivers; MPU-6050, QMC5883L, NEO-6M GPS, AS5048A encoders; HT16K33 AlphaNum4 displays.

**Mechanics (current choices, Oct 2026):**
- Azimuth drive: StepperOnline NEMA17 planetary-gearbox stepper (~5:1), with a self-locking worm-and-wheel stage under evaluation; 3D-printed spur reduction also under consideration. Slewing-ring pinion is the candidate reduction stage.
- Azimuth bearing: igus slewing ring, 100 mm OD (bolt pattern and tooth count being measured).
- Rotating joint: 8-channel 22 mm slip ring, with 100 Ω series resistors on I²C SDA/SCL to suppress brush noise.
- Elevation: MG996R servos (4-pack).
- Antenna: Arrow II (Arrow 146/437-10WBP), single linear polarization.

**Fabrication:** FDM 3D printing; mounts and gears are designed in CAD (Onshape, STEP exports kept in `cad/`).

## Repository layout

See [`docs/REPO_LAYOUT.md`](docs/REPO_LAYOUT.md) for conventions. In brief:

```
sat-tracker/
├── firmware/       # PlatformIO projects (ground station, rotator, simple variant)
├── cad/            # Source CAD (Onshape links/exports) and STEP/STL
├── electrical/     # Schematics, wiring tables, BOM
├── docs/           # Build guide, specs, this README's supporting docs
├── test/           # Bench test scripts, test logs, QC records
├── tools/          # TLE/pass-prediction helpers, calibration scripts
└── hardware/       # Printable drawings (PDF), mechanical dimensions
```

## Getting started

1. **Firmware:** open the relevant project in `firmware/` with PlatformIO (or Arduino IDE for the simple variant). Libraries are listed in each project's `platformio.ini`.
2. **Configuration:** copy `firmware/**/config.example.h` to `config.h` and set WiFi credentials and station grid locator. Do not commit `config.h`.
3. **Bench test first:** exercise each axis without the antenna attached, confirm encoder direction and limits, then run the PID loop.
4. **Calibrate:** set magnetic declination for your location (Swampscott, MA is about −14.2°) and verify the compass heading correction.
5. **TLEs:** refresh from Celestrak at least weekly. Stale TLEs produce negative elevations for every satellite.

## Known limitations

- AO-91 is solar-powered without battery margin, so eclipse passes produce no downlink.
- The Arrow II is linear-polarized only. Circular polarization for LEO work needs either two antennas at 90° with a phasing harness, or a purpose-built CP antenna.
- Pass prediction with AMSAT.org and Heavens-Above cannot be queried programmatically (server-side sessions); use their pages directly for cross-checks.

## Reference sources

- TLEs: [Celestrak](https://celestrak.org) (weekly updates)
- Pass prediction cross-check: [Heavens-Above](https://heavens-above.com) (AmateurSats), [hamsattracker.com](https://hamsattracker.com)
- Rotator control protocol: Hamlib `rotctld`

## Contributing and license

Placeholder: add license and contribution notes before making the repository public.
