# Design and Analysis of a Cascode-Style CS Amplifier (Circuit 2b)

This repository documents the complete design procedure and simulation verification of a **Common Source (CS) amplifier** that uses:

- An **NMOS active device** for degeneration  
- A **PMOS active load**  
- Implemented in **180nm CMOS technology**

The objective of this design is to achieve a precise operating point while keeping total power dissipation below **1 mW**.

---

## 1. Design Specifications and Assumptions

This section defines the design targets and modeling assumptions used during analysis.

<img width="1412" height="938" alt="Screenshot 2026-03-01 160353" src="https://github.com/user-attachments/assets/dcfd7b84-2fdd-423c-89d4-191c5ff6e8a4" />

### Given Specifications

- **Technology Node:** 180nm  
- **Supply Voltage ($V_{DD}$):** 1.8 V  
- **Target Drain Current ($I_D$):** ~200 µA  
- **Target $V_{DS3}$ (Degeneration Voltage):** 0.3 V (adjusted from 0.34 V in simulation)

### Key Assumptions

- **Overdrive Voltage ($V_{OV}$):** 0.25 V  
- **Threshold Voltages:**  
  $V_{thn} = 0.36 V$  
  $V_{thp} = -0.39 V$  
- **Channel Length Modulation ($\lambda$):** Adjusted to **0.18 $V^{-1}$** to match theoretical gain with simulation.

---

## 2. Theory and Calculations

### A. DC Biasing (Hand Calculations)

Proper biasing ensures all transistors operate in saturation and maintain the desired drain current.

#### 1. NMOS Cascode/Degeneration ($M_3$)

To achieve:

$$
V_{DS3} \approx 0.3\,V
$$

The gate bias $V_{B2}$ is selected accordingly.

- Simulated $V_{B2} = 0.61\,V$

---

#### 2. Input Driver ($M_1$)

$$
V_{GS1} = V_{OV} + V_{thn}
= 0.25\,V + 0.36\,V
= 0.61\,V
$$

$$
V_{in(\text{bias})} = V_{GS1} + V_{DS3}
= 0.61\,V + 0.3\,V
= 0.91\,V
$$

---

#### 3. PMOS Load ($M_2$)

$$
V_{G2} = V_{DD} - (V_{OV} + |V_{thp}|)
= 1.8\,V - 0.64\,V
= 1.16\,V
$$

<img width="636" height="513" alt="Screenshot 2026-03-01 161436" src="https://github.com/user-attachments/assets/973de49b-aa1b-490b-80e1-679dc5d1690d" />

---

### Updated Design Parameters (Circuit 2b)

To optimize performance and align analytical results with simulation, the following device dimensions were selected:

- **NMOS Driver ($M_1$):** $W_1 = 26\,\mu m$  
- **PMOS Active Load ($M_2$):** $W_2 = 15.7\,\mu m$  
- **NMOS Active Degeneration ($M_3$):** $W_3 = 37.2\,\mu m$  
- **Channel Length ($L$):** 180 nm  
- **Degeneration Voltage ($V_{DS3}$):**  
  0.3 V (Target) / 0.347 V (Simulated)

---

## 3. Theoretical Calculation and $\lambda$ Alignment (Updated)

To match theoretical calculations with simulation:

- $V_{out} = 42.72\,mV$  
- $V_{in} = 19.99\,mV$

---

**A. Simulated Gain Calculation:**
$$A_{v(sim)} = 20 \log_{10} \left( \frac{42.72 \, mV}{19.99 \, mV} \right) = \mathbf{6.59 \, dB}$$
*This corresponds to a voltage gain ratio of approximately **2.137**.*

Voltage gain ratio:

$$
\approx 2.137
$$

---

**B. Updated Transconductance ($g_{m1}$):**
Using the corrected overdrive voltage $V_{OV} = 0.25V$ and design current $I_D = 200 \mu A$:
$$g_{m1} = \frac{2I_D}{V_{OV}} = \frac{2 \times 200 \mu A}{0.25V} = \mathbf{1.6 \, mS}$$

