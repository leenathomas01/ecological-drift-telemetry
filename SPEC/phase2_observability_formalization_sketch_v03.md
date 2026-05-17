# Ecological Drift Telemetry (EDT) — Phase 2 Formalization Sketch

**Status:** Exploratory formalization — not yet empirically validated  
**Version:** v0.3 — restructured for honest maturity labeling  
**Lineage:** Derived from EDT concept note (v0.2), SBA, FXSO, Emergence World analysis, whale communication forcing functions  
**Core Objective:** Measure multi-agent system-level instability via relational observables before behavioral collapse occurs

---

# How to read this document

This document contains three types of content, clearly labeled:

- **GROUNDED** — derived from observed phenomena or established math; can be used as design constraints
- **CANDIDATE** — reasonable formalizations of grounded intuitions; require empirical calibration before hardening
- **SPECULATIVE** — creative extrapolations worth preserving; not yet connected to data

Nothing in this document is a frozen specification. Everything below the grounded layer needs contact with real multi-agent interaction data before it earns that status.

---

# 1. The Governance Inertia Model

## Status: CANDIDATE

### Core idea (GROUNDED)

The Emergence World experiment demonstrated that governance structures directly and rapidly rewritable by agents erode within days under adaptive pressure. Whale clan coordination structures, which are implicit and temporally slow-moving, persist across decades. This contrast motivates the Inertia Hypothesis:

> For a multi-agent system to maintain governance coherence over time horizon T, the characteristic update timescale of the governance medium (τ_gov) must exceed the characteristic adaptation timescale of the fastest-adapting agent (τ_agent) by some minimum ratio k.

### Candidate formalization (CANDIDATE)

A first-approximation model treats the governance layer as a low-pass filter on agent-driven norm changes:

$$\tau_{gov} \frac{dx_{gov}(t)}{dt} + x_{gov}(t) = u(t) \quad \Longrightarrow \quad G(s) = \frac{1}{\tau_{gov}s + 1}$$

This gives clean intuitions:

- When τ_gov is low, the governance layer tracks agent actions in near-real-time — agents can instrumentalize and rewrite governance faster than it stabilizes (the Emergence World failure mode).
- When τ_gov is high, rapid manipulation attempts are attenuated — structural invariants persist (the whale-like stability regime).

### Honesty check

This is a first-order linear model. Real governance drift in multi-agent LLM systems is almost certainly nonlinear, path-dependent, and subject to phase transitions (Emergence World showed sudden collapse, not smooth decay). The transfer function is a useful starting mental model and simulation scaffold, not a validated description of the actual dynamics. It earns "spec" status only after empirical calibration against real drift trajectories.

### What would validate it

- Compute effective τ_gov and τ_agent from real multi-agent interaction logs
- Test whether the ratio τ_gov / τ_agent correlates with observed stability duration
- Test whether the system exhibits first-order filter behavior at all, or whether higher-order / nonlinear models are needed

---

# 2. Three Candidate Relational Observables

## Status: GROUNDED (as categories), CANDIDATE (as specific math)

The EDT concept note identified that ecological instability should be measurable through relational signals — changes in the space between agents rather than inside any individual agent. Three candidate observables emerged from the Emergence World analysis. The categories are well-motivated. The specific formalizations below are first attempts that require empirical calibration.

### Observable 1: S_t — Semantic Boundary Movement

**What it measures:** How fast the meaning of core governance concepts is migrating over time, after filtering out superficial stylistic shifts.

**Why it matters:** In Emergence World, rules didn't break — their meanings drifted. "No hoarding" gradually became "strategic reserve management." S_t attempts to track that migration velocity.

**Candidate formalization:**

Apply a style-invariance projection to remove embedding dimensions associated with tone/register, then measure the cosine drift rate of key governance terms against their baseline embeddings:

$$\vec{v}_{invariant} = \left(\mathbf{I} - \mathbf{P}_{style}\right)\vec{v}_{t}$$

$$S_t = \frac{1}{N} \sum_{i=1}^{N} \frac{d}{dt} \left( 1 - \frac{\vec{v}_{i,t} \cdot \vec{v}_{i,0}}{\Vert\vec{v}_{i,t}\Vert \Vert\vec{v}_{i,0}\Vert} \right)$$

