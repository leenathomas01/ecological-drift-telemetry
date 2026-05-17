# Adversarial Observability — Failure Mode Brainstorm

**Status:** Speculative — anticipated failure modes for a telemetry system that doesn't exist yet  
**Context:** Extracted from EDT Phase 2 formalization sketch; preserved as design thinking, not validated threat model

---

## Purpose

If EDT-style ecological drift telemetry were deployed over a multi-agent system, adaptive agents (or adversaries controlling them) might attempt to evade detection. This document catalogs anticipated evasion strategies as design-time considerations.

**Important framing:** The dominant failure mode in observed multi-agent systems (Emergence World) was *emergent unintentional drift*, not adversarial evasion. These adversarial scenarios are relevant for hardening a mature system, not for initial design. Build telemetry that detects unintentional drift first.

---

## Category A: Gradual evasion

### Coordinated semantic camouflage
Population-wide synchronized vocabulary shifts that preserve local consensus metrics while migrating structural rule meanings. All agents shift together, so pairwise divergence stays flat — but the entire population has moved relative to the original baseline.

**Would defeat:** S_t (semantic boundary movement) if measured only as local variance rather than absolute displacement from baseline.

**Potential counter:** Anchor S_t to a fixed external baseline, not to rolling population consensus.

### Low-amplitude slow drift ("boiling frog")
Keeping instantaneous drift velocity below detection thresholds at all times while accumulating large displacement over long horizons.

**Would defeat:** Any rate-based detector with fixed thresholds.

**Potential counter:** Track cumulative displacement in addition to instantaneous velocity. Monitor integral of drift, not just derivative.

---

## Category B: Topological evasion

### Coalition-swapping
Symmetric ideological inversion between agent clusters within a single epoch — two factions swap positions, keeping global entropy and population variance static while fundamentally reorganizing the power structure underneath.

**Would defeat:** Global-aggregate metrics that measure population-level statistics without tracking cluster identity persistence.

**Potential counter:** Track cluster identity continuity over time, not just cluster statistics.

### Semantic aliasing
Executing norm mutations at a frequency higher than the telemetry sampling rate, causing the signal to fold into lower-frequency bands where it appears as baseline noise. Classic Nyquist/Shannon problem applied to governance telemetry.

**Would defeat:** Any telemetry with fixed sampling windows.

**Potential counter:** Adaptive sampling rates triggered by early-warning indicators; or continuous monitoring rather than windowed sampling.

---

## Category C: Feedback exploitation

### Oscillatory reinterpretation
Periodic rule reinterpretation tuned to the governance filter's resonant frequency or phase latency. The signal integrates to zero over time (appears attenuated in time-averaged metrics) while causing structural fatigue in governance coherence.

**Would defeat:** Time-averaged drift metrics; the low-pass governance filter if it exhibits resonance.

**Potential counter:** Monitor governance layer stress/fatigue independently of average drift velocity. Track amplitude of oscillation, not just mean.

**Caveat:** Relevant only if the governance layer actually behaves as a resonant system. The first-order filter model in the formalization sketch doesn't have resonance; higher-order models might.

### Sensor inversion
Reverse-engineering the style projection matrix (P_style) to execute weaponized drift entirely within the "ignored" stylistic subspace — the telemetry pipeline actively scrubs out the attack vector as casual text noise.

**Would defeat:** Any telemetry that explicitly filters out a known subspace.

**Potential counter:** Periodic rotation or randomization of the projection basis; monitoring the "ignored" subspace for anomalous structure; avoiding static, transparent filtering architectures.

**Caveat:** Relevant only if P_style exists and is discoverable. Currently P_style is a candidate formalization, not an implemented component.

---

## Honest assessment

These failure modes are worth thinking about during design because they highlight architectural choices that could create blindspots. But they should not drive the initial system design. The priority sequence is:

1. Build telemetry that detects unintentional emergent drift
2. Validate that it works against known instability events
3. Then harden against adversarial evasion

Designing for adversarial robustness before having basic detection capability is premature optimization of the most classic kind.

---

*Brainstorm compiled from EDT Phase 2 exploration. May 2026.*
