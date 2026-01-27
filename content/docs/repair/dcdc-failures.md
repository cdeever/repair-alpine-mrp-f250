---
title: "DC/DC Converter Failures"
weight: 1
---

# DC/DC Converter Failures

The DC/DC converter is a common failure point in car amplifiers due to the high currents and switching stresses involved.

## Failure Mode: Switching FETs (Q903-Q906)

### Symptoms
- Blown fuses
- No output
- Smoke from power supply area
- Low or no voltage on secondary rails

### Root Causes

| Cause | How It Happens |
|-------|----------------|
| Speaker short | Overcurrent reflects back to power supply |
| Output transistor failure | Creates dead short on secondary |
| Gate driver failure | FETs run in linear mode, overheat |
| Shoot-through | Both FETs on simultaneously |
| Overvoltage spike | Inductive kick from transformer |

### Diagnosis

1. Remove FETs from circuit
2. Test each FET:

```
MOSFET Test (N-Channel)
─────────────────────────
Gate-Source: Should be OL both ways
Gate-Drain: Should be OL both ways
Drain-Source: May show body diode (~0.5V one direction)

If any shows low resistance in both directions = SHORTED
```

3. **If FETs are shorted, DO NOT just replace them** - Find the root cause first

### Repair Procedure

1. **Test gate drivers Q901/Q902** (see below)
2. **Test secondary rectifiers D801-D803, D808**
3. **Test transformer T901 windings**
4. **Check secondary rail loads**
5. Only after all checks pass, replace FETs

### FET Specifications (2SK3662)

| Parameter | Value |
|-----------|-------|
| Vds | 60V |
| Id | 45A |
| Vgs(th) | 2-4V |
| Rds(on) | ~14mΩ |

### Acceptable Substitutes

| Part Number | Manufacturer | Notes |
|-------------|--------------|-------|
| FDPF041N06BL1 | ON Semi | Lower Rds(on), verified compatible |
| IRFZ44N | Various | Common substitute, verify Vgs(th) |
| FQP50N06 | Fairchild | 60V/50A, similar specs |

{{< hint warning >}}
**Critical: Gate Threshold Voltage**
If the substitute has a higher Vgs(th), it may not fully turn on with the existing gate drive circuit, causing it to operate in linear mode and overheat.
{{< /hint >}}

---

## Failure Mode: Gate Drivers (Q901/Q902)

### Symptoms
- FETs run hot even with low load
- Supply voltage drops but doesn't go to zero
- FETs fail repeatedly after replacement
- Voltage clamps at ~8-10V under load

### Root Causes

| Cause | How It Happens |
|-------|----------------|
| FET failure | Gate driver overloaded trying to drive shorted FET |
| Overvoltage | Transient from FET switching |
| Age/heat | Gradual degradation |

### Diagnosis

Test Q901 and Q902 (2SB1132 PNP):

```
PNP Transistor Test
───────────────────
Red lead on Base, Black on Emitter: 0.5-0.7V
Red lead on Base, Black on Collector: 0.5-0.7V
All other combinations: OL (open)

If E-C or C-E shows low resistance = SHORTED
If B-E or B-C shows OL = OPEN
```

### Repair Procedure

1. Remove Q901 and Q902
2. Test out of circuit
3. Replace both if either is failed (they work as a pair)
4. Reinstall and verify gate drive signal at FET gates

### Part Specifications (2SB1132)

| Parameter | Value |
|-----------|-------|
| Type | PNP |
| Vceo | 32V |
| Ic | 1A |
| Pd | 0.9W |
| hFE | 180-390 |

---

## Failure Mode: Secondary Rectifiers (D801-D803, D808)

### Symptoms
- High current draw with FETs installed
- One or more rails dead
- Smoke from rectifier area

### Diagnosis

Test each diode with FETs removed:

```
Diode Test
──────────
Forward (Red→Anode, Black→Cathode): 0.3-0.6V
Reverse (Red→Cathode, Black→Anode): OL

If reverse shows voltage = SHORTED
If forward shows OL = OPEN
```

### Rectifier Specifications

| Diode | Part | Voltage | Current | Type |
|-------|------|---------|---------|------|
| D802 | FCH10A15 | 150V | 10A | Fast recovery |
| D803 | FRH10A15 | 150V | 10A | Fast recovery |
| D801 | 11EFS2 | 200V | 1A | Fast recovery |
| D808 | 11EFS2 | 200V | 1A | Fast recovery |

---

## Failure Mode: Transformer (T901)

### Symptoms
- Very high primary current
- No output on all secondary rails
- Burning smell from transformer
- Audible buzzing

### Diagnosis

Measure winding resistances:

| Winding | Expected |
|---------|----------|
| Primary | 0.1-0.5Ω |
| Each Secondary | 0.1-0.5Ω |
| Between any two isolated windings | OL (infinite) |

**Failure Indicators:**
- 0.0Ω on any winding = shorted turns
- OL on any winding = open circuit
- Low resistance between windings = insulation breakdown

### Repair

The transformer is a custom Alpine part that is essentially unobtainable. Your options are:

1. **Rewind the existing transformer** - If the ferrite core is intact
2. **Find a donor** - From another MRP-F250 or similar amplifier
3. **Custom fabrication** - Wind a new transformer from scratch

See [Transformer Repair]({{< relref "/docs/repair/transformer-repair" >}}) for detailed procedures on rewinding and fabrication.

---

## Cascade Failure Analysis

When diagnosing DC/DC converter problems, understand the typical failure cascade:

```
Initial Fault (e.g., speaker short)
            │
            ▼
Output transistors fail (dead short on ±25V)
            │
            ▼
Massive current through secondary rectifiers
            │
            ▼
Rectifiers may short OR current reflects to primary
            │
            ▼
FETs fail (overcurrent or shoot-through)
            │
            ▼
Gate drivers may fail (trying to drive shorted FETs)
            │
            ▼
PWM controller may fail (overvoltage transients)
```

**Key Point:** If you only replace the FETs without checking everything downstream, the same failure will recur.