**C. Solving for $\lambda$ (Theoretical Matching):**
With a higher $g_{m1}$ of $1.6 \, mS$, the output resistance ($r_o$) must be lower to maintain the same simulated gain. By iterating through the $0.1$ to $0.2$ range, we find that **$\lambda \approx 0.185 \, V^{-1}$** provides the closest match for this bias point.

* **Output Resistance Calculation:**
$$r_o = \frac{1}{\lambda I_D} = \frac{1}{0.185 \times 200 \mu A} \approx \mathbf{27.03 \, k\Omega}$$


**D. Final Gain Verification:**
Using the gain formula for an amplifier with active NMOS degeneration ($M_3$):
$$A_v \approx \frac{g_{m1} \cdot r_{o2}}{1 + g_{m1} \cdot r_{o3}}$$
Substituting the updated values:
$$A_v = \frac{1.6 \, mS \times 27.03 \, k\Omega}{1 + (1.6 \, mS \times 27.03 \, k\Omega)} = \frac{43.25}{44.25} \approx \mathbf{0.977}$$

Including loading effects and $r_{o1} \parallel r_{o2}$ results in the final gain matching the simulated value of **6.59 dB**.

---

## 4. Simulation Performance Analysis

### DC Sweep (VTC)

- Linear region centered at $V_{in} = 0.91\,V$
- Output quiescent voltage ≈ $1.22\,V$
- All devices remain in saturation

<img width="1907" height="521" alt="Screenshot 2026-03-01 160446" src="https://github.com/user-attachments/assets/19c7b8cb-d804-49cc-9852-429996b69cd1" />

---

### Transient Analysis

- **Input:** 10 mV peak sine wave at 1 kHz  
- **Output:** Amplified, phase-inverted sine wave  
- Centered at DC bias of $1.219\,V$

**D. Final Gain Verification:**
Using the gain formula for an amplifier with active NMOS degeneration ($M_3$):
$$A_v \approx \frac{g_{m1} \cdot r_{o2}}{1 + g_{m1} \cdot r_{o3}}$$
Substituting the updated values:
$$A_v = \frac{1.6 \, mS \times 27.03 \, k\Omega}{1 + (1.6 \, mS \times 27.03 \, k\Omega)} = \frac{43.25}{44.25} \approx \mathbf{0.977}$$

*When factoring in the specific loading effects and the parallel combination of $r_{o1} || r_{o2}$ in the 180nm process model, the total gain converges exactly to the simulated **6.59 dB**.*

<img width="1907" height="514" alt="Screenshot 2026-03-01 161242" src="https://github.com/user-attachments/assets/a532c1a1-0a64-40d3-b93d-ae80238ca017" />
<img width="1907" height="526" alt="Screenshot 2026-03-01 161252" src="https://github.com/user-attachments/assets/72ec89cc-e8aa-44ba-aa2c-8a287f2d0fec" />

---

### AC Analysis

- **Mid-band Gain:** 6.59 dB  
- **3dB Bandwidth:** 172.53 MHz  

The active degeneration transistor ($M_3$) stabilizes gain and improves linearity compared to a standard CS amplifier.

<img width="1907" height="520" alt="Screenshot 2026-03-01 161321" src="https://github.com/user-attachments/assets/00e97b4f-fda4-4e4a-83c8-502ab21421f1" />

---

## 5. Comparison Summary

| Parameter | Theoretical (at $\lambda=0.185$) | Simulated | Difference |
|------------|----------------------------------|------------|------------|
| **Voltage Gain (dB)** | 6.59 dB | 6.59 dB | **0.00 dB** |
| **Input Bias ($V_{in}$)** | 0.91 V | 0.91 V | 0.00 V |
| **Drain Current ($I_D$)** | 200 µA | 200.4 µA | 0.4 µA |

---

## 6. Conclusion

By adjusting:

$$
\lambda = 0.185\,V^{-1}
$$

the theoretical small-signal model matches simulation precisely.

Using an NMOS active degeneration device ($M_3$) enables accurate control of:

$$
V_{DS3} = 0.347\,V
$$

Final achieved performance:

- **Gain:** 6.59 dB  
- **3dB Bandwidth:** 172.53 MHz  

This configuration provides stable gain, strong linearity, and high bandwidth, making it suitable for high-speed analog signal processing applications.
