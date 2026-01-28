---
title: "Test Plan"
weight: 1
---

# Diagnostic Test Plan

This test plan was developed for a specific failure case but applies to general DC/DC converter and power supply troubleshooting.

{{< hint info >}}
**Fresh Start Approach:** This plan is structured to verify everything from scratch, setting aside previous assumptions. Let the measurements guide the diagnosis.
{{< /hint >}}

## Symptom Summary

- Smoked twice (original failure + after FET replacement)
- No visible damage on PCB
- IC920 (PWM controller) verified producing pulses
- With FETs installed: 12V input dropped to 8V, current climbed to 1.2A before smoke
- Output transistors tested OK
- B+ to GND resistance unstable (rises then falls)

## Pre-Test Setup

- [ ] Remove Q903, Q904, Q905, Q906 (switching FETs)
- [ ] Disconnect any speaker connections
- [ ] Disconnect RCA inputs
- [ ] Ensure remote turn-on wire is connected (or jump REM to B+)

---

## Phase 0: Fresh Start - Visual & Basic Verification

{{< hint warning >}}
**START HERE** - Before any electrical tests, verify the fundamentals with fresh eyes.
{{< /hint >}}

### 0.1 Visual Inspection

Examine the entire board carefully under good lighting (magnification helps).

| Check | Location | Observations |
|-------|----------|--------------|
| Burned/discolored components | Entire board | |
| Burned traces or pads | Near power section | |
| Cracked solder joints | Around transformer T901 | |
| Bulging capacitors | C905, C906, C806, C807 | |
| Physical damage to FET pads | Q903-Q906 locations | |
| Solder bridges | All ICs, fine-pitch areas | |
| Loose wires or connectors | All connectors | |
| Signs of previous repair | Entire board | |

### 0.2 Basic Continuity (Power Off, FETs Removed)

| Test | From | To | Expected | Actual | Pass/Fail |
|------|------|-----|----------|--------|-----------|
| Fuse F901 | Pin 1 | Pin 2 | <0.1Ω (or OL if blown) | | |
| Fuse F902 | Pin 1 | Pin 2 | <0.1Ω (or OL if blown) | | |
| Ground continuity | CN921 GND | Heatsink | <0.5Ω | | |
| Ground continuity | CN921 GND | Secondary GND pad | <0.5Ω | | |
| B+ path | CN921 B+ | After L920 | <1Ω | | |

### 0.3 Dead Short Check (Power Off, FETs Removed)

Quick check for obvious shorts before applying power.

| Test | Positive | Negative | Expected | Actual | Pass/Fail |
|------|----------|----------|----------|--------|-----------|
| B+ to GND | B+ terminal | GND terminal | >10Ω initially, rising | | |
| Primary winding to GND | T901 primary | GND | OL or very high | | |

---

## Phase 1: PWM Controller Verification

{{< hint danger >}}
**RE-VERIFY FROM SCRATCH** - Previous tests indicated IC920 was working, but let's confirm with fresh measurements.
{{< /hint >}}

### 1.1 Unpowered IC920 Checks

| Test | Measurement | Expected | Actual | Pass/Fail |
|------|-------------|----------|--------|-----------|
| Pin 4 to GND | Continuity | <1Ω | | |
| Pin 7 to GND | Continuity | <1Ω | | |
| Pin 16 to GND | Continuity | <1Ω | | |
| Pin 11 to B+ rail | Continuity | Low Ω | | |
| Pin 12 to B+ rail | Continuity | Low Ω | | |

### 1.2 Powered IC920 Checks (FETs Removed)

**Setup:** Bench supply at 14.4V, 200mA current limit, REM jumpered to B+

| Pin | Function | Expected | Actual | Pass/Fail |
|-----|----------|----------|--------|-----------|
| 11 | Vcc | 14.4V | | |
| 12 | Vcc | 14.4V | | |
| 14 | Vref (5V reference) | 5.0V ±0.1V | | |
| 4, 7, 16 | GND | 0V | | |
| 5 | Ct (timing cap) | ~1.8V DC | | |
| 6 | Rt (timing resistor) | ~3.8V DC | | |
| 15 | Dead time control | ~2.5V DC | | |

