# Vicilin dimer: RMSF peak analysis, residues 150-190

Structural analysis of the principal RMSF hotspot of the vicilin dimer, **residues 150-190** (numbering per chain,
chains A and B), from the 500 ps simulations at 300 K and 400 K. Per-chain RMSF is integrated with per-residue SASA,
AlphaFold pLDDT, DSSP secondary structure and H-bonds. The window is a parameter (`--start`, `--end`).

## Contents
| Path | What |
|---|---|
| `analyze_hotspot_vicilin.py` | Reproduces every table and figure |
| `figures/fig1_stacked_150_190_{300K,400K}.png` | RMSF, SASA, pLDDT, H-bonds and DSSP strips on one residue axis |
| `figures/fig2_300_vs_400_150_190.png` | Per-residue SASA and RMSF at both temperatures |
| `figures/fig3_relationships_150_190.png` | SASA vs pLDDT, temperature response, H-bonds vs exposure |
| `figures/fig4_sequence_150_190.png` | Amino-acid composition and charge pattern |
| `figures/fig5_dssp_time_150_190.png` | Secondary structure of each residue over time |
| `figures/fig6_hbonds_150_190.png` | Dimer-level SASA and H-bonds vs time, window H-bond profile |
| `figures/fig7_rmsf_relationships_150_190.png` | RMSF vs SASA and vs pLDDT |
| `tables/per_residue_150_190_{300K,400K}.csv` | Residue-level values for both chains |
| `tables/summary_150_190.csv` | Window vs rest-of-chain summary |
| `tables/segments_150_190.csv` | RMSF, SASA and pLDDT for the three segments of the window |
| `tables/subpeaks_150_190.csv` | Local RMSF maxima inside the window |
| `tables/dimer_totals_vs_temperature.csv` | Whole-dimer SASA and H-bond totals |
| `tables/sequence_composition_150_190.csv` | Amino-acid composition |

Run: `python analyze_hotspot_vicilin.py --data-dir <folder with inputs> --out .`

## Methods
- **Simulations:** vicilin dimer (2 x 415 residues), 500 ps at 300 K and at 400 K, frames every 2 ps.
- **RMSF:** `gmx rmsf -res`, computed separately for chain A and chain B at each temperature.
- **SASA:** per-residue mean and SD from `gmx sasa -or`; whole-dimer totals from `gmx sasa -o`.
- **pLDDT:** B-factor column of the AlphaFold dimer model `vicilin_dimer.pdb`.
- **DSSP:** per-frame assignments. Figures separate six classes: coil (`~`), turn (`T`), bend (`S`), PPII (`P`), helix (`H/G/I`) and strand (`E/B`); the fractions in the tables group all non-helix, non-strand codes as coil/turn/PPII.
- **H-bonds:** per-residue counts per frame (400 K) and whole-dimer totals per frame (300 K and 400 K).

## Window sequence (150-190)
`FSKNILEAAFNTNYEEIEKVLLEQQEQEPQHRRSLKDRRQE`

## Results
**The hotspot is a helix-linker-helix module: a confident helix (150-169) leads into a disordered, exposed Glu/Gln-rich linker (170-183) that ends in a marginal helix (184-190) which melts on heating.**
It is the strongest RMSF region of chain A at both temperatures (the 41-residue window starting at residue 149 is the
top-ranked window of the chain) and contains the highest single peak of chain A (Q179, 0.34 nm at 300 K; K185, 0.51 nm at 400 K)
and of chain B at 300 K (Q176, 0.32 nm). At 400 K the strongest chain B region moves to 375-400.

| Segment | Mean RMSF nm (chain A / B) 300 K -> 400 K | Structure 300 K -> 400 K | pLDDT | SASA nm2/res (A / B) |
|---|---|---|---|---|
| 150-169 | 0.19 / 0.09 -> 0.34 / 0.15 | helix 70% -> 67-68% | 87-89 | 0.86 / 0.41 |
| **170-183** | **0.23 / 0.20 -> 0.36 / 0.25** | **100% coil, both temps** | **46-48** | **1.24 / 1.22** |
| 184-190 | 0.18 / 0.17 -> 0.34 / 0.22 | helix 99% / 71% -> 58% / 51% | 38-43 | 1.14 / 1.20 |
| Rest of chain | 0.08 / 0.08 -> 0.14 / 0.13 | - | 85 | 0.45 |

- **Disordered core 170-183:** it never forms helix or strand in either chain at either temperature (`fig5`). pLDDT drops from 87 to 48 at the 169/170 boundary, SASA is ~2.7x the rest of the chain, and almost no residue is hydrogen bonded (0.05-0.10 per residue per frame at 400 K versus a dimer average of 0.36). The sequence `LEQQEQEPQHRRS` is Glu/Gln-rich with a proline. It holds the chain-A peak Q179 and the chain-B peaks Q176 and R181 (`subpeaks`).
- **Temperature-sensitive helix 184-190 (`LKDRRQE`):** the only part of the window whose structure changes with heating. Despite pLDDT ~40 it is a helix at 300 K (99% in chain A, 71% in chain B) and drops to 58% and 51% at 400 K. K185 becomes the highest RMSF peak of chain A at 400 K (0.51 nm).
- **Chain-specific helix 150-169:** the helix is equally structured in both chains (70% helix), but SASA is 0.86 nm2 per residue in chain A versus 0.41 in chain B. This is matched by RMSF: chain A 0.19 nm (300 K) and 0.34 nm (400 K) versus chain B 0.09 and 0.15 nm. The helix is exposed and mobile in chain A and buried and rigid in chain B, so the dimer is asymmetric at this site.
- **RMSF tracks exposure inside the window:** r(RMSF, SASA) = 0.64 and 0.83 at 300 K (chain A and B) and 0.63 and 0.80 at 400 K (`fig7`).
- **Temperature response:** window RMSF rises 1.7x in chain A and 1.4x in chain B, comparable to the rest of the chains (1.7x and 1.6x). Heat scales the fluctuations of the whole protein; what is specific to the window is its large baseline and the unraveling of helix 184-190.
- **Composition:** Glu 20% (8), Gln 12% (5), Arg 10% (4), Leu 10% (4), Lys 7% (3), Asn 7% (3); net charge -2.
- **Dimer level:** whole-dimer SASA is 416.6 +/- 2.6 nm2 at 300 K and 418.4 +/- 4.0 nm2 at 400 K, so the dimer does not compact on heating. Total H-bonds fall from 167.5 +/- 9.3 to 142.5 +/- 10.2 per frame (-15%). The window is H-bond-poor relative to the rest of the chain (0.21 / 0.20 versus 0.36 per residue per frame at 400 K).
