---
title: "Heathkit SB-102 Transceiver — Restoration Plan"
subtitle: "Rev. 2 — checked against the original Heath manual (595-1058-06)"
date: "October 2026"
geometry: margin=0.9in
fontsize: 10pt
colorlinks: true
toc: true
---

<!-- Build: pandoc sb102-restoration-plan.md -o sb102-restoration-plan.pdf --pdf-engine=xelatex -V mainfont="DejaVu Serif" -->

> **Safety (manual p. 100).** "Lethal voltages are present both above and below the chassis." The HP-23 delivers about **+800 V** (meter reads +800 to +900 V on HV) to the finals, and that voltage stays on the supply filter capacitors after unplugging. Ground the chassis ground bolt to a good earth ground and to all test equipment. Discharge before every hands-in session; never run the final cage open; work one-handed when live. Use a **50 Ω non-reactive dummy load rated for 100 W, never light bulbs** (p. 101). **Never pull a tube or pilot lamp with power on.** The series-parallel filament string can take out other tubes (p. 122).

# 1. Source documents

| Document | Where | Use |
|---|---|---|
| **Heath SB-102 Assembly Manual**, 595-1058-06 (©1970) | W5RKL (w5rkl.com/manuals) | **Primary reference.** Page numbers below are the manual's printed pages. |
| SB-102 solid-state LMO schematic | archive.org (K6KJN upload) | LMO internals (the manual omits them). |
| **SB-102 Service Bulletins** -1 … -30 (1970–84) | Heath list (puck.nether.net), Aug 1997 / Apr 1998 | Factory fixes; many refer to SB-100, SB-101 and HW-100/101 bulletins. |
| W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting* | w5rkl.com | Field practice. |
| K5BCQ SB-101/102 repair site | qsl.net/k5bcq | Leakage limits, IF centering, dial mechanics. |
| Nostalgic Kits Central SB-10x troubleshooting | nostalgickitscentral.com | Measured SB-102 resistor drift and electrolytic leakage. |
| W2PA SB-101 rebuild notes | sites.google.com/site/radiow2pa | LMO connector, bandswitch rotors, driver neutralizing. |
| Heath list / Heathkit-Radios / heathkit (groups.io) | puck.nether.net, groups.io | Field experience. |
| **HP-23A AC Power Supply assembly manual** 595-924-01 (©1968) | Supplied by owner | HP-23A section checked against it (Rev. 2.2, §3.6.1). |
| **PS-23 AC Power Supply manual** 595-2314 (©1979) | Supplied by owner | Supply section checked against it (Rev. 2.1, §3.6.2). |

**Designator groups (p. 135):**

| Range | Board |
|---|---|
| 0–99 | Modulator |
| 100–199 | IF |
| 200–299 | Bandpass |
| 300–399 | Audio |
| 400–499 | RF-driver |
| 500–599 | Crystal |
| 600–699 | Heterodyne oscillator |
| 700–799 | Driver grid |
| 800–899 | Driver plate |
| 900–999 | Chassis |

Resistors are 1/2 W unless marked. Voltage charts are measured with an 11 MΩ VTVM, ±20% (Figs. 1-14A/B).

# 2. Transceiver at a glance

## 2.1 Factory specifications (pp. 131–134)

| Item | Spec |
|---|---|
| Coverage | 3.5–4.0, 7.0–7.3, 14.0–14.5, 21.0–21.5, 28.0–30.0 MHz (eight 500 kHz segments) |
| Sensitivity | **< 0.35 µV** for 10 dB (S+N)/N, SSB |
| Selectivity | SSB 2.1 kHz @ 6 dB / 5 kHz @ 60 dB (3.395 MHz filter 404-283). CW 400 Hz / 2.0 kHz with optional **SBA-301-2**. |
| Audio | 2 W, < 10% distortion; 8 Ω speaker and high-Z phones |
| Input / output | 180 W PEP SSB, 170 W CW input; **100 W out on 80–15 m, 80 W on 10 m** |
| Carrier / sideband suppression | 50 dB / 55 dB |
| Stability | < 100 Hz/hr after **10 min**; < 100 Hz for ±10% line |
| Dial | Visual within 200 Hz; **electrical within 400 Hz after calibration at the nearest 100 kHz point**; backlash < 50 Hz |
| ALC | Heath **TALC** (triple-action level control) |
| **Power required** | **700–850 V @ 250 mA** (1% ripple); **300 V @ 150 mA**; **−115 V @ 10 mA**; **12 V AC/DC @ 4.76 A** |
| Size / weight | 14-7/8 × 6-5/8 × 13-3/8 in.; 17.5 lb |
| Built | 1970–1975, $380 |

## 2.2 Tube complement (spec p. 133; V numbers from the circuit description)

| V | Tube | Function | Known issues (source) |
|----|----------|--------------------|----------------------------------------------|
| V1 | 6EA8 | Speech amp (A) / cathode follower (B) | GE 6EA8 oscillates → VOX chatter (SB-101-19). |
| V2 | 6AU6 | Isolation amp | — |
| V3 | 6AU6 | 1st IF amp (also the S-meter/ALC bridge) | **Brand-critical**: GE only (SB-102-24, -30). Leaky → S-meter drift (SB-101-22; chart 18E). |
| V4 | 6AU6 | 2nd IF amp | Same as V3. |
| V5 | 6EA8 | 1st transmit mixer (A) / crystal oscillator (B) | No LMO signal at pin 7 → chart 25C. |
| V6 | 6CB6 | 2nd transmit mixer | Under shield. |
| V7 | 6CL6 | Driver | Under shield. |
| V8, V9 | 6146 | Finals, class AB1 | Gassy → grid drive falls off (chart 44D). Tung-Sol vs. RCA alters drive (SB-102-3). |
| V10 | **6HS6** | Receiver RF amp | Leaky → poor AGC (SB-102-15). A 6AU6 substitutes in an emergency. |
| V11 | **6AU6** | 1st receive mixer | **Confirmed 6AU6** (Fig. 2-22, p. 151). |
| V12 | 6EA8 | 2nd receive mixer (A) / relay amp (B) | Bad V12B → relays won't energize (chart 37A) or stay in (SB-102-14). |
| V13 | 6BN8 | AVC (A/B) / product detector (C) | Faulty → no audio (chart 13C). |
| V14 | 6GW8 | Audio amp (A) / audio power amp (B) | C304 is its B+ filter. |
| V15 | 6EA8 | CW tone osc (A) / tone amp (B) | Not operating → won't key on CW (chart 33A). |
| V16 | 12AU7 | Carrier oscillators: A = USB (Y1 3396.4 kHz); B = LSB (Y2 3393.6) / CW (Y3 3395.4) | Plate resistors R6/R7 33 kΩ (chart 31E). |
| V17 | 12AT7 | VOX amp (A) / 100 kHz calibrator (B) | Defective → VOX won't trip (chart 40C). |
| V18 | OA2 | +150 V regulator | Bad → no regulated B+ (chart 5B). |
| V19 | 12AT7 | Heterodyne osc (A) / cathode follower (B) | **Wrong type → S-meter zero problems** (chart 18H). R212 change (SB-102-2). |

