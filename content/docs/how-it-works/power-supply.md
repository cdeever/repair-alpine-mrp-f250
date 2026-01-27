---
title: "Power Supply"
weight: 1
---

# Power Supply (DC/DC Converter)

The power supply is the heart of the amplifier, converting 12-14.4V automotive power into the multiple voltage rails required by the amplifier stages.

## Block Diagram

```
                                    ┌─────────────────────────────────────┐
                                    │         SECONDARY SIDE              │
┌────────────┐                      │                                     │
│  BATTERY   │     ┌─────────┐      │   ┌───────┐    ┌─────────┐         │
│  +12-14.4V │────▶│  FUSES  │──────┼──▶│ T901  │───▶│ D802/803│──▶ ±25V │
│            │     │ F901/902│      │   │ XFMR  │    │ Rectify │         │
└────────────┘     │  15A×2  │      │   └───┬───┘    └─────────┘         │
                   └────┬────┘      │       │                             │
                        │           │       │        ┌─────────┐         │
                        ▼           │       └───────▶│ D801/808│──▶ ±23V │
                   ┌─────────┐      │                │ Rectify │         │
                   │  L920   │      │                └─────────┘         │
                   │ Choke   │      │                                     │
                   └────┬────┘      └─────────────────────────────────────┘
                        │
                        ▼
┌───────────────────────────────────────────────────────────────┐
│                      PRIMARY SIDE                              │
│                                                                │
│   ┌─────────┐      ┌─────────────┐      ┌──────────────────┐  │
│   │  IC920  │─────▶│  Q901/Q902  │─────▶│  Q903-Q906       │  │
│   │ uPC494  │      │ Gate Drivers│      │  Switching FETs  │  │
│   │   PWM   │      │  2SB1132×2  │      │  2SK3662×4       │  │
│   └─────────┘      └─────────────┘      └────────┬─────────┘  │
│        │                                         │             │
│        │           ┌─────────────┐               │             │
│        └──────────▶│  Feedback   │◀──────────────┘             │
│                    │  & Control  │                             │
│                    └─────────────┘                             │
└───────────────────────────────────────────────────────────────┘

                              │
                              ▼
              ┌───────────────────────────────┐
              │      VOLTAGE REGULATORS       │
              │                               │
              │   Q801 (2SC3421) ──▶ +14V    │
              │   Q802 (2SA1358) ──▶ -14V    │
              └───────────────────────────────┘
```

## Component Details

### PWM Controller: IC920 (uPC494)

The uPC494 is a classic PWM controller IC that generates the switching signals for the DC/DC converter.

**Key Functions:**
- Generates two complementary PWM outputs (pins 8 and 11)
- Internal 5V reference (pin 14)
- Dead-time control to prevent shoot-through
- Error amplifier for voltage regulation feedback

**Pin Functions:**

| Pin | Function | Normal Voltage |
|-----|----------|----------------|
| 1 | Non-inv input | 1.6V (varies w/temp) |
| 2 | Non-inv input | 2.5V |
| 3 | Inv input | 0.1V |
| 4 | GND | 0V |
| 5 | Ct (timing cap) | 1.8V |
| 6 | Rt (timing res) | 3.8V |
| 7 | GND | 0V |
| 8 | Output A | Pulsing (to FETs) |
| 11 | Vcc | 14.4V |
| 12 | Vcc | 14.4V |
| 14 | Vref | 5.0V |
| 15 | Dead time | 2.5V |
| 16 | GND | 0V |

### Gate Drivers: Q901, Q902 (2SB1132)

These PNP transistors amplify the PWM signals from IC920 to drive the FET gates with sufficient current.

**Why They Matter:**
- FET gates require significant charge to switch quickly
- Weak gate drive = FETs stuck in linear mode = thermal runaway
- If damaged, FETs may partially conduct and overheat

**Normal Voltages:**

| Terminal | Q901 | Q902 |
|----------|------|------|
| Emitter | 5.6V | 5.6V |
| Collector | 0V | 0V |
| Base | 6V | 6V |

### Switching FETs: Q903-Q906 (2SK3662)

Four N-channel MOSFETs in a push-pull configuration drive the transformer primary.

**Specifications:**
- Vds: 60V
- Id: 45A continuous
- Vgs(th): 2-4V
- Rds(on): ~14mΩ

**Normal Voltages (running):**

| Terminal | Voltage |
|----------|---------|
| Gate | 5.6V |
| Drain | 14.4V |
| Source | 0V |

### Transformer: T901

Center-tapped push-pull transformer with 5:8:1 ratio.

**Windings:**
- Primary: Connected to FET drains
- Secondary 1: Feeds ±25V rectifiers
- Secondary 2: Feeds ±23V rectifiers

### Secondary Rectifiers

| Diode | Part | Rail | Type |
|-------|------|------|------|
| D802 | FCH10A15 | +25V | Fast recovery |
| D803 | FRH10A15 | -25V | Fast recovery |
| D801 | 11EFS2 | +23V | Fast recovery |
| D808 | 11EFS2 | -23V | Fast recovery |

### Voltage Regulators

| Transistor | Part | Output | Type |
|------------|------|--------|------|
| Q801 | 2SC3421 | +14V | NPN linear regulator |
| Q802 | 2SA1358 | -14V | PNP linear regulator |

## Common Failure Modes

### 1. FET Failure (Q903-Q906)
**Cause:** Overcurrent, shoot-through, or downstream short
**Symptom:** No output, blown fuses, possible smoke
**Note:** FETs rarely fail alone - always check root cause

### 2. Gate Driver Failure (Q901/Q902)
**Cause:** Often fails with or causes FET failure
**Symptom:** FETs run in linear mode, overheat
**Test:** Check transistor junctions with diode mode

### 3. Rectifier Diode Failure (D801-D803, D808)
**Cause:** Overcurrent from output stage failure
**Symptom:** Dead short on secondary, heavy primary current
**Test:** Check forward/reverse with diode mode

### 4. Transformer Failure (T901)
**Cause:** Shorted turns from overcurrent
**Symptom:** Heavy primary current, possible overheating
**Test:** Measure winding resistance, check for shorts between windings

## Diagnostic Entry Points

When troubleshooting the power supply:

1. **Start with IC920** - Verify 5V Vref and PWM pulses
2. **Check gate drivers** - Q901/Q902 junction tests
3. **Test rectifiers** - Forward/reverse diode tests
4. **Measure rail loads** - Resistance from each rail to GND
5. **Verify transformer** - Winding resistance and isolation
