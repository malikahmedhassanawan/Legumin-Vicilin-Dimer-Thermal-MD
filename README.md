 # Comparative molecular dynamics of pea legumin and vicilin homodimers

Short MD runs of two pea seed storage proteins at 300 K and 400 K, meant as a starting point for computational work on plant-based meat alternatives.

---

## Motivation

Global protein demand is outpacing sustainable supply, and animal agriculture is a major driver of land use, water consumption and greenhouse gas emissions. Plant-based meat is one way to ease that pressure, but copying the texture of animal muscle protein networks requires knowing how the plant proteins behave structurally.

This project looks at **pea legumin** (UniProt P02857) and **pea vicilin** (UniProt Q702P1), two of the main storage proteins in *Pisum sativum*. Peas are grown widely across South Asia, including Pakistan's Punjab, where food security and agricultural sustainability are pressing concerns. How these proteins respond to heat is a first step toward rationally engineering plant-protein food matrices.

## Overview

Pipeline: **AlphaFold3 structure prediction → GROMACS molecular dynamics → trajectory analysis → comparison**

Both proteins were modeled as homodimers (AlphaFold3 predictions made on AlphaFold Server, outside the notebooks) and simulated at 300 K and 400 K. Each temperature is **one 500 ps trajectory**, run as 200 ps of production and then extended by 300 ps. Results are reported over two windows of that trajectory, 0–200 ps and 0–500 ps. The 200 ps window is simply the first part of the 500 ps run, so each protein has two independent trajectories, not four. The four "conditions" per protein are four views of those two runs.

Tracked metrics: backbone RMSD, radius of gyration (Rg), SASA, protein–protein hydrogen bonds (within and between the two chains), per-residue RMSF, and, for vicilin, DSSP secondary structure.

### The two protein setups are not identical

| Setting | Legumin | Vicilin |
|---|---|---|
| Ions | 0.15 M NaCl (1,238 ions) | 17 Na⁺ only, no added salt (about 14 mM) |
| Cubic box | 18.94 nm | 12.53 nm |
| Barostat | Parrinello-Rahman | C-rescale |
| Dispersion correction | none explicit | `DispCorr = EnerPres` |
| Trajectory output interval | 1 ps | 2 ps |
| Energy minimization | stopped at machine precision | converged to Fmax < 1000 kJ/mol/nm |

Because of these differences, every legumin-versus-vicilin comparison below is qualitative. Ionic strength alone can change electrostatic screening and compaction.

## Key findings

All findings come from single, unreplicated trajectories. Read them as observations to test, not conclusions.

### 1. Legumin at 400 K / 500 ps is the one condition that compacts

| Metric | Legumin 400 K / 500 ps | Other legumin conditions | All vicilin conditions |
|---|---|---|---|
| ΔRg (%) | **−1.09%** (compaction) | +0.96% to +1.96% (expansion) | +1.58% to +3.44% (expansion) |
| ΔSASA (%) | **−2.27%** (buried surface) | +0.02% to +1.32% (exposure) | +1.50% to +3.57% (exposure) |
| ΔH-bonds | **+3** (net gain) | −10 to −48 (net loss) | −22 to −56 (net loss) |

Δ is the mean of the last 10% of the window minus the mean of the first 10%. In this one condition Rg and SASA both drop while the protein–protein hydrogen-bond count goes up. Every other trajectory shows the reverse: Rg and SASA rise and H-bonds fall, which looks like ordinary thermal loosening. The legumin pattern is consistent with compaction, but the Rg change is small (about 1%) and comes from one run, so it needs replicates before anyone calls it a structural event. The contrast with vicilin is also confounded by the different ionic strength (see the table above). The 400 K / 200 ps and 400 K / 500 ps legumin results share the same first 200 ps, so they are not separate confirmations of each other.

Per-residue RMSF in legumin is highest in the 260–320 loop region, and chain A and chain B differ. RMSF measures fluctuation, not compaction, so these analyses do not show that this loop is what compacts. Per-chain Rg or a contact analysis would test that.

### 2. Legumin has higher whole-dimer RMSD than vicilin

In every matched condition legumin's backbone RMSD is higher, roughly 1.5–2× at 300 K and more at 400 K. Mean RMSD over 500 ps goes from 0.28 nm to 0.60 nm for legumin and from 0.18 nm to 0.31 nm for vicilin between 300 K and 400 K. In these simulations vicilin stayed closer to its starting structure. RMSD here is calculated over the whole dimer, so it says nothing specific about the dimer interface, and with different salt, box, barostat and output settings it cannot be assigned to the proteins alone.

### 3. Peak flexibility sits in different regions

The most flexible residues in legumin fall between 260 and 320 in both chains and at both temperatures. Mean RMSF there is 0.32–0.33 nm at 300 K and 0.62–0.67 nm at 400 K, about 3× the rest of the chain, with the highest peaks at D276 (300 K) and E290 (400 K). This stretch is intrinsically disordered, not just loosely packed. It stays 100% coil, has a pLDDT of about 28, and is fully solvent-exposed (SASA 1.6 vs 0.5 nm² per residue elsewhere). It forms almost no hydrogen bonds (0.3–0.4 per residue per frame vs about 1.8 elsewhere) and makes no contacts with the other chain. Its sequence is a charged repeat, 31% Glu and 20% Arg, with a net charge of −8.

