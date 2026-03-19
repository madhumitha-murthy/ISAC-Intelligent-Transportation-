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

<img src="results/system_model.png" width="400"/>

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
| <img src="results/monostatic_config.png" width="350"/> | <img src="results/bistatic_config.png" width="350"/> |

---

### Static Clutter Cancellation

Before SCC, static reflections from buildings dominate the Range-Doppler Map, masking vehicles entirely. After SCC, moving vehicles are clearly distinguishable with distinct Doppler signatures.

**Monostatic**

| Before SCC | After SCC |
|---|---|
| <img src="results/Monostatic%20Algorithm%20before.png" width="350"/> | <img src="results/Monostatic%20Algorithm%20after.png" width="350"/> |

**Bistatic**

| Before SCC | After SCC |
|---|---|
| <img src="results/Bistatic%20algorithm%20before.png" width="350"/> | <img src="results/Bistatic%20algorithm%20after.png" width="350"/> |

---

### Urban Ray-Tracing Simulation

Real-time ray-tracing snapshot of the vehicle moving through the Fusionopolis road junction, capturing multipath reflections and dynamic occlusions.

<img src="results/vehicle%20moving.png" width="600"/>

---

### Vehicle Trajectory Prediction

The prediction algorithm closely tracks actual vehicle positions at both short and long update intervals.

<img src="results/tracking.png" width="600"/>

---

### Sensing-Aided Beamforming

<img src="results/pic1.jpeg" width="600"/>

Proactive beamforming using predicted DOA achieves BER performance comparable to perfect true-DOA beamforming, with significantly lower computational cost.

<img src="results/pic2.jpeg" width="500"/>

<img src="results/Comparison.png" width="500"/>

---

## 💡 Key Takeaways

- Clutter cancellation is essential at mmWave — without it, static infrastructure completely masks targets in dense urban environments
- Doppler geometry matters — measurements near 90° line-of-sight angle are discarded to avoid velocity ambiguity, a subtle but critical design decision
- Prediction intervals up to 2 seconds still yield accurate beam alignment, promising for real-world deployment where feedback latency is unavoidable
- Bistatic configurations introduce additional geometric complexity but remain manageable with the proposed framework

---

## 📂 Repository Structure

```
isac-digital-twin/
│
├── README.md
├── results/
│   ├── system_model.png
│   ├── monostatic_config.png
│   ├── bistatic_config.png
│   ├── Monostatic Algorithm before.png
│   ├── Monostatic Algorithm after.png
│   ├── Bistatic algorithm before.png
│   ├── Bistatic algorithm after.png
│   ├── tracking.png
│   ├── vehicle moving.png
│   ├── Comparison.png
│   ├── pic1.jpeg
│   └── pic2.jpeg
└── docs/
    └── publications/
        └── paper_info.md
```

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
