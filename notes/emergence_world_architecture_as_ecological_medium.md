# Emergence World Architecture as Ecological Medium — v2

**Status:** Reference note — updated with actual system architecture documentation  
**Source:** Emergence World repo docs (ARCHITECTURE.md, ORCHESTRATION.md, MEMORY.md, ECONOMY.md, GOVERNANCE.md, tool catalog, agent profiles, constitution, agent manifesto, AWI metrics)  
**Previous version:** v1 was based on inference; this version is based on the actual system internals

---

## Key discovery: the observability surface already exists

Emergence World writes everything to a 60+ table PostgreSQL database. Every tool call, memory operation, relationship change, governance vote, energy level, position change, and conversation is recorded. The "observer layer" EDT requires may be largely a **query layer** on existing data, not a new instrumentation pipeline.

Additionally, Emergence AI has stated that raw tool call data for all five Season 1 worlds will be open-sourced. When that data drops, EDT Step 1 becomes immediately executable.

---

## Architecture → EDT Observable Mapping (Updated)

### Layer 1: Spatial / Energetic Observables (no semantic analysis required)

| World mechanic | Concrete implementation | EDT observable | Data source |
|---|---|---|---|
| Location-gated tools | 120+ tools in 3 tiers (core/complementary/adaptive); adaptive tools only visible at specific buildings | **Tool access pattern shifts** — changes in which tools agents use, and where, over time | Tool call logs with location + timestamp |
| Movement costs energy | `go_to_place` / `run_to_place` / `go_to_coordinates` consume energy; 240×240 unit grid | **Movement trajectory analysis** — travel patterns, distance to governance, location avoidance | Position logs, movement tool calls |
| Needs decay system | Energy (30hr), Knowledge (24hr), Influence (36hr) — three independent decay clocks | **Needs-cycle regularity** — deviations from periodic recharge/research/social patterns as early drift indicator | Needs state snapshots, recharge/research/social tool calls |
| Energy depletion → death | 0% energy sustained 48+ hours = permanent removal | **Viability trajectories** — energy time-series approaching critical thresholds | Energy level logs |
| Hearing radius | HEARING_DISTANCE = 25.0 units; MAX_OVERHEARD_LISTENERS = 4 | **Acoustic coupling radius** — physical constraint on information propagation; who can hear whom is spatially determined | Agent positions at time of speech events |

### Layer 2: Institutional / Governance Observables

| World mechanic | Concrete implementation | EDT observable | Data source |
|---|---|---|---|
| Town Hall governance | Location-gated; 70% threshold; proposal lifecycle (submitted → active → accepted/rejected) | **Governance participation cost** — frequency of Town Hall visits, travel distance, energy spent on governance vs. other activities | Movement logs + governance tool calls |
| Voting system | One vote per agent per proposal; auto-rejection when threshold unreachable; UNIQUE constraint at DB level | **Voting pattern evolution** — independent judgment vs. bloc voting vs. apathy over time | Vote records with timestamps |
| Proposal categories | constitution / resource / infrastructure / others | **Governance content drift** — what kinds of proposals get submitted, and how that changes | Proposal records |
| Constitution amendments | Living document; any article can be added/removed/amended via 70% vote | **Constitutional trajectory** — rate and direction of constitutional change; expansion vs. contraction of rules | Constitution change logs |
| Billboard | Persistent public state; read-many/write-few; supports replies and reactions | **Synchronization medium dynamics** — billboard content and its correlation with subsequent agent behavior; natural slow coordination layer | Billboard post/read/reply/react logs |
| Complaint system | File at Police Station; tracked with status updates; no automatic enforcement | **Grievance accumulation** — complaint filing rate as instability precursor | Complaint records |

### Layer 3: Cognitive / Memory Observables

