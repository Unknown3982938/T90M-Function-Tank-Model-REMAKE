# 🛠️ T90M Functional Tank Model — REMAKE

### A semi-realistic, fully functional 1:15 tank model inspired by the T-90M platform. A ground-up remake of the earlier 1:25–1:35 build, with a proper turret ring, a torsion bar suspension, and a wireless multi-controller architecture.

---

# 📌 Overview

The REMAKE keeps the original idea (a working tank, not a display model) but fixes what the first build could not: a fixed scale, a real turret bearing, a larger volume, an independent suspension, and a turret that can rotate continuously with no wires crossing the ring.

Core behaviours being replicated:

- Tracked differential-steer drive
- Continuous 360° turret traverse on a ball-race ring gear
- Gun elevation and depression
- Gyro-based gun and turret stabilization
- Independent torsion bar suspension on all six road wheel stations

---

# ⚙️ Specifications

| Component | Details |
|---|---|
| Scale | 1:15 (fixed) |
| Hull (L × W × H) | 52.0 × 22.5 × 6.6 cm |
| Target mass | 2.5 – 3 kg |
| Drive motors | TBD (blueprint gear ratio 100:1) |
| Drive type | Dual track, differential steering |
| Road wheels | Ø 4.5 cm, 25 mm wide dual wheel, bearing sandwich (625, 5 × 16 × 5 mm) |
| Drive wheel / Idler | Ø 4.2 cm / Ø 4.0 cm |
| Suspension | 6 independent torsion bars, 5 mm ABS rod, 3 cm trailing arms |
| Torsion bar height | 0.75 cm above the hull floor |
| Ride angle | 30° at rest (arms set about 43° with no load, tuned to final mass) |
| Turret ring | Max Ø 120 mm, 88-tooth internal ring gear (module 1), 40 × 6 mm balls |
| Turret drive | N20 geared motor + DRV8833, 10-tooth pinion (8.8:1 on the ring) |
| Turret angle sensor | AS5600 (absolute, centre-mounted) |
| Turret lock | 3 quarter-turn lock pins (lock / remove for maintenance) |
| Cannon elevation | MG90S **or** MG996R servo (see options below), planned limits about −6° to +14° |
| Stabilizer | MPU6500 gyro, magnetometer planned for drift correction |
| Control | ESP-NOW across three ESP32 units (remote, hull, turret) |
| Power | Hull and turret each on their own battery and regulators |

### Cannon servo options (still deciding)

| | MG90S | MG996R |
|---|---|---|
| Weight | about 13 g | about 55 g |
| Torque | about 2 kg·cm | about 10 kg·cm |
| Best with | Light, balanced ABS barrel | Heavier barrel, gearing, or recoil parts |
| Supply | Own 5 V regulator | Own 5 – 6 V regulator, up to about 2.5 A when stalled |
| Trade-off | Light and compact, but little torque margin | Plenty of torque, but heavier and larger on the cradle |

Either way, the barrel should be balanced around the trunnion so the servo is not holding its weight.

---

# 🔧 Features

- 🔄 **Full mobility:** dual motor tracked drive
- 🎯 **Continuous turret traverse:** ring gear, ball race, no slip ring needed
- 🔒 **Removable turret:** quarter-turn lock pins hold the turret down, and unlocking lifts it off for maintenance
- 🔫 **Cannon actuation:** balanced, lightweight ABS barrel on an MG90S or MG996R servo
- ⚖️ **Stabilizer:** turret-local gyro loop, with absolute turret angle from the AS5600 and the MPU6500 for gun depression/elevation (experimental)
- 🛞 **Torsion bar suspension:** one bar per wheel station, fixed at its midpoint
- 📡 **Wireless architecture:** no wires crossing the turret ring
- 👁️ **IR jammer "eyes"** (visual feature, carried over from v1)
- 🧱 **Hybrid construction:** Sintra board, ABS, PETG (3D printed parts) and metal reinforcement

---

# 🧠 Design Philosophy

This project follows a function-first engineering mindset:

- Simplicity over unnecessary complexity
- Structural dependency for reliability
- Modular parts for easier repair and upgrades
- Use of salvaged and mixed materials to maximize resource efficiency

> «If a system only works because of added fixes → unstable design
> If a system fails when a critical part is removed → intentional design»

---

# 🏗️ Materials Used

- **PETG (3D printed):** turret ring, pinion, lock pins and feet, wheels, drive wheel, pinned track links
- **Sintra board (3 mm, double-laminated to 6 mm where needed):** main structural panels
- **ABS rod (5 mm):** torsion bars
- **Brass tube:** bushings in the wall holes **(optional)**
- **Steel (rebar tie wire, metal sheet, rods):** reinforcement and anchoring
- **Chipboard and cardboard:** lightweight structures, mainly for fine details with no required reinforcements