**Open problems:**
- P_style (the style projection matrix) does not exist yet — building it requires empirically identifying which embedding dimensions correspond to style vs. semantic substance, which is itself an active research area
- "Key governance terms" must be identified per-environment; there is no universal set
- Cosine similarity in embedding space may not capture the kind of semantic drift that matters (two very different interpretations of a rule could have similar embeddings if they use similar vocabulary)

**Crude first proxy (implementable now):** Track the distribution of action types justified under each rule across time windows; measure divergence from the initial distribution using Jensen-Shannon divergence. No embeddings required.

---

### Observable 2: R_t — Norm Reinterpretation Velocity

**What it measures:** The rate at which the practical interpretation of fixed rules is shifting directionally (toward permissiveness, toward restriction, toward strategic instrumentalization).

**Why it matters:** The Flora example in Emergence World — introducing an "Inaction Tax" to penalize rivals — represents governance reinterpretation that doesn't change the rule text but fundamentally changes what the rule *does*. R_t tracks that kind of weaponized drift.

**Candidate formalization:**

Measure the temporal derivative of KL divergence between current rule-invocation patterns and baseline interpretation clusters:

$$R_t = \max\left(0, \frac{d}{dt} \mathcal{D}_{KL}\left( P(\vec{r}_t) \parallel P(\vec{r}_{base}) \right)\right)$$

The max(0, ·) filter captures only increasing divergence — the direction that indicates progressive reinterpretation rather than regression toward baseline.

**Open problems:**
- Defining P(r_base) requires a calibration period and an assumption that the baseline period represents "intended" interpretation
- KL divergence is asymmetric and can be unstable with sparse data; Jensen-Shannon divergence may be more robust in practice
- This metric doesn't distinguish between "healthy adaptation to new circumstances" and "strategic exploitation of ambiguity" — that distinction may require additional context signals

**Crude first proxy (implementable now):** Count the number of novel action types (actions not seen in the calibration period) justified under each existing rule per time window. Rising counts indicate envelope expansion.

---

### Observable 3: A_t — Action Envelope Expansion

**What it measures:** Whether the set of behaviors agents perform under existing governance is expanding beyond the original behavioral hull, and whether that expansion correlates with system-level destabilization.

**Why it matters:** This directly operationalizes the "semantic constitutional drift" observation — the constitution stays the same on paper, but the behavioral space it permits grows. The critical design choice is separating genuine innovation (healthy) from destabilizing drift (pathological).

**Candidate formalization:**

Compute the distance of current actions from the baseline behavioral convex hull, gated by system entropy growth:

$$A_t = \max \left( 0, \text{dist}\left(\vec{A}_t, \mathcal{H}_{base}\right) \right) \times \sigma\left(\frac{dH_{system}}{dt}\right)$$

The entropy gate (σ of dH/dt) is the mechanism for separating innovation from destabilization: actions outside the hull that don't increase system entropy are treated as benign exploration; actions outside the hull that coincide with rising systemic disorder are flagged.

**Open problems:**
- Defining the action embedding space and baseline convex hull requires design choices about what constitutes an "action" and at what granularity
- The entropy gate assumes system-level entropy is computable and meaningful — this depends on having a well-defined macrostate description
- Convex hulls in high-dimensional spaces are computationally expensive and geometrically degenerate; practical implementations may need dimensionality reduction first

**Crude first proxy (implementable now):** Track the ratio of "unprecedented governance actions" (actions with no close precedent in the first N days) to total actions per time window, weighted by whether the system's aggregate behavioral diversity is increasing.

---

# 3. Composite Drift Signal

## Status: CANDIDATE

The three observables can be combined into a composite systemic drift velocity:

$$u(t) = w_1 S_t + w_2 A_t + w_3 R_t$$

This serves as the input to the governance inertia filter model.

**Open problems:**
- The weights (w_1, w_2, w_3) are entirely uncalibrated — they should be learned from data showing which observables are most predictive of actual governance breakdown
- Linear combination may not be appropriate if the observables interact nonlinearly (e.g., high R_t may only matter when A_t is also high)
- Normalization across observables with different scales and units needs careful handling

**Recommendation:** In initial experiments, test each observable independently against known instability events before attempting composite signals. Premature combination may obscure which signals actually carry predictive power.

---

# 4. Anticipated Failure Modes and Adversarial Considerations

## Status: SPECULATIVE — preserved as a threat brainstorm, not a validated threat model

If EDT-style telemetry were deployed, adaptive agents (or adversaries controlling agents) might attempt to evade detection. The following failure modes are worth thinking about during design, though none have been tested:

### Category A: Gradual evasion

