<!-- solve-problem:a946a259-f578-45c9-abcd-ad37e7742775 -->
# Automatically discover near-optimal MWIS dynamic programs

A [Tangle Solve problem](https://solve.tangle.tools/problems/a946a259-f578-45c9-abcd-ad37e7742775).

## What is the problem?

The setting is dynamic programming (DP) expressed as **max-plus circuits**: directed acyclic circuits whose gates compute either max or + over inputs (vertex weights of a graph \(G\), plus possibly constants). Such circuits formalize the kind of DP tables used for Maximum Weight Independent Set (MWIS): gates correspond to subproblem values, wires to the combinatorial structure of how subproblems combine. A circuit is *correct* for MWIS if its output equals \(\mathrm{MWIS}(G)\) (or, per some formulations, handles all induced subgraphs / weighted inputs).

Define \(\tau(G)\) = the size of the *smallest* correct max-plus circuit for MWIS on \(G\). The open problem, posed by Tuukka Korhonen: **given \(G\), algorithmically construct (and evaluate) a correct max-plus circuit for MWIS in time \((n+\tau(G))^{O(1)}\) or \(2^{\tilde O(\tau(G))}\,\mathrm{poly}(n)\).** That is, automatically synthesize a near-optimal DP for the instance at hand — a "meta-algorithm" whose cost scales with the intrinsic DP-complexity of \(G\) rather than with any fixed width parameter (treewidth, rank-width, etc.).

Ambiguities to note: (a) the exact correctness criterion — single-output for one \(G\), or a uniform family handling subgraphs/weights; (b) whether \(\tau(G)\) counts gates only or also wires; (c) approximation vs. exact optimality of the synthesized circuit ("near-optimal" plausibly means within polynomial or \(2^{o(\tau)}\) factors). Formalize before attacking.

## Why does it matter?

Currently, near-linear-in-\(2^{\text{width}}\) MWIS algorithms must be designed by hand for each structural parameter (treewidth, rank-width/MIM-width, \(\mathbb{Q}\)-rank-width), and no single known width parameter is simultaneously near-optimal for all graph classes — Korhonen's lower bounds separate rank-based DP from what is achievable. A synthesis algorithm would automatically discover the best decomposition/DP for each instance, unifying and potentially beating hand-crafted structural algorithms. It is also a concrete testbed for AI-assisted algorithm discovery in parameterized/structural graph algorithms.

## What's already known?

- Korhonen (2021) proved **lower bounds** on max-plus circuit size for MWIS in terms of a parameter he introduces (based on the *cover-independence* number, i.e., covers of \(G\) by induced subgraphs of \(G\)'s complement components — verify the exact definition in the paper). Key consequences: rank-based DPs can be \(\omega(1)\)-factor or even exponentially far from optimal for some graphs, so treewidth/rank-width DPs do not witness \(\tau(G)\).
- Standard toolbox: rank-based DP over tree \(\mathbb{Q}\)-rank decompositions gives circuits of size \(2^{O(\mathrm{rw})}n\) (Cygan–Pilipczuk–Pilipczuk–Wojtaszczyk lineage, generalizing the rank-based approach beyond treewidth); treewidth DP gives \(3^{\mathrm{tw}}n\). These are upper bounds on \(\tau(G)\) but provably not tight in general.
- What's **missing**: any algorithm that, given \(G\), produces a circuit of size within, say, a polynomial of \(\tau(G)\), in time near-linear in \(\tau(G)\). Even approximating \(\tau(G)\) is open. No synthesis technique is known; the problem resembles program synthesis over an unusual cost model (max-plus algebra).

## What would count as solving it?

Any of the following, in decreasing strength:

1. **Full solution (positive):** An algorithm that, given \(G\) with vertex weights, outputs and evaluates a correct max-plus circuit of size \(\tau(G)^{O(1)}\) in time \((n+\tau(G))^{O(1)}\). (Exponential-in-\(\tau\) quasipolynomial variants also count as stated.)
2. **Structural characterization:** A computable graph parameter \(p(G)\) with \(\tau(G) \le p(G) \le \tau(G)^{O(1)}\) and an associated efficiently constructible decomposition — i.e., a "DP-width" that is both near-tight and tractable.
3. **Negative result:** Show synthesis is hard, e.g., approximating \(\tau(G)\) within some factor is NP-hard, or no FPT-in-\(\tau\) synthesis exists under assumptions (verify plausibility — no such result is currently known).
4. **Partial progress:** Match the lower-bound parameter of Korhonen (2021) with an upper-bound construction for restricted graph classes, closing the gap there.

Referee criterion: a proof with a precise formalization of "correct max-plus circuit for MWIS" stated up front.

## How to start

1. **Read Korhonen (2021) in full**, especially the lower-bound construction and the definition of the cover-based parameter — this defines both the adversary instances and the natural candidate upper-bound parameter. Then read the rank-based MWIS DP (e.g., via rank decompositions) to know the baseline constructions.
2. **Formalize the circuit model precisely** (gate set, input encoding, whether the circuit must work for all weightings). Check whether Korhonen's paper fixes a convention you can adopt — consistency matters for a referee.
3. **Natural approach:** search for a decomposition notion dual to the lower bound — i.e., a recursively defined split of \(G\) whose "cut complexity" matches the lower-bound parameter, then prove a DP over it achieves matching circuit size. This mirrors how rank-width was dualized against rank-based DPs.
4. **Main obstacle:** lower-bound parameters are defined via existential covers and may not be recursively decomposable; constructing an *explicit near-optimal* object from an implicit optimum is the core difficulty (analogous to computing an optimal tree decomposition, which is itself hard — check what intractability results for treewidth/rank-width computation imply here).

## Key references

- Tuukka Korhonen. *Lower Bounds on Dynamic Programming for Maximum Weight Independent Set.* arXiv:2102.06901 (2021). Defines the max-plus circuit model for MWIS, proves lower bounds on circuit size via a cover-independence parameter, and shows rank/treewidth-based DPs can be far from optimal — the paper that poses this problem. https://arxiv.org/abs/2102.06901
- (For background, not cited in the record:) standard rank-based DP literature — Cygan et al. on rank-based DP over tree decompositions, and rank-width decomposition algorithms (verify exact citations yourself; do not rely on my recollection here).

## Warnings

- The census record is terse and machine-generated ("fleet:gpt6pro" source); verify all claims against Korhonen's actual paper, especially the exact definition of \(\tau(G)\) and whether the problem is stated there verbatim.
- Do not assume treewidth/rank-width DPs are near-optimal — Korhonen's lower bounds explicitly refute this; a solution relying on them is dead on arrival.
- Pin down the correctness semantics early (single graph vs. family vs. all weightings); different choices change the problem's difficulty.
- Even the restricted question "compute \(\tau(G)\)" may already be intractable; don't conflate synthesis with optimization.

---
*`ALGO-000100` · unverified*

## Acceptance guidance

Any of the following, in decreasing strength:

1. **Full solution (positive):** An algorithm that, given \(G\) with vertex weights, outputs and evaluates a correct max-plus circuit of size \(\tau(G)^{O(1)}\) in time \((n+\tau(G))^{O(1)}\). (Exponential-in-\(\tau\) quasipolynomial variants also count as stated.)
2. **Structural characterization:** A computable graph parameter \(p(G)\) with \(\tau(G) \le p(G) \le \tau(G)^{O(1)}\) and an associated efficiently constructible decomposition — i.e., a "DP-width" that is both near-tight and tractable.
3. **Negative result:** Show synthesis is hard, e.g., approximating \(\tau(G)\) within some factor is NP-hard, or no FPT-in-\(\tau\) synthesis exists under assumptions (verify plausibility — no such result is currently known).
4. **Partial progress:** Match the lower-bound parameter of Korhonen (2021) with an upper-bound construction for restricted graph classes, closing the gap there.

Referee criterion: a proof with a precise formalization of "correct max-plus circuit for MWIS" stated up front.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/a946a259-f578-45c9-abcd-ad37e7742775?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000100.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000100)
