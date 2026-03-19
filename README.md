# 🛰️ ISAC Digital Twin Framework for Intelligent Transportation Systems

> **Research Attachment** — Institute for Infocomm Research (I²R), A*STAR · Oct 2024 – Dec 2025

A high-fidelity simulation and algorithmic framework for **Integrated Sensing and Communications (ISAC)** in intelligent transportation systems (ITS), built on top of NVIDIA Sionna ray tracing. This repository documents the architecture, methodology, and results — code is proprietary and not included.

---

## 📌 Overview

This project develops an end-to-end **digital twin simulation pipeline** for 5G/6G-enabled vehicle sensing and communication. The framework combines physically accurate 3D urban environments with advanced signal processing algorithms to evaluate and prototype ISAC systems at scale.

Key contributions:
- 3D urban digital twin environment built with **Blender** + **OpenStreetMap** (Fusionopolis, Singapore)
- **NVIDIA Sionna** ray-tracing engine for realistic wireless channel simulation
- **OFDM radar** processing with **Static Clutter Cancellation (SCC)** for vehicle detection
- **Doppler-based trajectory prediction** for proactive sensing-aided beamforming
- Scalable Python pipelines for multi-scenario dataset generation and evaluation

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Digital Twin Environment                  │
│   Blender 3D Model + OpenStreetMap + Sionna Ray Tracing     │
└────────────────────────┬────────────────────────────────────┘
                         │
          ┌──────────────▼──────────────┐
          │      OFDM Signal Processing  │
          │   Static Clutter Cancellation│
          └──────────────┬──────────────┘
                         │
          ┌──────────────▼──────────────┐
          │   Trajectory Prediction      │
          │   (Doppler + Motion Model)   │
          └──────────────┬──────────────┘
                         │
          ┌──────────────▼──────────────┐
          │   Sensing-Aided Beamforming  │
          │   (Proactive DOA Alignment)  │
          └─────────────────────────────┘
