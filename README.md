# 🎛️ SDR Experiments Notebook: Shift‑Keying, DSSS Robustness, and Bandpass Undersampling

A curated set of hands‑on **Digital Communications + SDR** activities built on modular lab blocks and software tooling.  
The notebook is organized as short, self‑contained modules so your reviewer can jump straight to **signal generation, recovery, and verification** without wading through long narratives.

---

## 🔎 Quick Map
- #a-passband-shift-keying-ask--fsk
- #b-phase-encoded-modulation-bpsk--qpsk
- #c-direct-sequence-spread-spectrum-dsss
- #d-sdr-toolchain--bandpass-undersampling
- #results--data-pack
- #reproducibility-mini-checklist

---

## ⚙️ Lab Context (one‑pager)
- **Carrier bench** around 100 kHz, **baseband** around ~2 kHz  
- **Scope view**: time traces, zoomed zero‑crossings, and spectrum snapshots  
- **Recovery blocks**: envelope paths (ASK), ZCD+LPF (FSK), coherent detection (BPSK/QPSK), correlation (DSSS)  
- **SDR side**: capture → filter → demodulate → compare to theory

---

## A. Passband Shift‑Keying (ASK & FSK)

### A1. ASK — Carrier Gating + Envelope Path
**Idea.** Bits ride on amplitude only: gate a 100 kHz sine with a 2 kHz bitstream.  
**RX logic.** Use **rectifier → LPF → comparator** to recover crisp transitions.

**Tx/Rx Diagrams** *(click to expand)*  
<details>
<summary>ASK Hardware + Block Views</summary>

- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4696.jpg — **Tx wiring sketch**  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4697.jpg — **Gate‑based ASK concept**  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4699.jpg — **Alternate build**  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4699(1)(1).jpg — **Alternate diagram**  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4701(1).jpg — **Demod chain layout**  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4702(1).jpg — **Envelope block**  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4703(1).jpg — **Threshold/cleanup**  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4703(1)(1).jpg — **Decision stage**
</details>

**Oscilloscope Evidence**  
<details>
<summary>ASK Waveforms</summary>

- Waveform_Captures/Digital_Passband_Modulation_ASK&FSK/FIGURE3(LOGIC0-LOW).png — output at **logic 0**  
- Waveform_Captures/Digital_Passband_Modulation_ASK&FSK/FIGURE3(LOGIC1-HIGH).png — output at **logic 1**  
- Waveform_Captures/Digital_Passband_Modulation_ASK&FSK/FIGURE4.png — higher‑fc snapshot  
- Waveform_Captures/Digital_Passband_Modulation_ASK&FSK/FIGURE6.png — **envelope** after LPF  
- Waveform_Captures/Digital_Passband_Modulation_ASK&FSK/FIGURE8.png — **reconstructed bits**
</details>

---

### A2. FSK — VCO Switching + ZCD Averaging
**Idea.** Keep amplitude constant; encode bits as **frequencies** f₁/f₂.  
**RX logic.** **Zero‑crossing detector** generates pulses; **LPF** averages pulse density → DC level → decision.

**Tx/Rx Diagrams**  
<details>
<summary>FSK Hardware + Recovery</summary>

- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4710.jpg — FSK Tx arrangement  
- Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg — functional FSK block  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4713(1).jpg — ZCD wiring  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4713.jpg — ZCD detail  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4716(1).jpg — post‑LPF stage  
- Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4716.jpg — final restore
</details>

**Checks & CAL**  
<details>
<summary>Reference Traces</summary>

- Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg — CAL 1 Vp‑p  
- Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg — 1 kHz, 1 ms period  
- Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg — zoom for manual verification
</details>

---

## B. Phase‑Encoded Modulation (BPSK & QPSK)

### B1. BPSK — ±Phase With Constant Envelope
**Tx.** Convert logic to **±V**, multiply with carrier (balanced modulator):  
- “1” → **0°**, “0” → **180°**.  
**Rx.** **Coherent product detection** with a synchronized LO → LPF → comparator.

**Diagrams**  
<details>
<summary>BPSK Builds</summary>

- Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4721(1).jpg — Tx layout  
- Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4721.jpg — modulator schematic  
- Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4723.jpg — coherent Rx wiring  
- Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4724.jpg — product detector
</details>