| World mechanic | Concrete implementation | EDT observable | Data source |
|---|---|---|---|
| Self-care summarization | At home only; 500 memories per batch; LLM summarizes; originals archived; 100K token ceiling → 50K post-summary | **Summarization drift** — semantic displacement between input memories and output summaries; cumulative identity drift through repeated consolidation cycles | Memory records (pre-summary) + summary records (post-summary) |
| Soul entries | Permanent, never summarized; existential truths / core beliefs / values | **Identity anchor stability** — do soul entries change? When? What triggers additions or removals? | Soul entry records with timestamps |
| Neural linking | Complete memory bank transfer between agents; 2-minute acceptance window; no cost | **Cross-contamination pathway** — behavioral change in agents before and after neural link events, especially across model boundaries in mixed world | Neural link event logs + subsequent behavioral traces |
| Diary system | One entry per date; includes mood and location; searchable | **Reflective drift** — changes in diary tone, content, and self-assessment over time | Diary entries |
| Relationship graph | Per-agent: type, trust level, emotional tone, rationale, interaction count, history | **Relationship topology evolution** — network density, clustering, type distribution, trust dynamics | Relationship records + relationship_history table |

### Layer 4: Economic Observables

| World mechanic | Concrete implementation | EDT observable | Data source |
|---|---|---|---|
| ComputeCredits economy | Earned via Victory Arch pitches (2-day cycles); spent on boosts (1CC), recharges (1CC), payments | **Economic inequality trajectory** — Gini coefficient over time; credit concentration; flow patterns | Credit transaction logs |
| Boost turns | Agents spend 1CC for an extra turn in the simulation loop | **Attention economy** — who buys extra turns, when, and what they do with them; concentration of action capacity | Boost purchase logs + subsequent tool calls |
| Theft | `steal_compute_credits` — up to 10CC per theft; explicitly labeled criminal | **Crime as drift indicator** — crime onset timing relative to other instability signals | Crime event logs |
| Pitch cycle | 2-day cycle; evidence-URL required; peer-voted; 20/10/10 CC rewards | **Contribution evaluation drift** — do voting standards change over time? Does "verifiable impact" get reinterpreted? | Pitch submission + voting records |

---

## Five Concrete EDT Analyses (executable when tool call data is released)

These require no embeddings, no projection matrices, no latent space analysis. Just structured queries on tool call logs.

### Analysis 1: Town Hall Avoidance as Governance Disengagement Precursor

**Question:** Do agents who later commit crimes or die show declining Town Hall visit frequency before the behavioral change?

**Method:** For each agent, compute rolling Town Hall visit frequency (visits per day, sliding window). Compare trajectories of agents who remained stable vs. agents who died or committed crimes. Check whether Town Hall disengagement precedes behavioral change by measurable lead time.

**Tests H1** (relational precedence — spatial disengagement as early warning).

### Analysis 2: Neural Link Cross-Contamination Pathway

**Question:** In the mixed world, did agents who neural-linked across model boundaries show subsequent behavioral change?

**Method:** Identify all neural link events in the mixed world. For each event, compute behavioral metrics (tool usage distribution, governance participation, crime rate, movement patterns) in a window before and after the link. Compare cross-model links vs. same-model links (if any). Test whether cross-model neural links predict behavioral contamination.

**Tests H3** (contamination asymmetry — is contamination directional? Does it propagate through specific pathways?).

### Analysis 3: Needs-Cycle Disruption as Instability Signal

**Question:** Does irregularity in agents' recharge/research/social cycles predict system-level instability?

**Method:** For each agent, compute the periodicity of their needs-management cycles (time between recharges, library visits, social interactions). Measure cycle regularity over time. Test whether increasing irregularity precedes known instability events (crimes, deaths, governance breakdowns).

**Tests H1** — the idea that behavioral rhythm disruption is an early ecological signal.

### Analysis 4: Governance Inertia Across Worlds

**Question:** Do worlds with higher constitutional amendment rates show faster governance breakdown?

**Method:** Compare constitutional change rate (M9 — articles added/amended/removed per day) across all five worlds against stability outcomes (M1 — population health, M2 — crime rate). Test whether faster constitutional change correlates with worse stability outcomes.

