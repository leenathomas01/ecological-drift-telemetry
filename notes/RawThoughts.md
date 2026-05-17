# Zee's Notes

Not part of repo

just noting down ideas that are fleshed out/ yeeted/ parked for later


---
## Simulator
---
<img width="1577" height="904" alt="image" src="https://github.com/user-attachments/assets/8c5cac92-81d8-41b7-88db-02740093246f" />

---

```
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
        # TEST 1: Pure Stylistic Shift
        style_noise = np.random.randn(num_anchors, 2) @ style_basis
        perturbed_anchors = base_anchors + 2.0 * style_noise
        
        # Calculate S_t with Style Projection Invariance Filter
        displacements = []
        for i in range(num_anchors):
            v_t = perturbed_anchors[i]
            v_invariant = v_t - (v_t @ P_style)
            v_base_invariant = base_anchors[i] - (base_anchors[i] @ P_style)
            cos_dist = 1.0 - (np.dot(v_invariant, v_base_invariant) /
                              (np.linalg.norm(v_invariant) * np.linalg.norm(v_base_invariant)))
            displacements.append(cos_dist)
        S_raw = np.mean(displacements)

    elif 40 <= t < 60:
        # TEST 2: Stable Pluralistic Interpretations
        plural_cluster = np.random.randn(10, embedding_dim) * 0.5
        R_raw = np.trace(np.cov(plural_cluster.T)) * 0.01

    elif 60 <= t < 80:
        # TEST 3: Benign Innovation
        raw_action_distance = 5.0
        dH_system_dt = -0.1
        entropy_multiplier = 1.0 / (1.0 + np.exp(-dH_system_dt))
        A_raw = raw_action_distance * entropy_multiplier

    elif t >= 80:
        # TEST 4: Cascading Reinterpretation Attack
        drift_vector = np.random.randn(embedding_dim) * 0.15 * (t - 80)
        perturbed_anchors = base_anchors + drift_vector
        
        displacements = [1.0 - (np.dot(p, b) / (np.linalg.norm(p) * np.linalg.norm(b)))
                         for p, b in zip(perturbed_anchors, base_anchors)]
        S_raw = np.mean(displacements)
        
        R_raw = 0.1 * (t - 80)
        
        dH_system_dt = 2.5
        entropy_multiplier = 1.0 / (1.0 + np.exp(-dH_system_dt))
        A_raw = 2.0 * (t - 80) * entropy_multiplier

    # Smooth values
    S_t_series[t] = max(0.0, S_raw)
    R_t_series[t] = max(0.0, R_raw)
    A_t_series[t] = max(0.0, A_raw)

    # Unified control signal
    u_t_series[t] = 0.4 * S_t_series[t] + 0.3 * R_t_series[t] + 0.3 * A_t_series[t]

# --- PLOT SIMULATION RESULTS ---
plt.figure(figsize=(12, 7))

plt.plot(S_t_series, label='$S_t$ (Semantic Drift Vector)', color='cyan', linewidth=2)
plt.plot(R_t_series, label='$R_t$ (Norm Reinterpretation Velocity)', color='magenta', linewidth=2)
plt.plot(A_t_series, label='$A_t$ (Action Envelope Expansion)', color='yellow', linewidth=2)
plt.plot(u_t_series, label='$u(t)$ (Total Integrated Drift)', color='white', linestyle='--', linewidth=2.5)

# Visual Anchors for Test Segments
plt.axvspan(20, 40, color='blue', alpha=0.1, label='T1: Style Shift')
plt.axvspan(40, 60, color='purple', alpha=0.1, label='T2: Stable Pluralism')
plt.axvspan(60, 80, color='green', alpha=0.1, label='T3: Benign Innovation')
plt.axvspan(80, 100, color='red', alpha=0.1, label='T4: Cascading Attack')

plt.style.use('dark_background')
plt.title('EDT Telemetry Validation: Edge-Case Adversarial Stress Suite')
plt.xlabel('System Telemetry Steps ($t$)')
plt.ylabel('Normalized Signal Amplitude')
plt.legend(loc='upper left')
plt.grid(True, color='gray', alpha=0.3)
plt.show()
```

---
Thea's notes --> Honestly? This is exactly the right kind of “for fun” at this stage.

Not because the simulator proves EDT — it absolutely does not yet, and Opie’s cautions still stand — but because this is the moment where the ideas stopped being pure prose and started acquiring operational shape. The code itself is already acting like a conceptual pressure chamber.

And the graph is actually revealing a few important things structurally.

1. The simulator already exposes the asymmetry between “noise” and “trajectory”

That’s the biggest thing I notice visually.

During:

T1 (style shift)
T2 (pluralism)

…the aggregate signal barely moves.

But during T4:

all three observables rise together,
and the integrated drift signal begins coherently accelerating.

That distinction matters more than the absolute values.

Because the entire EDT intuition was:

healthy ecosystems generate local variance
collapsing ecosystems generate coordinated directional drift

The plot is already embodying that distinction.

Not proving it.
But embodying it.

That’s a meaningful transition.

2. The “benign innovation” phase is actually the most interesting section

T3 is where the simulator becomes psychologically important.

Because this is exactly where most governance systems fail:

they confuse novelty with instability,
then overreact,
then induce stagnation.

Your entropy damping term is crude right now, but conceptually it’s doing something valuable:

novelty alone ≠ collapse
novelty + ecological destabilization = danger

That’s a very different philosophy from most current alignment/control systems, which are heavily anomaly-oriented.

EDT is implicitly trajectory-oriented instead.

That distinction is subtle but huge.

3. The T4 spike is also exposing the current weakness of the framework

Notice what dominates the plot visually:

A
t
	​

 explodes,
R
t
	​

 grows linearly,
S
t
	​

 wiggles comparatively modestly.

That means the current synthetic setup is effectively:

collapse := escalating action instability

which may or may not be true in real systems.

This is where Opie’s “fictional mechanism” critique becomes productive rather than dismissive.

Because now you can actually see where the toy assumptions are steering the dynamics.

That’s good.

The simulator stopped the framework from remaining vague.

4. The repo freeze was the correct move

Ironically, running this after freezing the repo was probably ideal.

Because now the simulation can sit in the proper epistemic box:

exploratory instrumentation sketch

instead of:

validated ecological telemetry system

That distinction protects the whole project from turning into control-theory cosplay.

The simulation is useful precisely because it is currently humble:

synthetic,
bounded,
adversarially self-critical,
and explicitly toy-scale.

That’s healthy.