### 1.3 PWM Output Verification (Oscilloscope Required)

**Setup:** Same as 1.2, probe on IC920 outputs

| Test Point | Expected | Actual | Pass/Fail |
|------------|----------|--------|-----------|
| Pin 8 (Output A) | Square wave, ~100-200kHz | Freq: _____ | |
| Pin 11 (Output B) | Square wave, ~100-200kHz | Freq: _____ | |
| Duty cycle | ~45% (with dead time) | _____ % | |
| Amplitude | 0V to ~14V | _____ V | |
| Phase relationship | A & B opposite phase | Yes / No | |

{{< hint info >}}
**Note:** If PWM outputs are absent or abnormal, stop here - the controller or its support components need attention before proceeding.
{{< /hint >}}

---

## Phase 2: Gate Driver Transistors

{{< hint danger >}}
**HIGHEST PRIORITY** - If damaged, FETs receive improper gate drive and run in linear mode causing thermal runaway.
{{< /hint >}}

### Q901, Q902 - 2SB1132 (PNP)

**Test Method:** IN-CIRCUIT (preliminary), OUT-OF-CIRCUIT (if suspicious)

**Location:** Near transformer T901, power supply section (page 8, area D4-D5)

| Test | Red Lead | Black Lead | Expected | Actual | Pass/Fail |
|------|----------|------------|----------|--------|-----------|
| **Q901** |||||
| B-E forward | Base | Emitter | 0.5-0.7V | | |
| B-C forward | Base | Collector | 0.5-0.7V | | |
| E-B reverse | Emitter | Base | OL | | |
| C-B reverse | Collector | Base | OL | | |
| E-C | Emitter | Collector | OL | | |
| C-E | Collector | Emitter | OL | | |
| **Q902** |||||
| B-E forward | Base | Emitter | 0.5-0.7V | | |
| B-C forward | Base | Collector | 0.5-0.7V | | |
| E-B reverse | Emitter | Base | OL | | |
| C-B reverse | Collector | Base | OL | | |
| E-C | Emitter | Collector | OL | | |
| C-E | Collector | Emitter | OL | | |

**Failure Indicators:**
- Low resistance in any "OL" test = shorted junction
- No reading in forward tests = open junction
- In-circuit readings may show lower than expected due to parallel components

**Part Number:** 48E39095S01 (2SB1132)

---

## Phase 3: Secondary Rectifier Diodes

{{< hint warning >}}
**HIGH PRIORITY** - Shorted rectifier = dead short on transformer secondary = massive primary current
{{< /hint >}}

**Test Method:** IN-CIRCUIT (preliminary), OUT-OF-CIRCUIT (if suspicious)

### D802 - FCH10A15 (Fast Recovery)
### D803 - FRH10A15 (Fast Recovery)

**Location:** Near transformer T901, secondary side

| Diode | Forward (Red→Anode) | Reverse (Red→Cathode) | Pass/Fail |
|-------|---------------------|----------------------|-----------|
| D802 | Expected: 0.3-0.5V / Actual: _____ | Expected: OL / Actual: _____ | |
| D803 | Expected: 0.3-0.5V / Actual: _____ | Expected: OL / Actual: _____ | |

### D801, D808 - 11EFS2 (Fast Recovery)

| Diode | Forward (Red→Anode) | Reverse (Red→Cathode) | Pass/Fail |
|-------|---------------------|----------------------|-----------|
| D801 | Expected: 0.4-0.6V / Actual: _____ | Expected: OL / Actual: _____ | |
| D808 | Expected: 0.4-0.6V / Actual: _____ | Expected: OL / Actual: _____ | |

**Failure Indicators:**
- Reverse direction shows ANY voltage reading = shorted
- Forward shows OL = open
- Forward shows very low (<0.2V) = likely shorted

---

## Phase 4: Transformer T901

**Test Method:** IN-CIRCUIT

**Location:** Large component, center of power supply area

**Transformer Ratio:** 5:8:1 (Part: 25E39082S01)

