---
title: "Primary vs Secondary Side"
weight: 5
---

# Primary vs Secondary Side

Understanding the distinction between primary and secondary sides of the DC/DC converter is essential for effective troubleshooting.

## Overview

The transformer (T901) divides the power supply into two electrically isolated sections:

{{< graphviz >}}
digraph power_flow {
    rankdir=TB
    node [shape=box, style="rounded,filled", fontname="Helvetica"]
    edge [fontname="Helvetica", fontsize=10]

    subgraph cluster_primary {
        label="PRIMARY SIDE\n(Battery Input)"
        style="dashed"
        color="#4a90d9"

        battery [label="Battery\n12-14.4V", fillcolor="#e8f4e8"]
        fuses [label="Fuses\nF901/F902\n15A×2", fillcolor="#fff2cc"]
        filter1 [label="Input Filter\nL920, C905/906", fillcolor="#fff2cc"]
        pwm [label="PWM Controller\nIC920 (uPC494)", fillcolor="#d9e8fb"]
        drivers [label="Gate Drivers\nQ901/Q902", fillcolor="#d9e8fb"]
        fets [label="Switching FETs\nQ903-Q906", fillcolor="#ffd9d9"]
    }

    subgraph cluster_xfmr {
        label="ISOLATION"
        style="filled"
        color="#f0f0f0"

        xfmr [label="Transformer\nT901\n5:8:1", shape=box3d, fillcolor="#e8e8e8"]
    }

    subgraph cluster_secondary {
        label="SECONDARY SIDE\n(Amplifier Power)"
        style="dashed"
        color="#d94a4a"

        rect25 [label="Rectifiers\nD802/D803", fillcolor="#fff2cc"]
        rect23 [label="Rectifiers\nD801/D808", fillcolor="#fff2cc"]
        filter25 [label="Filter\nC806/C807", fillcolor="#fff2cc"]
        filter23 [label="Filter\nC808/C809", fillcolor="#fff2cc"]
        rail25 [label="±25V Rails\nOutput Stage", fillcolor="#d9fbd9"]
        rail23 [label="±23V Rails\nDrivers", fillcolor="#d9fbd9"]
        reg [label="Regulators\nQ801/Q802", fillcolor="#d9e8fb"]
        rail14 [label="±14V Rails\nPreamp", fillcolor="#d9fbd9"]
    }

    battery -> fuses -> filter1
    filter1 -> fets
    pwm -> drivers -> fets [style=dashed, label="control"]
    fets -> xfmr
    xfmr -> rect25
    xfmr -> rect23
    rect25 -> filter25 -> rail25
    rect23 -> filter23 -> rail23
    rail23 -> reg -> rail14
}
{{< /graphviz >}}

## Primary Side

The **primary side** is everything connected to the battery input, before the transformer.

### Components

{{< graphviz >}}
digraph primary_detail {
    rankdir=LR
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=11]
    edge [fontname="Helvetica", fontsize=9]

    bat [label="+B\n14.4V", fillcolor="#e8f4e8"]
    f1 [label="F901\n15A", fillcolor="#fff2cc"]
    f2 [label="F902\n15A", fillcolor="#fff2cc"]
    l920 [label="L920\nChoke", fillcolor="#fff2cc"]
    c905 [label="C905\n2200µF", fillcolor="#fff2cc"]
    c906 [label="C906\n2200µF", fillcolor="#fff2cc"]

    ic920 [label="IC920\nuPC494\nPWM", fillcolor="#d9e8fb"]
    q901 [label="Q901\n2SB1132", fillcolor="#d9e8fb"]
    q902 [label="Q902\n2SB1132", fillcolor="#d9e8fb"]

    q903 [label="Q903", fillcolor="#ffd9d9"]
    q904 [label="Q904", fillcolor="#ffd9d9"]
    q905 [label="Q905", fillcolor="#ffd9d9"]
    q906 [label="Q906", fillcolor="#ffd9d9"]

    t901 [label="T901\nPrimary", shape=box3d, fillcolor="#e8e8e8"]

    bat -> f1 -> l920
    bat -> f2 -> l920
    l920 -> c905
    l920 -> c906

    ic920 -> q901 [label="PWM A"]
    ic920 -> q902 [label="PWM B"]
    q901 -> q903 [label="gate"]
    q901 -> q904 [label="gate"]
    q902 -> q905 [label="gate"]
    q902 -> q906 [label="gate"]

    c905 -> q903 -> t901
    c905 -> q904 -> t901
    c906 -> q905 -> t901
    c906 -> q906 -> t901
}
{{< /graphviz >}}

### What Happens on the Primary Side