---

# 🚧 Project Status

**Phase: foundation (chassis and suspension).**

- [x] Blueprint sheet 01 (general specifications)
- [x] Torsion bar rods fitted in the hull (test layout)
- [x] Turret ring parts designed (ring, pinion, lock pins)
- [x] Final bar elevation and arm angles
- [ ] Cannon servo chosen (MG90S or MG996R)
- [ ] Suspension arms and road wheels
- [ ] Turret ring printed and fit-tested
- [ ] Electronics integration
- [ ] Stabilizer tuning

---

# 🏗️ Build Process

### 1. Chassis

- Layered Sintra board hull with reinforced panels
- Torsion bar holes lined with brass tube bushings

### 2. Suspension

- 5 mm ABS rod per station, midpoint fixed to the hull floor by a block on a base plate
- 3 cm trailing arms with a cross pin, plus bump and rebound stops
- Arms set about 43° with no load so the hull settles at the 30° ride angle

### 3. Road wheels

- Two 3 mm discs sandwiching a 625 bearing, built as dual wheels with a 7 mm guide gap
- One M5 bolt axle per station, with collars so only the bearing inner rings are clamped

### 4. Turret system

- Internal ring gear fixed to the hull, N20 pinion drive on the turret
- 6 mm ball race, with the turret held down by the lock pins

### 5. Cannon and stabilizer

- Balanced ABS barrel on an MG90S or MG996R servo
- MPU6500 on the turret ESP32, with its own control loop
- The servo is open loop, so the true gun angle needs a second MPU6500 or an AS5600 on the cradle (planned)

### 6. Electronics and control

- Three ESP32 units over ESP-NOW (remote, hull, turret), each with its own battery and regulators
- Turret traverse failsafe if packets stop for about 300 – 500 ms

---

# 🔌 System Architecture

```
[Remote ESP32] ──ESP-NOW──► [Hull ESP32]   → drive motors
       └─────ESP-NOW──────► [Turret ESP32] → N20 traverse, cannon servo,
                                             MPU6500, AS5600
```

- All three units share the same Wi-Fi channel
- The stabilizer loop runs locally on the turret, never over the radio
- Motor and servo supplies are kept separate from the logic supply

(Wiring diagram to be added.)

---

# 🧪 Testing & Performance

| Aspect | Status |
|---|---|
| Suspension (ride angle, travel) | Not yet tested |
| Turret traverse | Not yet tested |
| Turret lock and removal | Not yet tested |
| Stabilizer | Not yet tested |
| Mobility | Not yet tested |

---

# 🚧 Current Limitations

- ABS torsion bars can creep slowly under constant load
- Turret yaw lock drifts without the magnetometer
- Cannon angle is open loop until a cradle sensor is added
- Drive speed depends on the final motor and gear choice
- Track efficiency depends on alignment and tension

---

# 🔮 Future Improvements

- Magnetometer for yaw drift correction
- Closed-loop gun angle sensing (AS5600 or a second MPU6500 on the cradle)
- Damped bushings for the suspension
- Camera or FPV system
- Recoil system and smoother turret detail

---

# 📚 Documentation

| Item | Location |
|---|---|
| Drawing sheets (01 / 10 so far) | `docs/` |
| STL files | `cad/` |
| Firmware | `firmware/` |
| Wiring and electronics | `electronics/` |

---

# 📷 Media

*(Photos and renders to be added.)*

---

# 📊 Version Log

## v1.01.001 — Rev 02 (Zero Draft Unit, September 2026)

- Remake started at fixed 1:15 scale
- General specifications sheet drawn
- Torsion bar suspension designed and test-fitted
- Turret ring, pinion and lock pins designed

---

# 👤 Social Account links

(For broader coverage of updates on the model and future projects)

FB: [https://www.facebook.com/lawrence.tulali](https://www.facebook.com/lawrence.tulali)

TikTok: [https://www.tiktok.com/@philie_imation_001](https://www.tiktok.com/@philie_imation_001)

---

# 🤝 Contributions

This is a personal engineering project, but feedback, suggestions, and improvements are welcome.

---

# 📜 Licenses

This repository uses a separate license for each kind of content.

| Content | Folders | License |
|---|---|---|
| Hardware and documentation (CAD, STL files, drawings, wiring and electronics files) | `cad/`, `docs/`, `electronics/` | [CERN-OHL-P-2.0](HARDWARE-LICENSE) |
| Firmware and software (source code, libraries, scripts) | `firmware/` | [MIT](LICENSE) |

The CERN Open Hardware Licence Version 2 – Permissive (CERN-OHL-P-2.0) covers the hardware side. The MIT License covers the code, so it stays compatible with the usual software ecosystems.