| Winding | Pins | Expected | Actual | Pass/Fail |
|---------|------|----------|--------|-----------|
| Primary | (identify from schematic) | 0.1-0.5Ω | | |
| Secondary 1 | | 0.1-0.5Ω | | |
| Secondary 2 | | Very low | | |
| Pri to Sec1 | | OL (∞) | | |
| Pri to Sec2 | | OL (∞) | | |
| Sec1 to Sec2 | | OL (∞) | | |

**Failure Indicators:**
- Any winding reads 0.0Ω = shorted turns
- Any winding reads OL = open winding
- Reading between isolated windings ≠ OL = insulation breakdown

---

## Phase 5: Secondary Rail Load Test

**Test Method:** IN-CIRCUIT (FETs removed)

**Procedure:** Measure resistance from each rail point to GND. Readings will drift as capacitors charge - note the final settled value.

| Rail | Test Point | To GND | Expected | Actual (settled) | Pass/Fail |
|------|------------|--------|----------|------------------|-----------|
| +25V | Find on PCB | GND | >100Ω | | |
| -25V | Find on PCB | GND | >100Ω | | |
| +23V | Find on PCB | GND | >100Ω | | |
| -23V | Find on PCB | GND | >100Ω | | |
| +14V | Find on PCB | GND | >100Ω | | |
| -14V | Find on PCB | GND | >100Ω | | |

**Failure Indicators:**
- Very low resistance (<10Ω) that doesn't rise = dead short on that rail
- Resistance rises then falls continuously = leaky component

---

## Phase 6: Voltage Regulator Transistors

**Test Method:** IN-CIRCUIT (preliminary)

### Q801 - 2SC3421 (NPN) - +14V Regulator
### Q802 - 2SA1358 (PNP) - -14V Regulator

**Location:** See page 8, near C806/C807

| Test | Red Lead | Black Lead | Expected | Actual | Pass/Fail |
|------|----------|------------|----------|--------|-----------|
| **Q801 (NPN)** |||||
| B-E forward | Base | Emitter | 0.5-0.7V | | |
| B-C forward | Base | Collector | 0.5-0.7V | | |
| E-B reverse | Emitter | Base | OL | | |
| C-B reverse | Collector | Base | OL | | |
| **Q802 (PNP)** |||||
| B-E forward | Emitter | Base | 0.5-0.7V | | |
| B-C forward | Collector | Base | 0.5-0.7V | | |
| E-B reverse | Base | Emitter | OL | | |
| C-B reverse | Base | Collector | OL | | |

---

## Phase 7: Control/Protection Circuit Transistors

**Test Method:** IN-CIRCUIT (preliminary)

| Transistor | Type | B-E Fwd | B-C Fwd | All Others OL? |
|------------|------|---------|---------|----------------|
| Q920 | 2SA965 PNP | | | |
| Q921 | DTC114EK NPN w/R | ~0.7V | ~0.7V | |
| Q922 | DTC114EK NPN w/R | ~0.7V | ~0.7V | |
| Q923 | DTA114EK PNP w/R | ~0.7V | ~0.7V | |
| Q924 | DTC144EK NPN w/R | ~0.7V | ~0.7V | |
| Q925 | 2SA1037AK PNP | | | |

**Note:** DTC/DTA series have internal bias resistors - readings may vary from standard transistors

---

## Phase 8: Support Components Quick Check

**Test Method:** IN-CIRCUIT

### Gate Resistors

| Component | Value | Measured | Pass/Fail |
|-----------|-------|----------|-----------|
| R903 | 470Ω | | |
| R904 | 470Ω | | |

### Timing Components (IC920)

| Component | Value | Measured | Pass/Fail |
|-----------|-------|----------|-----------|
| R920 | 2.2KΩ | | |
| R921 | 10KΩ | | |
| C920 | 0.15µF | Check for short | |

---

## Phase 9: B+ Input Path (Detailed)

**Test Method:** IN-CIRCUIT

| Test | From | To | Expected | Actual | Pass/Fail |
|------|------|-----|----------|--------|-----------|
| Fuse F901 | Terminal 1 | Terminal 2 | <0.1Ω | | |
| Fuse F902 | Terminal 1 | Terminal 2 | <0.1Ω | | |
| L920 (choke) | Pin 1 | Pin 2 | <1Ω | | |

