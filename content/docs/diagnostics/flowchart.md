---
title: "Troubleshooting Flowchart"
weight: 3
---

# Troubleshooting Flowchart

Use these decision trees to quickly identify the likely failure area based on symptoms.

---

## Symptom: Amplifier Smokes on Power-Up

{{< graphviz >}}
digraph smoke {
    rankdir=TB
    splines=ortho
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=10]
    edge [fontname="Helvetica", fontsize=9]

    start [label="Amplifier smokes\non power-up", fillcolor="#ffcccc"]
    question [label="Where does smoke\ncome from?", shape=diamond, fillcolor="#fff2cc"]

    output_area [label="Output\nStage", fillcolor="#e0e0e0"]
    power_area [label="Power\nSupply", fillcolor="#e0e0e0"]
    unknown [label="Can't Tell", fillcolor="#e0e0e0"]
    preamp_area [label="Preamp\nArea", fillcolor="#e0e0e0"]

    check_output [label="Check\nQ161-162, Q261-262\nQ361-362, Q461-462", fillcolor="#d9e8fb"]
    check_power [label="Check\nQ903-906, Q901-902\nD801-803, D808", fillcolor="#d9e8fb"]
    remove_fets [label="Remove FETs Q903-906\nApply power\nw/current limit", fillcolor="#d9e8fb"]
    check_preamp [label="Check ±14V Rails\nand Op-Amps\nIC501-518, Q801-802", fillcolor="#d9e8fb"]

    still_draws [label="Still draws\nhigh current?", shape=diamond, fillcolor="#fff2cc"]
    yes_short [label="Short on secondary\nCheck D801-808\nT901, Rails", fillcolor="#ffcccc"]
    no_fets [label="FETs or gate drivers\nwere the problem", fillcolor="#d9fbd9"]

    output_fail [label="Output stage\nfailure", fillcolor="#ffcccc"]
    dcdc_fail [label="DC/DC converter\nfailure", fillcolor="#ffcccc"]

    start -> question
    question -> output_area
    question -> power_area
    question -> unknown
    question -> preamp_area

    output_area -> check_output -> output_fail
    power_area -> check_power -> dcdc_fail
    preamp_area -> check_preamp

    unknown -> remove_fets -> still_draws
    still_draws -> yes_short [label="YES"]
    still_draws -> no_fets [label="NO"]
}
{{< /graphviz >}}

---

## Symptom: No Output (Dead)

{{< graphviz >}}
digraph no_output {
    rankdir=TB
    splines=ortho
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=10]
    edge [fontname="Helvetica", fontsize=9]

    start [label="No output from\namplifier", fillcolor="#ffcccc"]
    led_q [label="Does LED\nlight up?", shape=diamond, fillcolor="#fff2cc"]

    led_yes [label="LED ON", fillcolor="#d9fbd9"]
    led_no [label="LED OFF", fillcolor="#ffcccc"]

    check_preamp [label="Check preamp\n& power amp", fillcolor="#d9e8fb"]
    check_power [label="Check power supply\n• Fuses\n• +B input\n• Remote\n• IC920", fillcolor="#d9e8fb"]

    v14_q [label="±14V OK?", shape=diamond, fillcolor="#fff2cc"]

    v14_yes [label="Check signal path\nthrough preamp", fillcolor="#d9e8fb"]
    v14_no [label="Check Q801/Q802\nDC/DC converter\noutput", fillcolor="#d9e8fb"]

    start -> led_q
    led_q -> led_yes [label="YES"]
    led_q -> led_no [label="NO"]

    led_yes -> check_preamp -> v14_q
    led_no -> check_power

    v14_q -> v14_yes [label="YES"]
    v14_q -> v14_no [label="NO"]
}
{{< /graphviz >}}

---

## Symptom: Distorted Output

{{< graphviz >}}
digraph distorted {
    rankdir=TB
    splines=ortho
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=10]
    edge [fontname="Helvetica", fontsize=9]

    start [label="Distorted output", fillcolor="#ffcccc"]
    question [label="All channels or\nspecific channel?", shape=diamond, fillcolor="#fff2cc"]

    all_ch [label="ALL channels", fillcolor="#e0e0e0"]
    specific [label="SPECIFIC channel", fillcolor="#e0e0e0"]

    check_rails [label="Check power supply rails\n±25V, ±23V, ±14V", fillcolor="#d9e8fb"]
    check_channel [label="Check that channel's:\n• Output transistors\n• Driver transistors\n• Bias components", fillcolor="#d9e8fb"]

    rails_q [label="Rails OK?", shape=diamond, fillcolor="#fff2cc"]
    dcdc_prob [label="DC/DC problem\nCheck converter", fillcolor="#ffcccc"]
    preamp_prob [label="Check preamp\nop-amps", fillcolor="#d9e8fb"]

    start -> question
    question -> all_ch [label="ALL"]
    question -> specific [label="ONE"]

    all_ch -> check_rails -> rails_q
    specific -> check_channel

    rails_q -> dcdc_prob [label="NO\n(low/unstable)"]
    rails_q -> preamp_prob [label="YES"]
}
{{< /graphviz >}}

