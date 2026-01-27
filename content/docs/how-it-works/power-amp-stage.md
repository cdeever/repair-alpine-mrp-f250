---
title: "Power Amplifier Stage"
weight: 3
---

# Power Amplifier Stage

The power amplifier converts the low-level preamp signal into high-current drive for speakers. The MRP-F250 uses a Class AB topology with discrete transistors for each of its four channels.

## Architecture

Each channel is identical in design. The schematic shows channels 1-4 with components numbered in the 100s, 200s, 300s, and 400s respectively.

```
┌────────────────────────────────────────────────────────────────────────────┐
│                         POWER AMP (Per Channel)                            │
│                                                                            │
│  From      ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐       │
│  Preamp ──▶│  INPUT  │───▶│ VOLTAGE │───▶│ DRIVER  │───▶│ OUTPUT  │──▶ SP │
│            │  STAGE  │    │  AMP    │    │  STAGE  │    │  STAGE  │       │
│            │  Q151   │    │Q153-158 │    │Q159-160 │    │Q161-162 │       │
│            └─────────┘    └─────────┘    └─────────┘    └─────────┘       │
│                 │              │              │              │             │
│                 └──────────────┴──────────────┴──────────────┘             │
│                              FEEDBACK NETWORK                              │
│                                                                            │
│                              ┌─────────┐                                   │
│                              │  MUTE   │◀── From Protection               │
│                              │ Q152    │                                   │
│                              └─────────┘                                   │
└────────────────────────────────────────────────────────────────────────────┘
```

## Stage-by-Stage Analysis

### Input Stage (Q151)

**Transistor:** 2SC3326 (NPN)

The input stage is a common-emitter amplifier that:
- Accepts the signal from the preamp output buffer
- Provides initial voltage gain
- Sets the input impedance

**Key Components:**
- R151: 100K input resistor
- C151: 10µF coupling capacitor
- D151: DAN217 protection diode

### Mute Stage (Q152)

**Transistor:** 2SC4207 (NPN)

The mute transistor shunts the signal to ground when protection circuits activate:
- When muted: Q152 conducts, shorting the signal path
- When unmuted: Q152 is off, signal passes through

### Voltage Amplifier Stage (Q153-Q158)

This multi-transistor stage provides the bulk of the voltage gain.

| Transistor | Part | Function |
|------------|------|----------|
| Q153 | 2SC2412K | Differential input |
| Q154 | 2SC2412K | Differential input |
| Q155 | 2SA988F | Current source |
| Q156 | 2SC1841F | Voltage amp |
| Q157 | 2SC2412K | Driver |
| Q158 | 2SC2713 | Driver |

**Bias Network:**
- R154, R155, R156: Set operating points
- C152-C154: Compensation capacitors (22pF, 47pF)

### Driver Stage (Q159, Q160)

**Transistors:**
- Q159: 2SC2235 (NPN) - Drives positive output transistor
- Q160: 2SA965 (PNP) - Drives negative output transistor

The driver stage:
- Provides current gain to drive the output transistors
- Operates in Class AB with slight overlap
- Powered from ±25V rails

### Output Stage (Q161, Q162)

**Transistors:**
- Q161: 2SC5100 (NPN) - Positive half-cycle
- Q162: 2SA1908 (PNP) - Negative half-cycle

The output stage:
- Complementary emitter-follower configuration
- Powered from ±23V rails
- Current sensing via emitter resistors (R168, R169 = 2.2Ω)

**Output Current Detection:**
- Q163 (2SC2412K) monitors output current
- Feeds back to protection circuit

## Transistor Inventory (Per Channel)

| Ref (CH1) | Part | Type | Function |
|-----------|------|------|----------|
| Q151 | 2SC3326 | NPN | Input amp |
| Q152 | 2SC4207 | NPN | Mute switch |
| Q153 | 2SC2412K | NPN | Diff amp |
| Q154 | 2SC2412K | NPN | Diff amp |
| Q155 | 2SA988F | PNP | Current source |
| Q156 | 2SC1841F | NPN | Voltage amp |
| Q157 | 2SC2412K | NPN | Pre-driver |
| Q158 | 2SC2713 | NPN | Pre-driver |
| Q159 | 2SC2235 | NPN | Driver (+) |
| Q160 | 2SA965 | PNP | Driver (-) |
| Q161 | 2SC5100 | NPN | Output (+) |
| Q162 | 2SA1908 | PNP | Output (-) |
| Q163 | 2SC2412K | NPN | Current sense |

**Note:** Channels 2, 3, 4 use identical transistors with 2xx, 3xx, 4xx numbering.

## Normal Operating Voltages

From service manual page 12, with 14.4V supply, Remote ON, no signal:

| Transistor | E | C | B |
|------------|---|---|---|
| Q151/251/351/451 | 0V | 0V | -7V |
| Q152/252/352/452 | -0.6V | 24V | 0V |
| Q153/253/353/453 | 24.5V | 25V | 25V |
| Q154/254/354/454 | -24.5V | -24V | -24V |
| Q155/255/355/455 | 24.5V | 1.1V | 24V |
| Q156/256/356/456 | -24.5V | -1.1V | -24V |
| Q157/257/357/457 | -0.5V | 1.1V | 1.1V |
| Q158/258/358/458 | -1.1V | 0.5V | -0.5V |
| Q159/259/359/459 | 0.5V | 25V | 1.1V |
| Q160/260/360/460 | -0.5V | -25V | -1.1V |
| Q161/261/361/461 | 0V | 23V | 0.5V |
| Q162/262/362/462 | 0V | -23V | -0.5V |
| Q163/263/363/463 | 0V | 23V | 0V |

## Common Power Amp Failures

### Output Transistor Failure (Q161/Q162)

**Symptoms:**
- Blown fuses
- DC on speaker output
- Smoke from output stage area

**Common Causes:**
- Speaker short circuit
- Operating into too-low impedance
- Thermal overload

**Test:** Check E-C and B-C junctions with diode mode

### Driver Transistor Failure (Q159/Q160)

**Symptoms:**
- Distorted output
- One half of waveform missing
- Can cause output transistor failure

**Test:** Check all junctions, compare to known-good channel

### DC Offset at Output

**Symptoms:**
- Speaker cone pushes in or out at idle
- Excessive heat in output stage
- Protection circuit may trigger

**Causes:**
- Mismatched or failed transistor in differential pair
- Failed bias components
- DC contamination from preamp

**Test:** Measure DC voltage at speaker output (should be <50mV)
