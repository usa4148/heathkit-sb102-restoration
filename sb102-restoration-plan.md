---
title: "Heathkit SB-102 Transceiver — Restoration Plan"
subtitle: "Rev. 1 — Heath service bulletins, W5RKL, K5BCQ and field sources (manual cross-check pending)"
date: "October 2026"
geometry: margin=0.9in
fontsize: 10pt
colorlinks: true
toc: true
---

<!-- Build: pandoc sb102-restoration-plan.md -o sb102-restoration-plan.pdf --pdf-engine=xelatex -V mainfont="DejaVu Serif" -->

> **Safety.** The SB-102 runs from an HP-23 supply delivering **~700–800 V DC** to the finals. That voltage stays on the filter capacitors after the supply is unplugged. Discharge before every hands-in session; never run the final cage open; work one-handed when live. Use a **50 Ω dummy load (not a light bulb)** for all transmit work.

# 1. Source documents

| Document | Where | Use |
|---|---|---|
| **Heath SB-102 Full Assembly Manual**; **SB-102 Alignment & Schematic** | W5RKL (w5rkl.com/manuals) | Primary reference. *Scans only; cross-check pending (§11).* |
| **SB-102 solid-state LMO schematic** | archive.org (K6KJN upload) | LMO circuit. |
| **SB-102 Service Bulletins** -1 … -30 (1970–84) | Heath list (puck.nether.net), Aug 1997 and Apr 1998 | Factory fixes. Many refer to SB-100, SB-101, HW-100 and HW-101 bulletins, also transcribed there. |
| **W5RKL**, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting* | w5rkl.com | SB-102 LMO power, electrolytics, bias and loading targets. |
| **K5BCQ** SB-101/102 repair site | qsl.net/k5bcq | Leakage limits, IF filter centering, dial mechanics. |
| **Nostalgic Kits Central** SB-10x troubleshooting | nostalgickitscentral.com | Measured SB-102 resistor drift and electrolytic leakage. |
| **W2PA** SB-101 rebuild notes | sites.google.com/site/radiow2pa | LMO connector, bandswitch rotors, driver neutralizing. |
| Heath list / Heathkit-Radios / heathkit (groups.io) | puck.nether.net, groups.io | Field experience. |

# 2. Transceiver at a glance

## 2.1 Key facts

| Item | Value |
|---|---|
| Built | 1970–1975, list price $380. Successor to the SB-101; parent design of the HW-101. |
| Bands | 80, 40, 20, 15, 10 m (eight 500 kHz segments, 3.5–29.5 MHz) |
| Modes | SSB (USB/LSB), CW; optional 400 Hz CW filter |
| IF | 3395 kHz; 2.1 kHz SSB filter; 1st IF 8.395–8.895 MHz bandpass |
| Output | About 100 W |
| **LMO** | **Solid-state** (not TRW), shared with the SB-303/313. Powered at about **+10 V** from the 12.6 VAC filament line **through a #1815 dial lamp**, a 56 Ω resistor, zener **D907**, a **2N3567** regulator and filter caps **C941/C942 (500 µF 25 V)**. |
| Metering | GRID, ALC, REL PWR, PLATE, HV (the HW-101 lacks GRID and HV) |
| Extras over SB-101 | Front-panel internal/external LMO switch; 2 m transverter (SB-500) socket; headphone and sidetone volume controls on the rear audio board |
| Supply | **HP-23** family (B+ switch on 300 V) |
| Boards | Modulator 85-127-3, Audio 85-130-3, Bandpass 85-129-4 |

## 2.2 Tube complement

These V numbers are inferred from the SB/HW-10x family and W5RKL. **Verify them against the manual before ordering.**

