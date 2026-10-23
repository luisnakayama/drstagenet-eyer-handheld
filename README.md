# DRStageNet on handheld fundus images

Analysis code for:

> Nakayama LF, Zacharowicz R, Ribeiro LZ, Ramon D, Zur D, Berkowitz E, Meisel M, Malerbi FK, Regatieri CVS, Behar JA. *Artificial Intelligence vs. Human Specialists for Diabetic Retinopathy Grading: Real-World Validation with a Handheld Fundus Camera and Ground Truth Benchmarking.* PLOS ONE (under review).

The study evaluates the first version of DRStageNet ([Men et al., 2025](https://doi.org/10.1088/1361-6579/ada86a)), applied without fine-tuning, on 415 macula-centred images from 252 patients acquired with the Eyer handheld camera (Phelcom Technologies) at the diabetic retinopathy outpatient clinic of the Federal University of São Paulo (UNIFESP) between March and September 2022. The model is compared with an adjudicated reference standard and with three independent ophthalmologists on two tasks: five-class ICDR grading and referable diabetic retinopathy (ICDR grade ≥ 2 and/or macular edema).

This repository contains the analysis only. Model inference is not run here: the data file already holds DRStageNet's continuous output and derived grade for each image.

**The results are already included.** The notebook is saved with its outputs, and every table and figure it produces is in `outputs/`, so nothing needs to be run to read the code or check the results. Running the notebook again requires the per-image data, which are available on request (see [Data availability](#data-availability)). Its last section recomputes the key values reported in the manuscript; in the saved run, all 41 were reproduced (`outputs/manuscript_check.csv`).

## Contents

| Path | Description |
|---|---|
| `analysis.ipynb` | Single notebook that reproduces every number, table and figure of the manuscript and its supporting information, saved with its outputs |
| `outputs/` | Tables (CSV) and figures (300-dpi TIFF) produced by the notebook; all are aggregate results, with no per-image data |
| `data/README.md` | Description of the input files (the data themselves are not distributed) |
| `requirements.txt` | Python dependencies |

## What the notebook produces

All files are in `outputs/`.

| Manuscript item | Notebook section | Output |
|---|---|---|
| Table 1 | 2 | `Table1.csv` |
| Table 2 | 5 | `Table2.csv` |
| Table 3 | 6 | `Table3.csv` |
| Figure 1 | 8 | `Figure_1.tif` |
| Figure 2 | 10 (needs the images) | `Fig2.tif` |
| Figure 3 | 6 | `Fig3.tif` |
| S1 Fig, flow diagram | 1 | `S1_Fig_flow_diagram.tif` |
| S2 Fig, 5×5 confusion matrices | 5 | `S2_Fig_confusion_matrices_5x5.tif` |
| S1 Table, 5×5 confusion matrices | 5 | `S1_Table_confusion_matrices_5x5.csv` |
| S2 Table, per-class sensitivity and specificity | 5 | `S2_Table_per_class_sens_spec.csv` |
| S3 Table, predictive values by prevalence | 6 | `S3_Table_ppv_npv_by_prevalence.csv` |
| S4 Table, referable DR by ICDR grade alone | 6 | `S4_Table_rdr_icdr_only.csv` |
| S5 Table, paired comparisons | 7 | `S5_Table_paired_comparisons.csv` |
| S6 Table, vision-threatening DR | 6 | `S6_Table_vtdr.csv` |
| Inter-grader agreement and kappa context | 3 | `intergrader_agreement.csv`, `kappa_context.csv` |
| Error analysis | 9 | `error_summary.csv`, `error_review_summary.csv` |

Section 11 recomputes the key values reported in the manuscript and lists any that do not match (`manuscript_check.csv`).

## Statistical methods

- Confidence intervals: non-parametric cluster bootstrap with the patient as the resampling unit (2,000 replicates, percentile method, seed 42), because 163 of the 252 patients contribute both eyes. The same replicates are used for every rater, which gives the intervals of the paired differences.
- Per-class sensitivity and specificity, and the secondary analyses in S4 and S6 Tables, use image-level Wilson score intervals.
- Paired comparisons between the model and each reader: exact McNemar test on discordant pairs.
- Predictive values at community prevalences: Bayes' theorem with the observed sensitivity and specificity.
- "Unweighted kappa" is Cohen's kappa without weights; "quadratic weighted kappa" uses quadratic weights.

## Running

Only needed to reproduce the results from the data:

```bash
pip install -r requirements.txt
# place the data files in data/ (see data/README.md)
jupyter execute analysis.ipynb      # or open it and run all cells
```

Figure 2 is generated only if the retinal images are placed in `data/images/`; it is not included in `outputs/`. Tested with Python 3.13, pandas 3.0, NumPy 2.5, scikit-learn 1.9, statsmodels 0.15 and matplotlib 3.11. A full run takes a few minutes, mostly for the bootstrap.

## Data availability

The per-image data and the retinal images are not publicly available because of ethical and legal restrictions on patient privacy and the scope of the consent waiver approved by the Institutional Review Board of UNIFESP (CEP/UNIFESP 0698/2020). They are available on request to researchers who meet the criteria for access to confidential data, subject to approval by the ethics committee. The aggregated confusion matrices of the model and of each reader (S1 Table of the article) allow the point estimates to be recomputed without the per-image data.

## Contact

Luis Filipe Nakayama, Department of Ophthalmology, Federal University of São Paulo — nakayama.luis@unifesp.br