---

## Test Results Summary

### Components Requiring Replacement

| Ref | Part Number | Description | Reason |
|-----|-------------|-------------|--------|
| | | | |
| | | | |
| | | | |

### Components Verified Good

| Ref | Description | Test Phase |
|-----|-------------|------------|
| | | |
| | | |
| | | |

{{< hint info >}}
**Note:** Starting fresh - populate this table only with components verified during THIS test sequence.
{{< /hint >}}

---

## Power-Up Test Procedure (After Repairs)

{{< hint danger >}}
**Only proceed after all above tests pass**
{{< /hint >}}

1. Reinstall FETs (Q903-Q906)
2. Set bench supply to 14.4V, **100mA limit**
3. Connect and observe:
   - Current draw should be low (<50mA) initially
   - LED should illuminate
4. Slowly raise current limit while monitoring:
   - Voltage should stay at 14.4V
   - Current should remain reasonable (<500mA at idle)
5. **If voltage drops or current spikes - STOP IMMEDIATELY**
6. Check for heat on any components

---

## Post-Repair Verification

{{< hint warning >}}
**Only proceed after Power-Up Test passes** - These tests verify the amplifier is fully functional before connecting to real speakers.
{{< /hint >}}

### Phase 10: Power Supply Rail Verification

**Setup:** Bench supply at 14.4V, 2A limit, no load connected

| Rail | Test Point | Expected | Actual | Pass/Fail |
|------|------------|----------|--------|-----------|
| +25V | C806 positive | +24 to +26V | | |
| -25V | C807 negative | -24 to -26V | | |
| +23V | C808 positive | +22 to +24V | | |
| -23V | C809 negative | -22 to -24V | | |
| +14V | Near IC501-518 | +13.5 to +14.5V | | |
| -14V | Near IC501-518 | -13.5 to -14.5V | | |
| Vref | IC920 pin 14 | +5.0V ±0.1V | | |

**Idle Current Draw:**

| Condition | Expected | Actual | Pass/Fail |
|-----------|----------|--------|-----------|
| No signal, no load | 0.5 - 1.2A | | |

---

### Phase 11: Signal Path Verification (No Load)

**Setup:**
- Bench supply at 14.4V, 2A limit
- Signal generator: 1kHz sine wave, 200mV RMS
- Oscilloscope on outputs
- No load connected

**Input to Output Test:**

| Channel | Input | Output Test Point | Expected | Actual | Pass/Fail |
|---------|-------|-------------------|----------|--------|-----------|
| CH1 | RCA L (Front) | Speaker terminal CH1 | Clean sine, no clipping | | |
| CH2 | RCA R (Front) | Speaker terminal CH2 | Clean sine, no clipping | | |
| CH3 | RCA L (Rear) | Speaker terminal CH3 | Clean sine, no clipping | | |
| CH4 | RCA R (Rear) | Speaker terminal CH4 | Clean sine, no clipping | | |

**Gain Control Test:** (VR503/VR504)
- [ ] Turning gain down reduces output amplitude
- [ ] Turning gain up increases output amplitude
- [ ] No distortion or oscillation at any gain setting

**Filter/Crossover Test:** (if applicable)
- [ ] HPF switch attenuates low frequencies
- [ ] LPF switch attenuates high frequencies

---

### Phase 12: Dummy Load Testing

{{< hint info >}}
**Dummy Load Resistors:** Use non-inductive power resistors. Mount on heatsink or allow air cooling.
{{< /hint >}}

**Recommended Dummy Loads:**

| Load Type | Resistance | Power Rating | Use For |
|-----------|------------|--------------|---------|
| 4Ω | 4Ω | ≥50W per channel | Normal speaker load |
| 8Ω | 8Ω | ≥25W per channel | Light load testing |
| 2Ω | 2Ω | ≥100W per channel | Stress testing (optional) |

#### 12.1 Low Power Load Test

