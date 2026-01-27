---
title: "Alpine MRP-F250 Repair Guide"
type: docs
---

# Alpine MRP-F250 Repair Guide

A comprehensive repair guide for the **Alpine MRP-F250 4-Channel Power Amplifier** (V-Power series).

---

## About This Guide

This documentation provides:

- **Circuit Theory** - Understanding how the amplifier works at a block and component level
- **Diagnostic Procedures** - Systematic troubleshooting with test plans and expected values
- **Repair Actions** - Common failure modes and repair procedures
- **Reference Data** - Specifications, terminal voltages, and parts lists

---

## The Amplifier

The MRP-F250 is a 4-channel car audio amplifier featuring:

| Specification | Value |
|---------------|-------|
| Power Output | 40W × 4 (4Ω), 50W × 4 (2Ω), 100W × 2 (BTL) |
| THD | 0.2% @ 40W/4Ω |
| S/N Ratio | 100dB |
| Frequency Response | 10Hz - 50kHz |
| Power Source | DC 14.4V (10.5-16V) |

---

## Current Repair Case

This guide was developed while diagnosing a specific failure:

**Symptoms:**
- Amplifier smoked on power-up (twice)
- Original 2 of 4 DC/DC converter FETs tested shorted
- After replacing all 4 FETs, unit smoked again
- No visible damage on PCB
- Output transistors tested OK
- IC920 (PWM controller) verified producing correct pulses
- Supply voltage dropped from 12V to 8V and clamped there before smoking at ~1.2A

**Diagnosis in Progress:**
- Root cause is downstream of the FETs
- Likely candidates: gate driver transistors (Q901/Q902), secondary rectifiers, or transformer

---

## Documentation Philosophy

This repair guide exists to:
- Document the systematic diagnostic process
- Preserve knowledge of common failure modes
- Provide reference data for future repairs
- Enable AI-assisted troubleshooting with complete context

---

## Source Material

Based on Alpine Service Manual **68E39196S01** (January 2006 revision).