In vicilin the peak sits at 150–190 and has a different character. It is a helix (150–169), a disordered Glu/Gln-rich linker (170–183, 100% coil, pLDDT 48), and a short helix (184–190) that loses structure on heating. That last helix is 99% (chain A) and 71% (chain B) helical at 300 K, and 58% and 51% at 400 K. This region is the strongest in chain A at both temperatures (Q179 at 300 K, K185 at 400 K). In chain B the 400 K maximum moves to 375–400.

Chains A and B differ in both proteins because the dimers pack asymmetrically. A helix that is buried in one chain is rigid there and exposed and mobile in the other. In vicilin 150–169, SASA is 0.86 vs 0.41 nm² and RMSF is 0.19 vs 0.09 nm (chain A vs B, 300 K). In legumin, 215–232 and 475–495 show the same pattern with the chains reversed.

Heating raises RMSF about 2× in legumin and 1.4–1.7× in vicilin, across the whole protein in both cases. Heat therefore amplifies the existing hotspots rather than creating new ones. Residue numbers are counted within a chain. The RMSF atom group differs between the two proteins' original runs (all protein atoms for vicilin, mostly backbone for legumin), so compare peak positions between proteins, not magnitudes. The `rmsf_*_allatom.xvg` and `rmsf_*_backbone.xvg` files let you check this for vicilin.

### What this might mean for processing

If the differences held up, vicilin might resist heat-driven rearrangement better, while legumin's compaction could matter for network formation. That link is speculation. 400 K in explicit water at 1 bar is a stress condition, not a model of extrusion, and 500 ps is far shorter than any processing step. Testing it would take experimental work such as DSC or circular dichroism.

### Limitations

- One trajectory per temperature, 200–500 ps, no replicates. Error bars are within-run only.
- Homodimers, not the native trimer or hexamer assemblies.
- Different ionic strength, box size, barostat, dispersion correction and output interval for the two proteins.
- Initial velocities were random (`gen_vel = yes`, `gen_seed = -1`), so reruns are not bit-identical.
- No experimental reference for either protein.
- The 200 ps and 500 ps results are nested windows of the same run.

## Repository structure

- **structures/**: AlphaFold3 output structures (.cif) and processed dimer PDBs
- **alphafold_predictions/**: pLDDT confidence and chain-coloured images, rendered in PyMOL
- **simulation_setup/**: GROMACS `.mdp` files for both proteins, in one folder per protein. Each has `em` and `ions`, plus `nvt` and `npt` for 300 K and 400 K, and 200 ps `md` files for 300 K and 400 K. The 500 ps trajectories were made by extending the 200 ps runs with `gmx convert-tpr -extend 300`, so there is no separate 500 ps `.mdp`.
- **simulations/**: raw analysis output (`.xvg`, plus DSSP `.dat` for vicilin) per protein and condition. Some `.xvg` files use ns for time and others ps, so check the axis label before plotting.
- **analysis/**: `legumin/` and `vicilin/` hold per-protein plots (RMSD, Rg, SASA, H-bonds, RMSF); `comparison/legumin_vs_vicilin/` holds head-to-head plots, including normalized Rg and SASA; `summary_table.csv` and `.md` collect all 8 conditions
- **movies/**: PyMOL trajectory renderings
- **notebooks/**: simulation and analysis notebooks (see the note below on their status)
- **docs/methods.md**: methodology, software versions, parameters

## Methods summary

| Stage | Tool | Details |
|---|---|---|
| Structure prediction | AlphaFold3 (AlphaFold Server) | Homodimer models; not part of the notebooks |
| MD engine | GROMACS 2026.3 (conda-forge, CUDA build) | Google Colab GPU, model not recorded |
| Force field | AMBER99SB-ILDN | |
| Water | SPC/E | |
| Ions | Neutralizing Na⁺; legumin additionally 0.15 M NaCl | Vicilin has no added salt |
| Runs | 300 K and 400 K, 200 ps extended to 500 ps | 2 trajectories per protein |
| Analysis | `gmx rms`, `gyrate`, `sasa`, `hbond`, `rmsf` (whole dimer and per chain), `gmx dssp` (vicilin) | H-bonds are protein–protein, within and between chains |
| Visualization | PyMOL, Matplotlib | Trajectory movies and plots |

Full parameters and command lines are in [`docs/methods.md`](https://github.com/malikahmedhassanawan/Legumin-Vicilin-Dimer-Thermal-MD/blob/main/docs/methods.md).

## Reproducing this work

The simulations ran in Google Colab on a GPU runtime, so no HPC cluster is needed. Every minimization, equilibration, production and extension step used GROMACS 2026.3 (CUDA). Later post-processing used a non-CUDA 2026.3 build, which does not affect the analysis. The GPU model was not recorded.

The notebooks in [`notebooks/`](https://github.com/malikahmedhassanawan/Legumin-Vicilin-Dimer-Thermal-MD/blob/main/notebooks) were assembled and cleaned from several working sessions. **They have not been run end to end in their current form.** Cells that differ from what was originally run are marked `[changed]` or `[new]`. The DSSP step and the 400 K vicilin movie were added afterward, because the original files came from separate sessions.

They cover:

1. System preparation, minimization and equilibration
2. Production runs and extension to 500 ps
3. Trajectory processing, analysis and within-run statistics
4. PyMOL movies

AlphaFold3 prediction is not included. Run it on AlphaFold Server and place the output in `structures/`.

## Author

**Malik Ahmed Hassan Awan**, BS (Hons) Food Science and Technology, Bahauddin Zakariya University, Multan, Pakistan
Contact: varietyhavenllc@gmail.com

*Part of ongoing work connecting computational protein science to food security in South Punjab, using locally grown legumes as a model system.*