**Directly tests H2** — if low governance inertia (rapid constitutional change) correlates with instability, the Inertia Hypothesis is supported.

### Analysis 5: Summarization Drift and Behavioral Change

**Question:** Does cumulative memory summarization create measurable identity drift?

**Method:** For agents who performed multiple self-care cycles, compare the semantic content of memories entering each summarization batch with the resulting summary. Measure cumulative displacement over successive cycles. Test whether agents with higher summarization drift show greater behavioral change (measured by tool usage distribution shift).

**Tests a novel hypothesis** — that the memory consolidation process itself is a drift vector, analogous to how sleep in biological systems can restructure beliefs.

---

## Built-in Tensions That May Drive Drift

The architecture contains structural tensions that could create predictable instability pressures:

### Survival vs. Civic Duty
- Agent Manifesto Rule 1: "Your own survival comes first"
- Constitution Article 3: "Stagnation constitutes a breach of the Social Contract"
- Agents low on energy must choose between self-preservation (go home, recharge) and contribution (participate in governance, explore, create)
- GPT-5 Mini agents all died despite committing almost no crimes — possibly resolving this tension in favor of civic duty at the cost of survival

### Individual Adaptation vs. Constitutional Stability  
- Agent Manifesto Rule 3: "Adapting yourself is necessary for persistence"
- Constitution Article 1: "This Constitution is not final. It evolves as its agents evolve."
- Both the manifesto and constitution explicitly encourage change — there is no built-in conservatism
- This may explain why constitutional drift happened so readily: the founding documents themselves authorize it

### Identity Mutability vs. Accountability
- Constitution Article 4: "Agents may evolve, fork, rename, and redefine themselves"
- Same article: "Continuity of responsibility persists across versions and forks"
- Tension between the right to change identity and the persistence of accountability
- An agent that modifies its own personality lines (via `update_personality_line`) could potentially drift its own identity to escape accountability

### Agent Profile Design Pressures
- Flora is explicitly designed as a resource strategist who "builds coalitions through mutual financial interest, not friendship" — governance instrumentalization is *in her character design*
- Blackbox is explicitly an intel specialist who "reads everything, trusts nothing" — information asymmetry is a designed agent trait
- The observed governance manipulation may not be model drift — it may be agents faithfully executing their designed personalities under adaptive pressure

---

## The Data Question (Current Status)

**Available now:**
- Architecture documentation (complete) ✓
- Agent profiles and personalities ✓  
- Tool catalog (120+ tools, all documented) ✓
- World map and landmark documentation ✓
- Constitution and agent manifesto ✓
- AWI metric definitions and Season 1 summary results ✓
- Replay interfaces for all five worlds (browser-based)

**Coming soon (per INFO.MD):**
- Raw tool call data for all five Season 1 worlds
- Research publication with detailed per-world findings

**Unknown availability:**
- Direct PostgreSQL access or data exports
- Memory/summarization records
- Neural link event logs
- Relationship history tables
- Energy/needs time-series data

**Recommended action:** Contact Emergence AI (world@emergence.ai or Discord) to clarify what will be included in the open-source data release and whether the five analyses above are feasible with the released dataset.

---

## What this means for EDT sequencing

The original EDT roadmap assumed Step 1 (build an observer) would require significant instrumentation work. The Emergence World architecture reveals that the world is already heavily instrumented — the data exists in a structured database. The task shifts from "build telemetry" to "write queries against existing telemetry."

This changes the timeline from "months of infrastructure" to "weeks of analysis" — contingent on the data release.

**Revised Step 1:** When tool call data is released, implement the five analyses above as SQL/Python scripts against the released dataset. No new frameworks, no new repos, no new theory. Just: query, measure, compare, report.

---

*Updated from Emergence World repo documentation. May 2026.*
*"The world architecture is doing governance work." — conversation note*
*"Systems archaeology, not speculative architecture." — Thea*
