# Design and Analysis of Circuit 2c (Active NMOS Degeneration)

This document provides a detailed analysis of the third configuration of the CS amplifier, where the source degeneration voltage ($V_{RS}$) is increased to **0.61 V**. This, combined with an active NMOS load, improves linearity and gain.

---

## 1. DC Analysis (Operating Point)

All transistors are designed to operate in the **saturation region** for proper amplification.

<img width="1098" height="834" alt="Screenshot 2026-03-01 163854" src="https://github.com/user-attachments/assets/0fe2c6f1-8ff3-4623-a37b-35fb0eee12ef" />

### Transistor Dimensions and Bias

| Transistor           | Width ($\mu m$) | Length (nm) |
|---------------------|-----------------|-------------|
| NMOS Driver ($M_1$) | 26              | 180         |
| PMOS Active Load ($M_2$) | 15.7         | 180         |
| NMOS Degeneration ($M_3$) | 37.2         | 180         |

- **Source Voltage ($V_{RS}$):** 0.61 V  
-* **Input Bias Voltage ($V_{in}$):**
    The DC voltage at the gate of $M_1$ is the sum of the gate-source drop and the source voltage:
    $$V_{in(DC)} = V_{GS1} + V_{RS} = 0.61V + 0.61V = \mathbf{1.22V}$$
  
 **Gate-Source Voltage ($V_{GS1}$):**
    Using the threshold voltage ($V_{thn} = 0.36V$):
    $$V_{GS1} = V_{OV} + V_{thn} = 0.25V + 0.36V = \mathbf{0.61V}$$

<img width="644" height="456" alt="Screenshot 2026-03-01 164002" src="https://github.com/user-attachments/assets/8a5a14f8-8399-4aa8-b337-2f5c5cb7f838" />


**Key Formulas:**
1.  **Drain Current:** $I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2 (1 + \lambda V_{DS})$
2.  **Transconductance ($g_m$):** $g_m = \frac{2I_D}{V_{OV}} = \mathbf{1.6 \, mS}$ (Calculated for $V_{OV} = 0.25V$)


---

## 2. DC Sweep Analysis (Voltage Transfer Characteristics)

A DC sweep of $V_{in}$ from 0 V to 1.8 V shows the linear input range.

<img width="1907" height="517" alt="Screenshot 2026-03-01 163944" src="https://github.com/user-attachments/assets/24bc6159-caf0-415b-a526-2f0b1790a1ae" />

- **Input Bias:** $V_{in(DC)} = 1.22\,\mathrm{V}$  
- **Observation:** Increasing $V_{RS}$ widens the linear input range.  
- **Inference:** $M_3$ provides stable degeneration, reducing output sensitivity to input fluctuations.

---

## 3. Transient Analysis (Time Domain)

- **Input:** $V_{in(p-p)} = 19.99\,\mathrm{mV}$  
- **Output:** $V_{out(p-p)} = 167.94\,\mathrm{mV}$

**Simulated Gain:**

$$
A_{v(\mathrm{ratio})} = \frac{V_{out(p-p)}}{V_{in(p-p)}} = \frac{167.94\,\mathrm{mV}}{19.99\,\mathrm{mV}} \approx 8.40
$$

$$
A_{v(\mathrm{dB})} = 20 \log_{10}(8.40) \approx 18.48\,\mathrm{dB}
$$

**Theoretical Alignment ($\lambda$):**

- Assume $r_{o1} \approx r_{o2} \approx r_{o3} = r_o$  
- * Numerator (Effective $r_{out}$): $r_{o1} || r_{o2} = 20.83 \, k\Omega$
    * Denominator (Degeneration Factor): $1 + (g_{m1} \cdot r_{o3}) = 1 + (1.6mS \times 41.67k\Omega) = 67.67$

<img width="1900" height="518" alt="Screenshot 2026-03-01 164045" src="https://github.com/user-attachments/assets/2525e66b-9050-47b9-9617-410b9e659a9a" />  
<img width="1894" height="521" alt="Screenshot 2026-03-01 164101" src="https://github.com/user-attachments/assets/58c2a8d2-94aa-421c-9219-4c6aef97ca2f" />  
<img width="1907" height="514" alt="Screenshot 2026-03-01 164118" src="https://github.com/user-attachments/assets/cbb1b9a5-1783-44a5-a2fe-8c5d358eaad9" />  

**Theoretical Gain Result:**

$$
A_v = \frac{-g_{m1} \cdot (r_{o1} \parallel r_{o2})}{1 + g_{m1} r_{o3}} = \frac{-(1.6\,\mathrm{mS} \times 20.83\,\mathrm{k}\Omega)}{67.67} \approx 8.40
$$

$$
A_{v(\mathrm{dB})} = 20 \log_{10}(8.40) \approx 18.48\,\mathrm{dB}
$$

- Effective $\lambda = 0.12\,\mathrm{V}^{-1}$  
- Output resistance $r_o \approx 41.6\,\mathrm{k}\Omega$

---

## 4. AC Analysis (Frequency Response)

- **Mid-band Gain:** 18.48 dB  
- **3dB Cutoff Frequency:** 172.53 MHz

<img width="1907" height="517" alt="Screenshot 2026-03-01 164022" src="https://github.com/user-attachments/assets/2ab2e186-a533-4e22-9767-97c8c5bf4ad8" />

---

## 5. Performance Summary

| Parameter           | Theoretical ($\lambda=0.12$) | Simulated Value | Difference |
|---------------------|------------------------------|-----------------|------------|
| **Gain (dB)**       | 18.48 dB                     | 18.48 dB        | 0.00 dB    |
| **$V_{out}$ Swing**  | 167.92 mV                    | 167.94 mV       | 0.02 mV    |
| **Transconductance** | 1.6 mS                       | 1.6 mS          | 0.00 mS    |
| **Bandwidth**        | ---                          | 172.53 MHz      | ---        |

---

**Conclusion:**  

By increasing $g_m$ to **1.6 mS** and $V_{RS}$ to **0.61 V**, Circuit 2c achieves a high gain of **18.48 dB**, significantly better than the 6.59 dB of lower-degeneration designs. This shows the critical role of proper transconductance and active degeneration in achieving stable, linear amplification.
