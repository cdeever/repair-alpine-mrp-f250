---
title: "Terminal Voltages"
weight: 2
---

# Terminal Voltage Reference

Reference voltages for all ICs and transistors under normal operating conditions.

## Measuring Conditions

| Parameter | Value |
|-----------|-------|
| Power Supply Voltage | DC 14.4V |
| Measuring Meter | Digital Multimeter |
| Measuring Point Reference | Between pin and GND |
| Measuring Condition | Remote ON, No signal input |

---

## IC920 (uPC494) - PWM Controller

| Pin | Voltage | Pin | Voltage |
|-----|---------|-----|---------|
| 1 | 1.6V* | 9 | 6V |
| 2 | 2.5V | 10 | 6V |
| 3 | 0.1V | 11 | 14.4V |
| 4 | 0V | 12 | 14.4V |
| 5 | 1.8V | 13 | 5V |
| 6 | 3.8V | 14 | 5V |
| 7 | 0V | 15 | 2.5V |
| 8 | 14.4V | 16 | 0V |

*Pin 1 varies with temperature

---

## Op-Amps IC501-IC518 (NJM4565M)

All preamp op-amps share the same power supply connections:

| Pin | Function | Voltage |
|-----|----------|---------|
| 1 | Output A | 0V (signal dependent) |
| 2 | Inv Input A | 0V |
| 3 | Non-Inv Input A | 0V |
| 4 | V- | -14V |
| 5 | Non-Inv Input B | 0V |
| 6 | Inv Input B | 0V |
| 7 | Output B | 0V |
| 8 | V+ | +14V |

---

## Power Supply Transistors

### DC/DC Converter

| Transistor | E | C | B |
|------------|---|---|---|
| Q901 (2SB1132) | 5.6V | 0V | 6V |
| Q902 (2SB1132) | 5.6V | 0V | 6V |

### Switching FETs

| Transistor | G | D | S |
|------------|---|---|---|
| Q903 (2SK3662) | 5.6V | 14.4V | 0V |
| Q904 (2SK3662) | 5.6V | 14.4V | 0V |
| Q905 (2SK3662) | 5.6V | 14.4V | 0V |
| Q906 (2SK3662) | 5.6V | 14.4V | 0V |

### Voltage Regulators

| Transistor | E | C | B |
|------------|---|---|---|
| Q801 (2SC3421) | 14V | 23V | 15V |
| Q802 (2SA1358) | -14V | -23V | -15V |

---

## Power Amplifier Transistors (All 4 Channels)

The four channels use identical circuits. Replace x with channel number (1, 2, 3, or 4).

| Transistor | E | C | B |
|------------|---|---|---|
| Q151/Q251/Q351/Q451 (2SC3326) | 0V | 0V | -7V |
| Q152/Q252/Q352/Q452 (2SC4207) | -0.6V | 24V | 0V |
| Q153/Q253/Q353/Q453 (2SC2412K) | 24.5V | 25V | 25V |
| Q154/Q254/Q354/Q454 (2SC2412K) | -24.5V | -24V | -24V |
| Q155/Q255/Q355/Q455 (2SA988F) | 24.5V | 1.1V | 24V |
| Q156/Q256/Q356/Q456 (2SC1841F) | -24.5V | -1.1V | -24V |
| Q157/Q257/Q357/Q457 (2SC2412K) | -0.5V | 1.1V | 1.1V |
| Q158/Q258/Q358/Q458 (2SC2713) | -1.1V | 0.5V | -0.5V |
| Q159/Q259/Q359/Q459 (2SC2235) | 0.5V | 25V | 1.1V |
| Q160/Q260/Q360/Q460 (2SA965) | -0.5V | -25V | -1.1V |
| Q161/Q261/Q361/Q461 (2SC5100) | 0V | 23V | 0.5V |
| Q162/Q262/Q362/Q462 (2SA1908) | 0V | -23V | -0.5V |
| Q163/Q263/Q363/Q463 (2SC2412K) | 0V | 23V | 0V |

---

## Protection/Control Transistors

| Transistor | E | C | B |
|------------|---|---|---|
| Q920 (2SA965) | 14.4V | 14.4V | 13.8V |
| Q921 (DTC114EK) | 0V | 0V | 3.3V |
| Q922 (DTC114EK) | 0V | 5V | 0.1V |
| Q923 (DTA114EK) | 14.4V | -7V | 14.4V |
| Q924 (DTC144EK) | 4.8V | 14.4V | 5V |
| Q925 (2SA1037AK) | 4.8V | 0V | 5V |
| Q926 (DTA114EK) | 14V | 0V | 14V |
| Q927 (DTC114EK) | 0V | 14.4V | 0V |
| Q928 (2SC2412K) | 0V | 14.4V | 0V |
| Q929 (2SC2412K) | 0V | 14.4V | 0V |
| Q930 (DTC114EK) | 0V | 0V | 2V |

---

## Quick Reference Card

### Critical Voltages to Check First

| Test Point | Expected | Indicates |
|------------|----------|-----------|
| IC920 Pin 14 (Vref) | 5.0V | PWM controller healthy |
| IC920 Pin 11/12 (Vcc) | 14.4V | Power to controller |
| Q901/Q902 Emitter | 5.6V | Gate drivers powered |
| +14V Rail | +14V | Preamp power OK |
| -14V Rail | -14V | Preamp power OK |
| +25V Rail | +25V | Output stage power OK |
| -25V Rail | -25V | Output stage power OK |

### Voltage Tolerance

Most voltages should be within ±10% of nominal values. Significant deviations indicate problems in that circuit.