**Setup:**
- Bench supply: 14.4V, 5A limit
- Signal: 1kHz sine, adjust for 1W output (~2V RMS into 4Ω)
- Dummy load: 4Ω per channel

| Test | Expected | Actual | Pass/Fail |
|------|----------|--------|-----------|
| Output voltage (1W into 4Ω) | ~2V RMS | | |
| Current draw (all 4 ch, 1W each) | ~2-3A | | |
| Output waveform | Clean sine | | |
| Any channel distorted? | No | | |
| Heatsink temperature after 1 min | Warm, not hot | | |

#### 12.2 Medium Power Load Test

**Setup:**
- Bench supply: 14.4V, 10A limit (or battery)
- Signal: 1kHz sine, adjust for 10W output (~6.3V RMS into 4Ω)
- Dummy load: 4Ω per channel

| Test | Expected | Actual | Pass/Fail |
|------|----------|--------|-----------|
| Output voltage (10W into 4Ω) | ~6.3V RMS | | |
| Current draw (all 4 ch, 10W each) | ~8-12A | | |
| Output waveform | Clean sine | | |
| Clipping onset | None at 10W | | |
| Heatsink temperature after 2 min | Hot but manageable | | |

#### 12.3 Full Power Test (Use Car Battery or High-Current Supply)

{{< hint danger >}}
**Caution:** Full power testing generates significant heat. Monitor continuously. Have fire extinguisher nearby.
{{< /hint >}}

**Setup:**
- Power: Car battery or 14.4V supply capable of 20A+
- Signal: 1kHz sine, adjust for rated power (40W = ~12.6V RMS into 4Ω)
- Dummy load: 4Ω per channel
- Duration: Brief bursts only (5-10 seconds)

| Test | Expected | Actual | Pass/Fail |
|------|----------|--------|-----------|
| Output voltage (40W into 4Ω) | ~12.6V RMS | | |
| Waveform at full power | Clean until clipping | | |
| Symmetrical clipping? | Yes (equal +/-) | | |
| Any channel weak? | No | | |
| Protection circuit trigger? | No (unless overdriven) | | |

---

### Phase 13: Thermal Verification

**Setup:** Run at medium power (10W/ch) for 5 minutes with dummy loads

| Component | Location | Expected | Actual | Pass/Fail |
|-----------|----------|----------|--------|-----------|
| Q903-Q906 (FETs) | On heatsink | Warm to hot, even heat | | |
| Q161/Q261/Q361/Q461 | Output transistors | Warm | | |
| Q901/Q902 | Gate drivers | Slightly warm | | |
| T901 | Transformer | Warm | | |
| Heatsink | Main chassis | Hot but touchable (<60°C) | | |
| Any component smoking? | Entire board | NO | | |
| Unusual smells? | Entire board | NO | | |

---

### Phase 14: Protection Circuit Verification

**Test each protection feature:**

| Protection | How to Test | Expected Response | Actual | Pass/Fail |
|------------|-------------|-------------------|--------|-----------|
| Thermal | Run until heatsink hot, then check | Output mutes, LED may blink | | |
| DC Offset | Inject DC at input (carefully) | Output mutes | | |
| Short circuit | Briefly short output (use fuse in line) | Output mutes, no damage | | |
| Overvoltage | Raise supply to 16V briefly | Should continue operating | | |
| Undervoltage | Lower supply to 10V | May mute or reduce output | | |

{{< hint warning >}}
**Short Circuit Test:** Place a 1A fast-blow fuse in series with the short. This protects both the amp and your test setup.
{{< /hint >}}

---

### Phase 15: Final Acceptance Checklist

| Item | Verified |
|------|----------|
| All 4 channels produce clean output | [ ] |
| Gain controls work on all channels | [ ] |
| No excessive idle current draw | [ ] |
| No overheating at moderate power | [ ] |
| Protection circuits functional | [ ] |
| No audible noise/hum at idle | [ ] |
| Power LED illuminates | [ ] |
| No burning smell during testing | [ ] |

**Sign-off:**

- Date: _____________
- Tested by: _____________
- Ready for installation: [ ] Yes  [ ] No - needs: _____________
