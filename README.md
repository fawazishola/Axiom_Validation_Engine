# Axiom Architecture: Verifiable Logic Handover

## Start Here

If you have 10 minutes: extract both archives, run the 9 commands in the Evaluation Protocol below, and compare each output against its expected result. The outputs are deterministic: there is no variance. Either they match or they do not.

If you want to understand what you're running before executing, open any `.flux` or `.tenet` file in a text editor. Every file is commented in plain English at the top, explaining exactly what it models, what the correct result is, and why.

The two BNF grammar files (`FLUX_BNF_GRAMMAR.md`, `TENET_BNF_GRAMMAR.md`) define the full language specifications for formal evaluation. The research papers in `research_papers/` provide the theoretical foundations.

The 9 tests cover two languages across five domains: graph routing, boolean logic, financial invariants, cooperative game theory, and mechanism design. You do not need prior knowledge of the Axiom architecture to evaluate them, the files are self-contained.

---

## Overview

This repository contains the macOS-compiled binaries for the **Flux Compiler** and the **Tenet Symbolic Engine**, as referenced in the paper *Imitation Is Not Intelligence: The Mathematical Case for Neurosymbolic Architecture*.

The immediate purpose of this evaluation is straightforward: verify that an operating system exists, that two programming languages built on top of it execute correctly, and that they function together as a cohesive system. These are engineering claims, not theoretical ones, and they are independently verifiable by any qualified observer.

The broader research context; a neurosymbolic AI-native operating system, is documented in the accompanying papers. This engineering foundation is the executable substrate for that larger research program.

---

## Research Lineage

This repository is the executable systems extension of the research thesis introduced in:

**Imitation Is Not Intelligence: The Mathematical Case for Neurosymbolic Architecture**

That paper formalizes the impossibility of purely imitation-based systems reliably solving structurally disjoint reasoning spaces and proposes the **Think Test**, a benchmark requiring deterministic verification traces.

The present repository operationalizes that thesis.

Where the paper establishes the theoretical necessity of deterministic verification, this validation engine demonstrates the concrete implementation of that principle through two real languages:

- **Flux** → mathematical reasoning and invariant enforcement
- **Tenet** → symbolic game-theoretic and mechanism design verification

Together, the binaries, test suite, and deterministic expected outputs serve as the engineering realization of the paper’s central claim:

> intelligence must be **provable, verifiable, and structurally constrained**, not merely statistically imitated.

---

## Theoretical Foundations

For the formal mathematical specifications and language definitions, please refer to the original research papers included in this repository:

- [Flux: Eliminating the ASCII Barrier in Mathematical Programming](./research_papers/flux.pdf)
- [Tenet: A Domain-Specific Language for Game-Theoretic Modeling](./research_papers/tenet.pdf)
- [Imitation Is Not Intelligence: The Mathematical Case for Neurosymbolic Architecture](./research_papers/Imitation_is_not_Intelligence.pdf)

---

## Setup

