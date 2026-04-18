# Axiom Architecture: Verifiable Logic Handover

Deterministic validation handover for the Axiom stack. This repository packages the macOS binaries for the **Flux Compiler** and the **Tenet Symbolic Engine** together with a compact, inspectable test suite.

## Start Here

If you have 10 minutes:

1. Extract both archives.
2. Run the 9 commands in the Evaluation Protocol below.
3. Compare each output with the expected result.

The outputs are deterministic. There is no variance: either they match or they do not.

If you want to inspect the logic before running anything, open any `.flux` or `.tenet` file in a text editor. Each file explains in plain English what it models, what the correct result is, and why.

The 9 tests cover two languages across five domains:

- graph routing
- boolean logic
- financial invariants
- cooperative game theory
- mechanism design

You do not need prior knowledge of the broader Axiom architecture to evaluate this repository. The files are designed to be self-contained.

---

## Overview

This repository contains the macOS-compiled binaries for the **Flux Compiler** and the **Tenet Symbolic Engine**, developed within the broader Axiom Stack.

The immediate purpose of the repository is straightforward: verify that an operating system exists, that two programming languages built on top of it execute correctly, and that they function together as a cohesive system. These are engineering claims, not theoretical ones, and they are independently verifiable by any qualified observer.

The broader research context, a neurosymbolic AI-native operating system, is documented in the accompanying papers. This repository is the executable substrate for that larger research direction.

---

## Research Lineage

This repository is an executable systems extension of the research thesis introduced in:

**Imitation Is Not Intelligence: The Mathematical Case for Neurosymbolic Architecture**

That paper formalizes the impossibility of purely imitation-based systems reliably solving structurally disjoint reasoning spaces and proposes the **Think Test**, a benchmark requiring deterministic verification traces.

Flux and Tenet are not introduced in that paper itself. They are documented separately in the companion papers included in this repository, and this validation engine shows how those later languages operationalize the broader thesis in executable form:

- **Flux** for mathematical reasoning and invariant enforcement
- **Tenet** for symbolic game-theoretic and mechanism design verification

Together, the binaries, test suite, and expected deterministic outputs serve as the engineering realization of the paper's central claim:

> intelligence must be **provable, verifiable, and structurally constrained**, not merely statistically imitated.

---

## Theoretical Foundations

For the formal mathematical specifications and language definitions, see the research papers included in this repository:

- [Flux: Eliminating the ASCII Barrier in Mathematical Programming](./research_papers/flux.pdf)
- [Tenet: A Domain-Specific Language for Game-Theoretic Modeling](./research_papers/tenet.pdf)
- [Imitation Is Not Intelligence: The Mathematical Case for Neurosymbolic Architecture](./research_papers/Imitation_is_not_Intelligence.pdf)

The BNF grammar files in the repository root provide the language-level specifications used for formal evaluation:

- [FLUX_BNF_GRAMMAR.md](./FLUX_BNF_GRAMMAR.md)
- [TENET_BNF_GRAMMAR.md](./TENET_BNF_GRAMMAR.md)

---

## Repository Contents

- `flux-1.1.0-macos-arm64.tar.gz`: Flux binary archive
- `tenet-1.0.0-macos-arm64.tar.gz`: Tenet binary archive
- `test_cases/`: deterministic evaluation suite
- `research_papers/`: supporting theoretical material
- `FLUX_BNF_GRAMMAR.md`: Flux grammar specification
- `TENET_BNF_GRAMMAR.md`: Tenet grammar specification

---

## Setup

### 1. Extract both archives

```bash
tar -xzf flux-1.1.0-macos-arm64.tar.gz
tar -xzf tenet-1.0.0-macos-arm64.tar.gz
```

### 2. Verify Java is installed

Tenet requires Java. Check with:

```bash
java -version
```

If Java is missing, install it from:
https://www.java.com/en/download

### 3. Allow the binaries through macOS Gatekeeper

macOS Gatekeeper will likely block both binaries on first run. After the first attempt fails:

1. Open `System Settings`
2. Go to `Privacy & Security`
3. Scroll down to the blocked-app notice
4. Click `Allow Anyway` for each binary
5. Re-run the command

