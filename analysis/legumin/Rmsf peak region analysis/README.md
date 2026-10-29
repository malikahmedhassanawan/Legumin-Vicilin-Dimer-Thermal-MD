# Legumin dimer: RMSF peak analysis, residues 260-320

Structural analysis of the selected RMSF hotspot, **residues 260-320** (numbering per chain, chains A and B),
from the 500 ps simulations at 300 K and 400 K. RMSF is integrated with per-residue SASA, AlphaFold pLDDT,
DSSP secondary structure and H-bonds.

## Contents
| Path | What |
|---|---|
| `analyze_hotspot_260_320.py` | Reproduces every table and figure |
| `figures/fig1_stacked_260_320_{300K,400K}.png` | RMSF, SASA, pLDDT, H-bonds and DSSP strips on one residue axis |
| `figures/fig2_rmsf_300_vs_400_260_320.png` | RMSF at both temperatures, top sub-peaks labeled |
| `figures/fig3_correlations_260_320.png` | RMSF vs SASA and vs pLDDT |
| `figures/fig4_sequence_260_320.png` | Amino-acid composition and charge pattern |
| `figures/fig5_dssp_time_260_320.png` | Secondary structure of each residue over time |
| `figures/fig6_hbonds_260_320.png` | H-bonds involving the window |
| `tables/per_residue_260_320_{300K,400K}.csv` | Residue-level values for both chains |
| `tables/summary_260_320.csv` | Window vs rest-of-chain summary |
| `tables/subpeaks_260_320.csv` | Local RMSF maxima inside the window |
| `tables/sequence_composition_260_320.csv` | Amino-acid composition |

Run: `python analyze_hotspot_260_320.py --data-dir <folder with inputs> --out .`

## Methods
- **Simulations:** legumin dimer (2 x 517 residues), 500 ps at 300 K and at 400 K.
- **RMSF and SASA:** per-residue values from `gmx rmsf` and `gmx sasa -or`.
- **pLDDT:** B-factor column of the AlphaFold model `legumin_legumin.pdb`.
- **DSSP:** per-frame assignments; H/G/I = helix, E/B = strand, all other codes = coil/turn/PPII.
- **H-bonds:** per-frame donor-acceptor lists (3.5 Angstrom, 120 degree cutoffs). Atom indices are assigned to
  residues from the heavy-atom PDB plus standard hydrogen counts per residue type.

## Window sequence (260-320)
`EKQARHQRGSRQEEDEDEEKQPRHQRGSRQEEEEDEDEERQPRHQRRRGEEEEEDKKERGG`

## Results
**Residues 260-320 form an intrinsically disordered, fully solvent-exposed, charged loop that is the dominant RMSF hotspot in both chains.**

| Metric (window, chain A / B) | 300 K | 400 K | Rest of chain (300 K) |
|---|---|---|---|
| Mean RMSF (nm) | 0.32 / 0.33 | 0.67 / 0.62 | 0.10 |
| SASA (nm2 per residue) | 1.64 / 1.61 | 1.56 / 1.58 | 0.53 |
| pLDDT | 27.9 / 27.7 | - | 82 |
| Coil/turn/PPII fraction | 100% / 100% | 100% / 99.8% | - |
| H-bonds per residue per frame | 0.39 / 0.34 | 0.73 / 0.58 | ~1.8 |
| Inter-chain H-bonds | 0 | 0 | - |

- **Disorder:** very low pLDDT and no helix or strand for essentially the whole trajectory (`fig5`). The only exceptions are a brief helix near 319-320 in chain A and late transient strand at 307 and 310 in chain B at 400 K.
- **Composition:** Glu 31% (19), Arg 20% (12), Gln 13% (8), Gly 8% (5), Asp 8% (5), Lys 7% (4); net charge -8 (24 D/E vs 16 K/R). Two ~18-20-residue repeats (`HQRGSRQEE...`, near 265-281 and 283-301) make the sequence low-complexity and repeat-rich, typical of a legumin hypervariable region.
- **Independent of the dimer interface:** the region makes no inter-chain H-bonds.
- **Sparse internal H-bonding:** ~0.3-0.4 H-bonds per residue per frame at 300 K versus ~1.8 in the rest of the chain. At 400 K this roughly doubles, consistent with the loop folding back on itself and with the lower total SASA at 400 K.
- **Temperature response:** RMSF roughly doubles (x2.0-2.1) and the window stays ~3x the rest of the chain, so heat amplifies the same region.
- **Chain-specific sub-peaks:** at 300 K chain A peaks around 270-285 (D276, 0.50 nm) and chain B around 295-315 (E297, E309). At 400 K the maxima are E290 (0.99 nm) in chain A and E298/E309 (~0.81-0.83 nm) in chain B. They fall in the Glu/Asp-rich stretches (`EEEEDEDEE`, `RRGEEEEE`) between the Arg/Gln-rich motifs.
- **Exposure is uniform across the window:** every residue is already solvent-exposed, so RMSF does not track SASA (r ~ 0) and pLDDT is flat (~25-30). Residue-to-residue differences reflect local sequence, charge repulsion and chain-specific loop conformation.
