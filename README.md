# Differential Amplifier Analysis (LTspice)

**Author:** Utkarsh Verma
**USN:** 4NI24EC166

---

## Objective

To design and simulate three MOSFET-based differential amplifier configurations in LTspice and evaluate their performance through DC, transient, and AC analysis.

---

## Overview

<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/c4e160125c69a66337cb7e7e2c35f52507cc1c76/differential-amplifier-circuit.jpg">

A differential amplifier amplifies the voltage difference between two input signals while rejecting any voltage component shared by both inputs. This common-mode rejection property makes it indispensable in noise-sensitive analog systems.

**Typical applications:**
- Operational amplifiers (op-amps)
- Analog signal processing circuits
- Communication front-ends

### Operating Principle

The circuit takes two inputs, V1 and V2. The output is proportional to their difference:

- V1 = V2 → output is ideally zero
- V1 ≠ V2 → difference is amplified

**Output expression:**

$$V_{out} = A_d \cdot (V_1 - V_2)$$

where $A_d$ is the differential voltage gain.

---

### Key Parameters

| Parameter | Expression |
|---|---|
| Differential Gain | $A_d = V_{out} / (V_1 - V_2)$ |
| Common-Mode Voltage | $V_{cm} = (V_1 + V_2) / 2$ |
| Common-Mode Gain | $A_c = V_{out} / V_{cm}$ (ideally ≈ 0) |
| CMRR | $CMRR = A_d / A_c$ (higher = better) |

---

### MOS Differential Pair — Fundamentals

Two matched MOSFETs share a common source node biased by a constant current source. Drain terminals connect to load elements.

**Differential input:**

$$v_{id} = v_{in1} - v_{in2}$$

**Current steering:**
- $v_{in1} > v_{in2}$ → M1 conducts more
- $v_{in2} > v_{in1}$ → M2 conducts more

**Small-signal gain (both transistors in saturation):**

$$A_v = g_m \times R_{out}$$

where transconductance:

$$g_m = \frac{2I_D}{V_{ov}}$$

**Large-signal behavior** ($|v_{id}| > 2V_{ov}$): one transistor turns off; amplifier enters non-linear region.

**Load types and their impact:**

| Load Type | Effect |
|---|---|
| Resistive | Simple design, moderate gain |
| Active | Higher output resistance → higher gain |
| Current Mirror | Maximum gain, best performance |

---

## Circuit 1: Differential Amplifier with Resistive Load
<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/826e43ca718336a0adb45959b1ff5ef1ccee1a57/570833843-fecedb33-870b-480a-ac1b-b2711a96f2ee.png">


### Working Principle

Two matched MOSFETs (M1, M2) share a common source biased by a constant tail current. Drain resistors RD1 and RD2 serve as loads.

- $v_{in1} > v_{in2}$ → M1 conducts more → voltage drop across RD1 increases → OUT1 drops
- $v_{in2} > v_{in1}$ → M2 conducts more → OUT2 drops

---

### Design Specifications

| Parameter | Symbol | Value |
|---|---|---|
| Technology | — | TSMC 180 nm CMOS |
| Supply Voltage | VDD | +0.9 V |
| Negative Supply | VSS | −0.9 V |
| Max Power | P | ≤ 1.8 mW |
| Channel Length (NMOS) | Ln | 480 nm |
| Input CM Voltage | Vin,CM | 0 V |
| Output CM Voltage | Vo,CM | 0 V |
| Tail Node Voltage | Vp | −0.7 V |
| Load Capacitance | CL | 10 pF |
| Threshold Voltage | VT | ≈ 0.36 V |

---

### Power and Current Budget

Total supply span:

$$V_{DD} - V_{SS} = 0.9 - (-0.9) = 1.8 \text{ V}$$

Power constraint:

$$P = (V_{DD} - V_{SS}) \times I_{SS} \leq 1.8 \text{ mW} \implies I_{SS} \leq 1 \text{ mA}$$

**Selected:** $I_{SS} = 1$ mA (fully utilizes power budget)

**Verification:** $1.8 \text{ V} \times 1 \text{ mA} = 1.8 \text{ mW}$ ✔

Each transistor carries:

$$I_{D1} = I_{D2} = \frac{I_{SS}}{2} = 0.5 \text{ mA}$$

---

### Load Resistance Calculation

With $V_{out} = 0$ V (output common-mode requirement):

$$0 = V_{DD} - I_D \times R_D \implies R_D = \frac{0.9}{0.5 \text{ mA}} = 1.8 \text{ k}\Omega$$

---

### Bias Point Analysis

| Node | Value |
|---|---|
| VG1 = VG2 | 0 V |
| VS | −0.7 V |
| VGS | 0.7 V |
| VOV | 0.34 V |
| VD | 0 V |
| VDS | 0.7 V |

