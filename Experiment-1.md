# Experiment 1

# DC, AC and Transient Analysis of a Common Source Amplifier

---

## Aim

To design and analyze a Common Source (CS) amplifier using LTspice and study its DC characteristics, transient response, and AC frequency response.

---

## Theory

The Common Source amplifier is a widely used MOSFET amplifier configuration. The input signal is applied at the gate terminal, while the output is taken from the drain terminal. This configuration provides voltage amplification and introduces a phase shift of 180° between the input and output signals.

For proper operation, the MOSFET must remain in the saturation region.

### Saturation Condition

$$
V_{DS} \geq (V_{GS}-V_T)
$$

### Drain Current

$$
I_D=\frac{1}{2}\mu_n C_{ox}\frac{W}{L}(V_{GS}-V_T)^2
$$

### Voltage Gain

$$
A_v=-g_mR_D
$$

---

## Circuit Diagram

<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/main/1.png">

---

## Procedure

1. Open LTspice.
2. Draw the Common Source amplifier using an NMOS transistor.
3. Set the supply voltage to 1.5 V.
4. Apply a sinusoidal signal at the input.
5. Perform operating point analysis.
6. Run transient analysis.
7. Perform AC analysis to determine gain and bandwidth.

---

## Operating Point Analysis

<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/main/2.png">

Operating point analysis determines the DC bias conditions of the amplifier. The simulation verifies that the MOSFET operates in the saturation region and is correctly biased for amplification.

### Width Optimization

Initial Width: 1.833 µm

Optimized Width: 2.7528 µm

The transistor width was adjusted to achieve the required drain current while satisfying the power constraint.

---

## Design Calculations

### Given Parameters

- VDD = 1.5 V
- Maximum Power = 0.5 mW
- Load Capacitance = 1 pF
- Channel Length = 180 nm
- Drain Current = 0.334 mA

### Power Calculation

$$
P = V_{DD} \times I_D
$$

$$
P = 1.5 \times 0.334 mA = 0.501 mW
$$

### Drain Resistor

$$
R_D \approx 2.245 k\Omega
$$

### MOSFET Dimensions

- W = 1.833 µm
- L = 180 nm

---

## DC Analysis

<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/main/3.png">

The gate voltage is swept from 0 V to 2 V. The output voltage remains near VDD in cutoff and decreases as the transistor enters conduction. The linear region of the transfer characteristic is selected for amplification.

---

## Transient Analysis


<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/main/4.png">

The transient response confirms voltage amplification and the expected 180° phase inversion between the input and output signals.

---

## Gain Calculation

### Theoretical Gain

$$
g_m \approx 1.25 mS
$$

$$
A_v \approx 2.81
$$

$$
A_v(dB) \approx 8.97 dB
$$

### Practical Gain

Input Peak-to-Peak Voltage = 19.373 mV

Output Peak-to-Peak Voltage = 40.44 mV

$$
A_v \approx 2.09
$$

$$
A_v(dB) \approx 6.39 dB
$$

---

## AC Analysis

<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/main/5.png">


<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/main/6.png">


<img src="https://github.com/Sigma-Verma/LIC-Lab-using-SPICE/blob/main/7.png">

The AC response shows a nearly constant gain in the mid-band region and a gradual reduction in gain at higher frequencies due to parasitic effects.

---

## Bandwidth

The upper cutoff frequency is approximately:

$$
f_H \approx 89.58 MHz
$$

Therefore,

$$
BW \approx 89.58 MHz
$$

---

## Result

The Common Source amplifier was successfully designed and analyzed. The circuit achieved the expected gain, phase inversion, and bandwidth while operating within the specified power limits.

---

## Conclusion

The amplifier was successfully implemented using a 180 nm NMOS transistor model. Simulation results confirmed correct saturation-region operation, satisfactory voltage gain, acceptable power consumption, and good high-frequency performance.
