---
title: "Troubleshooting Flowchart"
weight: 3
---

# Troubleshooting Flowchart

Use these decision trees to quickly identify the likely failure area based on symptoms.

---

## Symptom: Amplifier Smokes on Power-Up

```
Amplifier smokes on power-up
            │
            ▼
    ┌───────────────────┐
    │ Where does smoke  │
    │ come from?        │
    └─────────┬─────────┘
              │
    ┌─────────┼─────────┬─────────────────┐
    ▼         ▼         ▼                 ▼
 Output    Power     Can't Tell      Preamp
 Stage     Supply                    Area
    │         │         │                 │
    ▼         ▼         ▼                 ▼
 Check     Check     Remove FETs      Check
 Q161-162  Q903-906  Q903-906         ±14V
 Q261-262  Q901-902  Apply power      Rails
 Q361-362  D801-803  w/current        and
 Q461-462  D808      limit            Op-Amps
    │         │         │
    │         │         ▼
    │         │    ┌────────────┐     Still
    │         │    │Still draws │     smokes?
    │         │    │high current│        │
    │         │    └─────┬──────┘        ▼
    │         │          │           Check
    │         │    ┌─────┴─────┐     IC501-518
    │         │    ▼           ▼     Q801-802
    │         │   YES          NO
    │         │    │            │
    │         │    ▼            ▼
    │         │  Short on    FETs or
    │         │  secondary   gate drivers
    │         │  Check       were the
    │         │  D801-808    problem
    │         │  T901
    │         │  Rails
    ▼         ▼
 Output    DC/DC
 stage     converter
 failure   failure
```

---

## Symptom: No Output (Dead)

```
No output from amplifier
            │
            ▼
    ┌───────────────────┐
    │ Does LED light up?│
    └─────────┬─────────┘
              │
        ┌─────┴─────┐
        ▼           ▼
       YES          NO
        │            │
        ▼            ▼
    ┌────────┐   ┌────────────┐
    │Check   │   │Check power │
    │preamp  │   │supply      │
    │& power │   │            │
    │amp     │   │• Fuses     │
    └────┬───┘   │• +B input  │
         │       │• Remote    │
         ▼       │• IC920     │
    ┌────────┐   └────────────┘
    │±14V OK?│
    └────┬───┘
         │
    ┌────┴────┐
    ▼         ▼
   YES        NO
    │          │
    ▼          ▼
 Check      Check
 signal     Q801/Q802
 path       DC/DC
 through    converter
 preamp     output
```

---

## Symptom: Distorted Output

```
Distorted output
        │
        ▼
┌───────────────────┐
│All channels or    │
│specific channel?  │
└─────────┬─────────┘
          │
    ┌─────┴─────┐
    ▼           ▼
   ALL       SPECIFIC
    │         CHANNEL
    │            │
    ▼            ▼
 Check        Check that
 power        channel's:
 supply       • Output transistors
 rails        • Driver transistors
 ±25V         • Bias components
 ±23V
 ±14V
    │
    ▼
 If rails    If rails
 low or      OK, check
 unstable:   preamp
 DC/DC       op-amps
 problem
```

---

## Symptom: Blows Fuses

```
Blows fuses immediately
            │
            ▼
    ┌───────────────────┐
    │Remove FETs        │
    │Q903-Q906          │
    └─────────┬─────────┘
              │
              ▼
    ┌───────────────────┐
    │Still blows fuses? │
    └─────────┬─────────┘
              │
        ┌─────┴─────┐
        ▼           ▼
       YES          NO
        │            │
        ▼            ▼
    Short is      FETs were
    before the    shorted
    FETs:         │
    • Fuse holder ▼
    • Wiring      Check what
    • L920        killed the
    • Capacitors  FETs:
                  • Q901/Q902
                  • Secondary
                    diodes
                  • Transformer
```

---

## Symptom: Protection Triggers / Muted

```
Amplifier stays muted
(LED on but no output)
            │
            ▼
    ┌───────────────────┐
    │Wait 5+ seconds    │
    │(turn-on delay)    │
    └─────────┬─────────┘
              │
              ▼
    ┌───────────────────┐
    │Still muted?       │
    └─────────┬─────────┘
              │
        ┌─────┴─────┐
        ▼           ▼
       YES          NO
        │            │
        ▼            ▼
    Protection    Normal
    is active     operation
        │
        ▼
    ┌───────────────────┐
    │Check for:         │
    │• DC at outputs    │
    │• Overheating      │
    │• Shorted speaker  │
    │• Low supply volt  │
    └───────────────────┘
```

---

## Symptom: Supply Voltage Drops Under Load

```
Supply voltage drops when FETs installed
            │
            ▼
    ┌───────────────────┐
    │How far does it    │
    │drop?              │
    └─────────┬─────────┘
              │
    ┌─────────┼─────────┐
    ▼         ▼         ▼
 Near 0V   ~8-10V    ~12V but
 Dead      Clamped   unstable
 Short                  │
    │         │         ▼
    ▼         ▼      Marginal
 Check     FETs in   component
 for       linear    or
 shorted   mode -    connection
 FET or    check     issue
 diode     gate
           drivers
           Q901/Q902
```

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