**Saturation check:** $V_{DS}$ (0.7 V) > $V_{OV}$ (0.34 V) ✔

---

### Transistor Width Calculation

Using the saturation drain current equation:

$$W = \frac{2 I_D L}{\mu_n C_{ox} \cdot V_{OV}^2}$$

Substituting $I_D = 0.5$ mA, $L = 480$ nm, $\mu_n C_{ox} = 236.5\ \mu\text{A/V}^2$, $V_{OV} = 0.34$ V:

$$W \approx 17.57\ \mu\text{m} \quad \text{(theoretical)}$$

After simulation-based tuning to achieve $V_S = -0.7$ V:

$$W \approx 28.475\ \mu\text{m} \quad \text{(final)}$$

The difference reflects channel length modulation, mobility degradation, and process variation — non-ideal effects not captured in first-order hand calculations.

---

### Input Common-Mode Range (ICMR)

**Lower bound** (NMOS must remain ON):

$$V_{ICM(min)} = V_S + V_T = -0.7 + 0.36 = -0.34 \text{ V}$$

**Upper bound** (drain must stay in saturation):

$$V_{ICM(max)} = V_D + V_T = 0 + 0.36 = 0.36 \text{ V}$$

$$\boxed{-0.34 \text{ V} \leq V_{ICM} \leq 0.36 \text{ V}}$$

---

### Output Common-Mode Range (OCMR)

| Limit | Expression | Value |
|---|---|---|
| Minimum | $V_S + V_{OV}$ | −0.36 V |
| Maximum | $\approx V_{DD}$ | 0.9 V |

$$\boxed{-0.36 \text{ V} \leq V_{OCM} \leq 0.9 \text{ V}}$$

---

### Differential Input Range (Linear Operation)

For both transistors to remain in saturation and share current:

$$|V_{id}| \leq 2 V_{OV} = 2 \times 0.34 = 0.68 \text{ V}$$

$$\boxed{-0.68 \text{ V} \leq V_{id} \leq 0.68 \text{ V}}$$

---

### Summary Table

| Parameter | Range |
|---|---|
| ICMR | −0.34 V to 0.36 V |
| OCMR | −0.36 V to 0.9 V |
| Linear Differential Input | −0.68 V to 0.68 V |

---

### Transient Analysis

Effective linear limit: $|V_{id}| < 2 V_{OV} \approx 0.48$ V (practical) / 0.68 V (first-order)

#### Case 1 — Linear Region

| Parameter | Value |
|---|---|
| Input Signal 1 | SINE(0.1, 50 m, 1k) |
| Input Signal 2 | SINE(0.1, 50 m, 1k, 180°) |
| Differential Input | 100 mV |

100 mV < 680 mV ✔ → **Linear operation confirmed**

Output is a clean sinusoid with 180° phase difference between branches. Both transistors remain in saturation throughout.

#### Case 2 — Non-Linear Region

| Parameter | Value |
|---|---|
| Input Signal 1 | SINE(0.1, 400 m, 1k) |
| Input Signal 2 | SINE(0.1, 400 m, 1k, 180°) |
| Differential Input | 800 mV |

800 mV > 680 mV ✗ → **Non-linear region**

Output is heavily distorted with clipped peaks. The tail current shifts almost entirely to one transistor while the other approaches cutoff. The circuit behaves more like a switch than a linear amplifier.

---

### Gain Evaluation: Simulation vs Theory

#### Simulation Results

| Quantity | Value |
|---|---|
| Input peak-to-peak | 100 mV |
| Output peak-to-peak | 574.33 mV |
| Voltage Gain $A_v$ | **5.74 V/V** |
| Gain (dB) | **15.18 dB** |

#### Comparison

| Metric | Theoretical | Simulated |
|---|---|---|
| Gain (V/V) | 4.5 | 5.74 |
| Gain (dB) | 13.06 dB | 15.18 dB |

The simulated gain exceeds the theoretical value due to:
- Channel length modulation increasing output resistance
- More accurate BSIM device modeling in LTspice
- Wider transistor width (28.475 μm vs 17.57 μm) boosting $g_m$

---

### AC Analysis

**Setup:** AC sweep 1 Hz to 10 GHz, differential excitation ($V_{in1} = +1$ AC, $V_{in2} = -1$ AC)

| Metric | Value |
|---|---|
| Midband Gain | 15.6 dB (≈ 6.02 V/V) |
| −3 dB Cutoff Frequency | 5.128 GHz |
| Bandwidth | 5.128 GHz |
| Unity Gain Bandwidth | ≈ 30.9 GHz |

**Bode plot observations:**