---

## Evaluation Protocol

Run the following commands from the repository root in order.

### 1. Valid graph routing (Flux)

```bash
./flux-1.1.0-macos-arm64/flux test_cases/valid_graph.flux
```

Expected: all nodes active, all weights positive, routing accepted.

### 2. Hallucinated graph routing (Flux)

```bash
./flux-1.1.0-macos-arm64/flux test_cases/hallucinated_graph.flux
```

Expected: offline node and negative edge weight trapped before propagation. Execution halted.

### 3. De Morgan's Law verification (Flux)

```bash
./flux-1.1.0-macos-arm64/flux test_cases/demorgans_proof.flux
```

Expected: law verified across all four truth table entries.

### 4. State invariant proof (Flux)

```bash
./flux-1.1.0-macos-arm64/flux test_cases/invariant_proof.flux
```

Expected: transfer causing negative balance trapped. Conservation of value enforced.

### 5. Algorithmic game theory: Shapley value and Cournot Nash equilibrium (Flux)

```bash
./flux-1.1.0-macos-arm64/flux test_cases/game_theory_math.flux
```

Expected: Shapley value `phi = 3` per player, efficiency axiom `3phi = 9` confirmed. Nash equilibrium `q = 30`, social optimum `q = 22.5`, Price of Anarchy `= 1.125`.

### 6. Prisoner's Dilemma (Tenet)

```bash
./tenet-1.0.0-macos-arm64/tenet test_cases/game_theory/prisoners_dilemma.tenet
```

Expected: Nash equilibrium `(Defect, Defect)` with payoffs `(1, 1)`.

### 7. Stag Hunt (Tenet)

```bash
./tenet-1.0.0-macos-arm64/tenet test_cases/game_theory/stag_hunt.tenet
```

Expected: two Nash equilibria: `(Stag, Stag)` with payoffs `(10, 10)` and `(Hare, Hare)` with payoffs `(3, 3)`.

### 8. Cournot Duopoly (Tenet)

```bash
./tenet-1.0.0-macos-arm64/tenet test_cases/game_theory/cournot_duopoly.tenet
```

Expected: Nash equilibrium `(Low, Low)` with payoffs `(900, 900)`. Low is the dominant strategy.

### 9. Mechanism design: The Price of Civil Society (Tenet)

```bash
./tenet-1.0.0-macos-arm64/tenet test_cases/civil_mechanism_design.tenet
```

Expected: phase 1 Nash `(Exploit, Exploit)` with social welfare `4`. Phase 2 Nash under Pigovian mechanism `(tau = 5)` is `(Conserve, Conserve)` with social welfare `12`. Individual rationality is confirmed because the new Nash payoff `(6)` strictly exceeds the old Nash payoff `(2)`.

---

## Context

These testing binaries simulate the same backend verification process that the bare-metal Axiom OS executes at the hardware level.

---

## Verification Summary

A successful execution of this suite confirms the following:

1. **Flux is a real, executable programming language.** The compiler accepts structurally valid programs and rejects malformed ones at the syntax level.
2. **Tenet is a real, executable constraint engine.** It evaluates logical satisfiability and issues deterministic contradiction faults when physical or mathematical bounds are violated.
3. **The hallucination trap works.** When presented with logically inconsistent inputs such as invalid node states or negative edge weights, the system rejects execution before propagation. This is structurally enforced, not probabilistic.
4. **The game-theoretic solver is functional.** Nash equilibria for standard game theory frameworks such as Prisoner's Dilemma, Stag Hunt, and Cournot Duopoly are solved correctly and match established theoretical results.
5. **Flux operates as a mathematical engine.** Shapley value, continuous Nash equilibrium, and Price of Anarchy are computed using native language primitives with no external libraries.
6. **Tenet functions as a mechanism design tool.** Beyond solving static games, Tenet can model interventions: given an unregulated equilibrium, derive the minimum sufficient mechanism, apply it, and verify that the new equilibrium satisfies both efficiency and individual rationality.

If all nine tests produce their expected outputs, the Axiom neurosymbolic architecture is functioning as described in the accompanying research papers.
