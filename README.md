# 📡 SDR Lab Portfolio: Passband Modulation, DSSS, and Bandpass Undersampling

This repository documents a cohesive set of **Digital Communications** and **Software‑Defined Radio (SDR)** experiments. The work spans **ASK/FSK** carrier signaling, **BPSK/QPSK** phase modulation, **Direct‑Sequence Spread Spectrum (DSSS)**, and **sub‑Nyquist (bandpass) sampling**.  
Each section includes a concise theory recap, an implementation outline, and attached diagrams/waveforms for verification.

---

## 📑 Table of Contents
- [Part A — Digital Passband Modulation (ASK & FSK)](#part-a--digital-passband-modulation-ask--fsk)
- [Part B — Phase Shift Keying (BPSK & QPSK)](#part-b--phase-shift-keying-bpsk--qpsk)
- [Part C — Direct‑Sequence Spread Spectrum (DSSS)](#part-c--directsequence-spread-spectrum-dsss)
- [Part D — SDR & Bandpass Undersampling](#part-d--sdr--bandpass-undersampling)
- [Results & Data Resources](#results--data-resources)

---

## Part A — Digital Passband Modulation (ASK & FSK)

### A.1 Amplitude Shift Keying (ASK)
**Concept.** ASK conveys bits by **gating the carrier amplitude**. With a 100 kHz sine carrier and a 2 kHz bitstream as the control, the switch either passes the carrier (“1”) or blocks it (“0”).  
**Receiver idea.** Because the information rides in the envelope, a **rectifier → LPF → comparator** chain is sufficient to recover the bitstream (rectify to unipolar, low‑pass to extract the envelope, threshold to re‑square).

#### A.1.1 Transmitter (Carrier Gating)
- **Logic 1:** carry the 100 kHz sine to the output  
- **Logic 0:** no output (amplitude off)

#### A.1.2 Receiver (Envelope & Decision)
1) **Rectification** – convert to unipolar envelope  
2) **LPF** – suppress residual carrier ripple  
3) **Comparator** – restore crisp transitions using a DC threshold

#### ASK — Hardware Diagrams
<details>
<summary>Expand: A‑series diagrams</summary>

![A‑1](Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4696.jpg)  
*Fig. A‑1 — ASK transmitter wiring.*

![A‑2](Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4697.jpg)  
*Fig. A‑2 — ASK interconnection overview.*

![A‑3](Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4699.jpg)  
*Fig. A‑3 — Alternate transmitter build.*

![A‑4](Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_469form Captures
<details>
<summary>Expand: A‑series results</summary>

![A‑9png)  
*Fig. A‑9 — Output for logic “0”.*

!A‑10.png)  
*Fig. A‑10 — Output for logic “1”.*

![A‑11](Waveform_Captures/Digital_Passband_Modulation_ASK&FSK/FIGURE4.png)  
*Fig. A‑11 — ASK at increased carrier frequency.*

![A‑12](Waveform_Captures/Digital_Passband_Modulation_ASK&FSK/FIGURE6.png)  
*Fig. A‑12 — Envelope‑detected waveform.*

![A‑13](Waveform_Captures/Digital_Passband_Modulation_ASK&FSK/FIGURE8.png)  
*Fig. A‑13 — Recovered digital output.*
</details>

---

### A.2 Frequency Shift Keying (FSK)
**Concept.** FSK encodes bits by **changing the carrier frequency** while keeping the amplitude constant—making it less sensitive to amplitude noise than ASK.

#### A.2.1 Transmitter (VCO‑Driven)
- **Bit “0” →** frequency **f₁**  
- **Bit “1” →** frequency **f₂**

#### A.2.2 Receiver (Zero‑Crossing + Averaging)
- Zero‑crossing detector produces pulses at each crossing  
- LPF averages pulse density → higher DC for **f₂** than **f₁** → restore bits

#### FSK — Diagrams
<details>
<summary>Expand: F‑series diagrams</summary>

![F‑1](Diagrams/Digital-Pass-and-Modulation-ASKK generation hardware.*

Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg  
*Fig. F‑2 — Functional FSK diagram.*

!F‑3.jpg)  
*Fig. F‑3 — Demodulator wiring.*

![F‑4](Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4713.jpg)  
* — Zero‑crossing stage.*

!F‑5.jpg)  
*Fig. F‑5 — Bit reconstruction stage.*

![F‑6](Diagrams/Digital-Pass-and-Modulation-ASK&FSK/IMG_4716.jpg)  
*Fig. F‑6 — Final recovery connections.*
</details>

#### FSK — Waveform Captures
<details>
<summary>Expand: F‑series results</summary>

![F‑7](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)  
*Fig. F‑7 — CAL check (1 Vp‑p).*

![F‑8](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)  
*Fig. F‑8 — 1 kHz reference, 1 ms period.*

![F‑9](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)  
*Fig. F‑9 — Zoomed reference for manual reading.*
</details>

---

## Part B — Phase Shift Keying (BPSK & QPSK)

### B.1 BPSK (Binary Phase Shift Keying)
**Concept.** Bits modulate **carrier phase**: 0° for “1”, 180° for “0”, at constant amplitude—ideal for low‑SNR links.

#### B.1.1 Transmitter (Balanced Modulator)
- Convert 0/5 V logic to **±V (bipolar)**  
- Multiply bipolar data with carrier → BPSK (0°/180°)

#### B.1.2 Receiver (Coherent Product Detection)
- Multiply receive signal with synchronized LO  
- LPF the product to baseband  
- Comparator to restore clean logic

#### BPSK — Diagrams
<details>
<summary>Expand: B‑series diagrams</summary>

![B‑1](Diagrams/PhaseB‑2](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4721.jpg)  
*Fig. B‑2 — Modulator block view.*

![B‑3](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4723.jpg)  
*Fig. B‑3 — Coherent detector wiring.*

![B‑4](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4724.jpg)  
*Fig. B‑4 — Product detection diagram.*
</details>

#### BPSK — Waveform Captures
<details>
<summary>Expand: B‑series results</summary>

![B‑5](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/PART-A-fig2.png)  
*Fig. B‑5 — BPSK output.*

![B‑6](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/PART-B-fig4.png)  
*Fig. B‑6 — Coherent demodulation trace.*

![B‑7](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/PART-C-fig6.png)  
*Fig. B‑7 — Recovered bitstream.*
</details>

---

### B.2 QPSK (Quadrature PSK)
**Concept.** QPSK maps **two bits per symbol** to four phases (e.g., 45°, 135°, 225°, 315°). I/Q paths halve the per‑branch bit rate while doubling throughput in the same bandwidth.

#### B.2.1 Transmitter (I/Q Branches)
- Split serial bits → **I** and **Q**  
- Modulate **cos ωt** with I, **sin ωt** with Q  
- Sum I+Q to produce QPSK waveform

#### B.2.2 Receiver (Dual Coherent Paths)
- Multiply with cos ωt → I, sin ωt → Q  
- LPF both, then re‑serialize to bits

#### QPSK — Diagrams & Waveforms
<details>
<summary>Expand: Q‑series documentation</summary>

![Q‑TX1](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4733.jpg)  
![Q‑TX2](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4734.jpg)  
![Q‑TX3](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4735.jpg)  
![Q‑TX4](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4736.jpg](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4738.jpg)  
![Q‑TX8](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4739.jpg)  
![Q‑Pick1](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4741.jpg)  
![Q‑Pick2](Diagrams/Phase-Shift-Keying-BPSK&QPSK/IMG_4742t-Keying-BPSK&QPSK/IMG_4743.jpg)

![Q‑W1](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043043.png)  
![Q‑W2](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043056.png)  
![Q‑W3](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043102.png)  
![Q‑W4](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043109.png)  
![Q‑Sel0](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043114.png)  
*Q‑Sel0 — Phase discrimination at 0°.*

![Q‑Sel180](Waveform_Captures/Phase_Shift_Keying_BPSK&QPSK/Screenshot2026-03-11043120.png)  
*Q‑Sel180 — Phase discrimination at 180°.*
</details>

---

## Part C — Direct‑Sequence Spread Spectrum (DSSS)

**Idea.** Multiply the bitstream by a **high‑rate PN sequence** to widen the spectrum; at the receiver, **correlate** with a synchronized PN to collapse back to narrowband.

### C.1 Spreading (TX)
- PN chips ≫ bit rate  
- Message ⊕ PN (XOR or multiplier):  
  - bit 0 → PN unchanged  
  - bit 1 → PN inverted  
- Result: wideband, low‑PSD signal

### C.2 Despreading (RX)
- Align an identical PN generator  
- Multiply/XOR with incoming DSSS → original data appears  
- LPF + thresholding clean the recovered bits

#### DSSS — Diagrams
<details>
<summary>Expand: C‑series diagrams</summary>

![C‑1](Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4749.jpg)  
![C‑2](Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4750.jpg)  
![C‑3](Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4754.jpg)  
![C‑4](Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4755.jpg)  
![C‑5](Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4758.jpg)  
![C‑6](Diagrams/Direct-Sequence-Spread-Spectrum-DSSS/IMG_4761.jpg)
</details>

#### DSSS — Waveform Captures
<details>
<summary>Expand: C‑series results</summary>

![C‑TX](Waveform_Captures/Direct_Sequence_Spread_Spectrum_DSSS/FIG1.png)  
*Fig. C‑TX — DSSS transmit signal.*

![C‑REC1](Waveform_Captures/Direct_Sequence_Spread_Spectrum_DSSS/fig4.png)  
![C‑REC2](Waveform_Captures/Direct_Sequence_Spread_Spectrum_DSSS/fig7.png)  
*Fig. C‑REC — Product‑detector based recovery.*

![C‑INT1](Waveform_Captures/Direct_Sequence_Spread_Spectrum_D, sampling and signal translation can be configured in software. For **band‑limited passbands**, deliberate undersampling (sub‑Nyquist) can map a high‑frequency band into a lower **intermediate frequency (IF)** via aliasing—provided the sampling rate is chosen to avoid overlap.

### D.1 Architecture Sketch
- Conventional: \( f_s > 2 f_{\max} \)  
- Bandpass case: \( f_s > 2B \) (where **B** is signal bandwidth), with careful selection so the aliased band lands cleanly at a non‑overlapping IF.

### D.2 Practical Flow
- Sample‑and‑Hold → ADC  
- Software (FIR/IIR) performs the **channel selection**, **demodulation**, and **bit recovery**  
- A single RF front‑end can decode different schemes (ASK/FSK/BPSK) by **changing only the software chain**

#### SDR & Undersampling — Diagrams
<details>
<summary>Expand: D‑series diagrams</summary>

![D‑1](Diagramsgrams/SDR&Undersampling/IMG_4767.jpg  
Diagrams/SDR&Undersampling/IMG_4771.jpg  
!D‑4.jpg)  
![D‑5](Diagrams/SDR&Undersampling/IMG_4774.jpg)
</details>

#### SDR & Undersampling — Waveform Captures
<details>
<summary>Expand: D‑series results</summary>

![D‑CAL1](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)  
![D‑CAL2](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)  
![D‑CAL3](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)  
*D‑CAL — Internal calibration traces (1 Vp‑p, 1 kHz, period verification).*
</details>

---

## Results & Data Resources

- 📕 **Complete Experimental Data / Q&A (PDF):**  
  `Data/Q&A-Digital-Comms-SDR-Advanced-Modulation-Sub-Nyquist-Signal-Analysis.pdf`

- 🗂️ **Browse all captured waveforms:**  
  `Waveform_Captures/`

---

### Notes
- Filenames and paths above match the current repository layout you provided.  
- If you rename or move images, update the Markdown links accordingly.  
- This write‑up is deliberately **rephrased and reorganized** to be original while keeping your measured content and figure references intact.

