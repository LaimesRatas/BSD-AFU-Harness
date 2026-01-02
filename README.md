# AFU Package Harness (v1.1)

This folder provides a machine-readable AFU package registry (YAML) and a small harness script that:
1) validates required fields per package,
2) emits TeX snippets (registry table + translation dictionary),
3) enforces that the normalization “handshake” is explicit (lambda_trans stack, period/torsion/Tamagawa/local conventions).

## Files
- `afu_registry.yaml` — the package registry
- `harness.py` — validator + report / TeX generator
- `COVERAGE_ATLAS.md` — human-facing coverage matrix

## Minimal package contract (YAML)
Each package must declare:
- `id`, `name`, `gates_closed`
- `scope` (prime regime, curve family, ordinary/supersingular, analytic rank branch)
- `hypotheses` (short, concrete assumptions)
- `imports` (bibliographic anchor in the main document)
- `doc_hook_import`, `doc_hook_use` (and optionally per-gate witness hooks)

## Notes
- Dyadic prime p=2 is **opt-in**:
  - `D2_DYADIC_LOCAL` is “excluded by default” (matching common literature assumptions p>2).
  - If you want full integral scope, include explicit dyadic sources and keep the scope-switch ON.

## Dyadic policy (p=2)
- Default scope: only odd primes.
- Opt-in: include p=2 only if `D2_DYADIC_LOCAL` is supplied and the document scope-switch is enabled.

## Visible normalization modules (V0 → V1)
The visible-factor alignment is decomposed into independent sub-modules, so that the full
`V_vis` correction is a stack of small, checkable pieces:
- Period / Manin scaling (C0 → C1 → C2)
- Tamagawa + torsion translation (V0 → V1)
- Local-condition convention (L0 → L1)

This structure prevents “hidden normalizations”: every external arithmetic package must declare
which convention it uses, and the harness records the translation dictionary explicitly.
