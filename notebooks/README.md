# Notebook inventory and import plan

The numbered folders below are organizational folders, not new executable entry
points. Add the existing, reviewed notebooks; this starter creates no empty or
invented `.ipynb` files.

| Folder | Existing notebook stages to add |
| --- | --- |
| `00_data_preparation/` | B01 inventory/preparation; B02 and B02C corrected cohorts; relevant INbreast identity audit |
| `01_benchmark/` | B03 V2; B04A V2 preprocessing; final B04B screening |
| `02_hinton_kd/` | Fixed-budget B05B training, B05C test evaluation |
| `03_feature_kd/` | B06A audit, B06B calibrated training, B06C test evaluation |
| `04_complexity/` | B07 feature extraction/diagnostics; B07A balanced subsets; B08 DIME |
| `05_efficiency/` | B08D post-KD student efficiency |

Known existing filenames include:

- `B06A_SSID_inspired_feature_KD_audit.ipynb`
- `B06B_calibrated_feature_KD_training.ipynb`
- `B06C_feature_KD_test_evaluation.ipynb`
- `B07_Pycol_dataset_complexity.ipynb`
- `B07A_Pycol_label_aware_subsets.ipynb`
- `B08_DIME_feature_difficulty.ipynb`
- `B08D_post_KD_student_efficiency.ipynb`

The existing Hinton test filename `B05_Hinton_KD_teacher_student_V4_test.ipynb`
corresponds to the B05C stage. Keep it or document an explicit old-to-new mapping.
Select the final fixed-budget Hinton training notebook, not a superseded
early-stopping version. Other preparation/screening filenames should be imported
from the reviewed originals rather than guessed.

Keep authoritative source/engine hashes and artifact IDs intact. Editing a notebook
can change its file hash; editing embedded engine source may also break downstream
locks. Preserve the original privately and document public-release modifications.
Folder organization alone does not remove hard-coded Drive dependencies.