```

---

## 📂 Repository Structure

```
isac-digital-twin/
│
├── README.md
│
├── environment/                    # Digital twin scene setup
│   ├── blender/                    #   3D urban model assets & scene files
│   ├── openstreetmap/              #   OSM map data & building geometry imports
│   └── sionna_config/              #   Ray-tracing scene config & material params
│
├── channel_simulation/             # Wireless channel modeling
│   ├── ray_tracing/                #   Sionna RT simulation scripts
│   ├── channel_estimation/         #   H(k,m) estimation pipeline
│   └── dataset_generation/         #   Multi-scenario batch generation pipeline
│
├── sensing/                        # Radar sensing algorithms
│   ├── ofdm_radar/                 #   OFDM radar processing pipeline
│   ├── clutter_cancellation/       #   Static Clutter Cancellation (SCC) algorithm
│   └── range_doppler/              #   Range-Doppler map generation & visualization
│
├── tracking/                       # Vehicle tracking & prediction
│   ├── trajectory_prediction/      #   Doppler-based position prediction algorithm
│   ├── kalman_filter/              #   Kalman filter for multi-target tracking
│   └── evaluation/                 #   Prediction accuracy metrics & plots
│
├── beamforming/                    # Communication & beamforming
│   ├── sensing_aided/              #   Sensing-aided beamforming (predicted DOA)
│   ├── baselines/                  #   True DOA & estimation-based benchmarks
│   └── ber_analysis/               #   BER vs SNR evaluation scripts
│
├── configs/                        # Experiment configuration files
│   ├── monostatic.yaml             #   Monostatic ISAC config
│   ├── bistatic.yaml               #   Bistatic ISAC config
│   └── ofdm_params.yaml            #   OFDM radar parameter definitions
│
├── results/                        # Outputs and visualizations
│   ├── rdm_plots/                  #   Range-Doppler maps (before/after SCC)
│   ├── tracking_snapshots/         #   Vehicle tracking at multiple timesteps
│   └── ber_curves/                 #   BER performance comparison plots
│
├── docs/                           # Documentation & references
│   ├── system_model.md             #   Signal model and algorithm derivations
│   ├── simulation_setup.md         #   Step-by-step environment setup guide
│   └── publications/               #   Related IEEE/Nature paper references
│
└── requirements.txt                # Python dependencies
```

---

## ⚙️ Simulation Parameters

| Parameter                   | Value                    |
|-----------------------------|--------------------------|
| Number of OFDM Symbols (M)  | 140                      |
| Number of Subcarriers (N)   | 4096                     |
| Subcarrier Spacing          | 30 kHz                   |
| Carrier Frequency           | 59 GHz                   |
| Bandwidth                   | 100 MHz                  |
| Modulation Scheme           | QPSK                     |
| Transmit / Receive Antennas | 2 / 2                    |
| Ray-Tracing Engine          | NVIDIA Sionna            |
| Urban Scene                 | Fusionopolis, Singapore  |

---

## 🧠 Algorithms

### 1. OFDM Radar with Static Clutter Cancellation (SCC)
Targets are detected in the frequency-domain channel estimate `H(k,m)`. Static reflections are averaged across OFDM symbols and subtracted, leaving only moving targets. A 2D FFT then produces the Range-Doppler Map (RDM), from which delay (range) and Doppler (velocity) are extracted.

### 2. Sensing-Based Trajectory Prediction
Using past position estimates and Doppler measurements, the algorithm computes vehicle heading and resolves the radial velocity component to estimate true speed. The next position is predicted as:

```
p̂(k+1) = p̂(k) + v̂(k) · d̂(k) · Δt
```

Doppler measurements with a near-90° line-of-sight angle are discarded to avoid geometric ambiguity.

### 3. Sensing-Aided Beamforming
Predicted future positions are converted to azimuth/elevation angles, which are used to construct steering vectors and update beamforming weights **proactively** — before the vehicle moves — reducing misalignment in high-mobility scenarios.

---

## 📊 Key Results

- **SCC** effectively suppresses static clutter in both monostatic and bistatic configurations, making moving vehicles clearly distinguishable in RDMs.
- **Trajectory prediction** closely tracks actual vehicle positions across prediction intervals (0.333s to 2s update rates).
- **Sensing-aided beamforming** achieves BER performance comparable to true DOA-based beamforming, with significantly lower computational overhead.

---

## 🗺️ Deployment Scenarios

| Configuration  | Description                                              |
|----------------|----------------------------------------------------------|
| **Monostatic** | Transmitter and receiver co-located at the base station  |
| **Bistatic**   | Transmitter and receiver spatially separated             |

Both configurations are evaluated in the Fusionopolis urban ITS scenario with realistic multipath, dynamic occlusions, and Doppler effects.

---

## 📄 Publications

This work contributed to the following publications:

- **IEEE Conference Paper** — *ISAC for Intelligent Transportation: Ray-Tracing, Clutter Cancellation, and Sensing-Aided Beamforming*, Madhumitha Murthy et al., NTU & I²R A*STAR, 2025.
- Related work: *Kalman Filtering-based Target Tracking for Multistatic Sensing in ISAC Systems*, IEEE ISCAS 2025.

---

## 🙏 Acknowledgements

This research is supported by the **National Research Foundation, Singapore** and the **Infocomm Media Development Authority** under its Future Communications Research & Development Programme.

Conducted at the **Institute for Infocomm Research (I²R), A*STAR**, in collaboration with the School of Electrical and Electronic Engineering, **Nanyang Technological University (NTU)**.

---

## 📬 Contact

**Madhumitha Murthy**
School of Electrical and Electronic Engineering, NTU Singapore


---

> ⚠️ **Note:** Source code is proprietary and not included in this repository. This repo serves as public documentation of the research methodology, system design, and results.