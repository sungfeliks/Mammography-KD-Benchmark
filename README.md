# Mammography Knowledge Distillation Benchmark

An empirical study of output-based and feature-based knowledge distillation for
compact mammography classifiers across four separately trained datasets.

**Repository setup:** this starter contains documentation and example configuration
files. The experimental notebooks and reviewed result tables must still be added.
It is not an executable distribution of the full study yet.

## Research scope

The study evaluates Hinton-style output distillation and a supervised classification
adaptation inspired by SSID-KD feature consistency. VGG16 and DenseNet121 are the
teachers. MobileNetV2 and NASNetMobile are the students.

Models are trained and evaluated separately on CBIS-DDSM, CMMD, INbreast, and MIAS.
These experiments assess within-dataset classification, not cross-dataset
generalization. No new backbone or universal KD advantage is claimed.

The main study includes:

- Architecture benchmarking and preprocessing checks.
- Pipeline-specific supervised controls and all selected teacher–student pairings.
- Hinton KD and SSID-KD-inspired feature KD.
- Training-only Pycol and DIME diagnostics in frozen ResNet50 features.
- B08D profiling of control and KD student checkpoints.

Earlier MoE experiments are outside the final KD study. If retained for thesis
history, place them in `archive/` and identify their separate protocols and results.

## Datasets and target definitions

| Dataset | Experimental source | Negative class (0) | Positive class (1) |
| --- | --- | --- | --- |
| CBIS-DDSM | Kaggle mirror: awsaf49/cbis-ddsm-breast-cancer-image-dataset | Benign | Malignant |
| CMMD | Kaggle mirror: tommyngx/cmmd2022 | Benign | Malignant |
| INbreast | Kaggle mirror: tommyngx/inbreast2012 | BI-RADS 1–2 | BI-RADS 5–6 |
| MIAS | Official miasdbv1.21 distribution | Benign abnormality | Malignant abnormality |

The MIAS experiment excludes normal images. The INbreast target is assessment-based
and is not uniformly biopsy-confirmed malignancy. Scores from these distinct tasks
should not be pooled into a single mammography performance average.

Raw images, dataset archives, image caches, source clinical records, and trained
weights are not distributed here. Obtain data from the providers and comply with
their applicable access and use terms. See [data access](docs/data_access.md).

## Repository layout

| Path | Purpose |
| --- | --- |
| `notebooks/` | Reviewed B-series notebooks, organized by experimental stage |
| `configs/` | Documented example settings; authoritative per-run protocols remain separate |
| `environment/` | Software requirements and environment records |
| `docs/` | Data access, execution order, provenance, and methodological limitations |
| `results/` | Reviewed aggregate tables and selected figures |
| `results/protocols/` | Sanitized protocol copies with mappings to original hashes |
| `data/` | Local datasets; ignored by Git except its README |
| `checkpoints/` | Local model weights; ignored by Git except its README |
| `archive/` | Optional earlier studies excluded from the main KD analysis |

See [notebook inventory](notebooks/README.md) for the expected stages and known
notebook filenames. The starter does not include the notebook files themselves.

## Execution order

1. **Prepare data:** run the reviewed B01/B02 preparation and correction notebooks.
   Verify cohort labels, group membership, image mappings, and frozen splits.
2. **Benchmark:** run B03/B04 screening and preprocessing analyses to produce the
   dataset-specific teacher checkpoints used by KD.
3. **Hinton KD:** run the fixed-budget B05B training and B05C evaluation notebooks.
4. **Feature KD:** run B06A audit, B06B training, and B06C evaluation.
5. **Complexity:** run B07/B07A Pycol and B08 DIME on training data only.
6. **Efficiency:** run B08D on the previously evaluated student checkpoints.

Each stage depends on saved artifacts from earlier stages. Training and evaluation
notebooks must be run according to their existing lock/audit mechanisms; do not
enable test evaluation for model or hyperparameter selection. See
[reproduction notes](docs/reproducibility.md).

## Core experimental settings

| Setting | Main classification study |
| --- | --- |
| Classifier training seed | 42 |
| Saved split seed | 2026 |
| Input | Whole-image 512 × 512, grayscale replicated across three channels |
| Preprocessing | Architecture-specific ImageNet preprocessing |
| Training augmentation | Horizontal flipping; matching cached teacher view |
| Student training budget | 5 head-training epochs + 25 fine-tuning epochs |
| Early stopping | Disabled in the final fixed-budget student experiments |
| Batch size / precision | 8 / float32 |
| Optimizer | Adam; learning rates 1e-3 for head training and 1e-5 for fine-tuning |
| Batch normalization | Frozen, non-fused implementation |
| Evaluation threshold | Fixed at 0.5 |
| Checkpoint selection | Validation macro-F1, then positive-class recall |
| Hinton KD | T = 2; alpha = 0.5; Bernoulli KL with weighted supervised BCE |
| Feature KD | Training-only projection; standardized feature distributions; calibrated fixed loss coefficient |