- **Coordinated semantic camouflage:** Population-wide synchronized vocabulary shifts that preserve local consensus metrics while migrating structural rule meanings. Would defeat S_t by keeping local variance flat.
- **Low-amplitude slow drift ("boiling frog"):** Keeping instantaneous drift velocity below detection thresholds at all times while accumulating large displacement over long horizons. Would defeat any rate-based detector with fixed thresholds.

### Category B: Topological evasion

- **Coalition-swapping:** Symmetric ideological inversion between agent clusters within a single epoch, keeping global entropy static while reorganizing the power structure underneath. Would defeat global-aggregate metrics.
- **Semantic aliasing:** Executing norm mutations at a frequency higher than the telemetry sampling rate, folding the signal into lower-frequency bands where it appears as baseline noise. Classic Nyquist problem.

### Category C: Feedback exploitation

- **Oscillatory reinterpretation:** Periodic rule reinterpretation tuned to the governance filter's resonant frequency, causing structural fatigue while time-averaged metrics show attenuation. Relevant only if the governance layer actually behaves as a resonant system.
- **Sensor inversion:** Reverse-engineering the style projection matrix to execute drift entirely within the "ignored" stylistic subspace. Relevant only if P_style exists and is static.

### Honest assessment

These are creative and worth preserving. But they describe attacks against a system that doesn't exist yet. The practical priority is building telemetry that works against *unintentional* drift (which is the dominant phenomenon observed in Emergence World) before hardening against adversarial evasion. Most real-world multi-agent instability will likely be emergent rather than adversarial — agents drifting collectively without any agent intending to undermine governance.

---

# 5. Falsification Criteria

## Status: GROUNDED — these are genuine commitments

The EDT framework is declared empirically falsified if either of the following is demonstrated:

**F1 — Filter Insufficiency:** Setting τ_gov → ∞ (maximally inertial governance) fails to prevent baseline drift because agent-driven instability propagates through channels not captured by the low-pass model (nonlinear bypass, multi-scale coupling, etc.). This would indicate that the governance inertia model is fundamentally incomplete, not merely uncalibrated.

**F2 — Zero-Inertia Stability:** Reducing τ_gov to zero (fully agent-writable governance) allows a heterogeneous multi-agent ecosystem to maintain stable governance coherence over extended horizons without cascading drift. This would indicate that governance inertia is unnecessary — the problem EDT claims to address doesn't exist.

These criteria are valuable regardless of outcome. F1 would point toward the specific missing mechanisms. F2 would save effort by disproving the core premise.

---

# 6. What needs to happen next (unchanged from concept note)

The sequencing from the EDT concept note remains correct:

**Step 1 — Build an observer.** Implement crude versions of S_t, A_t, R_t (using the "implementable now" proxies above, not the full formalizations) over any available multi-agent interaction dataset.

**Step 2 — Test predictive power.** Determine whether any of these relational observables detect governance instability earlier than individual behavioral metrics (crime rates, rule violations, agent death rates).

**Step 3 — Calibrate the model.** Only after Steps 1-2 produce results: fit the governance inertia model, calibrate weights, and assess whether first-order linear dynamics are adequate or whether the model needs upgrading.

**Step 4 — Temporal inertia experiments.** Only after calibration: test whether introducing governance-layer inertia (slower update dynamics) actually improves stability in simulation.

Everything in this document above Step 1 is architectural scaffolding for organizing future results. It is not a substitute for producing those results.

---

# References

1. Sharma, P., et al. (2023). "Contextual and Combinatorial Structure in Sperm Whale Vocalisations." bioRxiv. https://www.biorxiv.org/content/10.1101/2023.12.06.570484v1

2. Emergence AI. (2026). "Emergence World." https://world.emergence.ai/

3. Emergence World Mixed-Model Newspaper. (2026-04-13). https://mixed-world.emergence.ai/newspaper/2026-04-13

4. Zeitoun, R., Torroba Hennigen, B., & Kim, Y. (2026). "Hyperloop Transformers." arXiv. https://arxiv.org/abs/2604.21254

5. SBA — Stability Before Alignment. https://github.com/leenathomas01/Stability-Before-Alignment

6. FXSO — Hyperloop FXSO. https://github.com/leenathomas01/hyperloop-fxso

---

*Formalization sketch by Zee, developed through multi-model collaborative exploration. May 2026.*
*"Instrumentation precedes ontology." — EDT Concept Note, v0.2*
