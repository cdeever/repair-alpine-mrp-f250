---
title: "Reference"
weight: 5
bookCollapseSection: false
---

# Quick Reference

Quick lookup tables for common repair tasks.

## Transistor Test Quick Reference

### NPN Transistor (e.g., 2SC series)

```
                Collector
                    │
                    │
              ┌─────┴─────┐
     Base ────┤           │
              └─────┬─────┘
                    │
                    │
                 Emitter

Test (Diode Mode):
  Red on Base, Black on Emitter: 0.5-0.7V ✓
  Red on Base, Black on Collector: 0.5-0.7V ✓
  All other combinations: OL ✓
```

### PNP Transistor (e.g., 2SA series)

```
                Collector
                    │
                    │
              ┌─────┴─────┐
     Base ────┤           │
              └─────┬─────┘
                    │
                    │
                 Emitter

Test (Diode Mode):
  Red on Emitter, Black on Base: 0.5-0.7V ✓
  Red on Collector, Black on Base: 0.5-0.7V ✓
  All other combinations: OL ✓
```

### N-Channel MOSFET (e.g., 2SK series)

```
              Drain
                │
        ┌───────┤
  Gate ─┤       │
        └───────┤
                │
              Source

Test (Diode Mode):
  Gate to Source: OL both ways ✓
  Gate to Drain: OL both ways ✓
  Source to Drain: May show body diode ~0.5V
```

---

## Voltage Rail Quick Check

| Rail | Test Point | Expected | If Wrong |
|------|------------|----------|----------|
| +B | Battery terminal | 12-14.4V | Check fuses |
| +25V | After D802 | +25V | Check DC/DC |
| -25V | After D803 | -25V | Check DC/DC |
| +14V | At op-amp pin 8 | +14V | Check Q801 |
| -14V | At op-amp pin 4 | -14V | Check Q802 |
| Vref | IC920 pin 14 | +5V | Check IC920 |

---

## Current Limits for Testing

| Test Phase | Current Limit | Notes |
|------------|---------------|-------|
| Initial power-up | 100mA | Just control circuits |
| IC920 check | 200mA | Verify PWM |
| FETs installed, no load | 500mA | DC/DC running |
| Light load test | 2A | With dummy load |
| Full test | 5A+ | Monitor temperature |

---

## Resistance to GND (FETs Removed)

| Point | Normal | Indicates Short |
|-------|--------|-----------------|
| +25V rail | >100Ω (rises) | <10Ω |
| -25V rail | >100Ω (rises) | <10Ω |
| FET drain pads | >100Ω | <10Ω |

---

## Component Cross-Reference

### Transistor Equivalents

| Original | Possible Substitutes |
|----------|---------------------|
| 2SK3662 | IRFZ44N, FQP50N06, FDPF041N06 |
| 2SC5100 | 2SC3281, MJE15032 |
| 2SA1908 | 2SA1302, MJE15033 |
| 2SC2235 | 2SC1815 (lower power) |
| 2SA965 | 2SA1015 (lower power) |

{{< hint warning >}}
Always verify specifications match, especially voltage ratings and Vgs threshold for MOSFETs.
{{< /hint >}}

---

## Pinouts

### uPC494 (IC920)

```
         ┌───────────┐
   1 NFB │1    U   16│ GND
   2 NFB │2        15│ DTC
   3 IFB │3        14│ Vref
     GND │4        13│ Out B
      Ct │5        12│ Vcc
      Rt │6        11│ Vcc
     GND │7        10│ Out B
   Out A │8         9│ Out A
         └───────────┘
```

### NJM4565 (IC501-518)

```
         ┌───────────┐
  Out A  │1    U    8│ V+
  -In A  │2         7│ Out B
  +In A  │3         6│ -In B
     V-  │4         5│ +In B
         └───────────┘
```

---

## Service Manual Page Reference

| Topic | Page |
|-------|------|
| Specifications | 5 |
| Block Diagram | 6 |
| Parts Layout (Component) | 7 |
| Parts Layout (Foil) | 8 |
| Schematic (Preamp) | 9 |
| Schematic (Power Amp) | 10 |
| Schematic (Power Supply) | 11 |
| Terminal Voltages | 12 |
| Exploded View | 13 |
| Parts List | 14-25 |