1. **Battery power enters** through fuses F901/F902 (protection)
2. **Input filter** (L920, C905/C906) smooths the DC and reduces noise
3. **IC920 generates PWM** at ~150kHz with dead-time control
4. **Q901/Q902 amplify** the PWM signals to drive FET gates
5. **Q903-Q906 switch** alternately, converting DC to high-frequency AC
6. **AC current flows** through the transformer primary winding

### Primary Side Characteristics

| Parameter | Value |
|-----------|-------|
| Input Voltage | 12-14.4V DC |
| Maximum Current | ~18A at full power |
| Switching Frequency | ~100-200kHz |
| FET Voltage (Vds) | 60V rating |

---

## Secondary Side

The **secondary side** is everything after the transformer, providing power to the amplifier stages.

### Components

{{< graphviz >}}
digraph secondary_detail {
    rankdir=LR
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=11]
    edge [fontname="Helvetica", fontsize=9]

    t901 [label="T901\nSecondary", shape=box3d, fillcolor="#e8e8e8"]

    d802 [label="D802\nFCH10A15", fillcolor="#fff2cc"]
    d803 [label="D803\nFRH10A15", fillcolor="#fff2cc"]
    d801 [label="D801\n11EFS2", fillcolor="#fff2cc"]
    d808 [label="D808\n11EFS2", fillcolor="#fff2cc"]

    c806 [label="C806\n1000µF", fillcolor="#fff2cc"]
    c807 [label="C807\n1000µF", fillcolor="#fff2cc"]
    c808 [label="C808\n220µF", fillcolor="#fff2cc"]
    c809 [label="C809\n220µF", fillcolor="#fff2cc"]

    p25 [label="+25V", fillcolor="#d9fbd9"]
    n25 [label="-25V", fillcolor="#d9fbd9"]
    p23 [label="+23V", fillcolor="#d9fbd9"]
    n23 [label="-23V", fillcolor="#d9fbd9"]

    q801 [label="Q801\n2SC3421", fillcolor="#d9e8fb"]
    q802 [label="Q802\n2SA1358", fillcolor="#d9e8fb"]

    p14 [label="+14V", fillcolor="#d9fbd9"]
    n14 [label="-14V", fillcolor="#d9fbd9"]

    out [label="Output\nTransistors\nQ161-Q462", fillcolor="#ffd9d9"]
    drv [label="Driver\nTransistors\nQ159-Q460", fillcolor="#ffd9d9"]
    pre [label="Preamp\nOp-Amps\nIC501-518", fillcolor="#d9e8fb"]

    t901 -> d802 -> c806 -> p25
    t901 -> d803 -> c807 -> n25
    t901 -> d801 -> c808 -> p23
    t901 -> d808 -> c809 -> n23

    p23 -> q801 -> p14
    n23 -> q802 -> n14

    p25 -> out
    n25 -> out
    p23 -> drv
    n23 -> drv
    p14 -> pre
    n14 -> pre
}
{{< /graphviz >}}

### What Happens on the Secondary Side

1. **Transformer induces AC** in secondary windings (magnetically coupled)
2. **Voltage is stepped up** by the turns ratio (~1.6× from 5:8)
3. **Rectifier diodes** convert AC back to pulsing DC
4. **Filter capacitors** smooth to clean DC
5. **Linear regulators** create stable ±14V for preamp

### Secondary Side Characteristics

| Rail | Voltage | Current | Powers |
|------|---------|---------|--------|
| ±25V | ±25V DC | High | Output transistors |
| ±23V | ±23V DC | Medium | Driver transistors |
| ±14V | ±14V DC | Low | Preamp op-amps |

---

## The Isolation Barrier

The transformer provides **galvanic isolation** - there is no direct electrical connection between primary and secondary.

{{< graphviz >}}
digraph isolation {
    rankdir=LR
    node [shape=box, style="rounded,filled", fontname="Helvetica"]

    subgraph cluster_pri {
        label="PRIMARY"
        style=filled
        color="#e8f0ff"
        pri_circuit [label="Battery\nGround\nReference", fillcolor="#d9e8fb"]
    }

    magnetic [label="Magnetic\nField\nOnly", shape=ellipse, fillcolor="#fff2cc"]

    subgraph cluster_sec {
        label="SECONDARY"
        style=filled
        color="#ffe8e8"
        sec_circuit [label="Amplifier\nGround\nReference", fillcolor="#ffd9d9"]
    }

    pri_circuit -> magnetic [label="Energy\nTransfer", style=dashed]
    magnetic -> sec_circuit [label="No Direct\nConnection", style=dashed]
}
{{< /graphviz >}}

### Benefits of Isolation

