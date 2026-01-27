---
title: "Transformer Repair"
weight: 4
---

# Transformer T901 Repair Options

The DC/DC converter transformer (T901) is a custom Alpine part (25E39082S01) that is essentially unobtainable. If damaged, your options are repair or fabrication.

## T901 Specifications

| Parameter | Value |
|-----------|-------|
| Part Number | 25E39082S01 |
| Type | Push-pull DC/DC converter |
| Turns Ratio | 5:8:1 |
| Core | Ferrite (likely EE or EI type) |
| Primary | Center-tapped |
| Secondary | Multiple windings for ±25V and ±23V |

## Diagnosing Transformer Failure

### Symptoms of Failed Transformer

| Symptom | Likely Cause |
|---------|--------------|
| Very high primary current | Shorted turns |
| No output on all rails | Open winding |
| Buzzing/humming | Loose laminations or cracked core |
| Burning smell from transformer | Insulation breakdown |
| One rail missing | Partial winding failure |

### Testing Procedure

**1. Resistance Measurements (power off, FETs removed)**

| Winding | Expected | Failed |
|---------|----------|--------|
| Primary (half to center tap) | 0.1-0.3Ω | 0Ω (shorted) or OL (open) |
| Primary (end to end) | 0.2-0.6Ω | 0Ω or OL |
| Each secondary | 0.1-0.5Ω | 0Ω or OL |

**2. Isolation Test**

Between any two electrically separate windings: Should be **OL (infinite)**

If you measure any resistance between primary and secondary, the insulation has failed.

**3. Inductance Test (if you have an LCR meter)**

Primary inductance should be consistent between both halves. Significant imbalance indicates shorted turns.

---

## Repair Option 1: Rewind the Existing Transformer

This is the most practical option if the ferrite core is intact.

### Disassembly

1. **Document everything** - Take photos, note wire colors, count turns if visible
2. **Remove potting compound** (if present) - Heat gently or use solvent
3. **Separate core halves** - Usually glued; gentle heat softens adhesive
4. **Carefully unwrap windings** - Note the winding order, direction, and layer insulation
5. **Inspect core** - Look for cracks, chips, or burn marks

{{< hint warning >}}
**Ferrite cores are brittle.** Handle with care. A cracked core cannot be repaired.
{{< /hint >}}

### Determining Winding Specifications

If you can't count the original turns, you'll need to calculate:

**Given Information:**
- Input voltage: 14.4V nominal
- Output voltages: ±25V, ±23V
- Turns ratio: 5:8:1 (from service manual)
- Switching frequency: ~100-200kHz (typical for uPC494)

**Estimating Turns:**

For a push-pull converter at ~150kHz with a typical ferrite core:

| Winding | Estimated Turns | Wire Gauge (approx) |
|---------|-----------------|---------------------|
| Primary (each half) | 5-8 turns | 18-20 AWG (heavy current) |
| Secondary ±25V | 8-12 turns | 20-22 AWG |
| Secondary ±23V | 7-11 turns | 22-24 AWG |

{{< hint info >}}
These are estimates. The actual values depend on the specific core size and material. Start with these as a baseline and adjust based on output voltage measurements.
{{< /hint >}}

### Rewinding Procedure

1. **Wind primary first** (or as per original construction)
   - Use appropriate gauge magnet wire
   - Keep windings tight and even
   - Add layer insulation (Kapton tape or transformer tape)

2. **Add interlayer insulation** between primary and secondary

3. **Wind secondary windings**
   - Maintain proper phasing (dot convention)
   - Each secondary needs a center tap for full-wave rectification

4. **Final insulation layer**

5. **Reassemble core**
   - Ensure no air gap (unless original had one)
   - Secure with tape or adhesive

6. **Test before installing**
   - Measure all winding resistances
   - Verify isolation between windings

---

## Repair Option 2: Substitute a Compatible Transformer

Finding an exact replacement is unlikely, but you may find a similar transformer from:

- **Donor amplifier** - Same model or similar Alpine/other brand
- **Generic push-pull transformer** - May require adaptation
- **Custom winding service** - Provide specs to a transformer shop

### What to Look For in a Substitute

| Parameter | Requirement |
|-----------|-------------|
| Topology | Push-pull (center-tapped primary) |
| Input voltage | 12-14.4V DC |
| Output voltage | ~±25V (can adjust with turns) |
| Power rating | ≥200W |
| Frequency | Compatible with uPC494 (~100-200kHz) |
| Physical size | Must fit in chassis |

---

## Repair Option 3: Custom Fabrication

If you have access to ferrite cores and magnet wire, you can wind a replacement from scratch.

### Core Selection

For a ~200W push-pull converter at 150kHz, consider:

| Core Type | Size | Notes |
|-----------|------|-------|
| EE | EE42, EE55 | Common, easy to wind |
| ETD | ETD44, ETD49 | Good for high power |
| RM | RM12, RM14 | Compact |

### Calculation Resources

- **Online calculators:** Search for "push-pull transformer calculator"
- **Application notes:** TI, ON Semi, and other power IC manufacturers
- **Software:** EPCOS/TDK design tools, Magnetics Designer

### Basic Design Approach

1. **Determine power requirement:** ~200W (50W × 4 channels)
2. **Select core** based on power and frequency
3. **Calculate turns** using:
   ```
   Np = Vin / (4 × f × Bmax × Ae)

   Where:
   Np = Primary turns (half winding)
   Vin = Input voltage (14.4V)
   f = Switching frequency (Hz)
   Bmax = Maximum flux density (typically 0.1-0.2T for ferrite)
   Ae = Core effective area (from datasheet)
   ```
4. **Calculate secondary turns** based on desired voltage ratio
5. **Select wire gauge** based on current and skin depth

---

## Professional Rewinding Services

If DIY isn't feasible, consider:

- **Local transformer repair shops** - Often do custom work
- **Electronics repair specialists** - May have experience with SMPS transformers
- **Online services** - Search for "custom transformer winding service"

**What to Provide:**
- Physical dimensions of original transformer
- Turns ratio (5:8:1)
- Input/output voltage requirements
- Switching frequency (~150kHz)
- Power rating (~200W)
- Photos of original (if possible)

---

## Testing After Repair/Replacement

Before installing the repaired/new transformer:

1. **Measure all winding resistances** - Should be very low, balanced
2. **Verify isolation** - OL between all separate windings
3. **Check inductance balance** - Both primary halves should match
4. **Bench test** - If possible, test with a function generator and scope before installing

### First Power-Up Test

1. Install transformer but leave FETs out
2. Apply power with current limit at 100mA
3. Verify IC920 is generating pulses
4. Install FETs and slowly increase current limit
5. Monitor output voltages - should see ±25V and ±23V rails
6. Check for excessive heat

{{< hint danger >}}
**If output voltages are significantly wrong,** stop immediately. Incorrect turns ratio can damage downstream components or cause the transformer to overheat.
{{< /hint >}}
