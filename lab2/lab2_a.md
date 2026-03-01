# Common Source Amplifier with Active Load and Source Degeneration (180nm CMOS)

This exp presents the complete design, theoretical analysis, and LTspice simulation of a **Common Source (CS) amplifier** built using 180nm CMOS technology.

The amplifier consists of:
- An **NMOS transistor** as the main amplifying device  
- A **PMOS transistor** acting as an active load  
- A **source degeneration resistor** for stability and linearity  
- A **10 pF load capacitor**

The goal of this design is to achieve stable gain while keeping total power dissipation below 1 mW.

---

# 1. Design Specifications

### Given Parameters

- Technology node: **180nm**
- Supply voltage (VDD): **1.8 V**
- Maximum allowed power: **≤ 1 mW**
- Load capacitance (CL): **10 pF**
- NMOS channel length: **180 nm**

From the power constraint:

P = VDD × ID  

ID = 1mW / 1.8V ≈ 555 µA  

To stay safely below the limit, the drain current was chosen as:

ID = **200 µA**

---

# 2. DC Biasing Design

Proper biasing ensures both transistors operate in the **saturation region**, which is required for amplification.

---

## A. NMOS Driver (M1)

### 1. Gate-Source Voltage

To operate in saturation:

VGS1 = VOV + Vthn  

Given:
- Overdrive voltage VOV = 0.25 V  
- Threshold voltage Vthn = 0.36 V  

VGS1 = 0.25 + 0.36 = **0.61 V**

---

### 2. Source Degeneration Resistor

A voltage drop of 0.2 V is selected across the source resistor to improve stability.

Rs = VRS / ID  

Rs = 0.2 V / 200 µA  

Rs = **1000 Ω**

This resistor:
- Improves linearity  
- Reduces gain sensitivity to process variation  
- Stabilizes bias current  

---

### 3. Gate Bias Voltage

VG1 = VGS1 + VRS  

VG1 = 0.61 + 0.2 = **0.81 V**

---

### 4. Output Bias Voltage

For symmetrical output swing:

Vout = (VDD + VRS) / 2  

Vout = (1.8 + 0.2) / 2  

Vout ≈ **1.1 V**

---

### 5. Saturation Check

For saturation:

VDS ≥ VOV  

VDS ≥ 0.25 V  

From simulation results, this condition is satisfied.

---

## B. PMOS Active Load (M2)

The PMOS transistor acts as a current source load to increase gain.

### 1. Required Source-Gate Voltage

VSG2 = VOV + |Vthp|  

Given:
- VOV = 0.25 V  
- |Vthp| = 0.39 V  

VSG2 = 0.25 + 0.39 = **0.64 V**

---

### 2. PMOS Gate Voltage

VG2 = VDD − VSG2  

VG2 = 1.8 − 0.64  

VG2 = **1.16 V**

---

# Circuit Diagrams

<img width="1200" height="650" src="https://github.com/user-attachments/assets/ed48dbc1-86b6-4d16-9128-658a4d413801" />

<br>

<img width="710" height="520" src="https://github.com/user-attachments/assets/b0d5538c-72f6-4e12-ab6f-609f152798f4" />

---

# 3. Small-Signal Gain Analysis

After setting the DC bias point, we calculate the voltage gain using small-signal modeling.

---

## Transconductance

gm = 2ID / VOV  

gm = (2 × 200 µA) / 0.25  

gm = **1.6 mS**

Lower overdrive voltage increases transconductance for the same bias current.

---

## Voltage Gain Calculation

Using:

rout = 25 kΩ  
Rs = 1000 Ω  
gm = 1.6 mS  

Step 1 – Intrinsic Gain  

gm × rout = 1.6 mS × 25 kΩ = 40  

Step 2 – Degeneration Factor  

1 + gmRs = 1 + 1.6 = 2.6  

Step 3 – Final Gain  

Av = 40 / 2.6  

Av ≈ **15.38**

Gain in dB:

20 log10(15.38) = **23.74 dB**

---

# 4. LTspice Simulation Results

## DC Operating Point

- Vin = 0.81 V  
- Vout = 1.118 V  
- ID = 199.8 µA  

The simulated values closely match theoretical calculations.

---

## AC Analysis

Measured magnitudes:

Vout = 246.399  
Vin = 19.99  

Av(sim) = 246.399 / 19.99  

Av(sim) ≈ 12.326  

Gain (dB):

20 log10(12.326) = **21.82 dB**

---

## Bandwidth

3 dB cutoff frequency = **275.829 MHz**

---

# Simulation Plots

<img width="1870" height="510" src="https://github.com/user-attachments/assets/d66b95b9-f130-4611-8b42-4b7b5fd84c36" />

<img width="1880" height="510" src="https://github.com/user-attachments/assets/a972f305-f742-4b90-991f-fc110242a2e0" />

<img width="1860" height="510" src="https://github.com/user-attachments/assets/59f1317d-cef5-4cc2-a212-b140446fb7c9" />

---

# 5. Theoretical vs Simulated Comparison

| Parameter | Theoretical | Simulated | Difference |
|------------|------------|------------|------------|
| Gain (dB) | 23.74 dB | 21.82 dB | 1.92 dB |
| Gain (ratio) | 15.38 | 12.33 | 3.05 |
| Drain Current | 200 µA | 199.8 µA | 0.2 µA |

---

# 6. Why Is the Simulated Gain Lower?

The 1.92 dB difference is mainly due to real transistor effects included in the 180nm model.

### 1. Body Effect

Because the source is at 0.2 V, a non-zero VSB exists.  
This increases threshold voltage and introduces additional body transconductance (gmb), slightly reducing gain.

Approximate gain becomes:

Av ≈ -gm rout / [1 + (gm + gmb)Rs]

---

### 2. Channel Length Modulation

Hand calculations assume:

ro = 1 / (λ ID)

However, real models include:
- Mobility degradation  
- DIBL  
- Velocity saturation  

This slightly lowers output resistance and therefore reduces gain.

---

### 3. Effect of Source Degeneration

Without Rs:

Av = -gm × rout = 40  
Gain ≈ 32 dB  

With Rs:

Gain reduces to **21.82 dB**, but:
- Linearity improves  
- Bias becomes stable  
- Temperature sensitivity decreases  

This is a deliberate design trade-off.

---

# 7. Conclusion

The designed amplifier:

- Satisfies the power constraint (< 1 mW)
- Maintains saturation operation
- Achieves more than 20 dB gain
- Provides approximately 275 MHz bandwidth
- Shows strong agreement between theoretical and simulated results

The small gain difference confirms realistic modeling effects rather than analytical error.
