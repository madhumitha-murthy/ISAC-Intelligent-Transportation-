# 🛰️ ISAC Digital Twin Framework for Intelligent Transportation Systems

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![NVIDIA Sionna](https://img.shields.io/badge/NVIDIA-Sionna-76B900?style=flat&logo=nvidia&logoColor=white)
![Status](https://img.shields.io/badge/Status-Research%20Complete-brightgreen?style=flat)
![Institution](https://img.shields.io/badge/A*STAR-I²R-red?style=flat)
![NTU](https://img.shields.io/badge/NTU-Singapore-003D7C?style=flat)

**Research Attachment** — Institute for Infocomm Research (I²R), A*STAR · Oct 2024 – Dec 2025

> ⚠️ Source code is proprietary to I²R, A*STAR and is not included in this repository. This repo documents the research scope, methodology, and results.

---

## 🧭 What This Project Is About

6G networks need to do two things at once — **communicate data** and **sense the environment** — using the same radio signal. This project builds a complete simulation framework to test exactly that, applied to real urban traffic in Singapore.

I designed and implemented an end-to-end **digital twin** of a city intersection, where a base station simultaneously tracks moving vehicles and maintains a reliable wireless link with them — all from a single transmitted waveform.

<img src="https://github.com/user-attachments/assets/2c121c32-e619-473f-b193-0d8f693dffde" width="500"/>

---

## 🎯 The Problem I Solved

| Challenge | Approach |
|---|---|
| Static buildings and infrastructure drown out moving vehicle signals in radar | Designed a **Static Clutter Cancellation (SCC)** algorithm to suppress static reflections and isolate moving targets |
| High-speed vehicles move faster than beamforming can react | Built a **Doppler-based trajectory prediction** algorithm to forecast vehicle position and steer the beam *before* the vehicle moves |
| No realistic testbed existed for joint sensing + communication at 59 GHz | Constructed a **3D digital twin** of Fusionopolis, Singapore using Blender + OpenStreetMap + NVIDIA Sionna ray tracing |

---

## 🏗️ System Overview

```
┌──────────────────────────────────────────────┐
│            Digital Twin Environment           │
│  Blender 3D Model · OpenStreetMap · Sionna RT │
└───────────────────┬──────────────────────────┘
                    │  Realistic multipath, Doppler,
                    │  occlusions, dynamic scatterers
                    ▼
┌──────────────────────────────────────────────┐
│         OFDM Radar Signal Processing          │
│   Channel estimation → Static Clutter        │
│   Cancellation → Range-Doppler Map (RDM)     │
└───────────────────┬──────────────────────────┘
                    │  Delay (range) + Doppler (velocity)
                    ▼
┌──────────────────────────────────────────────┐
│         Trajectory Prediction Algorithm       │
│   Past positions + Doppler → heading + speed  │
│   → predicted next position p̂(k+1)           │
└───────────────────┬──────────────────────────┘
                    │  Predicted azimuth / elevation
                    ▼
┌──────────────────────────────────────────────┐
│         Sensing-Aided Beamforming             │
│   Steering vector → proactive beam alignment  │
│   → reduced misalignment, improved BER        │
└──────────────────────────────────────────────┘
```

---

## ⚙️ Technical Stack

| Layer | Tools & Technologies |
|---|---|
| 3D Scene Construction | Blender, OpenStreetMap, Mitsuba Renderer |
| Wireless Channel Simulation | NVIDIA Sionna Ray Tracing (59 GHz mmWave) |
| Signal Processing | OFDM Radar, 2D FFT, Static Clutter Cancellation |
| Tracking & Prediction | Doppler-based motion model, Kalman Filter |
| Beamforming | Steering vector construction, DOA estimation |
| Scripting & Pipelines | Python |

---

## 📡 Simulation Parameters

| Parameter | Value |
|---|---|
| Carrier Frequency | 59 GHz |
| Bandwidth | 100 MHz |
| OFDM Symbols (M) | 140 |
| Subcarriers (N) | 4096 |
| Subcarrier Spacing | 30 kHz |
| Modulation | QPSK |
| Antennas (Tx / Rx) | 2 / 2 |
| Urban Scene | Fusionopolis, Singapore |
| Configurations | Monostatic & Bistatic |

---

## 📊 Results

### Deployment Configurations

| Monostatic | Bistatic |
|---|---|
| <img src="https://github.com/user-attachments/assets/383d21a3-42f5-499e-ab6e-2aecd735fa99" width="350"/> | <img src="https://github.com/user-attachments/assets/ecb5dc47-fd30-40cc-b8b8-c2d917e1ffe8" width="350"/> |

---

### Static Clutter Cancellation

Before SCC, static reflections from buildings dominate the Range-Doppler Map, masking vehicles entirely. After SCC, moving vehicles are clearly distinguishable with distinct Doppler signatures.

**Monostatic**

| Before SCC | After SCC |
|---|---|
| <img src="https://github.com/user-attachments/assets/b6de39b9-55d5-4e58-89ea-02536b8f7284" width="350"/> | <img src="https://github.com/user-attachments/assets/b63a407c-6f61-4f67-af5c-50a16e5a1ec3" width="350"/> |

**Bistatic**

| Before SCC | After SCC |
|---|---|
| <img src="https://github.com/user-attachments/assets/71b1f2f1-0180-4f25-a91e-ebcc1b24f673" width="350"/> | <img src="https://github.com/user-attachments/assets/44459f6d-46bb-4016-8f3e-fdca8bdbed45" width="350"/> |

---

### Urban Ray-Tracing Simulation

Real-time ray-tracing snapshot of the vehicle moving through the Fusionopolis road junction, capturing multipath reflections and dynamic occlusions.

<img src="https://github.com/user-attachments/assets/9b7f20df-d070-477d-9bca-92cbcbff1f7c" width="600"/>

---

### Vehicle Trajectory Prediction

The prediction algorithm closely tracks actual vehicle positions at both short and long update intervals.

<img src="https://github.com/user-attachments/assets/b6e04f11-a5d7-42bc-8e2a-a47b108ed03d" width="600"/>

---

### Sensing-Aided Beamforming

<img src="https://github.com/user-attachments/assets/8c53b564-1d53-4132-9186-65ce20cafd70" width="600"/>

Proactive beamforming using predicted DOA achieves BER performance comparable to perfect true-DOA beamforming, with significantly lower computational cost.

<img src="https://github.com/user-attachments/assets/7484cf28-8d6c-493b-a934-9151ff6f4ae9" width="500"/>

<img src="https://github.com/user-attachments/assets/4c3b23ee-ebbf-4540-b659-0338e5583aaf" width="600"/>

---

## 💡 Key Takeaways

- Clutter cancellation is essential at mmWave — without it, static infrastructure completely masks targets in dense urban environments
- Doppler geometry matters — measurements near 90° line-of-sight angle are discarded to avoid velocity ambiguity, a subtle but critical design decision
- Prediction intervals up to 2 seconds still yield accurate beam alignment, promising for real-world deployment where feedback latency is unavoidable
- Bistatic configurations introduce additional geometric complexity but remain manageable with the proposed framework

---

## 📄 Publications

- *ISAC for Intelligent Transportation: Ray-Tracing, Clutter Cancellation, and Sensing-Aided Beamforming*
  Madhumitha Murthy, Hao Lin, Xiaojuan Zhang, Yonghong Zeng, Zhiping Lin, Francois Chin, Sumei Sun
  NTU & I²R A*STAR · **IEEE 2025 *(accepted, forthcoming)***

- *Kalman Filtering-based Target Tracking for Multistatic Sensing in ISAC Systems*
  **IEEE ISCAS 2025 *(accepted, forthcoming)***

---

## 🙏 Acknowledgements

Supported by the **National Research Foundation, Singapore** and **IMDA** under the Future Communications R&D Programme.
Conducted at **I²R, A*STAR** in collaboration with **Nanyang Technological University (NTU)**.

---

## 📬 Contact

**Madhumitha Murthy** · NTU Singapore
`madhumit007@e.ntu.sg` · [LinkedIn](https://www.linkedin.com/in/madhumitha-murthy-4801b7223/)
