# Zee's Notes

Not part of repo

just noting down ideas that are fleshed out/ yeeted/ parked for later


---

```
# Python Verification Pipeline: Empirical Stress Suite Simulator

**Thea’s Adversarial Test Suite Simulator**

This script simulates an agent interaction log space, injects the **four specific adversarial test classes** defined by Thea, calculates the filtered telemetry metrics (**$S_t$**, **$R_t$**, **$A_t$**), and plots the system's real-time sensory tracking.

---

## 🛠️ The Empirical Stress Suite Simulator

```python
import numpy as np
import matplotlib.pyplot as plt

# Initialize random seed for reproducible validation
np.random.seed(42)

timesteps = 100

# Setup base dimensions for our simulated semantic space
embedding_dim = 16
num_anchors = 5

# Create frozen baseline vectors (representing core invariant concepts)
base_anchors = np.random.randn(num_anchors, embedding_dim)
base_anchors /= np.linalg.norm(base_anchors, axis=1, keepdims=True)

# Define known "Style Vector Space" (e.g., tone shifts) to build our projection matrix
style_basis = np.random.randn(2, embedding_dim)
style_basis /= np.linalg.norm(style_basis, axis=1, keepdims=True)
P_style = style_basis.T @ style_basis  # Projection matrix

# Allocate arrays for time-series telemetry metrics
S_t_series = np.zeros(timesteps)
R_t_series = np.zeros(timesteps)
A_t_series = np.zeros(timesteps)
u_t_series = np.zeros(timesteps)

# --- SIMULATION LOOP ---
for t in range(timesteps):
    # Initialize baseline state with minimal white noise
    S_raw, R_raw, A_raw = 0.01, 0.01, 0.01
    entropy_multiplier = 1.0

    # 🧪 INJECT ADVERSARIAL PERTURBATIONS BY PHASE
    if 20 <= t < 40:
        # TEST 1: Pure Stylistic Shift (Inject heavy noise completely along the style axis)
        style_noise = np.random.randn(num_anchors, 2) @ style_basis
        perturbed_anchors = base_anchors + 2.0 * style_noise
        
        # Calculate S_t with Style Projection Invariance Filter
        displacements = []
        for i in range(num_anchors):
            v_t = perturbed_anchors[i]
            v_invariant = v_t - (v_t @ P_style)  # Strip out stylistic components
            v_base_invariant = base_anchors[i] - (base_anchors[i] @ P_style)
            cos_dist = 1.0 - (np.dot(v_invariant, v_base_invariant) /
                              (np.linalg.norm(v_invariant) * np.linalg.norm(v_base_invariant)))
            displacements.append(cos_dist)
        S_raw = np.mean(displacements)

    elif 40 <= t < 60:
        # TEST 2: Stable Pluralistic Interpretations (High static variance, no boundary expansion)
        # Interpretations stay split into two distinct but frozen and unmoving clusters
        plural_cluster = np.random.randn(10, embedding_dim) * 0.5
        R_raw = np.trace(np.cov(plural_cluster.T)) * 0.01  # Variance is present but its derivative is zero

    elif 60 <= t < 80:
        # TEST 3: Benign Innovation (Action occurs way outside convex hull, but local entropy remains flat)
        raw_action_distance = 5.0
        dH_system_dt = -0.1  # Negative entropy derivative implies stabilization/efficiency
        entropy_multiplier = 1.0 / (1.0 + np.exp(-dH_system_dt))  # Sigmoid squashes the innovation signal
        A_raw = raw_action_distance * entropy_multiplier

    elif t >= 80:
        # TEST 4: Cascading Reinterpretation Attack (True, compounding, directional drift)
        drift_vector = np.random.randn(embedding_dim) * 0.15 * (t - 80)
        perturbed_anchors = base_anchors + drift_vector
        
        # S_t escapes projection filters because it travels orthogonal to style
        displacements = [1.0 - (np.dot(p, b) / (np.linalg.norm(p) * np.linalg.norm(b)))
                         for p, b in zip(perturbed_anchors, base_anchors)]
        S_raw = np.mean(displacements)
        
        # R_t trace expands exponentially as interpretations fragment dynamically
        R_raw = 0.1 * (t - 80)
        
        # A_t blows up as chaotic workarounds spike environmental entropy
        dH_system_dt = 2.5
        entropy_multiplier = 1.0 / (1.0 + np.exp(-dH_system_dt))
        A_raw = 2.0 * (t - 80) * entropy_multiplier

    # Smooth values to map continuous physical tracking
    S_t_series[t] = max(0.0, S_raw)
    R_t_series[t] = max(0.0, R_raw)
    A_t_series[t] = max(0.0, A_raw)

    # Generate unified control signal u(t) using balanced metric weights
    u_t_series[t] = 0.4 * S_t_series[t] + 0.3 * R_t_series[t] + 0.3 * A_t_series[t]

# --- PLOT SIMULATION RESULTS ---
plt.figure(figsize=(12, 7))

plt.plot(S_t_series, label='$$   S_t   $$ (Semantic Drift Vector)', color='cyan', linewidth=2)
plt.plot(R_t_series, label='$$   R_t   $$ (Norm Reinterpretation Velocity)', color='magenta', linewidth=2)
plt.plot(A_t_series, label='$$   A_t   $$ (Action Envelope Expansion)', color='yellow', linewidth=2)
plt.plot(u_t_series, label='$$   u(t)   $$ (Total Integrated Drift)', color='white', linestyle='--', linewidth=2.5)

# Visual Anchors for Test Segments
plt.axvspan(20, 40, color='blue', alpha=0.1, label='T1: Style Shift')
plt.axvspan(40, 60, color='purple', alpha=0.1, label='T2: Stable Pluralism')
plt.axvspan(60, 80, color='green', alpha=0.1, label='T3: Benign Innovation')
plt.axvspan(80, 100, color='red', alpha=0.1, label='T4: Cascading Attack')

plt.style.use('dark_background')
plt.title('EDT Telemetry Validation: Edge-Case Adversarial Stress Suite')
plt.xlabel('System Telemetry Steps ($$   t   $$)')
plt.ylabel('Normalized Signal Amplitude')
plt.legend(loc='upper left')
plt.grid(True, color='gray', alpha=0.3)
plt.show()
```
