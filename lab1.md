# LIC-LAB1  
### LIC Laboratory Files  

# Lab Report: CMOS Common Source Amplifier Design and Evaluation  

---

## 1. Introduction  

To simulate a Common Source (CS) amplifier using 180nm CMOS technology in LTspice and analyze DC bias, AC gain, bandwidth, and transient response.

---

## 2. Theory  

In a CS amplifier:  
- The **source** is connected to ground.  
- The **input** is applied at the **gate**.  
- The **output** is taken from the **drain**.  

For proper operation, the transistor must be in **saturation**, which requires:

VDS ≥ (VGS − Vth)  

Where:  
- VDS = drain-source voltage  
- VGS = gate-source voltage  
- Vth = MOSFET threshold voltage  

In saturation, the **drain current** depends on VGS. The **small-signal transconductance** is:

gm = 2 × ID / (VGS − Vth)

The voltage gain is approximately:

Av ≈ −gm × RD  

The negative sign indicates a **180° phase inversion** between input and output.

### Reference Circuit and Operating Curves  

<img width="1860" height="1085" src="https://github.com/user-attachments/assets/e73155db-5e74-46e0-9612-020b60ea5281" />
<img width="1870" height="1070" src="https://github.com/user-attachments/assets/8769c921-08c9-4dfe-8f82-8f0776ee9efb" />
<img width="1875" height="1078" src="https://github.com/user-attachments/assets/a5807e11-7534-4d07-a80d-9c249922a6a7" />
<img width="1868" height="1060" src="https://github.com/user-attachments/assets/f848e96c-8667-4125-afc8-006800125528" />

---

## 3. Design Parameters (180 nm Technology)

| Parameter | Value |
|-----------|--------|
| Supply Voltage (VDD) | 1.8 V |
| MOSFET Model | CMOSN (180 nm) |
| Gate Bias (VGS) | 0.9 V |
| Drain Resistance (RD) | 4.5 kΩ |
| Load Capacitance (CL) | 10 pF |
| Channel Length (L) | 180 nm |
| Channel Width (W) | 1.54 µm |

---

## 4. Theoretical Design (Target ID = 200 µA)

The CS amplifier was designed to operate below a **1 mW power limit**.  

**Target Drain Current:**

ID = 200 µA  

**Power Calculation:**

P = VDD × ID = 1.8 V × 200 µA = 0.36 mW  

**Width Selection:**  
To achieve this current with L = 180 nm and VGS = 0.9 V, the transistor width was optimized to:

W = 1.54 µm  

This ensures the device operates in the desired **saturation region** while keeping power under 1 mW.

---

## 5. DC Operating Point  

From LTspice:  
- Drain Current (ID) = 200.09 µA  
- Drain Voltage (Vout) = 0.8957 V  

**Saturation Check:**  

VDS = 0.8957 V ≥ VGS − Vth = 0.9 − 0.36 = 0.54 V ✅  

> The transistor is correctly biased in the **saturation region**.

<img width="1875" height="1095" src="https://github.com/user-attachments/assets/a5694314-26c9-421a-afe6-2edbea626613" />

---

## 6. Small-Signal Gain Calculation

**Transconductance:**

gm = 2 × ID / (VGS − Vth)  
gm = 2 × 200 µA / (0.9 − 0.36) ≈ 0.74 mA/V  

**Theoretical Voltage Gain:**

Av = −gm × RD  
Av = −0.74 mA/V × 4.5 kΩ ≈ −3.33  

**Gain in dB:**

Av(dB) = 20 log10(3.33) ≈ 10.45 dB  

### Reference Calculation Image

<img width="650" height="520" src="https://github.com/user-attachments/assets/c14e45f7-7d21-4a0e-a58f-fe85d7ce3898" />

---

## 7. Transient Simulation Results

From input/output waveforms:  
- Vin (p-p) = 19.97 mV  
- Vout (p-p) = 54.12 mV  

**Measured Gain:**

Av = Vout / Vin = 54.12 / 19.97 ≈ 2.71  
Av(dB) = 20 log10(2.71) ≈ 8.66 dB  

The slight reduction from the theoretical gain is due to **non-idealities in the transistor and parasitic capacitances**.

---

## 8. AC Analysis and Frequency Response

From frequency response:  
- Mid-band Gain ≈ 10.45 dB  
- High Cutoff Frequency (fH) = 4.04 MHz  

Theoretical bandwidth estimation using the output pole:

fH ≈ 1 / (2π × RD × CL)  
fH ≈ 1 / (2π × 4.5 kΩ × 10 pF) ≈ 3.53 MHz  

> The measured high cutoff frequency is slightly higher (4.04 MHz) due to small-signal effects.

<img width="1878" height="1078" src="https://github.com/user-attachments/assets/249d7753-ee1e-4192-ae6a-ae23351bbb88" />

---

## 9. Results Summary

| Parameter | Value |
|-----------|--------|
| Drain Current (ID) | 200.09 µA |
| Power | 0.36 mW |
| Theoretical Gain | 3.33 |
| Simulated Gain | 2.71 |
| Mid-band Gain | 10.45 dB |
| Bandwidth | 4.04 MHz |
| Phase Shift | 180° |

---

## 10. Conclusion

The 180 nm CS amplifier successfully meets the 1 mW power constraint and operates in saturation.  
- The measured gain is close to the theoretical prediction.  
- Bandwidth is mainly determined by the load capacitance.  
- The circuit provides predictable low-power amplification with a 180° phase shift as expected.