| V | Tube | Function | Known issues (source) |
|----|----------|--------------------|----------------------------------------------|
| V1 | 6EA8 | Speech amp / cathode follower | GE 6EA8 oscillates → VOX relay chatter (SB-101-19). |
| V2 | 6AU6 | Carrier isolation amp | — |
| V3 | 6AU6 | 1st IF amp | **Brand-critical.** Heath stocked only GE; non-GE → S-meter drift and IF oscillation (SB-102-30). Sylvania, RCA and Westinghouse → low output and poor VOX (SB-102-24). Leaky → S-meter drift (SB-101-22). |
| V4 | 6AU6 | 2nd IF amp | Same as V3. |
| V5 | 6EA8 | 1st transmit mixer | — |
| V6 | 6CB6 | 2nd transmit mixer | Under shield; shield to socket center pin. |
| V7 | 6CL6 | Driver | Under shield. |
| V8, V9 | 6146 / 6146B | Finals | **Tung-Sol vs. RCA 6146 capacitance changes drive and alignment** (SB-102-3). Re-neutralize after any change. |
| V10 | **6HS6** | Receiver RF amp | Leaky → poor AGC and S-meter decay (SB-102-15 → HW-101-24). Shorts → lost sensitivity and "static" on the S-meter. A 6AU6 substitutes directly. |
| V11 | **6AU6** | 1st receive mixer | The SB-102 uses a 6AU6 here; the HW-101 uses a 6HS6. |
| V12 | 6EA8 | 2nd receive mixer / relay amp | A bad tube holds the relays in after transmit (SB-102-14). |
| V13 | 6BN8 | Product detector / AVC | — |
| V14 | 6GW8 | Audio amp / output | Hot; C304 sits next to it. |
| V15 | 6EA8 | CW sidetone osc/amp | — |
| V16 | 12AU7 | Carrier oscillator | Plate resistors R6/R7 drift (§3.2). |
| V17 | 12AT7 | VOX amp / 100 kHz calibrator | One restorer found an open filament though it tested good elsewhere. |
| V18 | 0A2 | 150 V regulator | — |
| V19 | 12AT7 | Heterodyne osc / cathode follower | Repeated failure: R212 220 Ω → 330 Ω (SB-102-2). |

That is 19 tubes. Unlike the HW-101, there is **no V20 VFO buffer**; the LMO is self-contained.

## 2.3 Semiconductors (restoration-kit cross-reference)

| Heath p/n | Type | Use | Modern replacement |
|---|---|---|---|
| 57-27 (×11) | Silicon rectifier/switching | ALC, recovery, VOX, relays | 1N4005–1N4007 |
| FH-110 (×4) | Hot-carrier | Balanced modulator (CR1–CR4) | 1N5711 |
| 56-26 | Germanium | Calibrator / detector / REL PWR | 1N34A |
| 56-56 | Silicon signal | — | 1N4149/1N4148 |
| 56-25 | 15 V zener | — | 1N4744A |
| 56-67 | 10 V zener | LMO supply (D907 area) | 1N4740A |
| — | 2N3567 | LMO regulator | Any NPN of equal rating; check pinout |

Bulletin SB-101-4 covers diode leakage in this family. Leaky **D902/D903** make the ALC meter fall off after 30–60 min. **D101** and **D905** give slow or no receive recovery. **D201** gives poor VOX sensitivity or a long VOX delay. **D907 reversed** gives low sensitivity on 15/10 m (SB-101-10). Bulletin SB-102-28 is a germanium diode change, but its text was not available.

# 3. What fails, ranked

## 3.1 Capacitors

**All seven electrolytics** (W5RKL and restoration-kit lists; one restorer found most of them excessively leaky):

| Ref | Value | Role | Replace with |
|---|---|---|---|
| **C941, C942** | **500 µF 25 V** | **LMO power supply filter** (SB-102 only) | 470–500 µF 50 V |
| C945 | 10 µF 150 V | Bias filter | 10 µF 160–250 V; **negative supply, so check polarity** |
| C2 | 10 µF 15 V | Speech amp cathode (modulator board) | 10 µF 50 V |
| C212 | 10 µF 15 V | VOX amp cathode (bandpass board) | 10 µF 50 V |
| C12 | 20 µF 350 V | Speech amp B+ decoupling | 22 µF 450 V |
| **C304** | 20 µF 350 V | Audio output decoupling, beside V14 | 22 µF 450 V. One measured **3.6 mA** leakage (0.5 mA allowed) and would not reform. |

