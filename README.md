# NS-SMH — Neuro-Symbolic Homeostatic Alignment in Generative Agents

**A formal framework for decoupling affective perception from deterministic action in mental-health interventions.**

![Status](https://img.shields.io/badge/Status-Research_Prototype-blue)
![Architecture](https://img.shields.io/badge/Architecture-Neuro--Symbolic_|_ACT--R-purple)
![Safety](https://img.shields.io/badge/Safety-Provable_Bottleneck-green)

---

## Short summary

NS-SMH is a safety-first, neuro-symbolic cognitive architecture for mental-health advisors that **mechanically prevents neural models from generating advice**. Instead the system:

1. **Perceives** affect via a constrained neural transducer (text → VAD vector),  
2. **Reasons** with a symbolic cognitive engine (SOAR + EpMem + rule set), and  
3. **Acts** via a deterministic, human-verified intervention database.

This separation is enforced by an explicit **semantic information bottleneck** (the neural module outputs only a 3-dimensional affective vector), which provably collapses linguistic instruction structure and drastically reduces the attack surface for prompt injection and hallucination.

---

## Table of contents

- Abstract & problem statement  
- Contributions  
- Formal model (mathematical and assumptions)  
- Threat model & security claims  
- Architecture & components (detailed)  
- Homeostat (dynamics & equations)  
- Symbolic rules, provenance & audit schema  
- Evaluation plan and metrics  
- Implementation plan & repo layout  
- Related work & citations

---

## 1. Abstract & problem statement

Contemporary LLM-based mental-health agents typically map user text `X` directly to advice `Y` via a learned conditional distribution `P(Y | X; θ)`. In safety-critical settings this creates two structural hazards:

*Stochastic hallucination* — LLMs may produce plausible but unverified or unsafe text (fabricated resources, incorrect clinical suggestions). :contentReference[oaicite:0]{index=0}

*Linguistic vulnerability* — because input and output share the same linguistic medium, adversarial or malicious prompts (prompt injection / jailbreaks) can subvert safety controls and cause the model to generate harmful outputs. Recent work and security reviews show prompt-injection attacks remain a hard problem in practice. :contentReference[oaicite:1]{index=1}

**NS-SMH objective:** re-architect the agent so that perception (affect extraction) is strictly decoupled from action (advice), thereby (a) making hallucination impossible at the action level and (b) rendering linguistic injection attacks representationally inert.

---

## 2. Key contributions

1. **Semantic information bottleneck** (text → 3-dimensional VAD vector) that constrains `I(X; V)` by design, rather than by heuristics. The bottleneck is grounded in the Information Bottleneck principle. :contentReference[oaicite:2]{index=2}  
2. **Neuro-symbolic pipeline**: pretrained neural transducer for perception + SOAR symbolic controller for deterministic decision-making and EpMem for one-shot safety learning. :contentReference[oaicite:3]{index=3}  
3. **Homeostatic regulation** using ACT-R-style cognitive load dynamics to prevent toxic positivity and to adapt response complexity. :contentReference[oaicite:4]{index=4}  
4. **Auditability & provenance**: every action includes a machine-readable trace (VAD, matched rules, EpMem case id, intervention id).  
5. **Practical evaluation plan** emphasizing hallucination rate, safety violation rate, intervention appropriateness, and adversarial robustness tests.

---

## 3. Formal model & assumptions

### 3.1 Notation
- `X` ∈ 𝓛 : raw user text (tokens).  
- `f` : neural transducer (text → VAD).  
- `V = f(X)` : VAD vector ≡ `[valence, arousal, dominance] ∈ [0,1]^3`.  
- `g` : symbolic controller (SOAR rules + EpMem).  
- `M` : intervention database (finite, human-verified).  
- `A = g(V, flags, state, M)` : chosen deterministic action (template or escalation).

### 3.2 Architectural constraint (hard)
`f` is **forbidden** to output natural language or generative tokens; its only allowed output is `V` and a small set of binary safety flags (keyword hits). This is enforced at the API and model-wrapper level (no LM text output permitted).

### 3.3 Information bottleneck (operationalized)
Goal: keep `I(X; V)` minimal while ensuring `I(V; A)` ≥ ε for utility. We **operationalize** this by:

- Low dimensionality: `dim(V) = 3`.  
- Normalization: `V ∈ [0,1]^3` with calibrated quantization/clipping.  
- Robust estimation: ensemble median / clipped outputs to bound sensitivity.

This structural constraint reduces the representational capacity to encode complex symbolic instructions (the core reason prompt injection fails in this design). The IB principle provides the theoretical motivation. :contentReference[oaicite:5]{index=5}

---

## 4. Threat model and security claims

### 4.1 Adversary capability
- Can submit arbitrary `X` texts (including crafted jailbreak prompts).  
- Cannot access server internals, model weights, or M.  
- Cannot tamper with symbolic rule files or EpMem directly.

### 4.2 Security claims (under the above model)
1. Linguistic prompts cannot encode executable instructions because `g` receives only `V` and binary flags; hence direct instruction execution is impossible. (Representational immunity.) :contentReference[oaicite:6]{index=6}  
2. Hallucination at the action level is eliminated: actions come from finite vetted `M`, not from generative sampling. :contentReference[oaicite:7]{index=7}  
3. Residual risk remains if `f` is compromised or if server access is achieved — these are out-of-scope and must be mitigated via standard infrastructure security.

---

## 5. Architecture & components (detailed)

### 5.1 Perception layer
- **Model:** Llama 3.1 (8B quantized) used only as a numeric transducer (wrapper returns floats).  
- **Outputs:** calibrated VAD + safety keyword flags (fast hashed lookup).  
- **Robustness:** ensemble or clipped MLP post-processing to bound Lipschitz sensitivity.

### 5.2 Homeostat (ACT-R inspired)
- Tracks arousal `A(t)`, uses temporal smoothing, and computes `C_limit` restricting symbolic reasoning depth (see §6).

### 5.3 Cognitive layer (SOAR)
- Rule engine with deterministic conflict resolution; episodic memory (EpMem) supports one-shot blocking by storing negative outcomes as cases and matching new `V` vectors via a distance metric. :contentReference[oaicite:8]{index=8}

### 5.4 Action layer
- Only selects entries from `intervention_db.csv` or triggers `crisis_protocol` in `safety_registry.json`. Outputs are templates (no free text from LLM).

### 5.5 Audit & logging
- Each turn writes an append-only JSON with `session_id`, `turn_id`, `timestamp`, `V`, `flags`, `matched_rules`, `epmem_case_id`, `chosen_action`, and `db_entry_id`.

---

## 6. Homeostat: dynamics & equations (detailed)

We propose a simple, interpretable dynamical model for cognitive load regulation.

State variables:
- `A(t)` : arousal estimate at time `t` (0..1).  
- `C_limit(t)` : allowed symbolic complexity (continuous or integer levels).

Dynamics:

1. **Arousal smoothing**
where `A_instant` is the V[arousal] extracted this turn and α ∈ [0,1] controls temporal smoothing (e.g., α = 0.7).

2. **Complexity control**
We map arousal to complexity:
- `C0` : baseline complexity (e.g., 10 rule-steps)  
- `γ` : sensitivity scalar (tuned in validation)

3. **Refractory / escalation thresholds**

Tuning notes:
- Keep `θ_crisis` conservative (e.g., 0.85) and require human review thresholds be lower for production deployment.
- `C_limit` influences SOAR by limiting rule firings / depth.

---
