---
title: "How It Works"
weight: 1
bookCollapseSection: true
---

# How the MRP-F250 Works

This section explains the circuit architecture of the Alpine MRP-F250 amplifier at both block and component level.

## High-Level Architecture

{{< graphviz >}}
digraph MRP_F250 {
    rankdir=TB
    splines=ortho
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=11]
    edge [fontname="Helvetica", fontsize=9]

    // Main signal path (top row)
    input [label="INPUT\nSTAGE", fillcolor="#e3f2fd"]
    preamp [label="PREAMP\nSTAGE", fillcolor="#e3f2fd"]
    poweramp [label="POWER\nAMP", fillcolor="#fff3e0"]
    output [label="OUTPUT\nSTAGE", fillcolor="#fff3e0"]
    speakers [label="Speakers", shape=ellipse, fillcolor="#c8e6c9"]

    // Protection
    protection [label="PROTECTION\nCIRCUITS", fillcolor="#ffcdd2"]

    // Power supply
    battery [label="+12V Battery", shape=ellipse, fillcolor="#fff9c4"]
    powersupply [label="POWER SUPPLY (DC/DC)\n+B → DC/DC Conv → ±25V, ±23V, ±14V", fillcolor="#e1bee7"]

    // Signal flow (horizontal)
    { rank=same; input; preamp; poweramp; output; speakers }
    input -> preamp -> poweramp -> output -> speakers

    // Protection row
    { rank=same; protection }
    output -> protection [style=dashed, label="monitor"]

    // Power supply row
    { rank=same; battery; powersupply }
    battery -> powersupply
    protection -> powersupply [style=dashed, label="control"]

    // Power distribution (vertical)
    powersupply -> input [style=dotted]
    powersupply -> preamp [style=dotted]
    powersupply -> poweramp [style=dotted]
    powersupply -> output [style=dotted]
}
{{< /graphviz >}}

## Signal Flow

1. **Input Stage** - Accepts RCA (line-level) or speaker-level inputs via isolation amplifiers
2. **Preamp Stage** - Provides gain control, bass EQ, and variable crossover filters
3. **Power Amplifier** - Class AB amplifier stages (one per channel)
4. **Output Stage** - Final power transistors driving speakers
5. **Protection Circuits** - Monitor for thermal, DC offset, overcurrent, and overvoltage faults

## Power Supply Architecture

The amplifier converts automotive 12-14.4V DC to multiple regulated rails:

| Rail | Voltage | Purpose |
|------|---------|---------|
| ±25V | ±25VDC | Power amplifier output stages |
| ±23V | ±23VDC | Power amplifier driver stages |
| ±14V | ±14VDC | Preamp op-amps (IC501-IC518) |
| +5V | +5VDC | Reference voltage (IC920 Vref) |

## Key Subsystems

- [Power Supply]({{< relref "/docs/how-it-works/power-supply" >}}) - DC/DC converter and voltage regulators
- [Preamp Stage]({{< relref "/docs/how-it-works/preamp-stage" >}}) - Input processing, gain, EQ, and filters
- [Power Amplifier]({{< relref "/docs/how-it-works/power-amp-stage" >}}) - Class AB output stages
- [Protection Circuits]({{< relref "/docs/how-it-works/protection-circuits" >}}) - Thermal, DC, current, and voltage protection