The two pipelines have different supervised control predictions. Compare each KD
run with its own pipeline-specific control. The counts of improved pairings do not
establish statistical superiority between distillation methods.

## Results summary

The reviewed single-training-seed results show:

| Pipeline | Positive-class F1 improved | Tied | Declined |
| --- | ---: | ---: | ---: |
| Hinton KD | 9 / 16 | 0 / 16 | 7 / 16 |
| SSID-KD-inspired feature KD | 6 / 16 | 1 / 16 | 9 / 16 |

These are descriptive comparisons with pipeline-specific controls, not independent
trials or significance tests.

B08D completed 48 checkpoint profiles and 96 GPU jobs on an NVIDIA L4. Deployment
parameter counts and Conv/Dense MACs were unchanged within each student architecture.

| Metric | MobileNetV2 | NASNetMobile |
| --- | ---: | ---: |
| Deployment parameters | 2,259,265 | 4,270,773 |
| Conv/Dense GMACs per image | 1.5647 | 2.9400 |
| Batch-one latency (ms) | 5.531–5.697 | 22.038–23.446 |
| Batch-eight throughput (images/s) | 216.93–227.32 | 157.62–164.41 |

Ranges cover 24 checkpoints per architecture, including controls and KD runs.
They are not confidence intervals or training-seed variability. Timing excludes
image loading, preprocessing, and input transfer. Small latency differences do not
establish systematic acceleration or formal equivalence. Training cost and energy
were not measured. Only B08D is used for the main study's efficiency summary.

Add reviewed, aggregate result files to `results/`; this starter contains no raw
reports or prediction records. Record the source report, campaign, and original
artifact hash with every imported table.

## Limitations

- Classification results use one training seed. Pycol resamples and DIME estimator
  seeds do not make the classifiers multiseed.
- Test partitions had been inspected during earlier development. They are reused
  holdouts rather than untouched confirmatory test sets.
- INbreast and MIAS test cohorts are small. Individual images can materially change
  reported metrics.
- SSID-KD was developed for semi-supervised image deraining. This project uses a
  classification adaptation inspired by its feature-consistency concept.
- Pycol/DIME diagnostics depend on frozen features and do not establish causal
  explanations for KD effectiveness. Corrected DIME outputs on small cohorts are
  not certified error bounds.
- Models have not been validated for clinical decision-making.

## Software environment

The recorded classification/profiling stack includes TensorFlow 2.20.0 and
tf-keras 2.20.1. Install these only in a compatible environment. Additional
dependencies and exact Python/CUDA/driver details must be taken from the individual
notebooks and their environment records. The starter requirements file is scoped
to the common TensorFlow stack and is not a complete study-wide lock file.

For the existing Colab notebooks, follow their dependency cells and preserve their
working Google Drive layout. The JSON examples in `configs/` are documentation
templates and are not automatically read by the original notebooks.

## Attribution and licensing

The proposed license for original project software and accompanying documentation
is [MIT](LICENSE). It permits commercial reuse and does not impose a citation
requirement. Research citation is requested through [CITATION.cff](CITATION.cff).

Datasets, pretrained weights, third-party code, and manuscript/publisher PDFs are
outside the scope of this code license. Their original terms apply independently.
See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

Before a public release, confirm the copyright holders for the code, complete the
notebook/code provenance inventory, and supply the actual software release metadata.
The paper title and publication details remain subject to revision; no acceptance,
conference venue, or DOI is claimed here.

## Main methodological references

- Hinton et al., *Distilling the Knowledge in a Neural Network*:
  https://arxiv.org/abs/1503.02531
- Yeh et al., *A Lightweight Breast Cancer Mass Classification Model Utilizing
  Simplified Swarm Optimization and Knowledge Distillation*:
  https://doi.org/10.3390/bioengineering12060640
- Cui et al., *Semi-supervised image deraining using knowledge distillation*:
  https://doi.org/10.1109/TCSVT.2022.3190516
- Pycol: https://doi.org/10.1016/j.neucom.2025.130311
- DIME: https://openreview.net/pdf?id=kvqPFy0hbF

See [references](docs/references.md) for data sources and attribution notes.
