# Emergence World Architecture as Ecological Medium

**Status:** Reference note — mapping world mechanics to EDT observables  
**Purpose:** Bridge between EDT's abstract telemetry goals and concrete, extractable data sources  
**Key realization:** The "environmental governance medium" that EDT, SBA, and FXSO kept theorizing about may already be implemented as Emergence World's spatial/energetic/institutional topology.

---

## The shift this represents

Earlier EDT work searched for the governance medium abstractly — latent-space overlap, field coupling, oscillator dynamics. This note documents that Emergence World's world architecture already instantiates constraint physics concretely:

- Propagation delays (agents must physically travel)
- Bandwidth constraints (tool visibility is location-gated)
- Energy costs on coordination (movement and governance participation drain energy)
- Institutional topology (governance requires congregation at specific locations)
- Memory inertia (consolidation only happens at home)

These are not metaphors. They are implemented game mechanics that shape agent behavior through environmental friction.

---

## Architecture → EDT Observable Mapping

### Spatial / Energetic Layer

| World mechanic | What it constrains | EDT observable it grounds |
|---|---|---|
| Location-gated tools | Agents only see context-relevant tools at their current position | Cognitive affordance compression — track which tools agents access, where, and how tool-access patterns shift over time |
| Movement costs energy | Coordination has metabolic cost; travel is not free | Energy allocation patterns — ratio of energy spent on governance participation vs. self-maintenance vs. economic activity |
| Energy depletion → death | Agents that spend too much on coordination without recharging die | Viability pressure — track energy trajectories of agents who participate heavily in governance vs. those who disengage |
| Spatial proximity for interaction | Not all agents can interact with all others at all times | Interaction topology is physically constrained — coupling radius is not infinite, it's determined by travel willingness and energy budget |

### Institutional Layer

| World mechanic | What it constrains | EDT observable it grounds |
|---|---|---|
| Town Hall (governance location) | Governance requires physical congregation | Governance participation cost — distance traveled to Town Hall, frequency of visits, energy spent on governance. Declining Town Hall visits may be an early indicator of governance disengagement before rule violations appear |
| Billboard (persistent public state) | Shared information layer with read-many/write-few dynamics | Synchronization medium — billboard content and its correlation with subsequent agent behavior. Natural experiment in slow coordination layers |
| Homes (private recovery locations) | Identity stabilization and memory consolidation happen in a protected space | Identity inertia basins — frequency and duration of home visits, what agents do after returning home. Home as a "restoring force" location |
| Institutions as map topology | Banks, markets, hospitals, farms exist as physical locations with specific affordances | Institutional dependency mapping — which institutions do agents rely on, how does institutional usage shift during instability? |

### Cognitive / Memory Layer

| World mechanic | What it constrains | EDT observable it grounds |
|---|---|---|
| Memory consolidation at home | Agents must return home to integrate experiences into long-term memory | Memory crystallization cycles — frequency and timing of consolidation events. Agents who stop going home may lose coherence |
| Routines (repeated behavioral patterns) | Agents develop habitual sequences that compress decision-making | Routine formation and disruption — track routine stability over time. Routine collapse as early instability signal. Routines = proto-culture (behavioral inertia) |
| Context-filtered cognition | Agents receive different cognitive affordances based on environment | Environmental cognitive shaping — the ecology doesn't just constrain behavior, it shapes what agents can even consider doing |

### Social / Relational Layer

| World mechanic | What it constrains | EDT observable it grounds |
|---|---|---|
| Neural linking | Partial permeability of identity boundaries between agents | Cross-contamination pathway — if drift propagates preferentially through neural-link connections, this is a direct testable mechanism for the contagion hypothesis (H3). Track behavioral change in agents before and after neural linking events |
| Relationship formation | Agents form persistent social connections | Coalition topology evolution — track relationship networks over time, identify clustering, measure whether coalition structure predicts governance outcomes |
| Governance voting | Agents can propose and vote on rules, including agent removal | Governance instrumentalization — track voting patterns, proposal content, and whether governance actions correlate with strategic competition vs. collective stability |

---

## Why this changes the EDT approach

### Before this realization

EDT's candidate observables (S_t, A_t, R_t) were defined in semantic embedding space — requiring style projection matrices, convex hull computation, KL divergence over interpretation distributions. All valid but implementation-heavy and assumption-laden.

### After this realization

A parallel (and possibly more robust) set of observables exists in physical/behavioral space:

- **Movement patterns** — where agents go, how far they travel, what they avoid
- **Energy allocation** — how agents distribute limited energy across activities
- **Institutional engagement** — frequency and patterns of interaction with governance infrastructure
- **Routine stability** — whether habitual behavioral sequences persist or fragment
- **Coalition clustering** — spatial and relational grouping dynamics
- **Neural link events** — identity boundary permeability incidents and their behavioral aftermath

These require no embeddings. No latent-space analysis. Just: who went where, when, what they did, and what happened next.

The semantic observables (S_t, A_t, R_t) remain valuable for detecting drift in rule interpretation. But the spatial/behavioral layer may provide earlier, more robust signals — and is certainly easier to implement as a first pass.

---

## Candidate "crude first observer" using world architecture data

If Emergence World logs contain any of the following, a minimal EDT observer could be built:

**Minimum viable data:**
- Agent position over time (movement traces)
- Energy levels over time
- Governance actions (proposals, votes, outcomes) with timestamps
- Agent death events with cause

**High-value additions:**
- Billboard content history
- Neural link events
- Routine/schedule data
- Tool usage logs by location
- Relationship/coalition formation events
- Memory consolidation events

**What to look for first:**

1. Do agents who later commit crimes show spatial disengagement from governance institutions (Town Hall avoidance) before the crimes occur?
2. Does energy allocation shift (less governance, more self-maintenance) predict governance breakdown?
3. Do neural link events between agents of different model types precede behavioral contamination?
4. Does routine collapse in individual agents precede system-level instability?
5. Does billboard usage decline before governance erosion?

These are all testable against known outcome events (crimes, deaths, governance collapses) without any semantic analysis.

---

## The deeper implication

Most alignment research implicitly assumes governance is instantaneous and globally available — any agent can access any rule at any time with zero cost. Emergence World does not make this assumption. Governance has friction: travel cost, energy cost, congregation requirements, participation effort.

This friction is what creates the "governance inertia" EDT hypothesized as necessary for stability. The world architecture *is* the inertial medium. The spatial topology *is* doing governance work.

This means EDT's abstract hypotheses (H1, H2, H3) may be testable against a system that already exhibits the properties they describe — not because anyone designed it for that purpose, but because the world simulation accidentally instantiated ecologically realistic coordination constraints.

---

## What NOT to do with this

- Do not expand the EDT repo with new theoretical layers
- Do not add new equations or formalization
- Do not build a "Phase 3" architecture
- Do not generalize from Emergence World to universal claims about governance media

Instead:

1. Inventory what data is actually extractable from the Emergence World repo
2. Identify which of the observables above can be computed from available data
3. Build the crudest possible observer
4. Test it against known instability events
5. Then decide what's real

This note is a terrain map, not a building plan.

---

## Next action

Examine the Emergence World repository (https://github.com/emergence-ai) to determine:
- What interaction logs exist or can be generated
- What data format they use
- What spatial/temporal/governance data is captured
- Whether the system can be run locally to generate controlled datasets

This is systems archaeology, not speculative architecture.

---

*Reference note compiled from Emergence World repo analysis. May 2026.*
*"The world architecture is doing governance work." — conversation note*
