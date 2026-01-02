# AFU Coverage Atlas (v1.6)

This file describes, at a practical level, which **curve families / prime regimes / analytic rank branches**
are covered by the current architecture, depending on which external AFU packages are plugged in.

## Terms
- Gates URC/DLT/SME/L: internal gates (proved inside the document).
- AFU packages: imported theorems from the literature (Kato, IMC, Gross–Zagier, Kolyvagin, etc.), treated as plug-ins.
- “Good p”: the typical regime where p does not divide 2 N c_E and p ≠ 2 (the precise scope is declared by each package).

## Quick matrix: what closes what

## Coverage by curve family
Below is a navigation/communication matrix (not “proof content”): it tells the reader where additional hypotheses begin.

| Curve family / local type | Prime regime | Typical package(s) | Gate(s) closed | Notes |
|---|---:|---|---|---|
| Semistable, non-CM, ordinary at p | good ordinary p | M0_IMC_ORDINARY + W0_KATO_R0 | AFU-2 (A0_w → A0_m) | Standard “ordinary IMC” route after cotorsion/finite Selmer input |
| Supersingular (a_p = 0) | supersingular p | M0_IMC_SUPERSINGULAR_PLUSMINUS + W0_KATO_R0 | AFU-2 (A0_w → A0_m) | Signed Iwasawa theory (Pollack/Kobayashi); Wan-style results |
| Supersingular (a_p ≠ 0) | supersingular p | M0_IMC_SUPERSINGULAR_SHARPFLAT + W0_KATO_R0 | AFU-2 (A0_w → A0_m) | Sharp/flat / multi-signed frameworks (Sprung-type) |
| Additive reduction at ℓ | p ∈ S_AFU | L_BAD_REDUCTION_LOCAL | AFU-1G (local at bad primes) | Local condition conventions and Tamagawa matching |
| Manin constant issues (p | c_E) | p ∈ S_AFU | C_E_MANIN_CONSTANT | Visible/period normalization | Optimal vs Néron differential normalization alignment |
| Dyadic (p = 2) | p = 2 | D2_DYADIC_LOCAL (opt-in) | AFU-1G (local@2) | Explicit scope-switch: excluded by default, included only with a dyadic package |

## Package-level closure table (summary)

| Topic | Regime | Packages needed | Closes | Notes |
|---|---|---|---|---|
| Rank 0 (cotorsion / finiteness) | good p (usually odd) | W0_KATO_R0 | A0_w / closes F2 | “Selmer cotorsion/finiteness” input; often stated for p>2 |
| Rank 0 (Index-ID, ordinary) | ordinary good p | M0_IMC_ORDINARY | A0_m | IMC/reciprocity gives valuation/length identity |
| Rank 0 (Index-ID, supersingular a_p=0) | supersingular | M0_IMC_SUPERSINGULAR_PLUSMINUS | A0_m | signed theory (±); Wan-style IMC + p-part BSD (r≤1) |
| Rank 0 (Index-ID, supersingular a_p≠0) | supersingular | M0_IMC_SUPERSINGULAR_SHARPFLAT | A0_m | sharp/flat or multi-signed variants |
| Rank 1 (rank bridge) | L'(E,1) ≠ 0 | R1_GZ_KOLY | AFU-3 (R1) | Gross–Zagier + Kolyvagin/Kato rank-control |
| Rank 1 (p-part Index-ID) | L'(E,1) ≠ 0 | M1_RANK1_PPART_BSD | AFU-2 (rank-1 branch) | Distinct from R1: this is index/valuation identification |
| Bad primes p | N | p ∈ S_AFU | L_BAD_REDUCTION_LOCAL | AFU-1G (local@bad) | Local complex conventions / Tamagawa matching |
| p = 2 | dyadic | D2_DYADIC_LOCAL (opt-in) | AFU-1G (local@2) | Must be explicitly opted-in (scope switch) |

## Goal (v2)
Make the AFU registry fully “referee-proof”: every package should state
(i) scope, (ii) assumptions, (iii) what gate(s) it closes, (iv) where it is imported, and (v) where it is first used.