The spec list totals **19 tubes**; the introduction (p. 2) says "twenty tubes, four transistors, eighteen diodes", counting the LMO's internal parts. The LMO itself is transistorized; there is no VFO buffer tube.

## 2.3 Semiconductors (spec p. 133; parts lists pp. 4, 23)

| Ref | Type (Heath p/n) | Function | Notes |
|------|------------------|----------------------|------------------------------------|
| CR1–CR4 | 1N191 Ge (56-26-1) | Balanced modulator | Wrong type or failed → no SSB output (chart 28C). SB-102-28 is a germanium diode change. |
| CR201 | 1N191 Ge | Calibrator harmonic generator | — |
| CR901 | 1N191 Ge | REL PWR sampling | — |
| D1, D2 | Silicon 750 mA 500 PIV (57-27) | Anti-trip rectifiers | Reversed → relays stay in (chart 41B). |
| D101 | 57-27 | AVC gate | **Defective → slow recovery from transmit** (chart 43). |
| D106 | 57-27 | **LMO supply rectifier** (12.6 VAC → DC) | — |
| D201 | 57-27 | VOX rectifier | Leaky → poor VOX (SB-101-4); reversed → no VOX (chart 40D). |
| **D202** | **15 V zener, 1 W, 1N4166A** (56-25) | Relay amp V12B cathode | Reversed or bad → relays won't trip (chart 40E). |
| D301 | 57-27 | Transmit mixer/driver cutoff bias | — |
| D902, D903 | 57-27 | **ALC voltage doubler** | Leaky → ALC falls off after warm-up (SB-101-4). |
| D904 | 57-27 | CW keying | Shorted or open → won't key CW (chart 33C). |
| D905 | 57-27 | RF-gain/AVC diode gate | Leaky → slow recovery (SB-101-4). |
| **D907** | **10 V zener** (56-67) | **LMO +10 V regulator** | **Reversed → no 10 V to the LMO** (chart 39B). |
| D909 | 1N4149 (56-56) | Receive/transmit C955 switch | — |
| **Q1** | **2N3567** (417-114) | **LMO on/off switch**, not a regulator | Open → no LMO 10 V (chart 39C); shorted → no injection (chart 34E). |

## 2.4 LMO and its power supply (pp. 142–143)

- **LMO 110-48** is a sealed, factory-calibrated unit containing a capacity-tuned silicon transistor oscillator and a transistor bandpass amplifier. It tunes 5.0–5.5 MHz at 100 kHz per shaft turn.
  - A diode switch shifts it 2.8 kHz between LSB and USB/CW; the **LSB ADJUST** screw is on top.
  - It has an unused **FSK terminal** that gives up to about 1 kHz shift (RTTY needs a fan and reduced screen current, p. 116).
  - **"At no time should the LMO be opened or the warranty will be voided"** (p. 122).
