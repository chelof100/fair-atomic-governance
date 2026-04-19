# Fair Atomic Governance — Paper 3

**Paper 3 of the Agent Governance Series.**

> *Admission control determines what can happen. Fairness determines who gets to act.*
> *Correctness is local. Fairness is global.*

---

## Paper

**Fair Atomic Governance: Allocating Decision Boundaries under Shared Resource Constraints in Multi-Agent Systems**  
Marcelo Fernandez (TraslaIA), 2026

DOI: [10.5281/zenodo.19643928](https://doi.org/10.5281/zenodo.19643928) &nbsp;·&nbsp; arXiv: [TBD]

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
**arXiv (ACP):** https://arxiv.org/abs/2603.18829

---

## Repository contents

```
fair-atomic-governance/
├── main.tex          # Full LaTeX source (1 732 lines)
├── references.bib    # Bibliography (14 entries)
├── main.pdf          # Compiled paper (20 pages)
├── README.md
├── LICENSE
└── .gitignore
```

This is a theory paper. There is no code or benchmark; the contribution is entirely formal.

---

## Formal results

### Theorem 5.1 — Sybil Amplification

Under per-agent bounded enforcement with bound k₀ and an identity-oblivious allocation function, the actor share satisfies:

```
S_{u_j} = m_j / N
```

where m_j = |A_j| is the number of agents controlled by actor u_j. Registering additional agents increases actor share linearly: ∂S_{u_j}/∂m_j = 1/N > 0. No identity-oblivious mechanism can guarantee ε-actor-level proportionality for ε < max_j |m_j/N − 1/M|.

### Corollary 5.2 — Linear scaling of actor capacity

Under per-agent bounded enforcement, the adversary's share scales as `m_j/N` with the number of registered agents. The maximum deviation from fair share is `max_j |m_j/N − 1/M|`.

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

## The four-layer governance architecture

```
┌─────────────────────────────────────────────────────┐
│  Layer 4 — Allocation          [Paper 3, this repo] │  Who gets to act?
│  Fair scheduling across agents                      │
├─────────────────────────────────────────────────────┤
│  Layer 3 — IML                 [Paper 2, iml-bench] │  Has behavior drifted?
│  Behavioral drift within g⁻¹(0)                    │
├─────────────────────────────────────────────────────┤
│  Layer 2 — ACP                 [Paper 1, acp-en]    │  Is this action admissible?
│  Stateful per-action admission control              │
├─────────────────────────────────────────────────────┤
│  Layer 1 — Atomic Boundary     [Paper 0, dbm]       │  Can guarantees be made?
│  Decision + transition as a single LTS step         │
└─────────────────────────────────────────────────────┘
```

The layers are non-redundant: correctness at Layer k does not imply correctness at Layer k+1.

---

## Position in the series

| Paper | Title | Repo | Status |
|---|---|---|---|
| **Paper 0** | Atomic Decision Boundaries | [decision-boundary-model](https://github.com/chelof100/decision-boundary-model) | [Published — Zenodo](https://doi.org/10.5281/zenodo.19642166) · arXiv: TBD |
| **Paper 1** | Agent Control Protocol (ACP) | [acp-framework-en](https://github.com/chelof100/acp-framework-en) | [Published — arXiv:2603.18829](https://arxiv.org/abs/2603.18829) · [Zenodo](https://doi.org/10.5281/zenodo.19642405) |
| **Paper 2** | From Admission to Invariants (IML) | [iml-benchmark](https://github.com/chelof100/iml-benchmark) | [Published — Zenodo](https://doi.org/10.5281/zenodo.19643761) · arXiv: TBD |
| **Paper 3** | Fair Atomic Governance (this repo) | [fair-atomic-governance](https://github.com/chelof100/fair-atomic-governance) | [Published — Zenodo](https://doi.org/10.5281/zenodo.19643928) · arXiv: TBD |
| **Paper 4** | Irreducible Multi-Scale Governance | [compositional-governance](https://github.com/chelof100/compositional-governance) | [Published — Zenodo](https://doi.org/10.5281/zenodo.19643950) · arXiv: TBD |

**Series logic:**
- Paper 0 proves *when* admissibility can be guaranteed (structural necessity of atomic boundaries).
- Paper 1 builds a protocol that satisfies that condition (ACP, with TLA+ verification).
- Paper 2 operates above the boundary to detect behavioral drift invisible to enforcement.
- Paper 3 proves that correct enforcement does not imply fair allocation, and characterizes the allocation layer.

---

## Citation

```bibtex
@misc{fernandez2026fair,
  title   = {Fair Atomic Governance: Allocating Decision Boundaries under
             Shared Resource Constraints in Multi-Agent Systems},
  author  = {Fernandez, Marcelo},
  year    = {2026},
  doi     = {10.5281/zenodo.19643928},
  note    = {Zenodo: https://doi.org/10.5281/zenodo.19643928. arXiv: TBD. Paper~3 of the Agent Governance Series.}
}
```

---

## Author

**Marcelo Fernandez** — TraslaIA — info@traslaia.com  
https://agentcontrolprotocol.xyz
