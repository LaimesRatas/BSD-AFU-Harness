# AFU Package Harness (v1)

Sis katalogas pateikia masinini AFU paketu aprasymo formata (YAML) ir harness skripta, kuris:
1) validuoja, ar paketas deklaruoja butinus laukus,
2) isveda, kuriuos Gate jis uzdaro,
3) patikrina, ar normalizacijos handshake yra aiskiai deklaruotas (lambda_trans, periodu konvencijos, etc.).

## Failai
- afu_registry.yaml - pagrindinis paketu registras
- harness.py - paprastas validavimo/ataskaitu generatorius
- COVERAGE_ATLAS.md - zmogui skirta apimties matrica

## Minimalus paketo kontraktas (YAML)
Kiekvienas paketas turi tureti:
- id, name, gates_closed
- scope (p-rezimas, kreiviu klase, ordinary/supersingular, rank 0/1)
- hypotheses (sarasas)
- outputs (ka tiksliai duoda: corank=0, valuation identity, rank=1 ir pan.)
- normalization (period/torsion/Tamagawa + lambda_trans aprasas)

## Kaip paleisti
python3 harness.py afu_registry.yaml

Pastaba: harness.py naudoja PyYAML (pip install pyyaml).


## v2 papildymas
- Prideti paketai: `M0_IMC_SUPERSINGULAR_PLUSMINUS` (supersingular ±/signed IMC) ir trys local paketai: `L_BAD_REDUCTION_LOCAL`, `C_E_MANIN_CONSTANT`, `D2_DYADIC_LOCAL`.


## v3 papildymas

- v3: supersingular IMC split into `M0_IMC_SUPERSINGULAR_AP0` vs `M0_IMC_SUPERSINGULAR_APNE0`; added `M1_RANK1_PPART_BSD` (JSW/Wan-type) for rank-1 index identification.

## Real-world variantai (papildyta v4 → v5)
- Supersingular sakos yra atskirtos pagal a_p:
  - `M0_IMC_SUPERSINGULAR_AP0` (klasikine ± teorija, a_p=0),
  - `M0_IMC_SUPERSINGULAR_APNE0` (signed / sharp-flat / multi-signed, a_p≠0).
  Tai apsaugo nuo "viena etikete dengia viska" ir privercia paketa deklaruoti tikras hipotezes.

- Rank-1 yra dvi skirtingos importo klases (nesupainioti):
  - `R1_GZ_KOLY` = Rank Bridge (r_an = r_alg + finiteness kontroliuojama per GZ/Kolyvagin/Kato rank-1 ispl.).
  - `M1_RANK1_PPART_BSD` = Rank-1 p-part Index-ID (valuacijos/indeksu identifikacija).
  Registry tai zymi kaip `M1` (o rank bridge lieka `R1`).

- p=2 (dyadic) yra opt-in:
  - `D2_DYADIC_LOCAL` laikomas "excluded by default" (dazna literaturos prielaida: p>2).
  Jei norite pilnos apimties per Z (be lokalizacijos), reikia pasirinkti konkrecius dyadinius saltinius ir tai uzfiksuoti pakete.


## Dyadic policy (p=2)
- Default scope: only odd primes.
- Opt-in: include p=2 only if `D2_DYADIC_LOCAL` is supplied and document scope-switch is ON.



## c_E / periodų mastelio module (C0 → C1 → C2)

Šis module uždaro „visible factors“ periodų pusę taip, kad IMC/Kato paketai niekada nebūtų apkaltinti paslėptu periodų pasirinkimu.

- **C0_MANIN_INTEGERITY**: visada saugus minimalus sluoksnis – padaro periodų vertimą *skaidrų ir integralų* (λ_trans išreiškiamas per sveikąjį c_E).
- **C1_MANIN_SEMISTABLE_EQUAL1**: semistable režime nuima problemą visiškai (c_E=1).
- **C2_MANIN_ADDITIVE_DIVISIBILITY**: bendru atveju duoda ribas / apribojimus v_p(c_E), dažniausiai susiedamas galimą p-dalį su additive redukcijos pirminiais.

Praktikoje: jei dar nenorite (ar negalite) teigti c_E=1, užtenka C0 + aiškiai deklaruotos λ_trans taisyklės.




## Tamagawa/torsion module (V0 → V1)

Analogas periodų moduliui C0→C1→C2, tik čia tvarkome du klasikinius BSD “visible factors” komponentus:

- **V0_TAM_TORS_TRANSLATION**: minimalus, visada saugus sluoksnis. Jis **neįrodinėja**, kad faktoriai sutampa, o tik pateikia aiškų “translation dictionary” (λ_trans(vis)) tarp išorinio paketo konvencijų ir jūsų t_BK/Gate L/DLT konvencijų.
- **V1_TAM_TORS_MATCHING**: upgrade, kuris deklaruoja/įrodo, kad tame scope **λ_trans(vis)=1**, t. y. Tamagawa ir torsion konvencijos sutampa “be korekcijų”.

Praktinė taisyklė: kol neturite V1, naudokite V0 ir aiškiai laikykite λ_trans(vis) kaip apskaitos skaliarą; taip neįmanoma apkaltinti “paslėptu” V_vis.




## Local condition convention module (L0 → L1)

This module separates *local Selmer-condition convention alignment* from visible-factor (Tamagawa/torsion) alignment.

- **L0_LOCAL_TRANSLATION:** minimal layer requiring the external package to declare its local choice
  (e.g., BK/Greenberg vs. connected/Néron) and to provide an explicit translation
  (λ_loc(ℓ), λ_trans(local)).
- **L1_LOCAL_MATCHING:** upgrade asserting λ_trans(local)=1 because the external local conditions are proven
  equivalent to the Gate L convention in the declared scope.

Why this matters: otherwise a mismatch is often incorrectly blamed on Tamagawa factors when it is actually
a local-condition convention mismatch.

