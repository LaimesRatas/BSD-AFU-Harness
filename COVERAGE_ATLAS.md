# AFU Coverage Atlas (v1.5)

Sitas failas apraso, kokiai kreiviu / pirminiu skaiciu / rango situaciju klasei jusu architektura siandien duoda rezultata,
atsizvelgiant i tai, kokie isoriniai AFU paketai prijungiami.

## Terminai
- Gate URC/DLT/SME/L: vidiniai vartai (irodyti dokumente).
- AFU paketai: importuojamos literaturos teoremos (Kato, IMC, Gross-Zagier, Kolyvagin ir pan.), pateikiamos kaip plug-in.
- Good p: tipinis rezimas, kai p neskiria 2N c_E ir p != 2 (tikslu reziima deklaruoja konkretus paketas).

## Greita lentele: kas uzsidaro ir kada

## Aprėptis pagal kreivės šeimą (curve family)

Žemiau pateikiama praktinė matrica: **kurie paketai dažniausiai taikomi** priklausomai nuo kreivės tipo ir lokalios redukcijos.
Tai nėra „įrodymo turinys“ – tai **navigacijos/komunikacijos** sluoksnis, kad skaitytojas matytų, kur prasideda papildomos hipotezės.

| Šeima / lokalus tipas | Tipiškas p režimas | Rank 0 (A0_w + A0_m) | Rank 1 (Rank bridge + p-part) | Pastabos / rizikos |
|---|---|---|---|---|
| **Semistable, non-CM** (good/multiplicative) | p odd, p ∤ N, ordinary | W0_KATO_R0 + M0_IMC_ORDINARY + (C1_MANIN_SEMISTABLE_EQUAL1 arba bent C0_MANIN_INTEGERITY) | R1_GZ_KOLY (+ M1_RANK1_PPART_BSD jei reikia Index-ID rank 1) | „Mainline“ scenarijus; C1 nuima periodų mastelį; C0 suteikia skaidrų λ_trans; V0/V1 analogiškai tvarko Tamagawa+torsių suderinimą |
| **Semistable, non-CM** (supersingular) | p odd, good supersingular | W0_KATO_R0 + (M0_IMC_SUPERSINGULAR_AP0 arba M0_IMC_SUPERSINGULAR_APNE0) | R1_GZ_KOLY (+ M1, jei turite rank-1 p-part paketą supersingular setting) | Reikia signed/± formalizmo; aiškiai atskiriame a_p=0 ir a_p≠0 šakas |
| **Additive reduction at ℓ|N** | bet koks p; lokaliai ℓ|N | + L_BAD_REDUCTION_LOCAL + (C2_MANIN_ADDITIVE_DIVISIBILITY arba C0_MANIN_INTEGERITY) | + L_BAD_REDUCTION_LOCAL | Additive primes dažnai vieninteliai, kur c_E gali turėti p-dalį; C2 duoda ribas v_p(c_E); V0 visada duoda aiškų Tamagawa/torsion vertimą, V1 – pilną sutapdinimą (λ_trans(vis)=1) |
| **Split multiplicative at ℓ|N** | bet koks p; lokaliai ℓ|N | + L_BAD_REDUCTION_LOCAL + (C1 arba C0 periodų sulyginimui) | + L_BAD_REDUCTION_LOCAL | c_E paprastai nekliudo semistable atveju (C1), bet C0 palieka skaidrią λ_trans taisyklę |
| **CM curves** | p odd; CM-specifinės sąlygos | (CM-specifinis W0/M0 paketas) | (CM-specifinis R1/M1 paketas) | CM atvejis dažnai turi stipresnių arba kitokių įrankių; registry paliekame atskirą šaką (future) |
| **Dyadic (p=2)** | p=2 | D2_DYADIC_LOCAL (opt-in) | D2_DYADIC_LOCAL (opt-in) | Pagal nutylėjimą **excluded**; įjungti tik su aiškiu dyadic paketu ir scope-switch dokumente |