| Other capacitor | Likelihood | Symptom | Action |
|---|---|---|---|
| .22 µF blue Mylars (×6), C13 | Medium | Leakage | Replace with film (kit: 0.22 µF 250–630 V). |
| C938 (.05 µF) | Medium | — | 0.05 µF 630 V film. |
| C300, C301 (.022 µF, audio board) | Medium | — | 0.022 µF 400 V film. |
| C213 (VOX delay) | Medium | VOX delay drifts with warm-up | Check value and leakage (SB-100-3). |
| IF transformer internal micas | Low–medium | Corroded → value change or short | Measure; repair the can or replace (K5BCQ). |
| C14 carrier null trimmer | — | Nulls fully meshed | Move C18 (12 pF) to the other section (SB-102-18). |

Leakage limits for paper and film caps (K5BCQ): < 0.01 µF ≤ 0.5 µA; 0.01–1 µF ≤ 1 µA (aged). Electrolytics: I ≤ kC + 0.3 mA, with k = 0.02 (101–250 V) or 0.035 (251–350 V).

## 3.2 Resistors

**Measured drift in one SB-102** (Nostalgic Kits Central; 15 replaced):

| Nominal | Measured | Error |
|---|---|---|
| 100 kΩ (×5) | 115 k – **851 k** | +15 to +751% |
| 150 kΩ (×2) | 192 k / **open** | +28% / open |
| 330 kΩ | 463 k | +40% |
| 22 kΩ | 42 k | +92% |
| 3.3 kΩ | 5.5 k | +67% |
| 68 kΩ | 92 k | +36% |

**Designators to check first:**

| Ref | Value | Failure / symptom | Fix (source) |
|---|---|---|---|
| **R107** | 100 kΩ (82 kΩ after SB-101-1) | S-meter zero and drift | 100 kΩ 1 W (SB-101-9); check which value is fitted. |
| **R106** | 22 kΩ 1 W (IF board) | Heat drift → meter zero wander | 22 kΩ 2 W wirewound or 33 kΩ 2 W (field). |
| **R6, R7** | 33 kΩ (carrier osc plates) | Drift → low carrier on one sideband | 33 kΩ 1 W (restoration kit). |
| **R116** | (kit lists 4.7 kΩ; the description says 47 kΩ) | — | Confirm the value in the manual. |
| **R212** | 220 Ω | Heterodyne-osc tube failures | 330 Ω 1/2 W (SB-102-2). |
| R117 | 3.3 kΩ (AVC) | Open → fast AVC/S-meter decay | Replace (via HW-101-55). |
| R940, R701, R904 | Driver / switch boards | Burned from a bandswitch lug shorting to the shield | Replace; bend the lug clear (W5RKL). |
| **56 Ω** (LMO supply) | — | LMO voltage low or unstable | Measure. |
| 10 kΩ plate-supply resistors (driver/audio) | — | Drift → low output or weak audio | Measure and replace. |

Lift one lead to measure, because parallel paths mislead.

## 3.3 Tubes, by risk

1. **6AU6 at V3/V4:** brand matters; use GE (SB-102-24, -30).
2. **6HS6 V10:** leakage or shorts; check pin 1 for > +1 V (bad).
3. **6146 pair:** emission, gas, brand-dependent drive.
4. **6EA8 (V1, V12, V15):** oscillation, relay hang, sidetone.
5. **12AT7 (V17, V19):** calibrator and heterodyne oscillator; check filaments.

Use a tester with a **grid-leakage** test (an emission tester misses grid emission). Return each tube to its original socket.

## 3.4 LMO (the SB-102's weak point)

| Symptom | Cause | Fix (source) |
|---|---|---|
| **LMO dead or intermittent** | **#1815 dial lamp open.** The lamp is in series with the LMO supply. Also check D907, the 2N3567, C941/C942 and the 56 Ω resistor. | Replace the lamp (keep a spare); recap C941/C942; check about +10 V regulated (W5RKL). |
| **Instability, jumps** | Chassis stress at the mounting lugs | Replace lockwashers and nuts with #6 flat washers and **hex lock nuts**; tighten one turn at a time, criss-cross (SB-102-4). |
| Jumps when the cable moves | Dirty LMO output phono jack/plug | DeoxIT the collar and shell (SB-101-16; W2PA). |
| Erratic tuning (early LMOs) | Tuning-cap wiper | Clean the wiper; use a metal washer in place of the fiber washer at the gear assembly (SB-101-12). |
| **Dial correct at 100 kHz points, off by 3–10 kHz between** | **Characteristic** of the solid-state LMO (K9STH, five units) | Not a fault. Zero-set on the nearest calibrator point before operating. Bending the slotted rotor plates is possible but tedious. |
| Stiff tuning, slipping knob | Dial pinch roller and ball vernier | Engage the roller, then tighten the bushing just enough to stop slipping; drive-disk gap 0–0.002 in. (K5BCQ). Raise the knob-shaft bushing to loosen, lower to stiffen (W2PA). |
| Cracked or crazed dial | Lamp heat | Correct lamp type and shield position; replacement dials exist. |

