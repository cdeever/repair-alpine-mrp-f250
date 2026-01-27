---
title: "Repair Procedures"
weight: 3
bookCollapseSection: true
---

# Repair Procedures

This section covers common failure modes and step-by-step repair procedures for the Alpine MRP-F250.

## Repair Philosophy

1. **Find the Root Cause** - Don't just replace failed components; understand why they failed
2. **Check Related Components** - Failures often cascade; check upstream and downstream
3. **Use Correct Parts** - Substitutes must match critical specifications
4. **Test Before Reassembly** - Verify repairs with current-limited power supply
5. **Document Everything** - Record what was replaced and why

## Common Failure Categories

### [DC/DC Converter Failures]({{< relref "/docs/repair/dcdc-failures" >}})
FET failures, gate driver issues, and rectifier problems.

### [Transformer Repair]({{< relref "/docs/repair/transformer-repair" >}})
Options for repairing or rewinding the unobtainable T901 transformer.

### [Output Stage Failures]({{< relref "/docs/repair/output-failures" >}})
Output transistor failures, driver failures, and bias problems.

### [Preamp Failures]({{< relref "/docs/repair/preamp-failures" >}})
Op-amp failures, regulator issues, and signal path problems.

## Parts Sourcing

### Original Parts
Alpine part numbers are listed in the [Parts List]({{< relref "/docs/specifications/parts-list" >}}). Original parts may be difficult to source.

### Substitutes
When substituting parts, match these critical specifications:

**For Transistors:**
- Voltage ratings (Vce, Vbe)
- Current ratings (Ic)
- Power dissipation
- hFE range (gain)
- Package/pinout

**For MOSFETs:**
- Vds (drain-source voltage)
- Id (drain current)
- Vgs(th) (gate threshold voltage)
- Rds(on) (on-resistance)
- Package/pinout

**For Diodes:**
- Reverse voltage rating
- Forward current rating
- Recovery time (for switching diodes)
- Package

## Safety Warnings

{{< hint danger >}}
**Capacitor Discharge**
Large filter capacitors (C905, C906 = 2200µF) can hold charge. Discharge before working.
{{< /hint >}}

{{< hint warning >}}
**Heat Sink Mounting**
Output transistors and FETs are mounted to the heat sink. Ensure proper thermal compound and mounting pressure when reinstalling.
{{< /hint >}}

{{< hint warning >}}
**ESD Precautions**
MOSFETs are extremely ESD sensitive. Ground yourself before handling.
{{< /hint >}}
