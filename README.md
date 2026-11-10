# Football Optical Tracking Analytics ⚽📊

This repository contains the signal processing and data engineering pipeline for analyzing high-frequency optical tracking data in football. It demonstrates the integration of Data Engineering (DE) and Data Science Analytics (DSA) to audit signal quality and evaluate physical performance deterioration.

**Dataset:** Metrica Sports Game 1 (Optical Tracking)  
**Context:** Signal Processing Assignment at Brawijaya University  
**Author:** Keenan Gadi Palwono

### 📌 Project Objective
Football players' physical movements are continuous spatial signals recorded by high-speed stadium cameras. However, raw optical sensor data is often plagued with high-frequency noise (jitter) and occlusion artifacts (glitches). This project aims to design a stable digital filter pipeline to denoise the raw data and subsequently diagnose the anaerobic fatigue points of specific players (Goalkeeper, Midfielder, Defender) in the second half of a match.

### 🧠 Methodology & Architecture
*   **Data Engineering & Ingestion:** 
    *   Parsed multi-header raw tracking data to construct a structured schema of unique $(X, Y)$ coordinates for 145,006 tracking samples.
    *   Transformed relative coordinates to standard FIFA metric dimensions ($105\text{ m} \times 68\text{ m}$).
*   **Signal Processing & Aliasing Audit:** 
    *   Validated the stability of the sampling period at $T_s = 0.04\text{ s}$ ($f_s = 25\text{ Hz}$).
    *   Empirically proved the Nyquist-Shannon Theorem by demonstrating severe peak velocity distortion when downsampling the signal to $5\text{ Hz}$ and $1\text{ Hz}$ (aliasing effect).
*   **LTI Filter Design & Denoising:** 
    *   Designed a 13-point Linear Time-Invariant (LTI) Moving Average filter to smooth spatial trajectories and extract clean velocity signals ($v_{\text{filt}}[n]$), effectively mitigating camera jitter without truncating actual sprint peaks.
    *   Proved the system's Bounded-Input Bounded-Output (BIBO) absolute stability using Z-domain Pole-Zero plots (all poles at origin $|p| = 0 < 1$).
*   **Data Science Analytics:** 
    *   Evaluated FIFA physical metrics (work rate, high-speed running, sprint duration) across match halves.
    *   Utilized Fast Fourier Transform (FFT) spectrum analysis and AR(1) dynamic modeling to assess pacing shifts and movement persistence.

### 🚀 Results & Insights
*   **Nyquist Violation Impact:** Downsampling the $25\text{ Hz}$ signal to $1\text{ Hz}$ artificially truncated a player's peak acceleration from $31.4\text{ km/h}$ down to $17.0\text{ km/h}$, proving that reckless data compression destroys crucial explosive action information.
*   **Successful Denoising:** The M=13 filter successfully reduced extreme occlusion glitches (e.g., $99.0\text{ km/h}$ false spikes) down to realistic physiological bounds without distorting true running kinematics.
*   **Tactical Fatigue Diagnosis:** DSA evaluation revealed that the Midfielder's (Player 5) High-Speed Running (HSR) and sprint durations dropped drastically by $55.4\%$ in the second half, despite maintaining a high total distance covered. This pinpoints anaerobic fatigue in the transition pivot, justifying tactical substitutions around the 60th minute.

### 📁 Repository Structure
*   `notebooks/`: The main Jupyter Notebook executing the 3-stage pipeline (3A: DE & Aliasing, 3B: LTI Denoising, 3C: DSA & Tactics).
*   `docs/`: Contains the standalone technical presentation slides summarizing the signal processing audit and findings.
*   `visualizations/`: Generated plots demonstrating downsampling aliasing, pole-zero stability, denoising comparisons, and FFT spectrums.

### ⚙️ How to Run
1. Clone this repository.
2. Install the required dependencies using `pip install -r requirements.txt`.
3. Download the `Sample_Game_1_RawTrackingData_Home_Team.csv` from the [Metrica Sports open dataset](https://github.com/metrica-sports/sample-data).
4. Run the Jupyter Notebook in the `notebooks/` directory to reproduce the signal processing pipeline.