# SDR‑Centric Digital Comms: Shift‑Keying, DSSS, and Sub‑Nyquist Experiments (ETT‑101)

A modular, experiment‑centric repository for **Software‑Defined Radio (SDR)** and **Digital Communications** using **GNURadio** with the **Emona ETT‑101** platform. This suite documents **three experiments**:

1) **Shift‑Keying Passband Modulation (ASK/FSK/BPSK/QPSK)**  
2) **DSSS (Direct‑Sequence Spread Spectrum)** — processing gain & interference resistance  
3) **Sub‑Nyquist (Bandpass) Sampling** — undersampling, alias planning, and reconstruction

Each experiment follows a **repeatable lab workflow**:

**Overview → Setup → Procedures → Data → Analysis → Results → Discussion → QA**

## Why this structure?
- Clear separation per experiment (short mini‑reports)  
- Strong **traceability** via a shared `Data_Dictionary.md`  
- Clean path to **grading** and **portfolio** use

## Platform & References
- **ETT‑101 SDR Utilities Board (ETT‑101‑23)** supports GNURadio SDR flows, sampling/resampling, and digital modulation demos (zero‑install Linux image). [2](https://www.emona-tims.com/wp-content/uploads/2023/03/ETT-101-23-SDR.pdf)
- The overall topic footprint (passband shift‑keying, DSSS, undersampling) matches common SDR coursework and aligns with your peer’s scope, but this repository uses a **different, modular structure**. [1](https://github.com/EarlArugay/Digital-Comms-SDR-Advanced-Modulation-Sub-Nyquist-Signal-Analysis)

## Repository Map