---

## Symptom: Blows Fuses

{{< graphviz >}}
digraph fuses {
    rankdir=TB
    splines=ortho
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=10]
    edge [fontname="Helvetica", fontsize=9]

    start [label="Blows fuses\nimmediately", fillcolor="#ffcccc"]
    remove [label="Remove FETs\nQ903-Q906", fillcolor="#d9e8fb"]
    still_q [label="Still blows\nfuses?", shape=diamond, fillcolor="#fff2cc"]

    yes_short [label="Short is before FETs:\n• Fuse holder\n• Wiring\n• L920\n• Capacitors", fillcolor="#ffcccc"]

    no_fets [label="FETs were shorted", fillcolor="#fff2cc"]
    check_cause [label="Check what killed FETs:\n• Q901/Q902\n• Secondary diodes\n• Transformer", fillcolor="#d9e8fb"]

    start -> remove -> still_q
    still_q -> yes_short [label="YES"]
    still_q -> no_fets [label="NO"]
    no_fets -> check_cause
}
{{< /graphviz >}}

---

## Symptom: Protection Triggers / Muted

{{< graphviz >}}
digraph protection {
    rankdir=TB
    splines=ortho
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=10]
    edge [fontname="Helvetica", fontsize=9]

    start [label="Amplifier stays muted\n(LED on but no output)", fillcolor="#ffcccc"]
    wait [label="Wait 5+ seconds\n(turn-on delay)", fillcolor="#d9e8fb"]
    still_q [label="Still muted?", shape=diamond, fillcolor="#fff2cc"]

    normal [label="Normal\noperation", fillcolor="#d9fbd9"]
    protection [label="Protection\nis active", fillcolor="#ffcccc"]

    check [label="Check for:\n• DC at outputs\n• Overheating\n• Shorted speaker\n• Low supply voltage", fillcolor="#d9e8fb"]

    start -> wait -> still_q
    still_q -> normal [label="NO"]
    still_q -> protection [label="YES"]
    protection -> check
}
{{< /graphviz >}}

---

## Symptom: Supply Voltage Drops Under Load

{{< graphviz >}}
digraph voltage_drop {
    rankdir=TB
    splines=ortho
    node [shape=box, style="rounded,filled", fontname="Helvetica", fontsize=10]
    edge [fontname="Helvetica", fontsize=9]

    start [label="Supply voltage drops\nwhen FETs installed", fillcolor="#ffcccc"]
    question [label="How far does\nit drop?", shape=diamond, fillcolor="#fff2cc"]

    near_zero [label="Near 0V\nDead Short", fillcolor="#ff9999"]
    clamped [label="~8-10V\nClamped", fillcolor="#ffcccc"]
    unstable [label="~12V but\nunstable", fillcolor="#fff2cc"]

    check_short [label="Check for shorted\nFET or diode", fillcolor="#d9e8fb"]
    check_gate [label="FETs in linear mode\nCheck gate drivers\nQ901/Q902", fillcolor="#d9e8fb"]
    check_marginal [label="Marginal component\nor connection issue", fillcolor="#d9e8fb"]

    start -> question
    question -> near_zero
    question -> clamped
    question -> unstable

    near_zero -> check_short
    clamped -> check_gate
    unstable -> check_marginal
}
{{< /graphviz >}}

---

## Quick Diagnosis Table

| Symptom | Most Likely Cause | First Check |
|---------|-------------------|-------------|
| Smokes immediately | Shorted FET or diode | Remove FETs, check D801-808 |
| Smokes after delay | Thermal runaway | Gate drivers Q901/Q902 |
| No LED | No power input | Fuses, wiring, remote |
| LED on, no sound | Mute active or preamp | ±14V rails, mute circuit |
| One channel dead | Channel-specific failure | That channel's transistors |
| Distortion all channels | Power supply issue | Rail voltages |
| Blows fuses | Short circuit | Isolate by removing FETs |
| Gets very hot | Bias problem or short | Check output DC offset |
