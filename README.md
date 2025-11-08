# 🌬️ Wind Data Analysis and Transfer Function Modeling using Davenport Spectrum

### 📍 Project Overview
This project focuses on **statistical analysis and transfer function modeling** of measured wind velocity data using the **Davenport Spectrum**.  
The work was carried out at the **Giant Metrewave Radio Telescope (GMRT)**, **National Centre for Radio Astrophysics – TIFR**, Khodad, Pune, India.

### 👩‍🔬 Author
**Priyanka Uddhav Ghodke**  
*B.E. Electronics and Telecommunication Engineering*  
Government College of Engineering and Research, Avasari Khurd, Pune  

Under the guidance of:  
- **Mr. S.K. Bagde**, Servo Systems Group Head, GMRT, NCRA-TIFR  
- **Mr. Thiyagrajan B.**, Engineer, Servo Systems, GMRT, NCRA-TIFR  

---

## 📚 Abstract
Wind turbulence significantly affects the performance and stability of large antenna structures such as GMRT dishes.  
This project involves:
- Preprocessing of measured wind data (10 Hz sampling)
- Statistical analysis (mean ≈ 3.15 m/s, σ ≈ 1.09 m/s)
- Estimation of Power Spectral Density (PSD) using FFT and Welch methods
- Comparison with the **Davenport Spectrum**
- Development of a **transfer function model** representing wind turbulence

The validated transfer function was discretized using the **Tustin method**, demonstrating excellent agreement with the theoretical Davenport model — enabling accurate simulation of wind disturbances for antenna control applications.

---

## 🛰️ 1. Introduction
- **GMRT** is an array of thirty 45-m parabolic antennas located 80 km north of Pune.
- Wind turbulence induces **pointing errors**, **torques**, and **structural vibrations**.
- Understanding and modeling these effects are essential for **antenna servo stability**.

---

## 🧩 2. Wind Data Processing and Analysis
### Dataset
- File: `31Oct2024.dat`  
- Duration: ~24 hours (864,256 samples @ 10 Hz)
- Parameters: Wind speed (m/s) and direction (°)

### Preprocessing
- Median filter despiking  
- ±3σ outlier removal  
- Covariance (raw vs cleaned): 0.77 → real turbulence preserved  

### Statistical Parameters
| Metric | Value | Description |
|--------|--------|-------------|
| Mean Wind Speed | 3.15 m/s | Average airflow velocity |
| Std. Deviation | 1.09 m/s | Turbulence intensity |
| Turbulence Intensity | 34.4 % | Ratio of σ/μ |

### Power Spectral Density
- **FFT-based PSD** – high-resolution, noise-sensitive  
- **Welch Method** – smoother, averaged spectrum  
Both PSDs show strong agreement with the Davenport model at low frequencies.

---

## 🌪️ 3. Davenport Spectrum and Transfer Function Modeling
The **Davenport Spectrum** models wind turbulence as:

\[
S_v(f) = \frac{4800 U b k}{(1 + (b^2 \omega^2))^{4/3}}
\]

where  
\( b = \frac{600}{\pi U} \), \( k = (\frac{1}{2.5 \log(z/z_0)})^2 \)  

### MATLAB Implementation
```matlab
U = 3.15; z = 23.16; z0 = 0.03;
b = 600 / (pi * U);
k = (1 / (2.5 * log(z/z0)))^2;
f = linspace(0.001,3,2000);
w = 2*pi*f;
Sv = (4800 * U * b * k) ./ ((1 + (b^2) * w.^2).^(4/3));
```

### Transfer Function Derivation
A **4th-order transfer function** was identified using `invfreqs` to fit the Davenport PSD.  
The continuous-time model was then discretized using the **Tustin transform** (`c2d(H_fit, 0.1, 'tustin')`).

---

## ⚙️ 4. Validation
- **Bode plots** and **PSD comparisons** confirm strong agreement between:
  - Experimental data  
  - Theoretical Davenport model  
  - Derived discrete transfer function  

✅ The model accurately reproduces low-frequency wind turbulence (< 1 Hz) relevant to GMRT antenna dynamics.

---

## 🌀 5. GMRT Wind Force and Torque Simulation
Using the validated wind model:

| Parameter | Symbol | Value |
|-----------|---------|--------|
| Dish Diameter | D | 45 m |
| Frontal Area | A | 1590.43 m² |
| Mean Force | Fₘ | 5.44 kN |
| Steady Torque | Tₘ | 49.00 kN·m |
| RMS Gust Torque | T<sub>w,RMS</sub> | 14.44 kN·m |

Dynamic loads were simulated using the Davenport filter excited by white noise.  
The **RMS gust statistics** and **PSD of simulated signals** validated the turbulence model’s accuracy.

---

## 🧠 6. Conclusion and Future Work
### ✅ Conclusion
- A 4th-order discrete transfer function successfully modeled wind turbulence.  
- Simulation revealed realistic wind forces and torques on GMRT antennas.  
- The model forms a strong foundation for servo system design and stability analysis.

### 🚀 Future Scope
- **Advanced control strategies** (LQG, H∞) for disturbance rejection  
- **Refined turbulence models** at higher frequencies  
- **Integration of nonlinear effects** (stiction, backlash)  
- **Multi-axis modeling** for azimuth-elevation coupling  

---

## 🧾 References
1. Davenport, A. G. (1961). *The Spectrum of Horizontal Gustiness near the Ground in High Winds.*  
2. GMRT Technical Documentation – NCRA-TIFR, Khodad.  
3. MATLAB Documentation, *Signal Processing Toolbox.*

---

### 🏗️ Keywords
`Wind Data Analysis` • `Davenport Spectrum` • `Transfer Function` • `MATLAB` • `GMRT` • `Antenna Control` • `Tustin Method`