Do **not** fit an SB-102 LMO into an SB-101; it may be unstable (W5RKL).

## 3.5 Mechanical, switches, relays

| Item | Symptom | Fix (source) |
|---|---|---|
| **Bandswitch rotor/wafer orientation** | Dead or partial bands | Check the notches; W2PA found 3 of 4 wrong. |
| **SSB/CW filter slide switch** | Low TX **and** low RX | Clean (SB-102-25). |
| **RL2 contact carbon** | Chirp, slow recovery | Clean or replace RL2 (SB-102-21). |
| RL1 power lead | No PTT/TUNE | Check the grey lead (W2PA). |
| Relay chatter in VOX | GE 6EA8 at V1; noise | SB-102-19; SB-100-3 signal tracing. |
| Relays stay energized | V12 | Replace V12 (SB-102-14). |
| **Comb brackets** (aluminum) | Oscillation, low drive on 15/10 m | Steel brackets 204-2096, soldered (SB-102-9); tighten the RF driver board screws and add lockwashers (SB-102-10). |
| Low 40 m output | Ground-wire routing | Re-route the ground wires; remove 4 spring clips; realign (SB-102-5). |
| **Preselector drive belt and cap** | Poor tracking (worst on 10 m) | New belt (silicone 50/56/3 mm); lubricate the bearings; replace cap 26-122 if it binds (SB-102-17). |
| Loading cap turns with plate | — | Grommet 73-3 (SB-102-20). |
| Grounds | Hum, instability | Tighten the phono sockets, final socket rivets (drill out → 6-32 if loose) and rails (SB-101-5D, SB-100-4). |
| Filament wire | Short to a board screw | Re-route the brown/white wire (W2PA). |
| Lifted foil pads | Open circuits | Secure the pad. |

## 3.6 HP-23 power supply (do first)

Same as for the HW-101:

- Recap the high-voltage section (C1–C4 → 150 µF 500 V).
- Recap C5 (40 µF B+). An open C5 causes a tone on CW and relay chatter (SB-102-29 matches this symptom).
- Recap the bias caps C6/C7 (often open).
- Replace the bleeders (100 kΩ 2 W) and diodes (1N4007).
- Fit a grounded cord.
- **B+ switch on 300 V.**
- Expected outputs: −130 V bias, 300 V B+, about 700–800 V HV, 12.6 VAC filaments.
- **Reform or bring up slowly on a variac** (5–10 min).

# 4. Restoration plan

## 4.1 Phase 0: Assess cold

- Photograph everything. Note mods, wrong-value parts (W2PA found four) and non-stock tubes. Record tube brands.
- Pull and label the tubes; test with a grid-leakage tester.
- Check bandswitch rotor orientation.
- Inspect: C304 area, R940/R701, RL1/RL2, belts, the LMO phono cable and the **#1815 lamp**.
- Resistance-check B+, HV, bias and filament lines to chassis against the manual before connecting a supply.

## 4.2 Phase 1: HP-23

Recap and test the HP-23 unloaded on a variac (§3.6).

## 4.3 Phase 2: Clean

Vacuum and brush. Use cleaner on swabs, not sprayed. DeoxIT all rotary switches, pots, the **SSB/CW slide switch** and the **internal/external LMO switch**. Keep cleaner off the dial plastic.

## 4.4 Phase 3: Recap and resistor survey

1. Replace all **7 electrolytics**, including **C941/C942**. Watch C945's polarity.
2. Replace the Mylars and film caps listed in §3.1.
3. Survey resistors by board: audio → modulator → IF → bandpass → RF driver. Replace anything > 10% out (or near the limit), using carbon film.
4. Apply the drift fixes: R106, R107, R6/R7.
5. Replace leaky silicon diodes (SB-101-4 list). Optionally replace FH-110s with 1N5711s.
6. Clean RL1/RL2.

