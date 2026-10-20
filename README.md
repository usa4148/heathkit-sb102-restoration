# Heathkit SB-102 Restoration Plan

A restoration plan for the Heathkit SB-102 HF SSB/CW transceiver (1970–1975) and its HP-23 power supply. It covers which parts fail with age, the solid-state LMO and its power supply, a phased restoration procedure, alignment and neutralization, a symptom-to-service-bulletin index, and maintenance.

**Status:** Rev. 1 is built from Heath service bulletins and field sources (W5RKL, K5BCQ, Nostalgic Kits Central, W2PA, the Heath list). The cross-check against the original Heath manual is pending.

## Files

- [`sb102-restoration-plan.md`](sb102-restoration-plan.md) — source (pandoc markdown)
- [`sb102-restoration-plan.pdf`](sb102-restoration-plan.pdf) — rendered PDF

## Build

```sh
pandoc sb102-restoration-plan.md -o sb102-restoration-plan.pdf \
  --pdf-engine=xelatex -V mainfont="DejaVu Serif"
```

## Safety

The HP-23 supplies about 700–800 V DC, which stays on the filter capacitors after unplugging. Discharge them before working inside.

## Sources

- Heath SB-102 manual and alignment/schematic: W5RKL (w5rkl.com/manuals)
- Heath SB-102/SB-101/SB-100/HW-101 service bulletins, transcribed on the Heath mailing list
- W5RKL, *Heathkit HW-101 and SB-10x Restoration and Troubleshooting*
- K5BCQ SB-101/102 Repair Site; Nostalgic Kits Central; W2PA SB-101 rebuild notes

See also: [heathkit-sb301-restoration](https://github.com/usa4148/heathkit-sb301-restoration) · [heathkit-hw101-restoration](https://github.com/usa4148/heathkit-hw101-restoration)