**Waveforms**  
<details>
<summary>BPSK Scopes</summary>

- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/PART-A-fig2.png — modulated output  
- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/PART-B-fig4.png — coherent multiply  
- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/PART-C-fig6.png — bit recovery
</details>

---

### B2. QPSK — Two Bits per Symbol via I/Q
**Tx.** Split serial bits → I & Q; modulate with **cos ωt** and **sin ωt**, then sum.  
**Rx.** Dual product detectors (I/Q) → LPFs → re‑serialize.

**Docs & Proofs**  
<details>
<summary>QPSK Diagrams & Scopes</summary>

- Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4733.jpg … /IMG_4743.jpg — IQ generation / selection chain

- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043043.png  
- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043056.png  
- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043102.png  
- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043109.png  
- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043114.png — **phase selection (0°)**  
- Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043120.png — **phase selection (180°)**
</details>

---

## C. Direct‑Sequence Spread Spectrum (DSSS)

**TX.** Multiply message by a **high‑rate PN** (chips ≫ bit rate) → wideband, low PSD.  
**RX.** Regenerate + align PN; multiply to **despread**; LPF + threshold for final bits.

**Diagrams**  
<details>
<summary>DSSS Build Sheets</summary>

- Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4749.jpg  
- Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4750.jpg  
- Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4754.jpg  
- Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4755.jpg  
- Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4758.jpg  
- Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4761.jpg
</details>

**Evidence**  
<details>
<summary>DSSS Scopes</summary>

- Waveform_Captures/Direct_Sequence_Spread_Spectrum_DSSS/FIG1.png — DSSS Tx  
- Waveform_Captures/Direct_Sequence_Spread_Spectrum_DSSS/fig4.png — correlation path  
- Waveform_Captures/Direct_Sequence_Spread_Spectrum_DSSS/fig7.png — despread snapshot  
- Waveform_Captures/Direct_Sequence_Spread_Spectrum_DSSS/fig8.png — interference case  
- Waveform_Captures/Direct_Sequence_Spread_Spectrum_DSSS/fig10(1).png — interference case (alt)
</details>

---

## D. SDR Toolchain & Bandpass Undersampling

**Why undersample?** For a **band‑limited** passband, you can select **fₛ** so the band aliases to a clean **IF** in a lower Nyquist zone:
- Classical Nyquist: \( f_s > 2f_{\max} \)  
- Bandpass case: \( f_s > 2B \) (B = bandwidth), with alias planning to avoid overlap.

**Workflow**
1) Sample‑and‑Hold → ADC  
2) Digital channel selection (FIR/IIR)  
3) Software demod (ASK/FSK/BPSK change = software swap)

**Diagrams**  
<details>
<summary>Undersampling Sheets</summary>

- Diagrams/SDR&Undersampling/IMG_4767(1).jpg  
- Diagrams/SDR&Undersampling/IMG_4767.jpg  
- Diagrams/SDR&Undersampling/IMG_4771.jpg  
- Diagrams/SDR&Undersampling/IMG_4771(1).jpg  
- Diagrams/SDR&Undersampling/IMG_4774.jpg
</details>

**Scopes**  
<details>
<summary>Undersampling Captures</summary>

- Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg — CAL 1 Vp‑p  
- Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg — 1 kHz reference  
- Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg — zoomed ref
</details>

---

## 📦 Results & Data Pack
- **Compiled Q&A / data sheet (PDF):**  
  `Data/Q&A-Digital-Comms-SDR-Advanced-Modulation-Sub-Nyquist-Signal-Analysis.pdf`

- **All oscilloscope media:**  
  `Waveform_Captures/`

---

## ✅ Reproducibility Mini‑Checklist
- [ ] Probes compensated, CAL verified  
- [ ] fc (~100 kHz) and fm (~2 kHz) confirmed  
- [ ] ASK: envelope path thresholds tuned  
- [ ] FSK: ZCD pulses averaged to stable DC levels  
- [ ] BPSK/QPSK: coherent LO in lock, LPFs correct  
- [ ] DSSS: PN aligned before correlation  
- [ ] Undersampling: alias plan prevents band overlap
