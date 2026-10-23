# Input data

The files below are not distributed (see "Data availability" in the main README). The notebook expects them in this folder.

## `final_dataframe_with_edema.csv`

One row per macula-centred Eyer image after the initial exclusion of previously treated eyes (439 rows). The columns used by the analysis are:

| Column | Description |
|---|---|
| `pat_id` | Patient identifier (used for the cluster bootstrap) |
| `extended_img_id_eyer` | Image file name |
| `post_treatement` | `yes` if the eye had previous DR treatment (excluded) |
| `quality` | `no` if the image was ungradable (excluded) |
| `age`, `dm_time`, `sex`, `race`, `hypertension` | Table 1 variables |
| `gt_icdr`, `gt_edema` | Adjudicated reference standard: ICDR grade (0–4) and macular edema (0/1) |
| `icdr_luis`, `edema_luis`, `icdr_lzr`, `edema_lzr` | Grades of the two reference-standard graders before adjudication |
| `DRStageNetPred_eyer` | DRStageNet continuous output |
| `LabelPred_eyer` | DRStageNet grade: output rounded to the nearest integer and limited to 0–4 |
| `icdr_eran`, `edema_eran` | Reader 1 |
| `icdr_meishar`, `edema_meishar` | Reader 2 |
| `icdr_dinah`, `edema_dinah` | Reader 3 |

A missing macular edema value is treated as absent.

## `error_review_sheet_v1.csv`

Qualitative review of every image misgraded by the model (section 9 of the notebook): `ImageID`, `abs_delta` (absolute grade difference), `artefacts` and `other_disease` (`yes`/`no`) and `which` (free-text description of the finding).

## `images/` (optional)

The retinal images, needed only for Figure 2.
