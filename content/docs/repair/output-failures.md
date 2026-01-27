---
title: "Output Stage Failures"
weight: 2
---

# Output Stage Failures

The output stage handles the highest power levels and is subject to thermal and electrical stress.

## Failure Mode: Output Transistors (Q161/Q162, etc.)

### Symptoms
- Blown fuses
- DC voltage at speaker output
- One channel dead
- Smoke from output stage area
- Distortion on one channel

### Root Causes

| Cause | How It Happens |
|-------|----------------|
| Speaker short | Excessive current destroys transistor |
| Low impedance load | Operating below rated impedance |
| Thermal overload | Insufficient cooling, blocked airflow |
| Driver failure | Incorrect bias destroys output device |
| Oscillation | High-frequency instability |

### Diagnosis

Test each output transistor (with power off, FETs removed):

**Q161/Q261/Q361/Q461 (2SC5100 NPN):**
```
B-E: Red on B, Black on E = 0.5-0.7V
B-C: Red on B, Black on C = 0.5-0.7V
E-C, C-E: OL
```

**Q162/Q262/Q362/Q462 (2SA1908 PNP):**
```
B-E: Red on E, Black on B = 0.5-0.7V
B-C: Red on C, Black on B = 0.5-0.7V
E-C, C-E: OL
```

### Output Transistor Specifications

| Part | Q161 (2SC5100) | Q162 (2SA1908) |
|------|----------------|----------------|
| Type | NPN | PNP |
| Vceo | 100V | -100V |
| Ic | 8A | -8A |
| Pd | 80W | 80W |
| hFE | 55-160 | 55-160 |

### Repair Procedure

1. Test both complementary transistors (Q161 AND Q162)
2. **Replace as matched pairs** - Even if only one is failed
3. Check driver transistors (Q159/Q160)
4. Verify bias voltage after replacement
5. Check DC offset at output (<50mV)

{{< hint warning >}}
**Always replace complementary pairs together.** Mismatched transistors cause bias drift and reduced performance.
{{< /hint >}}

---

## Failure Mode: Driver Transistors (Q159/Q160, etc.)

### Symptoms
- Distorted output
- One half of waveform clipped
- Output transistor failure
- Excessive heat in driver area

### Driver Transistor Specifications

| Part | Q159 (2SC2235) | Q160 (2SA965) |
|------|----------------|---------------|
| Type | NPN | PNP |
| Vceo | 120V | -120V |
| Ic | 0.8A | -0.8A |
| Pd | 0.9W | 0.9W |

### Diagnosis

Same testing procedure as output transistors. Check all junctions.

---

## Failure Mode: DC Offset at Output

### Symptoms
- Speaker cone pushed in or out at idle
- Speaker runs warm with no audio
- Protection circuit triggers
- Gradual speaker damage

### Acceptable Range
DC offset should be **less than 50mV** at the speaker terminals.

### Causes

| Cause | Likely Failed Component |
|-------|------------------------|
| Mismatched transistors | Q153/Q154 differential pair |
| Failed bias components | R154, R155, R156 |
| DC leaking from preamp | Check preamp coupling caps |
| Failed output transistor | Leaky junction |

### Diagnosis

1. Measure DC voltage at speaker output (should be <50mV)
2. If high, measure voltages in that channel per [Terminal Voltages]({{< relref "/docs/diagnostics/terminal-voltages" >}})
3. Compare to a known-good channel
4. Identify which stage has incorrect voltages

---

## Failure Mode: Bias/Thermal Runaway

### Symptoms
- Output stage runs very hot at idle
- Idle current increases over time
- May eventually smoke or fail

### Causes
- Failed bias transistor (Q158)
- Damaged thermistor
- Incorrect replacement parts

### Diagnosis

1. Measure quiescent (idle) current
2. Normal: ~50-100mA per channel
3. If much higher, bias circuit has a problem

---

## Channel-by-Channel Component Reference

All four channels are identical. Use this table to find the equivalent component:

| Function | CH1 | CH2 | CH3 | CH4 |
|----------|-----|-----|-----|-----|
| Input amp | Q151 | Q251 | Q351 | Q451 |
| Mute | Q152 | Q252 | Q352 | Q452 |
| Diff amp | Q153/154 | Q253/254 | Q353/354 | Q453/454 |
| Current source | Q155 | Q255 | Q355 | Q455 |
| Voltage amp | Q156 | Q256 | Q356 | Q456 |
| Pre-driver | Q157/158 | Q257/258 | Q357/358 | Q457/458 |
| Driver + | Q159 | Q259 | Q359 | Q459 |
| Driver - | Q160 | Q260 | Q360 | Q460 |
| Output + | Q161 | Q261 | Q361 | Q461 |
| Output - | Q162 | Q262 | Q362 | Q462 |
| Current sense | Q163 | Q263 | Q363 | Q463 |
