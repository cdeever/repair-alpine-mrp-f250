---
title: "Diagnostics"
weight: 2
bookCollapseSection: true
---

# Diagnostic Procedures

This section provides systematic troubleshooting procedures for the Alpine MRP-F250 amplifier.

## Diagnostic Approach

When troubleshooting this amplifier, follow these principles:

1. **Understand Before Testing** - Know what the circuit should do before measuring
2. **Power Off First** - Most tests can be done with power removed
3. **Current Limit Always** - Use a current-limited supply when powering up
4. **Isolate Sections** - Remove components (like FETs) to isolate failures
5. **Document Everything** - Record measurements for comparison

## Available Procedures

### [Test Plan]({{< relref "/docs/diagnostics/test-plan" >}})
Complete systematic test procedure organized by priority, based on the current failure case.

### [Terminal Voltages]({{< relref "/docs/diagnostics/terminal-voltages" >}})
Reference voltage tables for all ICs and transistors under normal operating conditions.

### [Troubleshooting Flowchart]({{< relref "/docs/diagnostics/flowchart" >}})
Decision tree for common symptoms.

## Equipment Required

| Equipment | Purpose |
|-----------|---------|
| Digital Multimeter | Voltage, resistance, diode tests |
| Current-Limited Power Supply | Safe power-up testing |
| Oscilloscope (recommended) | PWM signal verification |
| Soldering Equipment | Component removal for testing |

## Safety Reminders

{{< hint danger >}}
**High Voltage Warning**
The ±25V rails can deliver significant current. Always discharge capacitors before working on the board.
{{< /hint >}}

{{< hint warning >}}
**Heat Sink Caution**
The heat sink can become extremely hot during operation. Allow cooling before handling.
{{< /hint >}}

{{< hint info >}}
**ESD Precautions**
MOSFETs and ICs are ESD sensitive. Use proper grounding when handling components.
{{< /hint >}}
