<div align="center">

<!-- Drop your Gemini banner here: save it as assets/banner.png -->
<img src="assets/banner.png" alt="Luxera One" width="100%"/>

# LUXERA ONE

### Normal glasses. Nothing normal inside.

**Standalone AR smart glasses in a thick-acetate frame — a monocular green MicroLED HUD, an always-available camera, a depth sensor, bone-conduction audio, and an on-device Arabic-first assistant. No phone tether. No visor. Just glasses that happen to contain a computer.**

![Status](https://img.shields.io/badge/status-schematic%20frozen%20v0.1-brightgreen)
![EDA](https://img.shields.io/badge/KiCad-9.0-blue)
![Fab](https://img.shields.io/badge/fab-JLCPCB%204--layer-orange)
![Compute](https://img.shields.io/badge/SoC-Rockchip%20RK3566-red)
![Made in](https://img.shields.io/badge/made%20in-Sharjah%2C%20UAE%20🇦🇪-black)
![Program](https://img.shields.io/badge/Hack%20Club-Stardance-yellow)

</div>

---

## ✦ What is this?

Luxera One is a from-scratch attempt to build what the big labs keep teasing: **AR glasses that look like actual glasses.** Everything — compute, batteries, optics, sensors, audio — seals inside a chunky acetate frame. Nothing clips on. Nothing sits in front of the lens.

This repository is the **hardware design**: KiCad schematics, PCB layouts, and the component library for every board in the device, built in public, one verified pin at a time.

| | |
|---|---|
| 🟢 **Monocular green HUD** | MicroLED engine + waveguide in the right lens — time, navigation, notifications, prayer times, live translation |
| 📷 **Duty-cycled vision** | Global-shutter camera in the front corner; low-power awareness pipeline, full capture on demand |
| 📡 **Depth sensing** | VL53L8CX 8×8 ToF for obstacle nudges + a toggleable low-vision assist mode *(an aid, not a certified medical device)* |
| 🦴 **Bone-conduction audio** | Ears stay open to the world; a voice-pickup sensor at the bridge hears only the wearer |
| 🧠 **On-device AI** | RK3566 quad-A55 + 0.8 TOPS NPU running LuxeraOS — nothing about how you move ever leaves the glasses |
| 🔋 **Two curved cells** | One per temple, paralleled — balanced weight, all-day light use |

## ✦ Architecture — three boards, one device

```
        ┌──────────────── FRONT (flex) ────────────────┐
        │  camera · ToF · 2× mics · voice pickup · ALS │
        └──────┬────────────────────────────────┬──────┘
          40-pin FFC                       24-pin FFC
        ┌──────┴───────────┐        ┌───────────┴──────┐
        │   CORE (right)   │        │    AUX (left)    │
        │ RK3566 · power   │        │ nRF52 always-on  │
        │ USB-C · charger  │        │ IMU + mag · BLE  │
        │ MicroLED DSI     │        │ antenna · cell   │
        │ 2× I²S amps      │        │ transducer · mic │
        │ touch · button   │        │                  │
        └──────────────────┘        └──────────────────┘
```

Two physical generations of the same circuit:

| | Version A — Bench | Version B — In-frame |
|---|---|---|
| Purpose | Prove the circuit on a desk | The product, inside the temples |
| Compute | RK3566 SoM on DF40 sockets | Bare RK3566 + LPDDR4 on 6-layer HDI |
| Boards | Flat 4-layer (Core 55×45 mm) | Thin curved rigid-flex (~95×6 mm) |
| Status | **🔨 in progress — this repo** | Designed, awaits Rev-B |

## ✦ Current state

- [x] Product brief, hardware design guide, master build guide (36 pp)
- [x] Component selection — **every part verified in stock** at JLCPCB/LCSC
- [x] **Core bench schematic — FROZEN v0.1** *(power chain, RK3566 socket, USB, console, audio, touch, camera/display lanes, inter-board FFC)*
- [ ] Core PCB layout (4-layer, 55×45 mm) ← **you are here**
- [ ] Aux + Front schematics & layouts
- [ ] Fab + assembly via JLCPCB · bring-up · LuxeraOS boot
- [ ] Rev-B: bare-SoC in-frame boards (professionally reviewed)

## ✦ Selected silicon

| Block | Part | LCSC |
|---|---|---|
| Compute (bench) | Luckfox Core3566 — RK3566, 4 GB, 32 GB eMMC, Wi-Fi | module |
| Module socket ×2 | Hirose DF40C-100DS-0.4V(51) | `C597931` |
| Charger | MCP73831T-2ACI/OT | `C424093` |
| Fuel gauge | MAX17048G+T10 | `C2682616` |
| 3.3 V buck | TPS62840DLCR | `C2071859` |
| 1.8 V LDO | TLV70018DDCR | `C79924` |
| 5 V boost | TPS61023DRLR | `C919459` |
| Audio amps ×2 | MAX98357AETE+T | `C910544` |
| Touch | Azoteq IQS227B (TSOT23-6) | `C3827640` |
| USB-C / ESD | TYPE-C-31-M-12 / USBLC6-2SC6 | `C165948` / `C7519` |
| Front link | AFC01-S40FCA-00 · 40P 0.5 mm FFC | `C262674` |

## ✦ Repository layout

```
luxera/
├── core/      Core board — brain, power, display, audio   (schematic frozen)
├── aux/       Aux board — always-on island, radio          (pending)
├── front/     Front flex — perception                      (pending)
└── lib/       Shared KiCad library (symbols · footprints · 3D)
```

**Open it:** KiCad ≥ 9.0 → open `core/core.kicad_pro`. The `lib/` library ships in-repo, so everything resolves out of the box. Parts were imported with [`easyeda2kicad`](https://github.com/uPesy/easyeda2kicad.py) straight from LCSC IDs.

## ✦ The story

Designed and built by **[Mohamad Nihad Alsufe](https://nihad.codes)** — 16, Syrian, based in Sharjah 🇦🇪 — as part of **Hack Club Stardance**. Schematic drawn from a blank KiCad install to frozen in a single day of pin-by-pin, datasheet-verified work. Follow the devlogs on the [Stardance project page](https://stardance.hackclub.com/projects/27774).

> ⚠️ **Honest notes:** this is a live work-in-progress, not a kit — build at your own risk. Li-ion cells demand respect. The low-vision assist is marketed strictly as an aid and never as a replacement for a cane, guide dog, or certified mobility device.

## ✦ License

Hardware sources released under **CERN-OHL-S v2** (see `LICENSE`). Product name, logo, and industrial design remain © Mohamad Nihad Alsufe.

<div align="center">

**نور** · *built to be worn, not carried*

</div>