### 1. Extract both archives
```bash
tar -xzf flux-1.1.0-macos-arm64.tar.gz
tar -xzf tenet-1.0.0-macos-arm64.tar.gz

## Setup

**1. Extract both archives.**
```
tar -xzf flux-1.1.0-macos-arm64.tar.gz
tar -xzf tenet-1.0.0-macos-arm64.tar.gz
```

**2. Tenet requires Java.** Verify with `java -version`. If not installed: https://www.java.com/en/download

**3. macOS Gatekeeper will block both binaries on first run.** After the first attempt fails, go to System Settings → Privacy & Security → scroll down → click Allow Anyway for each binary. Then re-run.

---

## Evaluation Protocol

Run the following from the repository root in order.

**1. Valid graph routing (Flux)**
```
./flux-1.1.0-macos-arm64/flux test_cases/valid_graph.flux
```
Expected: All nodes active, all weights positive. Routing accepted.

**2. Hallucinated graph routing (Flux)**
```
./flux-1.1.0-macos-arm64/flux test_cases/hallucinated_graph.flux
```
Expected: Offline node and negative edge weight trapped before propagation. Execution halted.

**3. De Morgan's Law verification (Flux)**
```
./flux-1.1.0-macos-arm64/flux test_cases/demorgans_proof.flux
```
Expected: Law verified across all four truth table entries.

**4. State invariant proof (Flux)**
```
./flux-1.1.0-macos-arm64/flux test_cases/invariant_proof.flux
```
Expected: Transfer causing negative balance trapped. Conservation of value enforced.

**5. Algorithmic game theory — Shapley value + Cournot Nash equilibrium (Flux)**
```
./flux-1.1.0-macos-arm64/flux test_cases/game_theory_math.flux
```
Expected: Shapley value φ=3 per player, efficiency axiom 3φ=9 confirmed. Nash equilibrium q=30, social optimum q=22.5, Price of Anarchy=1.125.

**6. Prisoner's Dilemma (Tenet)**
```
./tenet-1.0.0-macos-arm64/tenet test_cases/game_theory/prisoners_dilemma.tenet
```
Expected: Nash Equilibrium = (Defect, Defect) with payoffs (1, 1).

**7. Stag Hunt (Tenet)**
```
./tenet-1.0.0-macos-arm64/tenet test_cases/game_theory/stag_hunt.tenet
```
Expected: Two Nash Equilibria — (Stag, Stag) with payoffs (10, 10) and (Hare, Hare) with payoffs (3, 3).

**8. Cournot Duopoly (Tenet)**
```
./tenet-1.0.0-macos-arm64/tenet test_cases/game_theory/cournot_duopoly.tenet
```
Expected: Nash Equilibrium = (Low, Low) with payoffs (900, 900). Low is the dominant strategy.

**9. Mechanism design — The Price of Civil Society (Tenet)**
```
./tenet-1.0.0-macos-arm64/tenet test_cases/civil_mechanism_design.tenet
```
Expected: Phase 1 Nash = (Exploit, Exploit) with social welfare 4. Phase 2 Nash under Pigovian mechanism (τ=5) = (Conserve, Conserve) with social welfare 12. Individual rationality confirmed: new Nash payoff (6) strictly exceeds old Nash payoff (2).

## Context
These testing binaries simulate the exact backend verification process that the bare-metal Axiom OS executes at the hardware level.

## Verification Summary

A successful execution of this test suite confirms the following:

1. **Flux is a real, executable programming language.** The compiler accepts
   structurally valid programs and rejects malformed ones at the syntax level.

2. **Tenet is a real, executable constraint engine.** It evaluates logical
   satisfiability and issues deterministic contradiction faults when physical
   or mathematical bounds are violated.

3. **The hallucination trap works.** When presented with logically inconsistent
   inputs (invalid node states, negative edge weights), the system rejects
   execution before it can propagate. This is not probabilistic — it is
   structurally enforced.

4. **The game-theoretic solver is functional.** Nash Equilibria for standard
   game theory frameworks (Prisoner's Dilemma, Stag Hunt, Cournot Duopoly)
   are solved correctly and match established theoretical results.

5. **Flux operates as a mathematical engine.** Shapley value, continuous Nash
   equilibrium, and Price of Anarchy are computed using native language
   primitives — recursive factorial, rational arithmetic, and built-in
   equation solving — with no external libraries.

6. **Tenet functions as a mechanism design tool.** Beyond solving static games,
   Tenet can model interventions: given an unregulated equilibrium, derive the
   minimum sufficient mechanism, apply it, and verify that the new equilibrium
   satisfies both efficiency and individual rationality.

If all nine test categories produce their expected outputs, the Axiom
neurosymbolic architecture is functioning as described in the accompanying
research papers.
## Research Lineage

This repository is the executable systems extension of the research thesis introduced in:

**Imitation Is Not Intelligence: The Mathematical Case for Neurosymbolic Architecture**

That paper formalizes the impossibility of purely imitation-based systems reliably solving structurally disjoint reasoning spaces and proposes the **Think Test**, a benchmark requiring deterministic verification traces.

The present repository operationalizes that thesis.

Where the paper establishes the theoretical necessity of deterministic verification, this validation engine demonstrates the concrete implementation of that principle through two real languages:

- **Flux** → mathematical reasoning and invariant enforcement
- **Tenet** → symbolic game-theoretic and mechanism design verification

Together, the binaries, test suite, and expected deterministic outputs serve as the engineering realization of the paper’s central claim:

> intelligence must be **provable, verifiable, and structurally constrained**, not merely statistically imitated.
