# ECG Signal Processing Module

This repository module contains the mathematical foundation and the embedded implementation for cleaning, filtering, and analyzing electrocardiogram (ECG) signals for the Portable ECG Device project.

Due to the noisy nature of physiological signals (baseline wander, powerline interference, and muscle artifacts), this module operates on a two-step engineering workflow: generating a "Golden Model" in Python, followed by a high-performance C++ implementation using hardware acceleration.

---

## Directory Structure

* **`python_prototypes/`**: Contains offline Python scripts for algorithm design, filter coefficient generation, and dataset testing (e.g., MIT-BIH Arrhythmia Database).
* **`src/`**: Contains the final C++ source code (`.cpp` and `.h` files) optimized for the ESP32 microcontroller.
* **`docs/`**: Reserved for mathematical derivations, filter response graphs (Bode plots), and performance metrics.
* **`datasets/`**: (Ignored in version control) Local directory for downloading raw ECG data samples used during the prototyping phase.

---

## Development Roadmap

### Phase 1: Python Prototyping (The Golden Model)
* Load raw, noisy ECG data using standard physiological datasets.
* Design Infinite Impulse Response (IIR) Biquad filters using SciPy.
* Extract static floating-point coefficients for embedded translation.

### Phase 2: ESP32 Core Implementation (C++)
* Port the Python filter coefficients to C++.
* Implement real-time signal processing using the **ESP-DSP** library.
* Ensure the pipeline utilizes ESP32 hardware-accelerated instructions to prevent CPU blocking.

### Phase 3: Feature Extraction
* Implement a lightweight version of the Pan-Tompkins algorithm in C++.
* Detect R-peaks in real-time to calculate Heart Rate (BPM) and RR-intervals.
* Format the extracted features to feed the `ECG Arrhythmia AI` module.

### Phase 4: RTOS Integration
* Wrap the DSP pipeline into a dedicated FreeRTOS task.
* Assign the processing task to a specific ESP32 core to ensure continuous sampling from the Analog Front-End (AFE) without dropping data.

---

## Filter Pipeline Specification

The standard processing pipeline for this device includes:
1. **Notch Filter:** Targets specific powerline noise (50 Hz or 60 Hz depending on the target region) to eliminate electromagnetic interference from mains power.
2. **High-Pass Filter:** Targets frequencies below 0.5 Hz to eliminate baseline wander caused by patient respiration and electrode impedance changes.
3. **Low-Pass Filter:** Targets frequencies above 40-100 Hz to reduce electromyographic (EMG) muscle noise.

---

## Dependencies & Tools

**For Prototyping (Python):**
* `numpy` and `scipy` (Filter design and math)
* `matplotlib` (Signal visualization)
* `wfdb` (Waveform Database library for PhysioNet datasets)

**For Embedded (ESP32):**
* `ESP-IDF` (Espressif IoT Development Framework)
* `ESP-DSP` (Digital Signal Processing library for optimized Biquad execution)

---

## Getting Started

1. Navigate to `python_prototypes/` and run the filter design script to generate the required C++ arrays based on your target sampling rate (fs).
2. Copy the generated arrays into `src/ecg_filters.cpp`.
3. Ensure the ESP-DSP library is properly linked in your main ESP32 `CMakeLists.txt` file before compiling.

---

## Acknowledgments & Citations

This project utilizes data from the MIT-BIH Arrhythmia Database and PhysioNet for algorithmic prototyping and validation. When referencing or publishing work based on this module's validation data, please include the following standard citations:

* **MIT-BIH Arrhythmia Database:**
  Moody GB, Mark RG. The impact of the MIT-BIH Arrhythmia Database. *IEEE Eng in Med and Biol* 20(3):45-50 (May-June 2001). (PMID: 11446209)

* **PhysioNet Resource:**
  Goldberger, A., Amaral, L., Glass, L., Hausdorff, J., Ivanov, P. C., Mark, R., ... & Stanley, H. E. (2000). PhysioBank, PhysioToolkit, and PhysioNet: Components of a new research resource for complex physiologic signals. *Circulation [Online]*. 101 (23), pp. e215–e220. RRID:SCR_007345.