- **Supply path:**
  1. 12.6 VAC filament → **D106** rectifier → **C941, C942 (500 µF each, 25-199)** and **R936** filter.
  2. → **pilot lamp PL1 (#1815) as a voltage-dropping element** → **D907 10 V zener** → about **+10 V**.
  3. For mobile operation, a jumper between pins 6 and 8 of the power socket feeds 12.6 VDC straight to PL1, bypassing D106.
- **Q1 2N3567** turns the internal LMO on when +150 V reaches its base through **R941 (68 kΩ)** (R939 47 kΩ to ground). In the AUX positions, the +150 V goes to the crystal oscillator V5B instead.
- **R928 (56 Ω)** terminates the FREQ CONTROL/LMO output line. It is **not** in the supply path.
- At first power-up, **"the lamp in the LMO power supply should glow dimly"** (p. 100).
- **Chart 39, no 10 V to the LMO:**
  - A. voltage-dropping lamp open;
  - B. zener reversed;
  - C. Q1 open;
  - D. mobile jumper missing.

## 2.5 Pilot lamps and supply

- **Lamps (parts list p. 24):** one **#47** and three **#1815** (one #1815 is PL1 in the LMO supply). They are part of a series-parallel filament arrangement; wrong lamps unbalance it (chart 4B). Panel lamps should light with equal brilliance (p. 100).
- **Supply (pp. 98–99):** HP-23/23A (117 VAC) or HP-13/13A (12 VDC).
  - The HP-23A must be on its **+300 V** connection: AC OFF switch **right (350 VDC no-load)** (§3.6.1).
  - **With the HP-23A, the supply's BIAS control is inoperative; bias is set by the transceiver's BIAS ADJUST.** The rig uses the HP-23A's fixed −130 V on pin 1; the BIAS control only feeds pin 11 (§3.6.1).
  - With the HP-13A, set the supply's bias control fully clockwise.
- **11-pin plug** (SB-102 / transceiver end; the HP-23A's supply-end socket differs, §3.6.1):

| Pin | Function |
|---|---|
| 1 | Bias −130 V |
| 2 | Ground (filament) |
| 3 | LV +300 V |
| 4 | HV +800 V |
| 5, 11 | N.O. relay (for a linear amplifier) |
| 6 | Filament 12 VAC |
| 7 | Ground (B− and bias) |
| 8 | N.C. (mobile jumper to 6) |
| 9, 10 | Supply switch |

- SB-100, SB-101 and HW-100 sockets need any wire on pin 8 removed.
- **The 9-terminal jumper plug (screen supply) must be on its chassis connector** or there is no transmit (p. 100).

# 3. What fails, ranked

## 3.1 Capacitors

**All seven electrolytics** (parts lists pp. 4, 23; one restorer found most leaky). The parts lists give no voltage ratings; the ratings below are safe replacements.

| Ref | Value (p/n) | Role (manual figure) | Replace with |
|---|---|---|---|
| **C941, C942** | **500 µF** (25-199), chassis | **LMO supply filter** (p. 143) | 470–500 µF **≥ 35 V** (kits use 50 V) |
| C12 | 20 µF tubular (25-135), modulator | Speech-amp B+ filter, V1B (Fig. 2-6) | 22 µF 450 V |
| **C304** | 20 µF tubular (25-135), audio | Audio output B+ filter, V14 (Fig. 2-26) | 22 µF 450 V. **Shorted or reversed → no audio** (chart 10D). One measured 3.6 mA leakage (0.5 mA allowed). |
| C2 | 10 µF (25-115), modulator | V1A cathode bypass (Fig. 2-6) | 10 µF 50 V |
| VOX cathode bypass | 10 µF (25-115), bandpass | V17A cathode (Fig. 2-3; field sources call it **C212**) | 10 µF 50 V |
| (chassis 10 µF) | 10 µF (25-147) | Chassis; field kits call it **C945**, bias filter | Confirm position and polarity on the schematic before removal; bias is a **negative** supply |

**Other capacitors:**

| Part | Likelihood | Symptom (manual ref) | Action |
|---|---|---|---|
| **.2 µF resin (27-34)**: C213 VOX delay, C25 anti-trip, C3 and others | Medium | VOX delay drift (SB-100-3); poor anti-trip | Replace with film (kit: 0.22 µF). |
| .05 µF tubular (23-59), 1 µF tubular (27-19) | Medium | Leakage | Film replacements. |
| C4–C8, C16, C17 (carrier oscillator) | Low–medium | No carrier on LSB/USB (chart 31D) | Measure. |
| C208, C223, C604 (heterodyne osc) | Low–medium | No heterodyne injection (chart 36D, 36G) | Measure. |
| C111, C402 | Low | No first-mixer output (chart 25F) | Measure. |
| C317 | Low | Won't key CW (chart 33B) | Measure. |
| C14 carrier-null capacitor; C18 | — | Null at a rotor extreme | **p. 104:** if it nulls fully meshed toward the LMO, remove the 12 pF mica at 18; if meshed away from the LMO, replace it with the 24 pF. Also SB-102-18. |
| IF transformer internal micas | Low–medium | Corroded → value change | Measure (K5BCQ). |

Leakage limits (K5BCQ): < 0.01 µF ≤ 0.5 µA; 0.01–1 µF ≤ 1 µA (aged). Electrolytics: I ≤ kC + 0.3 mA, with k = 0.02 (101–250 V) or 0.035 (251–350 V).

## 3.2 Resistors

**Measured drift in one SB-102** (Nostalgic Kits Central; 15 replaced): 100 kΩ resistors at 115 k–**851 k**; 150 kΩ at 192 k / **open**; 330 kΩ → 463 k; 22 kΩ → 42 k; 3.3 kΩ → 5.5 k; 68 kΩ → 92 k.

| Ref | Value (manual figure) | Failure / symptom (manual chart) | Fix |
|---|---|---|---|
| **R106** | **22 kΩ 1 W**, V3 screen (Fig. 2-28) | Heats and drifts → S-meter zero wander (chart 18D) | 22 kΩ 2 W wirewound or 33 kΩ 2 W (field). |
| **R107** | **100 kΩ** (Fig. 2-28) | S-meter drift (chart 18D) | 100 kΩ 1 W (SB-101-9); SB-101-1 changed it to 82 kΩ, so check which is fitted. |
| R103, R105, R110, R115 | 100 kΩ, **56 Ω**, 330 Ω, 150 Ω | S-meter won't zero or shifts (chart 18D) | Measure. *(R104 and R105 are **56 Ω** in this manual, not 47 Ω as in the HW-101.)* |
| **R6, R7** | **33 kΩ**, V16 plates (Fig. 2-7) | No LSB/USB carrier (chart 31E, which also lists R8, R9, R11) | 33 kΩ 1 W (kit). |
| **R116** | **4700 Ω** (Fig. 2-25, product detector) | — | Kit value 4.7 kΩ confirmed. |
| **R212** | **220 Ω** (Fig. 2-14) | Heterodyne-oscillator tube failures | 330 Ω 1/2 W (SB-102-2). |
| R117 | 3300 Ω (Fig. 2-24, AVC) | Open → fast AVC decay | Replace. |
| R304, R305 | (regulator) | No or wrong +150 V (chart 5C, 6B) | Measure. |
| R307 | — | LMO won't shift with mode (chart 35C) | Measure. |
| R18, R19, R23, R24, R937, R938 | — | No isolation-amp output (chart 27B) | Measure. |
| R915 | 3.3 MΩ (ALC) | Open → ALC upset | Replace. |
| R936 | — | LMO supply filter resistor | Measure. |
| R940 | 100 Ω (driver plate B+) | Burned | Replace; dress the lead from the shield. |
| R216, R413 etc. | — | — | Survey. |

Precision **R921 / R942 (500 kΩ 1%)** are the HV meter multiplier. Replace them only with 1% parts.

Lift one lead to measure, because parallel paths mislead.

## 3.3 Tubes, by risk

1. **6AU6 at V3/V4:** brand matters; use GE (SB-102-24, -30).
2. **6HS6 V10:** leakage.
3. **6146 pair:** gassy tubes reduce grid drive (chart 44D); brand affects drive.
4. **6EA8 (V1, V5, V12, V15)** and **12AT7 (V17, V19).** V19 must be a 12AT7 (chart 18H).
5. **OA2 V18.**

Use a tester with a **grid-leakage** test. Put each tube back in its original socket.

## 3.4 LMO

| Symptom | Cause | Fix (source) |
|---|---|---|
| **LMO dead / no injection** | **PL1 #1815 dropping lamp open**, D907 reversed, Q1 open, mobile jumper (chart 38–39); FREQ CONTROL switch position or its violet/orange/white coax (chart 34) | Check for about +10 V at the LMO. Replace the lamp (keep spares), recap C941/C942, check D106, D907, Q1, R936. |
| **Instability, jumps** | Chassis stress at the mounting lugs | Replace lockwashers with #6 flat washers and **hex lock nuts**; tighten criss-cross, one turn at a time (SB-102-4). |
| Jumps when the cable moves | Dirty LMO phono jack/plug | DeoxIT (SB-101-16; W2PA). |
| **Dial off between 100 kHz points** | Spec is within **400 Hz after calibration at the nearest 100 kHz point** (p. 132). K9STH measured 3–10 kHz on five aged units. | Recalibrate at the nearest point. Larger errors are out of spec; Heath's remedy was LMO exchange. |
| Zero setting changes band to band | **Calibrator not exactly on 100 kHz** (chart 45) | Re-zero the CAL XTAL trimmer against WWV. |
| Wrong USB/LSB shift | LSB ADJUST; R307; mode wafer 1F (chart 35) | LSB ADJUST procedure (§4.7). |
| Stiff tuning, slipping knob | Dial drive (100-450 assembly), rubber belts 268-7 ×3 | Clean; set roller tension (K5BCQ, W2PA). The kit supplied silicone grease (352-13). |

Do not open the LMO casually. If it is defective, the manual says to return it (chart 34F).

## 3.5 Mechanical, switches, relays

| Item | Symptom | Fix (source) |
|---|---|---|
| **Switch-wafer rotors 180° out** | Dead bands, no injection, no final output | Chart 21G (driver plate board), 36I (crystal/heterodyne boards); W2PA found 3 of 4 wrong. |
| **SSB/CW FILTER switch** | Left on CW without an SBA-301-2 → no RF output, unintelligible receive | Chart 23A, 26B; clean the switch (SB-102-25). |
| **FREQ CONTROL switch** | Wrong position → no LMO or no output | Use **LOCKED NORMAL** for normal operation (chart 14M, 23B, 34B). |
| RL1, RL2 wiring and contacts | Many chart entries (8, 10G/H, 19, 20, 21C) | Clean the contacts (manual says relays should be trouble-free; field: RL2 carbon → chirp, SB-102-21). |
| **Board mounting hardware and lockwashers** | Receiver birdies or oscillation; transmitter instability | Lockwashers on the foil side, tight (chart 16A, 42C; p. 122). Steel comb brackets (SB-102-9/-10). |
| Final/loading cap hardware, coil cover, tube-shield clips | Transmit instability | Chart 42B, G, H. |
| Low 40 m output | Ground routing | SB-102-5. |
| Preselector drive | Poor tracking | Belts, bearings, cap 26-122 (SB-102-17). |
| Loading cap turns with plate | — | Grommet 73-3 (SB-102-20). |
| Final knobs or shafts loose | No RF | Chart 21H. |

## 3.6 HP-23 power supply (do first)

- Recap the high-voltage section (C1–C4 → 150 µF 500 V).
- Recap C5 (40 µF B+). An open C5 causes a CW tone and relay chatter (SB-102-29 symptom).
- Recap the bias caps C6/C7 (**20 µF in the HP-23A**, 40 µF in the PS-23).
- Replace the bleeders (100 kΩ 2 W) and diodes (1N4007).
- Fit a grounded cord.
- **+300 V B+ connection** (HP-23A switch right; PS-23 350 VDC wiring).
- Must meet the spec: **700–850 V @ 250 mA, 300 V @ 150 mA, −115 V @ 10 mA, 12 V @ 4.76 A**.
- Reform or ramp on a variac over 5–10 min.

### 3.6.1 HP-23A supply (checked against the HP-23A manual, 595-924-01, ©1968)

Page numbers are the HP-23A manual's printed pages.

**How the HP-23A works:**

- **The front-panel AC OFF switch is also the B+ selector** (DPDT, 61-12). **Right = 350 VDC** (high tap), **left = 275 VDC** (low tap), **center = OFF** (p. 14). Plate and filament voltages switch together.
  - The field "300/OFF/250" labels are the **loaded** values of the same two taps.
  - With remote switching wired (pins 9–10 to the equipment's switch), the panel switch still selects B+, and the equipment's switch turns the supply on and off.
  - The switch is easy to bump into OFF, or through it to the other tap.
- **Two bias outputs** (pp. 13, 16, 17):
  - **fixed −130 V** (−100 V at 20 mA) on **pin 1**;
  - **adjustable −40 to −80 V** (1 mA max) on **pin 11**, set by BIAS ADJUST R9.
  
  When the cable is made up, its green wire goes to pin 1 *or* pin 11 (p. 12). These transceivers take the **fixed −130 V on pin 1** and set final bias with their own BIAS control. **That is why the rig manuals call the HP-23A's BIAS control inoperative: R9 only feeds pin 11.**
- **Filaments:** 12.6 VAC @ 5.5 A on **pin 6**, or 6.3 VAC @ 11 A on pin 8. The cable's red wire goes to one or the other (p. 12). These rigs use **pin 6 (12.6 V)**.
- **Primary protection is a circuit breaker** (65-17; the manual doesn't give its rating), not a fuse. Line: 120/240 VAC, 50/60 Hz, 350 W max (p. 16).
- The line cord (89-19) is listed without a conductor count. **[field]** Fit an 18/3 grounded cord.
- The HP-23A manual doesn't name specific transceivers. It says connection instructions come with the Heath equipment (p. 13).

**SB-102: switch RIGHT (350 VDC).** The SB-102 needs **300 V @ 150 mA** (spec), which is the HP-23A's high tap under load. Its initial test expects 800–900 V on HV (p. 100), matching the HP-23A's 820 V no-load figure. Make up the cable per **Fig. 1-5 (p. 99)**, with **green on pin 1** (fixed −130 V) and **red on pin 6** (12.6 V). The SB-102's own plug table (§2.5) is the **transceiver end**; pins 5, 8 and 11 differ from the HP-23A's supply-end socket below.

**Specifications (p. 16):**

| Output | No load | Loaded | Ripple / duty |
|----------------------|----------|----------------------------------|------------------------------------|
| HV (pin 4) | 820 V | 700 V @ 250 mA (62.5 µF effective) | < 1% @ 250 mA; continuous to 150 mA, 50% @ 300 mA |
| LV high tap, switch **right** (pin 3) | 350 V | 300 V @ 150 mA (with 100 mA on HV) | < 0.05%; continuous to 175 mA |
| LV low tap, switch **left** (pin 3) | 275 V | 250 V @ 100 mA (with 100 mA on HV) | < 0.05%; continuous to 175 mA |
| Fixed bias (pin 1) | −130 V | −100 V @ 20 mA | — |
| Adjustable bias (pin 11) | −40 to −80 V | 1 mA max | — |
| Filament (pin 6 / pin 8) | — | 12.6 VAC @ 5.5 A / 6.3 VAC @ 11 A | — |

**Supply-end socket pinout (schematic; cable wiring p. 12; Table 1 p. 13):**

| Pin | Cable wire | Function |
|----|-----------|--------------------------------------------------------------|
| 1 | Green (fixed option) | −130 V fixed bias |
| 2 | Black | Filament common |
| 3 | Orange | LV +350 V or +275 V (no load), per the switch |
| 4 | Yellow | HV +820 V (no load) |
| 5 | — | No connection |
| 6 | Red (12 V option) | 12.6 VAC filament |
| 7 | White | Ground |
| 8 | Red (6 V option) | 6.3 VAC filament |
| 9, 10 | Brown, blue | AC switch loop in the equipment. If the equipment can't switch AC, **jumper pins 9–10 at the plug** and leave brown and blue unconnected (p. 12). |
| 11 | Green (adjustable option) | −40 to −80 V adjustable bias |

**Recap list, with the HP-23A manual's values (parts list p. 3; price list p. 19; schematic; circuit description p. 17):**

| Ref | Manual value (Heath p/n) | Function | Notes |
|------------|----------------------|------------------------------|----------------------------------------|
| C1, C2 | 125 µF tab-mount (25-34) | HV full-wave voltage doubler, from about 282 VAC | Replace together. The voltage rating isn't in the parts list; **[field]** use 500 V (the PS-23 rates the same position 500 V), or 150 µF 500 V. |
| C3, C4 | 125 µF tab-mount (25-34) | LV half-wave doubler from the 125/95 VAC taps; C4 is also the pi-filter input | Same as C1/C2. |
| C5 | 40 µF (25-36) | LV pi-filter output, with choke **L1 6 H** (46-22) | **[field]** 450 V. An open C5 causes a CW tone and relay chatter (HW-101-73). |
| C6, C7 | **20 µF** (25-19) | Bias filter, with R7 | **Negative supply: the + lead goes to ground.** Rating not printed; **[field]** 160 V+. *(PS-23 and Latta's field list: 40 µF.)* |
| R1–R4 | 100 kΩ 2 W (1-24-2) | HV bleeders / equalizers across C1, C2 | Drift high (field: ~150 kΩ). |
| R5 | 100 kΩ 2 W | LV bleeder | — |
| R7 | 1 kΩ 2 W (1-15-2) | Bias filter series | Chart: no bias → D7, R7, R8 or R9. |
| R8, R10 | 10 kΩ 1 W (1-9-1) | Limit the BIAS ADJUST range | *(The PS-23's R8 is a 27 kΩ bias bleeder; the HP-23A has R8/R9/R10 instead.)* |
| R9 | 10 kΩ tab-mount control (10-57) | BIAS ADJUST (pin 11 only) | Pin 11 resistance changes with its setting. |
| D1–D7 | Silicon diode (57-27) | D1–D4 HV doubler; D5, D6 LV; D7 bias half-wave | Type not named in the HP-23A manual (field: 1N2071). **[field]** 1N4007. |
| Breaker | 65-17 | Primary | Opens on reset → short in the transformer, rectifier/filter or load, or overload. |

**Initial test (pp. 12–13).** Do this **before** the first power-on and before the equipment connector is fitted.

1. Ohmmeter on R × 1: find the lead polarity. About **10 Ω** between terminal strip **J** lugs 3 (−) and 4 (+). Put the correct lead on socket **pin 7**.
2. Switch to R × 10k. Expect:
   - **pin 1 ≥ 22 kΩ**;
   - **pins 3 and 4 ≥ 75 kΩ** (the needle creeps up as the caps charge);
   - **pin 7 = 0 Ω**;
   - **pin 11 = 10–20 kΩ**, varying with BIAS ADJUST;
   - pins 2, 5, 6, 8, 9 and 10 infinite.

**Troubleshooting chart (p. 15):**

| Difficulty | Possible cause |
|---------------------------|------------------------------------------------------------|
| Completely dead | Breaker open, switch, pin 9/10 wiring, the equipment's remote switch, transformer |
| Breaker opens when reset | Short in transformer windings, rectifier/filter, or the equipment; overload |
| No HV | D1–D4; pin 4 wiring; transformer |
| No LV | D5/D6 open; L1 open; pin 3 wiring; switch; diode or filter-cap polarity reversed |
| No bias | D7 open; R7, R8 or R9 open; pin 1/11 wiring |
| HV half with no load, lower under load | D1, D2, D3 or D4 open |
| LV half | D5 or D6 open |
| All outputs half | Primary wired for 240 V on a 120 V line |

**Caution (p. 13).** The outputs are lethal, and the big capacitors keep their charge.

- Let the bleeders discharge the caps. The manual allows shorting the cap lugs to chassis with a screwdriver blade; a **bleeder stick (resistor to ground)** is gentler.
- Stand on dry insulating material and keep one hand behind your back.
- **Never work on the supply with the line cord plugged in.**
- The manual's voltages are no-load readings with an 11 MΩ VTVM (p. 14).

### 3.6.2 PS-23 supply (checked against the PS-23 manual, 595-2314, ©1979)

The PS-23 is the later (1979) kit supply in the HP-23 family. Page numbers are the PS-23 manual's printed pages.

**How the PS-23 differs from the HP-23/HP-23A:**

- **There is no front-panel B+ switch.** The low-voltage tap is chosen by an internal jumper at assembly: **350 VDC wiring** (Detail 5B, p. 10) or **275 VDC wiring** (Detail 5C, p. 11). On the schematic (illustration booklet p. 4) the brown transformer lead feeds 350 VDC and the brown-yellow lead feeds 275 VDC.
- **There is no bias control.** The PS-23 gives a fixed **−130 V** with no load (−100 V at 20 mA); bias is set on the transceiver.
- **A 2.92 A circuit breaker** (65-17) replaces the fuse. Primary current goes through the breaker and through pins 9–10, the AC switch in the equipment (p. 21).
- A **3-wire grounded line cord** is standard; the green wire goes to chassis lug F (p. 13).
- The output cable is a captive 8-wire cable with an 11-pin plug. The connector for the far end comes with the equipment (p. 17).

**SB-102: LV tap.** Needs **300 V @ 150 mA**, so the PS-23 must have the **350 VDC wiring** (Detail 5B). The SB-102 initial test expects 800–900 V on HV, matching the PS-23's 820 V no-load figure.

**Specifications (p. 20):**

| Output | No load | Loaded | Duty |
|---|---|---|---|
| HV | 820 V | 700 V @ 250 mA (62.5 µF effective; < 1% ripple) | Continuous to 150 mA; 50% at 300 mA |
| LV high tap (350 VDC wiring) | 350 V | 300 V @ 150 mA (with 100 mA on HV) | Continuous to 175 mA |
| LV low tap (275 VDC wiring) | 275 V | 250 V @ 100 mA (with 100 mA on HV) | Continuous to 175 mA |
| Fixed bias | −130 V | −100 V @ 20 mA | — |
| Filament | — | 12.6 VAC @ 5.5 A | — |
| Line | 120/240 VAC, 50/60 Hz, 350 W | | |

**Output pinout (schematic; Table 1, p. 17; Detail 7F, p. 14):**

| Pin | Cable wire | Function |
|---|---|---|
| 1 | Green | −130 V fixed bias |
| 2 | Black | Filament common |
| 3 | Orange | LV +350 V or +275 V (no load) |
| 4 | Yellow | HV +820 V (no load) |
| 6 | Red | 12.6 VAC filament |
| 7 | White | Ground |
| 9, 10 | Brown, blue | AC switch loop in the equipment. If the equipment has no AC switching, jumper pins 9–10 at the plug and leave brown and blue unconnected (p. 14). |
| 5, 8, 11 | — | No connection |

**Recap list, with the PS-23 manual's values (parts list p. 3; schematic; circuit description p. 21):**

| Ref | Manual value | Function | Notes |
|---|---|---|---|
| C1, C2 | 125 µF 500 V (25-265 can) | HV voltage doubler | Replace both together; field practice is 150 µF 500 V. |
| C3, C4 | 125 µF 500 V (25-265 can) | LV half-wave doubler; C4 is also the pi-filter input | Same type of can as C1/C2. |
| C5 | 40 µF 450 V (25-36) | LV pi-filter output (with choke L1, 6 H) | An open C5 causes a CW tone and relay chatter (HW-101-73). |
| C6, C7 | 40 µF 150 V (25-20) | Bias filter | **Negative supply: the + lead goes to the ground lug** (p. 12). |
| R1–R4 | 100 kΩ 2 W | HV bleeders / equalizers across C1, C2 | Drift high (field: ~150 kΩ); metal film 2 W. |
| R5 | 100 kΩ 2 W | LV bleeder | — |
| R7 | 1000 Ω 2 W | Bias filter series | Chart: no bias → D7, R7 or R8. |
| R8 | 27 kΩ 1 W | Bias bleeder | Field practice uprates to 2 W. |
| D1–D7 | 1N2071 (57-27) | D1–D4 HV doubler (two per leg); D5, D6 LV; D7 bias | 1N4007 is the usual replacement. |
| L1 | 46-56, 6 H | LV filter choke | Chart: no LV → D5/D6, L1. |
| Breaker | 2.92 A (65-17) | Primary protection | Opens on reset → transformer, rectifier/filter or load short. |

**Initial test (p. 15).** Use a VTVM or VOM; a solid-state ohmmeter won't turn the diodes on.

- Find the meter polarity on R × 1 (about 10 Ω between strip H lugs 3 and 4). Put the correct lead on ground and switch to R × 10k.
- Socket readings: **pin 1 ≥ 22 kΩ; pins 3 and 4 ≥ 75 kΩ** (they rise slowly as the caps charge); **pin 7 = 0 Ω**; all others infinite.

**Troubleshooting shortcuts (p. 19):**

- **HV only half the normal value** → D1–D4 or C1/C2.
- **LV only half** → D5/D6.
- **All outputs half** → primary wired for 240 V on a 120 V line.
- **Dead** → breaker, the pins 9–10 loop, or the equipment's switch.

**Caution (p. 17).** The outputs are lethal and the large capacitors hold their charge. The manual allows shorting the cap lugs to chassis with a screwdriver blade. A **bleeder stick (resistor to ground)** is gentler on the caps and terminals. Work with one hand, on an insulating surface.

# 4. Restoration plan

## 4.1 Phase 0: Assess cold

1. Photograph everything. Note mods, wrong-value parts and tube brands.
2. Pull and label the tubes; test with a grid-leakage tester. Confirm V19 is a 12AT7.
3. Check wafer rotor orientation.
4. Inspect C304, the 2 W and 1 W resistors, RL1/RL2, belts, the LMO coax and **PL1**.
5. Run the **resistance chart, Fig. 1-15 (p. 123)**: all controls fully counterclockwise, power plug disconnected, measured to chassis. Typical points:

| Point | Expected |
|---|---|
| V8/V9 grids | 95 kΩ |
| V7 | 91 kΩ / 168 kΩ / 39 kΩ |
| V10 | 300 kΩ, 220 Ω |
| V11 | 2 MΩ |
| V3 | 2.1 MΩ |
| V4 | 1.9 MΩ |
| V13 | 9.2 MΩ |

## 4.2 Phase 1: HP-23

Recap, test unloaded on a variac and wire per **Fig. 1-5 (p. 99)**. On an HP-23A, set the switch **right (350 VDC)** and run its socket resistance check (§3.6.1). On a PS-23, confirm the **350 VDC wiring** and run its socket resistance check (§3.6.2).

## 4.3 Phase 2: Clean

DeoxIT all rotary wafers, pots, the **FILTER**, **FREQ CONTROL** and MODE switches. Keep solvents off the dial plastic.

## 4.4 Phase 3: Recap and resistor survey

1. Replace all **7 electrolytics** (§3.1), including C941/C942.
2. Replace the resin, tubular and Mylar caps.
3. Resistor survey board by board (audio → modulator → IF → bandpass → RF-driver → chassis), with priority on §3.2.
4. Replace suspect silicon diodes (D101, D201, D902, D903, D905).
5. Clean the relays.

## 4.5 Phase 4: Mechanical and LMO

Belts; comb brackets and lockwashers; LMO mounting fix (SB-102-4); clean the LMO jack; set the dial roller tension.

## 4.6 Phase 5: Initial test (manual p. 100)

1. Controls: FINAL (round) 80; FINAL (lever) over 50 Ω; MODE LSB; RF GAIN fully CW; FILTER SSB; others fully CCW. **BIAS ADJUST (top of chassis) fully CCW.**
2. Check the 9-terminal jumper plug is fitted. Connect the 8 Ω speaker; never run without a speaker or phones.
3. FUNCTION → PTT. Panel lamps light with **equal brilliance**; **the LMO supply lamp glows dimly.**
4. METER → HV: **+800 to +900 V**.
5. METER → PLATE: **zero**.
6. AF GAIN up: receiver noise is heard.
7. Check **+300 V** at bandpass board point 5 and **−111 V** bias at audio board point 4.
8. Compare with the **receiver and transmitter DC voltage charts**, Figs. 1-14A/B (fold-out from p. 122; ±20%; bias at 50 mA plate current). Key values:
   - B+ about 300 V;
   - V18 regulated 150 V;
   - V19 test point;
   - V5 cathode about −5 V with the crystal oscillator, 0 V with the LMO;
   - V14 plate about 245 V.

## 4.7 Phase 6: Alignment (manual pp. 101–105)

**Equipment:** high-impedance voltmeter (or a 20 kΩ/V VOM); 50 Ω 100 W non-reactive dummy load; a receiver for WWV (2.5/5/10/15 MHz) or a broadcast station on a 100 kHz multiple. A monitor scope is recommended.

**Preset:**

| Control | Setting |
|---|---|
| CAL XTAL trimmer | Notch toward the 100 kHz crystal |
| DRIVER PRESELECTOR | 12 o'clock |
| MIC/CW LEVEL | Fully CCW |
| MODE | LSB |
| BAND | 3.5 |
| Dial | 3.7 MHz |
| FUNCTION | PTT |
| FREQ CONTROL | LOCKED NORMAL |
| RF GAIN | Fully CW |
| FILTER | SSB |
| METER | ALC |
| AF GAIN | 9 o'clock |

**Do not transmit until directed.**

**S-meter:** after warm-up, adjust **METER ZERO** (IF board, behind the FREQ CONTROL switch) for zero.

**Receiver:**

1. **Heterodyne oscillator:** voltmeter on −DC at **TP** (bandpass board, foil side near V19). If TP reads 0, use the adjacent lead of the 100 kΩ resistor.
   - Target is **−0.5 to −2 V on every band**. **−0.3 V means the oscillator isn't running.**
   - Adjust in the direction where the voltage changes **slowly**.
   - Coils 3.5, 14 and 28.5 are on top (RF-driver board, coil 3.5 near V11). The others are from the bottom, marked on the shield cover.
2. FUNCTION CAL, BAND 3.5, tune 3.7 MHz. Confirm the calibrator signal (it should stop in VOX and peak with the DRIVER PRESELECTOR). Reset the preselector to 12 o'clock.
3. **IF:** peak **T201** for volume, then **T102** top and bottom slugs, then **T103** for S-meter. Repeat. Don't turn T201 or T103 more than two turns.
4. **Driver grid and plate coils**, in order **3.5 → 29 (29.2) → 21 (21.2) → 14 (14.2) → 7 (7.2)**, with the preselector at each marked position, for maximum S-meter.

**Benchmark (p. 103):** calibrator **S9+20 dB at 3700 kHz, decreasing to S6 at 29.2 MHz.**

**Receiver signal-voltage chart (fold-out p. 122)** for signal-generator troubleshooting:
- 3.395 MHz at 20 mV at point 1 gives 4 V RMS at the 8 Ω jack.
- **100 µV at the antenna (3.900 MHz) gives about S9.**
- Calibrator signal is about S9+20 dB.

**Transmitter** (p. 103–104; coil cover on; 100 W dummy load):

1. **Neutralizing capacitor** (on the RF cage) at **½ meshed, slot vertical**. FINAL 80; lever over 50 Ω; MODE LSB; BAND 3.5; 3.7 MHz; PTT; FILTER SSB; FREQ CONTROL LOCKED NORMAL; METER PLATE.
2. **Bias:** press PTT and set **BIAS ADJUST for 50 mA** plate current. If it reads over 100 mA, don't hold PTT more than a few seconds until it's set.
3. METER → REL PWR (RELATIVE POWER control mid-range). TUNE mode. Advance MIC/CW LEVEL until RF shows; peak the DRIVER PRESELECTOR and FINAL.
4. MIC/CW LEVEL to about **¼ maximum output**; peak **T1**. No more than one turn should be needed.
5. MIC/CW LEVEL and preselector for maximum. METER → **GRID: should read full scale.**
6. **Neutralization:**
   - METER → PLATE: tune FINAL for minimum plate current.
   - METER → REL PWR: tune for maximum.
   - Both must fall at the **same FINAL setting**. Move the neutralizing capacitor a small amount at a time until they coincide.
7. **Carrier null:** MIC/CW fully CCW; LSB, PTT. Adjust the **CARRIER NULL control**, then the **CARRIER NULL capacitor**, for minimum RF. Repeat until the null is about the same on LSB and USB, down to **¼ V or less** on an RF voltmeter. See §3.1 for the C18 12/24 pF fix. **The 6.8 MHz trap is sealed; don't turn it.**
8. **For each band from 3.5:**
   - Peak FINAL and preselector.
   - METER → GRID and set MIC/CW for about **0.3 mA**.
   - Peak **that band's heterodyne oscillator coil for maximum grid current**.
   - Advance MIC/CW for **near or over full-scale** grid.
9. **Driver neutralization (21.2 MHz):** put the free end of the **driver neutralizing wire into hole W** on the RF-driver board (Fig. 1-1). Peak the preselector; if peaking is ragged, reposition or bend the wire for smooth peaking.
10. **Final neutralization again at 14.2 MHz** (adequate for all bands).
11. **Rezero the S-meter on 29.5**, then check zero on every band. If a band doesn't read zero, readjust **that band's heterodyne oscillator coil** (chart 18A).
12. **Crystal calibrator (p. 104–105):** lay the antenna wire near Y4/V19. FUNCTION CAL, AF GAIN fully CCW. Adjust the **CAL XTAL trimmer (C220, 8–50 pF, bandpass board)** for zero beat on WWV in an external receiver; confirm by toggling VOX/CAL. **A 20 Hz error at 100 kHz becomes 740 Hz at 3.7 MHz.**
13. **LSB ADJUST:**
    - Set 3.7 MHz (BAND 3.5, upper dial at 2, circular dial at 0). FUNCTION CAL, MODE USB.
    - Zero beat and peak the preselector.
    - Switch to LSB without touching the dial.
    - Turn the **LSB ADJUST screw on the LMO** for exact zero beat.
    - Recheck USB and repeat as needed.
14. **Dial calibration:**
    - Zero beat at 3.7 MHz and set the Zero Set dial.
    - If the hairline is off center, remove the knob and hold the LMO drive shaft with a screwdriver through the escutcheon hole.
    - Loosen the circular-dial setscrew and center 0, then retighten. **Tape the Allen wrench** so it can't short the pilot lamp socket.
    - Make sure the nylon spiral follower is engaged.

**Grid drive (SB-102-3):** peak for **maximum grid drive**; full-scale grid current after tune-up is normal.

**Operation tune-up (pp. 117–118):**
- TUNE should read **50 mA** at rest.
- Set FINAL loading for REL PWR 50, then advance MIC/CW for maximum.
- Plate current should be about **250 mA** at maximum output.
- Don't hold full output for extended periods. The **transmitter chart warns the fuse may blow if run at full TUNE output for more than 1 minute.**

## 4.8 Phase 7: Optional improvements (reversible)

The WB8JKR and K4JPN SB-10x improvements apply:

- 10 MΩ meter-zero resistor.
- C17 change and R6/R7 → 27 kΩ for BFO injection.
- C119 500 pF → .001 µF.
- C11 .001 → .01 µF.
- R328 470 kΩ → 1 kΩ (the R328 VOX-tone resistor is shown as 470 kΩ in Fig. 2-18).
- Sidetone fixes.
- .01 µF filament bypass.
- Multi-turn carrier-null pot.

Keep the original LMO power circuit unless PL1 or D907 cause repeated trouble.

# 5. Troubleshooting quick-reference

## 5.1 Manual troubleshooting chart (pp. 123–130), selected

| # | Symptom | Key causes |
|---|---|---|
| 4 | Uneven filament/lamp voltages | Brown filament-balance leads wrong or missing |
| 5–7 | No / high / low regulated +150 V | RL2 wiring; **OA2 V18**; R304/R305; shorted bypass |
| 10 | No audio | T301; **C304 shorted or reversed**; V14; RL2 leads |
| 13 | Hum only | RFC101 open; V13; no carrier injection; T102; V3/V4 |
| 14 | Noise, no signal | T201; no LMO at V12A; V3; V12A; no heterodyne at V11; V10/V11; RL1; T202; FL1; FILTER/FREQ CONTROL |
| 16 | Receiver oscillates or birdies | Board hardware and lockwashers; antenna; high supply; low cutoff bias |
| 18 | S-meter faults | R103/R105/R106/R107/R110/R115; V3/V4; AVC short; RL2 lugs; **V19 wrong type** |
| 21–25 | No RF at a stage | HV, L901, RL1 lugs, bias, finals; RFC801, V7; V6 heterodyne; V5A, C111/C402, T202 |
| 28–31 | No SSB or no CW carrier | CR1–CR4; V1; V16; Y1/Y2/Y3; R6–R9, R11; C4–C8, C16, C17 |
| 33 | Won't key CW | V15; C317; D904 |
| 34–39 | LMO / heterodyne faults | LMO 10 V supply; Q1; **lamp**; **D907**; Y501–Y508; V19; L601–L608; rotors 180° |
| 40–41 | VOX won't trip / stays in | V17A; D201; **D202**; anti-trip D1/D2 |
| 42 | Transmitter unstable | Neutralizing; hardware; lockwashers; L801–L805; coil cover; shield clips |
| 43 | Slow receive recovery | **D101** |
| 44 | Grid drive falls off | Heat; bias; load; **gassy 6146s** |
| 45 | Zero shifts band to band | Calibrator not exactly 100 kHz |

## 5.2 Service bulletins

| Symptom | SB-102 bulletin (→ referenced) |
|---|---|
| Meter zero / S-meter drift | -1 (→ SB-101-1), -22, -30 |
| Heterodyne tube failures | -2 (R212 220 → 330 Ω) |
| Low grid drive | -3, -9, -26 |
| LMO instability | -4; SB-101-12, -16 |
| Low 40 m output | -5 |
| VOX / audio | -6 (→ SB-100-3), -19, -24, -29 |
| Instability / oscillation | -7, -9, -10 |
| Neutralizing | -8 (→ SB-100-2), -27 |
| Relays hang / chirp | -14, -21 |
| AGC / IF sensitivity | -15, -23, -25 |
| Calibrator spurs | -16 |
| Preselector / loading | -17, -20 |
| Carrier null | -18 |
| Germanium diodes | -28 |

# 6. Operation notes (manual pp. 116–118)

- A 50–75 Ω non-reactive load must be connected before transmitting.
- **FREQ CONTROL:**
  - LOCKED NORMAL: transmit and receive on the LMO.
  - LOCKED AUX: both on crystal Y5.
  - UNLOCKED AUX: transmit on Y5, receive on the LMO.
  - Y5 operation is limited to about ±25 kHz of each band (Special Crystal Considerations, p. 106).
- Meter scales: S-meter 0–60 dB (ALC position, receive); REL PWR 0–10; GRID 0–1 mA; HV 0–1000 V; PLATE 0–500 mA.
- Heterodyne crystals may be **up to 1500 Hz** off nominal (p. 106). That's another reason to zero-set on the nearest calibrator point.
- **RTTY via the LMO FSK terminal requires a fan and a reduced screen voltage** (keep input ≤ 150 W; p. 116).

# 7. Tips and tricks

- Keep spare **#1815** lamps. A dead PL1 kills the LMO.
- Keep GE 6AU6s, a 12AT7 for V19, a 6HS6 and a matched 6146 pair.
- Never pull tubes or lamps with power on (series-parallel filaments).
- Tape the Allen wrench during dial calibration.
- Measure resistors with one lead lifted.
- Reform any electrolytics you keep with a slow ramp.

# 8. Maintenance schedule

| Interval | Task |
|---|---|
| Each session | 10–30 min warm-up; zero-set on the nearest calibrator point. |
| 6 months | Exercise switches; METER ZERO check. |
| Yearly | Bias (50 mA), carrier null, grid drive; receive benchmark (S9+20 / S6); LMO jack clean. |
| 2–3 years | Neutralization check (14.2 MHz); retighten board hardware; DeoxIT. |
| 5 years | Tube test (V3/V4/V10/V19, finals); belts; HP-23 check; C941/C942 ESR; check that PL1 glows dimly. |
| ~15–20 years | Re-check electrolytics. |

# 9. Parts sources

- SB-102 restoration kits (e.g. W8AOR): electrolytics including C941/C942, films, diodes, zeners, R6/R7, carrier-null pot.
- Mouser / Digi-Key; HP-23 rebuild kits.
- Heathkit-Radios and heathkit (groups.io): LMOs, crystals, dials, belts, #1815 lamps.

# 10. Sources

- Heath, *SB-102 Assembly Manual* 595-1058-06 (W5RKL); SB-102 LMO schematic (archive.org).
- Heath list (puck.nether.net): SB-102 bulletins (Aug 1997, Apr 1998); SB-101 and SB-100 bulletins (Apr 1998); HW-101 bulletins (Jul 1997).
- W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting*.
- K5BCQ SB-101/102 Repair Site; Nostalgic Kits Central SB-10x troubleshooting; W2PA SB-101 rebuild notes.
- Heath list threads: "SB-102 LMO" (Jun 2002, K9STH); "SB-101 vs 102" (Oct 2009).
- W8AOR SB-102 restoration kit parts list.
- Heath, *Heathkit Assembly Manual, AC Power Supply Model HP-23A*, 595-924-01 (©1968).
- Heath, *Heathkit Manual for the AC Power Supply Model PS-23*, 595-2314 (©1979).

# 11. Rev. 2 change log (manual review)

| Item | Rev. 1 | Rev. 2 (per manual) |
|---|---|---|
| LMO power path | Lamp described as a **dial lamp**; "56 Ω resistor, D907, **2N3567 regulator**" | **PL1 #1815 is a voltage-dropping lamp in the LMO supply** (D106 rectifier, C941/C942, R936 → PL1 → **D907 10 V zener**). **Q1 2N3567 is an on/off switch** driven from +150 V via R941. **R928 56 Ω** terminates the LMO line (pp. 142–143). |
| D907 | Linked to 15/10 m sensitivity (SB-101-10) | In the SB-102 it is the **LMO 10 V zener**; reversed → no LMO (chart 39B). The SB-101-10 note does not apply. |
| Specs | Field values only | Added the factory spec table: 0.35 µV, 100/80 W, 400 Hz electrical dial accuracy, power 700–850 V / 300 V / −115 V / 12 V @ 4.76 A (pp. 131–134). |
| Tube list | Inferred V numbers | **Confirmed** against the circuit description; V11 = 6AU6, V5B = crystal oscillator, V16A USB / V16B LSB-CW. |
| Diodes | Kit cross-reference only | Full designator table: D1/D2, D101, D106, D201, **D202 15 V zener**, D301, D902/D903, D904, D905, D907, D909, CR1–4, CR201, CR901. |
| Electrolytics | C945 "bias filter" and voltage ratings asserted | Parts lists confirm **7 electrolytics** (2 × 500 µF, 3 × 10 µF, 2 × 20 µF) but **print no voltages**; C945 designator unverified. |
| R104/R105 | Implied 47 Ω (HW-101 values) | **56 Ω** in the SB-102. |
| R116 | 4.7 kΩ vs 47 kΩ unresolved | **4700 Ω** (Fig. 2-25). |
| Pilot lamps | "#1815 dial lamp" | One #47 + three #1815 (p. 24); one #1815 is PL1. |
| Supply | "HP-23, 300 V" | Also: HP-23A bias control inoperative (bias set in the transceiver); HP-13A bias fully CW; 11-pin pinout; 9-terminal jumper plug required (pp. 98–100). |
| Initial test | Generic | Manual steps: BIAS ADJUST CCW, HV 800–900 V, +300 V at pt 5, −111 V at pt 4, LMO lamp glows dimly (p. 100). |
| Alignment | Shared SB/HW method | Manual sequence with TP −0.5 to −2 V (−0.3 V = dead), coil locations, T201/T102/T103, neutralizer ½-mesh start, bias 50 mA, T1, grid 0.3 mA per band, hole-W driver wire, 14.2 MHz re-neutralize, S-meter rezero on 29.5, C220 calibrator, LSB ADJUST, dial calibration (pp. 101–105). |
| Carrier null | Bulletin reference only | Manual's C18 12/24 pF rotor fix (p. 104). |
| Troubleshooting | Bulletins only | Added the manual chart summary (§5.1). |
| Dial error | "3–10 kHz between points is normal" | Spec is **within 400 Hz** after calibration; the K9STH figures describe aged or out-of-spec units. |
| Supply (Rev. 2.1) | HP-23 recap bullets | Added the PS-23 section (now §3.6.2), checked against the **PS-23 manual**: 350 VDC internal wiring for 300 V, fixed −130 V bias with no control, 2.92 A breaker, recap table with designators and ratings, pinout, initial resistance test, troubleshooting. |
| Supply (Rev. 2.2) | HP-23 bullets from field sources | Added §3.6.1, checked against the **HP-23A manual** (595-924-01): the AC OFF switch is the B+ selector (right 350 VDC / left 275 VDC no-load = 300/250 V loaded; SB-102 uses **right**); fixed −130 V bias on pin 1 and adjustable −40 to −80 V on pin 11 (R9), which explains why the HP-23A BIAS control is inoperative with the SB-102; supply-end pinout (differs from the transceiver-end plug in §2.5); breaker, not fuse; recap table — **C6/C7 are 20 µF in the HP-23A** (40 µF in the PS-23), R8/R10 10 kΩ 1 W bias-range limiters; initial resistance test; troubleshooting chart. Capacitor voltage ratings and the breaker rating are not printed in the HP-23A manual. |