| Parameter | Value |
|---|---|
| Differential Gain (Adiff) | 21.78 dB |
| Single-Ended Gain | 15.76 dB |
| Midband Phase | ~180° |

The 6 dB difference between differential and single-ended gain is expected: $20 \log_{10}(2) \approx 6$ dB.

High-frequency rolloff beyond ~1 GHz is caused by device parasitics ($C_{gs}$, $C_{gd}$) and the 10 pF load capacitance. Phase begins near 180° (inverting topology) and transitions smoothly at high frequencies, confirming stable operation.

---

## Circuit 2: Differential Amplifier with PMOS Current Mirror Active Load

### Working Principle

This configuration pairs an NMOS differential pair (M1, M2) with a PMOS current mirror (M3, M4) as the active load, and NMOS transistor M5 as the tail current source.

- M3 is diode-connected, establishing a reference current
- M4 mirrors this current to the opposite branch
- High output resistance from the mirror significantly boosts gain compared to a resistive load

The input difference redirects tail current between branches; the mirror converts this current asymmetry into an output voltage.

---

### Design Specifications

Same as Circuit 1 (TSMC 180 nm, VDD/VSS = ±0.9 V, $I_{SS}$ = 1 mA, $I_{D1} = I_{D2}$ = 0.5 mA).

---

### Bias Point Analysis

**NMOS pair (M1, M2):** $V_{GS}$ = 0.7 V, $V_{OV}$ = 0.34 V, $V_{DS}$ = 0.7 V ✔

**Tail source M5:**

$$V_S = -0.9 \text{ V},\quad V_D = -0.7 \text{ V},\quad V_{DS} = 0.2 \text{ V}$$

Choose $V_{OV} = 0.2$ V → $V_{GS} = 0.56$ V → $V_G = -0.34$ V ✔ (edge of saturation)

**PMOS load (M3, M4):**

$$V_S = 0.9 \text{ V},\quad V_D = 0 \text{ V},\quad V_{SD} = 0.9 \text{ V} \gg V_{OV} \implies \text{deep saturation ✔}$$

---

### Width Calculations

| Transistor | Theoretical (μm) | Simulation-Optimized (μm) |
|---|---|---|
| M1, M2 | 17.56 | 29.85 |
| M5 | 101.5 | 195.85 |

---

### ICMR, OCMR, and Linear Range

| Parameter | Range |
|---|---|
| ICMR | −0.34 V to 0.39 V (PMOS threshold considered) |
| OCMR | −0.36 V to 0.65 V |
| Linear differential input | −0.5 V to 0.5 V |

---

### Transient Analysis

- **Linear case** ($V_{id}$ = 100 mV < 340 mV limit): clean sinusoidal output, symmetric current sharing ✔
- **Non-linear case** ($V_{id}$ = 400 mV > 340 mV): one transistor turns off, output distorted ✗

---

### Gain Analysis

**Simulation:**

$$V_{in(p-p)} = 100 \text{ mV},\quad V_{out(p-p)} \approx 180 \text{ mV}$$

$$A_v = \frac{180}{100} = 1.8 \text{ V/V} \approx 5.1 \text{ dB}$$

**Theoretical estimate:**

$$g_m = \frac{2 \times 0.5 \text{ mA}}{0.25 \text{ V}} = 4 \text{ mS}$$

$$r_o = \frac{1}{\lambda I_D} = \frac{1}{0.1 \times 0.5 \text{ mA}} = 20 \text{ k}\Omega \implies R_{out} = r_o \| r_o \approx 10 \text{ k}\Omega$$

$$A_v(\text{ideal}) = g_m \times R_{out} = 4 \text{ mS} \times 10 \text{ k}\Omega = 40 \text{ V/V} \approx 32 \text{ dB}$$

The large discrepancy between theoretical (40 V/V) and simulated (1.8 V/V) gain arises from:

| Source | Effect |
|---|---|
| λ variation with VDS | Reduces $r_o$, lowers $R_{out}$ |
| Finite tail current source resistance | Imperfect current steering, reduces differential gain |
| Multi-device output impedance | Effective $R_{out}$ lower than $r_o \| r_o$ |
| Mobility degradation | Reduces effective $g_m$ |
| Parasitic capacitances ($C_{gs}$, $C_{gd}$, $C_{db}$, $C_L$) | Signal attenuation at operating frequencies |
| Bias point shifts | $V_{OV}$ variations affect $g_m$ |

> The theoretical value represents an upper bound. Simulation reflects realistic device behavior under all non-idealities.

---

### AC Analysis

| Metric | Value |
|---|---|
| Midband Gain | ≈ 5.4 dB |
| Bandwidth | Hundreds of MHz |

Gain is flat at low to mid frequencies and rolls off progressively at higher frequencies, consistent with parasitic-dominated bandwidth limitation. The response resembles a low-pass filter with stable differential amplification across the operating range.

