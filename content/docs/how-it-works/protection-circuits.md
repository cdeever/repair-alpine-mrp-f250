---
title: "Protection Circuits"
weight: 4
---

# Protection Circuits

The MRP-F250 includes multiple protection circuits to prevent damage from fault conditions. These circuits monitor various parameters and activate a mute function when problems are detected.

## Protection Overview

{{< graphviz >}}
digraph protection_system {
    rankdir=TB
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=11]
    edge [fontname="Helvetica", fontsize=9]

    subgraph cluster_protection {
        label="PROTECTION SYSTEM"
        style="dashed"
        color="#d94a4a"

        // Detection circuits
        subgraph cluster_detectors {
            label=""
            style="invis"
            rank=same

            current [label="Output Current\nDetect\nQ163/263/363/463", fillcolor="#fff2cc"]
            thermal [label="Thermal\nDetect\nTH920-924", fillcolor="#fff2cc"]
            voltage [label="+B Voltage\nDetect\nOver/Under", fillcolor="#fff2cc"]
            dc [label="DC Offset\nDetect", fillcolor="#fff2cc"]
            settime [label="Set Time\nMute\nQ930", fillcolor="#fff2cc"]
        }

        // Logic and driver
        logic [label="Mute Logic\n(Combines all fault signals)", fillcolor="#d9e8fb"]
        driver [label="Mute Driver\nQ926/Q927", fillcolor="#d9e8fb"]

        // Output
        mute [label="Mute Transistors\nQ152/Q252/Q352/Q452", fillcolor="#ffd9d9"]
    }

    // Connections to logic
    current -> logic
    thermal -> logic
    voltage -> logic
    dc -> logic
    settime -> logic

    // Logic to driver to mute
    logic -> driver -> mute

    // Output to amp stages
    amp [label="Power Amp\nSignal Shunted\nto Ground", fillcolor="#e8e8e8"]
    mute -> amp
}
{{< /graphviz >}}

## Protection Types

### 1. Output Current Detection

**Purpose:** Protects against speaker shorts and excessive load current

**Components:**
- Q163/Q263/Q363/Q463 (2SC2412K) - Current sense transistors
- R168/R268/R368/R468 (2.2Ω) - Current sense resistors
- R169/R269/R369/R469 (2.2Ω) - Current sense resistors

**Operation:**
- Emitter resistors in the output stage develop a voltage proportional to current
- Q163 monitors this voltage
- When current exceeds threshold, protection activates

### 2. DC Offset Detection

**Purpose:** Prevents DC voltage from reaching speakers (which causes damage)

**Components:**
- Part of Q163/Q263/Q363/Q463 circuit
- Integrating network to detect sustained DC

**Operation:**
- Monitors output for DC offset
- Brief transients are ignored
- Sustained DC (>threshold) triggers mute

### 3. Thermal Detection

**Purpose:** Prevents overheating damage to amplifier and speakers

**Components:**
- TH920-TH924: 100K NTC thermistors

**Locations:**

| Thermistor | Location | Monitors |
|------------|----------|----------|
| TH920 | Near DC/DC | Power supply temperature |
| TH921-TH924 | Near output stages | Amplifier temperature |

**Operation:**
- Thermistor resistance decreases as temperature increases
- At threshold temperature, protection activates
- Amplifier mutes until temperature drops

### 4. Over-Voltage Detection

**Purpose:** Protects against excessive supply voltage

**Components:**
- Zener diodes in voltage sensing network
- D922 (UDZS15B) - 15V Zener

**Operation:**
- Monitors +B supply voltage
- If voltage exceeds ~16V, protection activates
- Prevents damage from alternator voltage spikes

### 5. Under-Voltage Detection

**Purpose:** Prevents operation at too-low voltage (which can cause distortion and damage)

**Components:**
- D920 (SECS1E01C M) - LED (power indicator)
- Voltage divider network

**Operation:**
- Monitors +B supply voltage
- If voltage drops below ~10.5V, amplifier may shut down

### 6. Set Time Mute (Turn-On Delay)

**Purpose:** Prevents turn-on/turn-off transients (pops) from reaching speakers

**Components:**
- C933 (4.7µF) - Timing capacitor
- Q930 (DTC114EK) - Delay transistor

**Operation:**
- On power-up, mute is held active for ~1-2 seconds
- Allows all voltages to stabilize before unmuting
- On power-down, mute activates before rails collapse

## Mute Driver Circuit

**Components:**
- Q926 (DTA114EK) - Mute control
- Q927 (DTC114EK) - Mute control

**Operation:**
When any protection condition is detected:
1. Mute signal goes active
2. Q152/Q252/Q352/Q452 turn ON
3. Audio signal is shunted to ground
4. LED may indicate fault (depends on fault type)

## Key Transistors in Protection Circuit

| Transistor | Part | Function |
|------------|------|----------|
| Q920 | 2SA965 | Remote switch control |
| Q921 | DTC114EK | Remote on detection |
| Q922 | DTC114EK | DC/DC control |
| Q923 | DTA114EK | Voltage detection |
| Q924 | DTC144EK | Timing control |
| Q925 | 2SA1037AK | Mute timing |
| Q926 | DTA114EK | Mute driver |
| Q927 | DTC114EK | Mute driver |
| Q928 | 2SC2412K | Thermal detect |
| Q929 | 2SC2412K | Thermal detect |
| Q930 | DTC114EK | Set time mute |

**Note:** DTC/DTA series transistors have internal bias resistors (10K-47K typical).

## Normal Operating Voltages

| Transistor | E | C | B |
|------------|---|---|---|
| Q920 | 14.4V | 14.4V | 13.8V |
| Q921 | 0V | 0V | 3.3V |
| Q922 | 0V | 5V | 0.1V |
| Q923 | 14.4V | -7V | 14.4V |
| Q924 | 4.8V | 14.4V | 5V |
| Q925 | 4.8V | 0V | 5V |
| Q926 | 14V | 0V | 14V |
| Q927 | 0V | 14.4V | 0V |
| Q928 | 0V | 14.4V | 0V |
| Q929 | 0V | 14.4V | 0V |
| Q930 | 0V | 0V | 2V |

## Troubleshooting Protection Issues

### Amplifier Won't Unmute

1. **Check remote turn-on signal** (5-7.5V required)
2. **Verify set-time delay** - Wait 2+ seconds
3. **Check for fault conditions:**
   - Measure temperature (overheating?)
   - Check output for DC offset
   - Verify supply voltage is in range

### Protection Triggers Intermittently

1. **Thermal issue** - Check for blocked airflow, verify thermistors
2. **Marginal output transistor** - May be failing under load
3. **Loose connection** - Can cause transients that trigger protection

### Protection Never Triggers (When It Should)

1. **Failed protection transistors** - Test Q926, Q927
2. **Failed thermistors** - Test resistance (100K @ 25°C)
3. **Open sense resistors** - Check R168/R169 etc.