## 4.5 Phase 4: Mechanical and LMO

Fit the belts; apply the comb-bracket/lockwasher checks; do the LMO mounting fix (SB-102-4); clean the LMO jack; clean the wiper; set the dial roller tension.

## 4.6 Phase 5: First power-up

1. HP-23 on a variac; dummy load and speaker connected.
2. Confirm the dial lamp is lit and the LMO supply is about +10 V.
3. Log voltages against the manual chart.
4. Calibrator receive check on all bands.
5. **Set bias before any transmit.**

## 4.7 Phase 6: Alignment

Order: **receiver first.** Warm up 30 min. Use the plastic tool. **Never adjust the sealed 6.8 MHz trap.** Leave T202 alone unless sweep-aligning.

**Receiver** (shared SB/HW-10x method; verify the manual's test-point values)

1. **S-meter zero:** RF gain fully CW, antenna off. If it won't zero, see R107 (SB-102-1 → SB-101-1/-9).
2. **Heterodyne oscillator coils:** 11 MΩ meter at the bandpass-board test point near V19. Target as high as possible between −0.5 and −2.0 V, **not exceeding −2.0 V**; approach from the slow-changing side.
3. **IF:** peak T201, T102 (top and bottom slugs) and T103 on the calibrator. Keep slugs toward the outside of the cans to avoid overcoupling (K5BCQ).
4. **LMO:**
   - Endpoints 5.000/5.500 MHz.
   - Zero-set the dial.
   - Adjust the LMO frequency shifter for zero beat in LSB vs. USB at the same dial setting.
   - Expect 3–10 kHz error between 100 kHz points (§3.4).
5. **Driver grid/plate coils** in order: 3.5 → 29 → 21 → 14 → 7, preselector matched, peak the S-meter. On 40 m, check SB-102-5 has been done.

**Receiver benchmark (W5RKL):** calibrator about S9+20 at 3.7 MHz, about S6 at 29.2 MHz.

**IF centering check:**
- Switch LSB ↔ USB on a quiet band. The noise level should match.
- Carriers should sit 1.4 kHz each side of the filter center (USB 3396.4, LSB 3393.6 kHz for a 3395.0 kHz center).
- To retrim a crystal, 100 pF in series moves it up about 100 Hz; 10 pF in parallel moves it down about 100 Hz.

**Transmitter** (dummy load; short key-downs; cool down between tries)

1. **Bias:**
   - PTT, no drive, METER on PLATE.
   - Set BIAS ADJUST for about **50 mA** idle (W5RKL).
   - If it reads > 100 mA, hold PTT for only a few seconds.
2. **Grid drive (SB-102-3):**
   - METER on GRID. Advance MIC/CW LEVEL only until grid current just shows, then peak for **maximum grid drive**.
   - Full-scale grid current is normal after tune-up.
   - Roughly 0.3 mA on 3.5 MHz during initial setup (W5RKL).
3. **Carrier isolation (T1)** and **driver neutralizing wire:**
   - Grid current low on some bands with normal output usually means the driver neutralizing wire needs repositioning (W2PA).
   - A wire pushed too deep into hole W makes final neutralizing difficult.
4. **Final neutralization:**
   - Peak REL PWR and plate dip must coincide on FINAL.
   - Alternative (SB-102-8 → SB-100-2): **remove the accessory plug** to kill screen voltage, and disconnect HV (discharge first). Then on 28.5 in TUNE, peak the preselector and FINAL into a dummy load with an RF probe, and null the neutralizing capacitor.
   - If C913 bottoms out, see SB-102-27.
5. **Carrier null:**
   - Equal on USB/LSB; spec −45 dB or better.
   - If the trimmer is fully meshed, see SB-102-18.
6. **Loading:**
   - Dip to about **225–250 mA** at full drive; REL PWR peaks with the dip.
   - Limit key-down to 30 s and cool ≥ 1 min.
7. **Calibrator:** zero-beat against WWV (10/15 MHz) during an unmodulated period.
8. Re-neutralize after any tube change.

## 4.8 Phase 7: Optional improvements (reversible)

The HW-101/SB-10x improvements apply (WB8JKR, K4JPN):

- 10 MΩ meter-zero resistor for transmit.
- C17 12 → 100 pF and R6/R7 → 27 kΩ for more BFO injection.
- C119 500 pF → .001 µF for less raspy audio.
- C11 .001 → .01 µF.
- R328 470 kΩ → 1 kΩ for faster CW relay pull-in.
- Sidetone bleed fixes.
- .01 µF filament bypass for birdies.
- 200 Ω multi-turn carrier-null pot.
- A replacement solid-state LMO regulator (e.g. LM317-based) is used by some owners. Keep the original if possible.

# 5. Bulletin quick-reference (symptom → bulletin)

| Symptom | SB-102 bulletin (→ referenced) |
|---|---|
| Meter won't zero / S-meter drift | -1 (→ SB-101-1), -22 (→ HW-100-17), -30 |
| Heterodyne-osc tube failures | -2 (R212) |
| Low grid drive / alignment | -3, -9, -26 |
| LMO instability | -4; SB-101-12, -16 |
| Low 40 m output | -5 |
| VOX / audio | -6 (→ SB-100-3), -19, -24, -29 |
| Instability / oscillation | -7 (→ SB-100-4), -9, -10 |
| Neutralizing | -8 (→ SB-100-2), -27 |
| Relays hang / chirp | -14, -21 |
| AGC / IF sensitivity | -15, -23, -25 |
| Calibrator spurs | -16 |
| Preselector / loading | -17, -20 |
| Carrier null | -18 |
| Germanium diodes | -28 |

# 6. Operation notes

- HP-23 switch on **300 V**.
- Always use a load.
- Zero-set on the nearest 100 kHz calibrator point, because of the LMO's between-point error.
- The internal/external LMO switch must be on INT unless an SB-640 or similar is connected. A wrong setting looks like a dead receiver.

# 7. Tips and tricks

- Keep spare **#1815** lamps; a dark dial can mean a dead LMO.
- Keep GE 6AU6s for V3/V4, a 6HS6 (or a 6AU6 substitute), and a matched 6146 pair.
- Label tubes and photograph switch rotors before disassembly.
- Measure resistors with one lead lifted.
- Reform electrolytics slowly (5–10 min ramp) if you are keeping any.

# 8. Maintenance schedule

| Interval | Task |
|---|---|
| Each session | 30 min warm-up; calibrator zero-set. |
| 6 months | Exercise switches; check S-meter zero. |
| Yearly | Bias, carrier null, grid drive; receive benchmark; LMO phono jack clean. |
| 2–3 years | Neutralization check; retighten the driver board and comb screws; DeoxIT. |
| 5 years | Tube test (V3/V4/V10 and finals first); belts; HP-23 check; C941/C942 ESR. |
| ~15–20 years | Re-check electrolytics. |

# 9. Parts sources

- SB-102 restoration kits (e.g. W8AOR; eBay kit sellers): electrolytics including C941/C942, films, diodes, zeners, R6/R7, carrier-null pot.
- Mouser / Digi-Key; HP-23 rebuild kits.
- Heathkit-Radios and heathkit (groups.io): LMOs, crystals, dials, belts.

# 10. Sources

- Heath, *SB-102 Assembly Manual* and *Alignment & Schematic* (W5RKL); SB-102 LMO schematic (archive.org).
- Heath list (puck.nether.net): SB-102 bulletins (Aug 1997, Apr 1998); SB-101 and SB-100 bulletins (Apr 1998); HW-101 bulletins (Jul 1997).
- W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting*.
- K5BCQ, SB-101/102 Repair Site; Nostalgic Kits Central SB-10x troubleshooting; W2PA SB-101 rebuild notes.
- Heath list threads: "SB-102 LMO" (Jun 2002, K9STH), "SB-101 vs 102" (Oct 2009).
- W8AOR SB-102 restoration kit parts list.

# 11. Pending manual cross-check

Items to verify against the manual:

- V-number assignments.
- The full electrolytic list (C2, C12, C212, C304, C941, C942, C945) and ratings.
- The LMO supply circuit (lamp, 56 Ω, D907, 2N3567).
- Bias and grid-drive procedure and readings.
- Heterodyne test-point values.
- Pilot lamp types.
- R116 value.
- The manual's alignment and troubleshooting charts.
