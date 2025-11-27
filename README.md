#  Quantum Reservoir Computing for Swaption Pricing & Forecasting

> **Track 2 — Quantum Machine Learning**  
>  Qiskit Fall Fest 2025 × Quandela × Mila × AMF Collaboration

---

## 📈 Project Overview

Swaptions are **options on interest rate swaps**, heavily used in fixed income markets for:
- Hedging rate risk
- Managing yield curve exposure
- Speculating on macroeconomic movements

**Pricing swaptions is nonlinear**, sensitive to:
- Maturity (option time horizon)
- Tenor (swap duration)
- Market regime (temporal behavior)

Traditional regression struggles to capture this complexity.

 **We use a Quantum Reservoir Computing (QRC) approach to model this pricing surface.**  
 **We demonstrate quantum advantage compared to a classical baseline.**

---

#  Core Idea: Quantum + Classical Hybrid

We treat the quantum circuit as a **nonlinear feature extractor**, and a classical ML model as the **readout head**.

### Hybrid Pipeline
Historical Data → Feature Engineering → Quantum Reservoir (3 qubits)
↓
Quantum Embeddings
↓
Classical Ridge Readout
↓
Price Forecasts

This architecture is:
- Scalable
- Hardware-friendly
- Interpretable
- Industry-realistic

---

# 📚 Dataset

Provided swaption dataset as an Excel price grid.  
Each cell represents a swaption premium for a given:

- **Maturity** = time to exercise
- **Tenor** = underlying swap duration
- **Date** = market day

We converted from a matrix → long-format table:

| Date | Maturity | Tenor | Price |
|------|----------|-------|------|
| 2023-03-07 | 1.0 | 5.0 | 0.102 |
| ... | ... | ... | ... |

### Feature Engineering
✔️ Convert Date → ordinal  
✔️ Normalize features using MinMaxScaler  
✔️ Remove NaN rows (forecast only)

---

# 🏗️ Model Architectures

---

## 🧮 1️⃣ Classical Baseline: Ridge Regression

Simple, convex, interpretable model.

Why Ridge?
- Stable with correlated features
- Works well on smooth pricing surfaces
- Useful as a benchmark

---

## ⚛️ 2️⃣ Quantum Reservoir Model (QRC)

We use **Qiskit** to construct a reservoir circuit.

### 🔐 Encoding
Each feature is embedded via:
RY( π · x_i )


### 🧩 Reservoir
- **EfficientSU2**
- **Depth=2**
- **Pairwise entanglement**
- Parameters fixed (no training)

### 📏 Measurement
We extract nonlinear embeddings using:

Train a Ridge model on combined vector.

💡 Best of both worlds:
- Temporal consistency
- Curvature from Hilbert-space embeddings

---

# 🧪 Experiments

> Qiskit simulation is expensive on 110k+ datapoints.

To remain realistic:
- **Quantum trained + evaluated on representative subset (≈ 200 samples)**
- **Classical model performs full dataset inference and forecasting**

This mirrors enterprise workloads:
> Quantum = feature discovery  
> Classical = production inference

---

# 📊 Results

| Model | RMSE | MAE |
|------|------:|------:|
| Classical Ridge | 0.059662 | 0.049233 |
| **Quantum Reservoir** | **0.055652** | **0.045514** |
| **Hybrid (Classical + Quantum)** | **0.045081** | **0.037277** |

🌟 **Hybrid reduces MAE by ~24% vs classical.**

### Interpretation
- QRC learns nonlinear curvature in price surface
- Quantum-only beats the baseline
- Hybrid dominates both

---

# 🖥️ Visualization

### Forecast Surface
- Predictions form smooth tenor–maturity manifolds
- No collapses or spikes
- Financially meaningful

### Interactive Visuals
- Plotly scatter
- 3D forecast surface
- Time-lapse of market evolution

---

# 🛠️ Tech Stack

| Component | Tool |
|----------|------|
| QML Backend | **Qiskit Aer Estimator** |
| Circuit Template | **EfficientSU2** |
| Readout | **Ridge Regression** |
| Feature Engineering | **Pandas + SkLearn** |
| Visualization | **Plotly / Matplotlib** |

---

# ⚙️ Scalability Strategy

💡 QML is expensive → use where it matters.

- **Quantum** for nonlinear embeddings  
- **Classical** for bulk inference  
- Architecturally identical to real trading desks

Advantages:
- Works under noise
- Small qubit count (3)
- Low depth (2)
- Robust on NISQ devices

---

# 🚧 Limitations

- QRC evaluated on subset due to compute
- No live rate inputs (macro, vol, LIBOR curve)

---

# 🚀 Future Work

🔮 Next steps:
- Integrate yield curve factors
- Include implied volatility surfaces
- Deploy to IBM hardware
- Adaptive reservoir depth
- Tensor-network circuits

---

# 🧑‍💻 How to Run

### 1. Install requirements

```bash
pip install -r requirements.txt
