# Fair Atomic Governance — Paper 3

**Paper 3 of the Agent Governance Series.**

> *Admission control determines what can happen. Fairness determines who gets to act.*  
> *Correctness is local. Fairness is global.*

---

## Paper

**Fair Atomic Governance: Allocating Decision Boundaries under Shared Resource Constraints in Multi-Agent Systems**  
Marcelo Fernandez (TraslaIA), 2026

DOI: [10.5281/zenodo.19672597](https://doi.org/10.5281/zenodo.19672597) &nbsp;·&nbsp; arXiv: under review

---

## What this is

This repository contains the LaTeX source for **Paper 3** of the Agent Governance Series — a formal theory paper that characterizes the allocation layer above the atomic decision boundary.

**The core problem:** Atomic decision boundaries (Paper 0) guarantee that every individual admission decision is correct. In a multi-agent system, however, per-agent bounded enforcement creates a global capacity K = N · k₀ that scales linearly with agent count. A single actor controlling *m* agents can capture *m · k₀* admissible actions — its entire pro-rata share — without any individual agent exceeding its local bound. The system is correct at every decision point and yet arbitrarily unfair in aggregate.

The paper introduces:
- The **allocation layer**: the formal mechanism that determines which agent's pending request is presented to the atomic boundary at each step — independent of the correctness of the boundary itself.
- Three **failure modes** consistent with full atomic correctness: Sybil amplification, temporal domination, and resource contention unfairness.
- A **fairness hierarchy** for atomic governance: share fairness, actor-level proportionality, envy-freeness, and strategy-proofness.
- Four **allocation mechanisms** (M1–M4) with proofs of which fairness properties each achieves.

**Paper 0 (DBM):** https://github.com/chelof100/decision-boundary-model  
**Paper 1 (ACP):** https://github.com/chelof100/acp-framework-en  
**Paper 2 (IML):** https://github.com/chelof100/iml-benchmark  
**Paper 4 (Compositional):** https://github.com/chelof100/compositional-governance  
**Paper 5 (RAM):** https://github.com/chelof100/reconstructive-authority-model

---

## Repository contents

```
fair-atomic-governance/
├── main.tex          # Full LaTeX source
├── references.bib    # Bibliography
├── main.pdf          # Compiled paper (27 pages)
├── README.md
├── LICENSE
└── .gitignore
```

This is a theory paper with formal experiments. The contribution is entirely formal.

---

## Formal results

### Theorem 5.1 — Sybil Amplification

Under per-agent bounded enforcement with bound k₀ and an identity-oblivious allocation function, the actor share satisfies:

```
S_{u_j} = m_j / N
```

where m_j = |A_j| is the number of agents controlled by actor u_j. Registering additional agents increases actor share linearly: ∂S_{u_j}/∂m_j = 1/N > 0. No identity-oblivious mechanism can guarantee ε-actor-level proportionality for ε < max_j |m_j/N − 1/M|.

### Theorem 5.3 — Allocation Necessity

There exists a multi-agent system in which F satisfies the atomic decision boundary property (every admitted action is atomically correct) and the trivial FCFS allocation function, such that actor-level proportionality fails with maximum deviation |S_{u_j} − 1/M| = 1/2. Atomic correctness does not imply fair allocation; an explicit allocation layer is a necessary additional component.

### Theorem 5.4 — Strategy-Proofness Impossibility

Under per-agent independent enforcement, no allocation mechanism can simultaneously achieve:
- (i) ε-actor-level proportionality, and  
- (ii) strategy-proofness against identity fragmentation.

Any actor-aware mechanism that achieves both must aggregate state across agents of the same actor, breaking per-agent independence. This is a governance-layer analogue of the Gibbard–Satterthwaite theorem.

### Mechanism comparison

| Mechanism | Share Fair | Actor Prop. | Strategy-Proof | Starvation-Free |
|---|:---:|:---:|:---:|:---:|
| M1 — Per-Agent Token Bucket (ACP baseline) | ≈ | ✗ | ✗ | ✓ |
| M2 — Round-Robin Fair Queuing | ✓ | ✗ | ✗ | ✓ |
| M3 — Actor-Aware Rate Limiting | ✓ | ✓ | ✓ | ✓ |
| M4 — WFQ (uniform weights) | ✓ | ✗ | ✗ | ✓ |
| M4 — WFQ (actor weights 1/m_j) | ≈ | ✓ | ✓ | ✓ |

### Interaction with Escalation and IML

- **Escalation queue fairness:** The Escalate outcome (Paper 0, Corollary 4.5) creates a pending-review queue that is itself a resource subject to contention. Actor-aware queuing (M3 applied to the escalation scheduler) prevents actors from dominating supervisor attention.
- **Allocation bias as behavioral drift:** Persistent allocation skew induces IML drift. An over-served agent's empirical tool distribution diverges from its admission-time baseline P_{E₀}, increasing D̂(τ, A₀) even though g(τ) = 0 throughout. Allocation fairness and IML monitoring are complementary: M3/M4 prevent false IML positives caused by over-service.

---

## The governance architecture

```
┌──────────────────────────────────────────────────────┐
│  L5 — RAM               [Paper 5, RAM]               │  When to execute under partial observability?
├──────────────────────────────────────────────────────┤
│  L3 — Allocation        [Paper 3, this repo]         │  Who gets to act?
│  Fair scheduling across agents                       │
├──────────────────────────────────────────────────────┤
│  L2 — IML               [Paper 2, iml-benchmark]    │  Has behavior drifted?
│  Behavioral drift within g⁻¹(0)                     │
├──────────────────────────────────────────────────────┤
│  L1 — ACP               [Paper 1, acp-framework-en] │  Is this action admissible?
│  Stateful per-action admission control               │
├──────────────────────────────────────────────────────┤
│  L0 — Atomic Boundary   [Paper 0, decision-boundary] │  Can guarantees be made?
│  Decision + transition as a single LTS step          │
└──────────────────────────────────────────────────────┘
```

---

## Position in the series

| Paper | Title | Repo | Status |
|---|---|---|---|
| **Paper 0** | Atomic Decision Boundaries | [decision-boundary-model](https://github.com/chelof100/decision-boundary-model) | [Zenodo](https://doi.org/10.5281/zenodo.19670649) · [arXiv:2604.17511](https://arxiv.org/abs/2604.17511) |
| **Paper 1** | Agent Control Protocol (ACP) | [acp-framework-en](https://github.com/chelof100/acp-framework-en) | [Zenodo](https://doi.org/10.5281/zenodo.19672575) · [arXiv:2603.18829](https://arxiv.org/abs/2603.18829) |
| **Paper 2** | From Admission to Invariants (IML) | [iml-benchmark](https://github.com/chelof100/iml-benchmark) | [Zenodo](https://doi.org/10.5281/zenodo.19672589) · [arXiv:2604.17517](https://arxiv.org/abs/2604.17517) |
| **Paper 3** | Fair Atomic Governance (this repo) | [fair-atomic-governance](https://github.com/chelof100/fair-atomic-governance) | [Zenodo](https://doi.org/10.5281/zenodo.19672597) · arXiv: under review |
| **Paper 4** | Irreducible Multi-Scale Governance | [compositional-governance](https://github.com/chelof100/compositional-governance) | [Zenodo](https://doi.org/10.5281/zenodo.19672608) · arXiv: under review |
| **Paper 5** | Reconstructive Authority Model (RAM) | [reconstructive-authority-model](https://github.com/chelof100/reconstructive-authority-model) | [Zenodo](https://doi.org/10.5281/zenodo.19669430) · arXiv: under review |

**Series logic:**
- Paper 0 proves *when* admissibility can be guaranteed (structural necessity of atomic boundaries).
- Paper 1 builds a protocol that satisfies that condition (ACP, with TLA+ verification).
- Paper 2 operates above the boundary to detect behavioral drift invisible to enforcement.
- Paper 3 proves that correct enforcement does not imply fair allocation, and characterizes the allocation layer (this paper).
- Paper 4 composes all layers and proves their joint necessity (irreducibility).
- Paper 5 provides the operational closure: given partial observability, determines when execution is valid at runtime (RAM).

---

## Citation

```bibtex
@misc{fernandez2026fair,
  title        = {Fair Atomic Governance: Allocating Decision Boundaries under
                 Shared Resource Constraints in Multi-Agent Systems},
  author       = {Fernandez, Marcelo},
  year         = {2026},
  doi          = {10.5281/zenodo.19672597},
  howpublished = {\url{https://doi.org/10.5281/zenodo.19672597}},
  note         = {Paper~3 of the Agent Governance Series. Zenodo. arXiv: under review.}
}
```

---

## Author

**Marcelo Fernandez** — TraslaIA — info@traslaia.com  
https://agentcontrolprotocol.xyz