| Sritis | Rezimas | Reikia paketu | Uzdaro | Pastabos |
|---|---|---|---|---|
| Rank 0 (cotorsion / finiteness) | p good (dazn. odd) | `W0_KATO_R0` | A0_w / F2 | Selmer cotorsion/finiteness input; p>2 daznai numanoma |
| Rank 0 (Index-ID, ordinary) | good ordinary p | `M0_IMC_ORDINARY` | A0_m | IMC/reciprocity -> valuation identity |
| Rank 0 (Index-ID, supersingular a_p=0) | supersingular p, a_p=0 | `M0_IMC_SUPERSINGULAR_AP0` | A0_m | ± teorija (Pollack/Kobayashi); Wan duoda IMC + p-part BSD (r<=1) |
| Rank 0 (Index-ID, supersingular a_p≠0) | supersingular p, a_p≠0 | `M0_IMC_SUPERSINGULAR_APNE0` | A0_m | signed / sharp-flat / multi-signed (Sprung ir pan.) |
| Rank 1 (Rank Bridge) | L'(E,1) != 0 | `R1_GZ_KOLY` | AFU-3 (R1) | r_an=r_alg + finiteness (GZ + Kolyvagin/Kato rank-1) |
| Rank 1 (p-part Index-ID) | L'(E,1) != 0 | `M1_RANK1_PPART_BSD` | AFU-2 (M1) | separate nuo R1: tai indeksu/valuaciju identifikacija (JSW/Wan tipo) |
| Bad primes p|N | p in S_AFU | `L_BAD_REDUCTION_LOCAL` | AFU-1G (Local@bad) | Tamagawa matching ir lokaliu complex'u suderinimas |
| Manin konstanta p|c_E | p in S_AFU | `C_E_MANIN_CONSTANT` | AFU-1G (Manin) | periodu/optimal vs Neron normalizaciju suderinimas |
| p=2 | dyadic | `D2_DYADIC_LOCAL` (opt-in) | AFU-1G (Local@2) | pagal nutylejima p=2 yra "excluded"; reikia konkretaus dyadinio paketo |

## Goal (v2)
- Isplesti atlasa iki coverage by curve family (semistable, additive, CM, non-CM) ir iki ordinary/supersingular saku.
- Kiekvienam langeliui prideti: paketo pavadinima, hipotezes ir citatas.


## Dyadic policy (p=2) kaip scope-switch

Kad dokumentas niekada neatrodytų kaip „pilnai integralus (per Z)“ uždarymas, kai **p=2** paliktas opt-in,
naudojame aiškų *scope-switch*:

- **Default scope:** `p odd` (t. y. p ≠ 2). Visi M0/M1/W0/R1 paketai laikomi *p>2* režimo paketais, nebent aiškiai nurodyta kitaip.
- **Opt-in dyadic scope:** `DYADIC=ON` įjungia tik lokalinį paketą `D2_DYADIC_LOCAL`, ir tik tada leidžiama teigti rezultatus „including p=2“.
- **Publishing rule:** Jei DYADIC=OFF, formuluotės turi sakyti „for all odd primes p“ arba „away from p=2“.

Rekomenduojama LaTeX'e turėti vieną boolean jungiklį, pvz. `\ifdyadic ... \else ... \fi`, ir scope eilutę įdėti į Introduction/Scope.
Harness gali sugeneruoti paruoštą LaTeX snippet'ą (žr. `emit-tex`).



## V_vis dekompozicija: periodai vs Tamagawa/torsion

Kad “visible factors” (V_vis) būtų įkandami, skaidome jį į nepriklausomus sub-modulius:

- **Periodai:** C0 → C1 → C2 (c_E ir periodų mastelis; λ_trans(period)).
- **Tamagawa + torsion:** V0 → V1 (λ_trans(vis) per Tamagawa/torsion konvencijas).

Tada bendras matomas vertimas yra:
λ_trans(total) = λ_trans(period) · λ_trans(vis),
o “pilnas sutapdinimas” reiškia λ_trans(total)=1 atitinkamame scope.



## Local conditions (L0 → L1) separate from Tamagawa

In the literature, discrepancies at bad primes are sometimes attributed to Tamagawa factors even when the real source
is a *local Selmer-condition convention mismatch* (e.g., Bloch–Kato/Greenberg vs. a connected/Néron-component condition).
To prevent this conflation, we isolate local-condition alignment into an explicit module:

- **L0_LOCAL_TRANSLATION:** records explicit local translation scalars λ_loc(ℓ) and the product λ_trans(local).
- **L1_LOCAL_MATCHING:** upgrade stating λ_trans(local)=1 in a declared scope (i.e., conventions match Gate L).

Practical rule: decompose the total translation as
λ_trans(total)=λ_trans(period)·λ_trans(vis)·λ_trans(local),
and keep these layers logically independent in both claims and scope.