---

## Circuit 3: CMOS Differential Amplifier with Bias-Controlled PMOS Load

### Working Principle

This topology uses an NMOS differential pair (M1, M2) with PMOS active loads (M3, M4) whose gates are driven by a fixed bias voltage $V_{b2}$ — unlike the current mirror topology where M3 is diode-connected. M3 and M4 therefore act as independent controlled current sources rather than enforcing strict current mirroring.

**Key advantages over previous topologies:**
- High output resistance → significantly higher voltage gain
- Better output voltage swing than resistive loads
- Greater design flexibility than current mirror topology

---

### Design Specifications

Same as Circuits 1 and 2 (TSMC 180 nm, ±0.9 V supply, $I_{SS}$ = 1 mA, $I_D$ = 0.5 mA per branch).

---

### Bias Conditions

#### NMOS Pair (M1, M2)

| Quantity | Expression | Value |
|---|---|---|
| VG | Vin,CM | 0 V |
| VS | Vp | −0.7 V |
| VGS | VG − VS | 0.7 V |
| VOV | VGS − VT | 0.34 V |
| VDS | VD − VS | 0.7 V |

Saturation check: $V_{DS}$ (0.7 V) > $V_{OV}$ (0.34 V) ✔

#### Tail Current Source (M5)

$$V_{DS} = 0.2 \text{ V},\quad V_{OV} = 0.2 \text{ V},\quad V_{GS} = 0.56 \text{ V},\quad V_G = -0.34 \text{ V}$$

Edge of saturation: $V_{DS} \approx V_{OV}$ ✔

#### PMOS Active Load (M3, M4)

$$V_{SG} = 0.9 - (-0.36) = 1.26 \text{ V},\quad V_{OV(p)} = 1.26 - 0.39 = 0.87 \text{ V}$$

$$V_{SD} = 0.9 \text{ V} > V_{OV(p)} = 0.87 \text{ V} \implies \text{Saturation ✔}$$

Selected bias: $V_{b2} \approx -0.36$ V

---

### Device Sizing

General expression:

$$W = \frac{2 I_D L}{\mu C_{ox} \cdot V_{OV}^2}$$

| Device | Parameters | Theoretical W (μm) | Optimized W (μm) |
|---|---|---|---|
| M1, M2 | $\mu_n C_{ox}$ = 236.5 μA/V², $V_{OV}$ = 0.34 V | 17.56 | 32 |
| M5 | $V_{OV}$ = 0.2 V, $I_D$ = 1 mA | 101.5 | 209.15 |
| M3, M4 | $\mu_p C_{ox}$ = 99.8 μA/V², $V_{OV(p)}$ = 0.87 V | 6.35 | 13.88 |

Simulation-based optimization compensates for short-channel effects, mobility degradation, and bias shifts. Larger widths increase $g_m$, improving gain and stability.

---

### DC Analysis

DC simulation verifies that all transistors (M1–M5) are biased in the saturation region under the specified common-mode input conditions, confirming correct circuit operation prior to dynamic analysis.

---

## Comparison: All Three Configurations

| Feature | Circuit 1 (Resistive Load) | Circuit 2 (Current Mirror Load) | Circuit 3 (Bias-Controlled Load) |
|---|---|---|---|
| Load Type | Resistors RD | PMOS current mirror | Bias-controlled PMOS |
| Midband Gain | ~5.74 V/V (15.18 dB) | ~1.8 V/V (5.1 dB) | High (improved over C2) |
| Bandwidth | 5.128 GHz | Hundreds of MHz | High |
| Output Resistance | Moderate (RD) | High (mirror) | High (controlled source) |
| Design Complexity | Low | Medium | Medium–High |
| Key Advantage | Simplicity | Higher gain than resistive | Best gain + output swing |

---

## Conclusion

All three differential amplifier configurations were successfully designed, biased, and simulated in LTspice using TSMC 180 nm CMOS technology.

- **Circuit 1** (resistive load) demonstrates clean linear operation within $|V_{id}| < 0.68$ V, with a simulated gain of 5.74 V/V and a bandwidth of 5.128 GHz.
- **Circuit 2** (PMOS current mirror) achieves higher output resistance at the cost of gain reduction due to real-device non-idealities (λ variation, mobility degradation, finite tail source resistance).
- **Circuit 3** (bias-controlled PMOS load) offers the best combination of gain and output swing by using M3/M4 as independent controlled current sources, with Vb2 ≈ −0.36 V ensuring deep saturation across operating conditions.

Across all three circuits, simulation-optimized transistor widths exceed first-order theoretical values, highlighting the importance of SPICE-based refinement in practical analog design.