1. **Voltage transformation** - Step up 14V to ±25V efficiently
2. **Ground isolation** - Secondary can have independent ground reference
3. **Noise rejection** - Switching noise partially blocked
4. **Safety** - Fault on one side doesn't directly affect the other

---

## Fault Reflection: The Critical Concept

**A fault on the secondary side reflects back to the primary as increased load.**

{{< graphviz >}}
digraph fault_reflection {
    rankdir=TB
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=11]
    edge [fontname="Helvetica", fontsize=10]

    fault [label="Secondary Fault\n(e.g., D802 shorted)", fillcolor="#ff9999"]
    short [label="Dead Short on\nSecondary Winding", fillcolor="#ffcc99"]
    reflect [label="Reflected as\nLow Impedance\non Primary", fillcolor="#ffcc99"]
    current [label="Primary Current\nSkyrockets", fillcolor="#ff9999"]
    damage [label="FETs Overheat\nor Fuses Blow", fillcolor="#ff6666"]

    fault -> short -> reflect -> current -> damage
}
{{< /graphviz >}}

### Why This Matters for Diagnosis

| If Fault Is On... | You'll See... | But the Cause Is... |
|-------------------|---------------|---------------------|
| Secondary | FETs overheating | Shorted diode/transistor downstream |
| Secondary | Fuses blowing | Short on ±25V or ±23V rail |
| Primary | Same symptoms | Bad FETs or gate drivers |

**This is why you can't just replace FETs** - if a secondary fault caused the original failure, it will kill the new FETs too.

---

## Diagnosis Strategy

{{< graphviz >}}
digraph diagnosis {
    rankdir=TB
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=11]
    edge [fontname="Helvetica", fontsize=10]

    start [label="Symptom:\nFETs Failed", fillcolor="#d9e8fb"]
    remove [label="Remove FETs\nQ903-Q906", fillcolor="#fff2cc"]

    check_sec [label="Check Secondary\nDiodes & Rails", fillcolor="#fff2cc"]
    sec_ok [label="Secondary OK?", shape=diamond, fillcolor="#e8e8e8"]

    check_gate [label="Check Gate Drivers\nQ901/Q902", fillcolor="#fff2cc"]
    gate_ok [label="Gate Drivers OK?", shape=diamond, fillcolor="#e8e8e8"]

    check_pwm [label="Check PWM\nIC920", fillcolor="#fff2cc"]

    fix_sec [label="Fix Secondary\nFault First", fillcolor="#ff9999"]
    fix_gate [label="Replace Gate\nDrivers", fillcolor="#ff9999"]
    fix_pwm [label="Replace IC920", fillcolor="#ff9999"]

    install [label="Install New FETs\nTest with Current Limit", fillcolor="#d9fbd9"]

    start -> remove -> check_sec -> sec_ok
    sec_ok -> check_gate [label="YES"]
    sec_ok -> fix_sec [label="NO"]

    check_gate -> gate_ok
    gate_ok -> check_pwm [label="YES"]
    gate_ok -> fix_gate [label="NO"]

    fix_sec -> install
    fix_gate -> install
    check_pwm -> install [label="OK"]
    check_pwm -> fix_pwm [label="BAD"]
    fix_pwm -> install
}
{{< /graphviz >}}

---

## Your Symptom Analysis

Your observation: **Voltage clamped at 8V, current rising to 1.2A**

{{< graphviz >}}
digraph your_symptom {
    rankdir=LR
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=11]

    symptom [label="8V Clamp\n1.2A Rising", fillcolor="#ffcc99"]

    not_short [label="NOT a dead short\n(would be ~0V)", fillcolor="#d9fbd9"]
    partial [label="Something conducting\nheavily but not fully", fillcolor="#fff2cc"]

    cause1 [label="FETs in Linear Mode\n(Gate Driver Problem)\nPRIMARY SIDE", fillcolor="#ffd9d9"]
    cause2 [label="Partial Secondary Short\nReflecting Back\nSECONDARY SIDE", fillcolor="#ffd9d9"]
    cause3 [label="Transformer Issue\nBOTH SIDES", fillcolor="#ffd9d9"]

    symptom -> not_short
    symptom -> partial
    partial -> cause1
    partial -> cause2
    partial -> cause3
}
{{< /graphviz >}}

**Most likely cause:** Q901/Q902 gate drivers not providing enough drive, causing FETs to operate in linear region (high resistance, high power dissipation).

**Test priority:**
1. Q901/Q902 (primary side gate drivers)
2. D801-D803, D808 (secondary side rectifiers)
3. Secondary rail resistance to ground
4. Transformer winding resistance
