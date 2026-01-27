---
title: "Preamp Stage"
weight: 2
---

# Preamp Stage

The preamp stage processes the input signal before it reaches the power amplifier. It provides input selection, isolation, gain control, equalization, and crossover filtering.

## Signal Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                            PREAMP STAGE (Per Channel Pair)                  │
│                                                                             │
│  RCA Input ─┐                                                               │
│             ├──▶ ISO AMP ──▶ GAIN ──▶ BASS EQ ──▶ VARIABLE ──▶ To Power   │
│  SP Input ──┘    IC501-504   VR501-4   SW501/503   FILTER       Amp        │
│                                                    VR503/504               │
│                                                                             │
│  LINE OUT ◀──── BUFFER ◀──── MIX                                           │
│                 IC507-508                                                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Input Section

### Input Types

The amplifier accepts two input types:

| Input | Connector | Sensitivity Range |
|-------|-----------|-------------------|
| RCA (Line Level) | PJ501 | 0.2V - 4.0V |
| Speaker Level | CN501 | 0.4V - 8.0V |

### Isolation Amplifiers (IC501-IC504)

Each channel has an isolation amplifier (NJM4565M dual op-amp) that:
- Provides input buffering
- Allows mixing of RCA and speaker-level inputs
- Isolates the input from downstream stages

**Normal Voltages (all NJM4565 op-amps):**

| Pin | Function | Voltage |
|-----|----------|---------|
| 1-3 | Inputs/Outputs | 0V (signal dependent) |
| 4 | V- | -14V |
| 8 | V+ | +14V |

## Gain Control

### Variable Gain (VR501-VR504)

Four 9-turn precision potentiometers control input sensitivity:

| Control | Channels | Range |
|---------|----------|-------|
| VR501 | CH1/CH2 | 10K dual pot |
| VR502 | CH1/CH2 | 10K dual pot |
| VR503 | CH3/CH4 | 50K quad pot |
| VR504 | CH3/CH4 | 50K quad pot |

## Bass EQ

### Bass Boost (SW501, SW503)

Channels 3 and 4 feature selectable bass EQ:

| Switch | Position | Effect |
|--------|----------|--------|
| SW501 | FLAT | No boost |
| SW501 | BOOST | Bass enhancement |
| SW503 | FLAT | No boost |
| SW503 | BOOST | Bass enhancement |

The bass EQ circuit uses additional op-amp stages (IC513, IC515) to provide low-frequency boost when enabled.

## Variable Crossover Filters

### Filter Types

Each channel pair has independent crossover filters controlled by VR503/VR504 and SW502:

| Setting | Filter Type | Frequency Range |
|---------|-------------|-----------------|
| HPF | High Pass | Variable |
| LPF | Low Pass | Variable |
| FLAT | Full Range | No filtering |

### Subsonic Filter

A subsonic filter removes ultra-low frequencies (<20Hz) that waste amplifier power and can damage speakers.

## Op-Amp Inventory

The preamp stage uses 18 NJM4565M dual op-amps:

| IC | Function |
|----|----------|
| IC501-504 | Input isolation amplifiers |
| IC505-506 | Gain stages |
| IC507-508 | Buffer/Line out |
| IC509-510 | Filter stages |
| IC511-512 | Filter stages |
| IC513-515 | Bass EQ / Filter |
| IC516-518 | Output buffer to power amp |

All op-amps are powered from the ±14V regulated rails.

## Line Output

### Pre-Out (RCA)

The amplifier provides buffered line outputs for daisy-chaining to additional amplifiers:

| Output | Channels | Level |
|--------|----------|-------|
| PRE OUT CH1+3 | Mixed 1&3 | 0.5V @ 0.5V input |
| PRE OUT CH2+4 | Mixed 2&4 | 0.5V @ 0.5V input |

## Common Preamp Issues

### No Output (One or More Channels)

1. Check op-amp supply voltages (±14V rails)
2. Verify input connections
3. Test gain potentiometer continuity
4. Check for failed op-amps (rare)

### Distorted Output

1. Check ±14V rail voltages (should be stable)
2. Look for damaged filter capacitors
3. Verify gain settings aren't too high

### Noise/Hum

1. Check ground connections
2. Verify input isolation
3. Look for damaged input protection diodes (D101-D502)
