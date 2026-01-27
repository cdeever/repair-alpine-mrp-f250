---
title: "Preamp Failures"
weight: 3
---

# Preamp Stage Failures

Preamp failures are less common than power supply or output stage failures, but can cause complete loss of signal or distortion.

## Failure Mode: No ±14V Rails

### Symptoms
- No audio output (all channels)
- LED may still light
- Power supply otherwise functional

### Cause
The ±14V regulators (Q801/Q802) have failed.

### Diagnosis

1. Measure +14V and -14V rails
2. If missing, check:
   - Q801 (2SC3421) for +14V
   - Q802 (2SA1358) for -14V
3. Test transistors as described in [DC/DC Failures]({{< relref "/docs/repair/dcdc-failures" >}})

### Repair
Replace Q801 and/or Q802 as needed.

---

## Failure Mode: Op-Amp Failure (IC501-IC518)

### Symptoms
- One or more channels dead
- Distortion on specific channel
- No signal at preamp output

### Diagnosis

1. Verify ±14V present at op-amp power pins
2. Signal trace through preamp stages
3. Compare voltages to working channel

### Op-Amp Specifications (NJM4565M)

| Parameter | Value |
|-----------|-------|
| Type | Dual op-amp |
| Supply | ±4V to ±18V |
| Package | SOP-8 |

All 18 op-amps are the same part (51E38257S01).

### Repair

Op-amps are surface-mount. Replacement requires:
1. Hot air rework station or fine-tip soldering
2. Verify polarity (pin 1 indicator)
3. Check solder bridges after installation

---

## Failure Mode: Input Protection Diode Failure

### Symptoms
- No input signal passes
- Input shorted to ground

### Components
- D101/D102, D201/D202, D301/D302, D401/D402 (DAP202/DAN202)
- Located at input stage

### Diagnosis
Check diodes in circuit - should show normal diode readings, not shorted.

---

## Failure Mode: Gain Potentiometer Failure

### Symptoms
- No sound or very low output on channel pair
- Scratchy sound when adjusting
- Intermittent output

### Components
- VR501, VR502: 10K dual pots (CH1/2)
- VR503, VR504: 50K quad pots (CH3/4)

### Diagnosis
1. Measure resistance across pot (should match rated value)
2. Check for open or intermittent wiper
3. Clean with contact cleaner or replace

---

## Failure Mode: Coupling Capacitor Failure

### Symptoms
- DC offset at output
- Loss of low frequencies
- Intermittent sound

### Common Coupling Capacitors
- C151/C251/C351/C451 (10µF) - Input coupling
- C122/C222/C322/C422 (0.1µF) - Stage coupling

### Diagnosis
Check capacitors for:
- Leakage (should be very high resistance)
- Open circuit (no signal passes)
- Low capacitance (check with capacitance meter)

---

## Signal Tracing Procedure

To isolate a preamp fault:

1. **Apply test signal** to RCA input (1kHz sine, ~0.5V)
2. **Trace signal** at these points:

| Point | Component | Expected |
|-------|-----------|----------|
| Input | PJ501 | 0.5V AC |
| After isolation amp | IC501 pin 7 | ~0.5V AC |
| After gain | VR501/502 wiper | Variable |
| After filter | IC511/512 output | Signal present |
| Preamp output | To power amp | Signal present |

3. **Signal disappears?** Fault is between last good point and dead point
4. **Signal distorted?** Check that stage's op-amp and associated components
