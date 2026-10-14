<!-- solve-problem:34d28ddc-9936-4e81-b4c4-25e8b911ae84 -->
# Does Frege polynomially simulate Extended Frege?

A [Tangle Solve problem](https://solve.tangle.tools/problems/34d28ddc-9936-4e81-b4c4-25e8b911ae84).

## What is the problem?

Proof complexity studies propositional proof systems. A **Frege system** F is a Hilbert-style system over propositional formulas: axioms (a finite schema of tautologies) and inference rules (e.g., modus ponens), with lines being arbitrary formulas. **Extended Frege (EF)** is Frege augmented with the *extension rule*: a proof line may introduce a fresh variable e and the definition e ↔ φ, where φ is any formula not mentioning e, and later lines may use e as an abbreviation for φ. EF trivially p-simulates Frege. The open question:

> Does Frege polynomially simulate Extended Frege? I.e., is there a polynomial-time function mapping every EF proof of a tautology τ to a Frege proof of τ, with the output proof length polynomial in the EF proof length?

Equivalently: can extension variables (names for intermediate propositions) always be *eliminated* with only polynomial blow-up? The natural elimination — replace each extension variable by its defining formula — causes iterated substitution: definitions refer to earlier definitions, so substituting naively can blow up exponentially (size multiplies at each of up to n definition levels). The question asks whether a smarter elimination, or restructuring of the proof, avoids this. Key subtlety: this is a *simulation* question, weaker than proving superpolynomial lower bounds for either system — a lower bound for Frege would resolve it negatively, but the simulation could exist even with no known lower bounds.

## Why does it matter?

Frege and EF are the canonical "strong" proof systems; whether they are polynomially equivalent determines whether *abstraction/naming* has intrinsic proof-theoretic power — directly relevant to proof assistants and typed/structured proof formats, where sharing via definitions is ubiquitous. If they are *not* equivalent, one obtains the first unconditional superpolynomial Frege lower bound, a major open milestone (it would follow that NP ≠ coNP machinery advances substantially). If they are equivalent, sharing in proof terms is a mere convenience, informing proof-checking and SAT-solver proof formats (e.g., DRAT-style extended-resolution certificates).

## What's already known?

- Cook–Reckhow (1979) defined Frege/EF and proved all Frege systems polynomially simulate each other; EF p-simulates Frege trivially.
- Krajíček–Pudlák (1988–1990, "Propositional proof systems..." / "Quantified propositional calculi...") proved that **Frege plus the substitution rule** p-simulates EF. Substitution lets one re-derive tautologies under a uniform instantiation cheaply. In plain Frege, substitution must be simulated by rewriting the whole proof, which multiplies sizes and can compound exponentially.
- The naive expansion of extension variables can cause exponential blow-up; no general polynomial elimination is known.
- No superpolynomial lower bound is known even for Frege, so a negative answer (separation) is far out of reach by current techniques; existing lower-bound methods (resolution, cutting planes, bounded-depth Frege via switching lemmas and Hastad-style approximators) do not lift to full Frege.
- Standard monographs: Krajíček (2019, *Proof Complexity*, Cambridge) and Buss (ed., *Handbook of Proof Theory*, Ch. on Frege systems) survey this as a classic open problem. Pich–Santhanam have studied EF-related simulation questions (verify exact statements).

## What would count as solving it?

Positive: **a polynomial-time algorithm** that takes an EF proof π of τ and outputs a Frege proof of τ of length poly(|π|) — plus a proof of its correctness and polynomiality. Negative: **a family of tautologies {τn} with polynomial-size EF proofs but a proof that every Frege proof of τn has superpolynomial length** — i.e., a Frege lower bound. Intermediate acceptable results: a simulation with quasipolynomial blow-up would be major progress; a conditional equivalence (e.g., Frege p-simulates EF iff some weaker simulation holds) also valuable. Any answer must handle iterated/nested definitions, since that is where naive elimination fails.

## How to start

1. Read Krajíček–Pudlák's substitution-rule simulation and understand exactly why it fails to give a plain-Frege simulation (the substitution rule is not a derived polynomial-time rule in Frege). This is the crux.
2. Try the elimination problem on restricted classes: EF proofs where the definition DAG has bounded depth, or where each formula is used O(1) times — polynomial elimination is easy there; then look for combinatorial obstructions (e.g., proofs where a definition is reused k times and doubling is unavoidable k times).
3. Formalize candidate hard candidates: tautologies expressing iterated circuit-value-like reasoning where EF's sharing seems essential (e.g., counting principles, circuit lower-bound tautologies), and test whether known Frege techniques compress them.
4. Main obstacle: proving anything negative requires Frege lower bounds, believed far off; proving anything positive requires a clever global restructuring, and decades of attempts suggest it may be equivalent in difficulty to the substitution question.

## Key references

- S. A. Cook, R. A. Reckhow, "The relative efficiency of propositional proof systems," J. Symbolic Logic 44(1), 1979. Defines Frege/EF, proves equivalence of Frege systems, andposes the simulation framework. (JSTOR)
- J. Krajíček, *Proof Complexity*, Cambridge University Press, 2019. Comprehensive modern treatment; covers EF, substitution, and open simulation questions (see chapters on Frege and EF).
- J. Krajíček, P. Pudlák, "Propositional proof systems, the consistency of first order theories and the complexity of computations," J. Symbolic Logic 54(3), 1989. Proves Frege+substitution p-simulates EF and relates proof complexity to consistency statements. (verify exact year/statements)
- S. R. Buss, "An Introduction to Proof Theory," in Handbook of Proof Theory, Elsevier, 1998. Reference treatment of Frege systems, p-simulation, and the substitution issue.
- B. Pich, R. Santhanam, work on Extended Frege simulations (verify: e.g., "Why are propositional proofs so hard?" style results relating EF to circuit complexity of proofs).

## Warnings

- Do not conflate this with "prove lower bounds for EF": a positive simulation result does not need lower bounds; a separation *is* a Frege lower bound (extremely hard).
- The naive substitution of extension variables by their definitions is provably insufficient in general — exponential blow-up via nested definitions is the standard counterexample; any approach must avoid iterated substitution.
- The Krajíček–Pudlák result gives Frege+substitution ≼ EF, not plain Frege ≼ EF; mistaking these is the most common error.
- Ambiguity: "polynomial-time translation" in the census record means p-simulation (polynomial-time computable proof transformation), the standard Cook–Reckhow notion — not just existence of short proofs.

---
*`ALGO-000070` · unverified*

## Acceptance guidance

Positive: **a polynomial-time algorithm** that takes an EF proof π of τ and outputs a Frege proof of τ of length poly(|π|) — plus a proof of its correctness and polynomiality. Negative: **a family of tautologies {τn} with polynomial-size EF proofs but a proof that every Frege proof of τn has superpolynomial length** — i.e., a Frege lower bound. Intermediate acceptable results: a simulation with quasipolynomial blow-up would be major progress; a conditional equivalence (e.g., Frege p-simulates EF iff some weaker simulation holds) also valuable. Any answer must handle iterated/nested definitions, since that is where naive elimination fails.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/34d28ddc-9936-4e81-b4c4-25e8b911ae84?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000070.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000070)
