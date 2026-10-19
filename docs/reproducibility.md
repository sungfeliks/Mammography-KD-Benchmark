# Reproduction and release notes

## Existing notebook workflow

Keep the original `Research/datasets` and `Research/outputs/benchmark_4datasets`
Drive layout until a deliberate path-portability revision is made. Follow
`notebooks/README.md` and run stages in dependency order. The JSON examples in
`configs/` do not alter the notebooks by themselves.

Before training, verify patient-group disjointness, labels, image identity,
initialization policy, preprocessing, and the frozen split. Preserve the actual
protocol and source hashes. Train separately on each dataset; use training data
for class weights and complexity diagnostics, validation data for checkpoint
selection, and the saved threshold for test inference.

Final student budgets are 5 head epochs plus 25 fine-tuning epochs without early
stopping. Compare Hinton and feature-KD runs to their own controls. A rerun with
another seed is a new experiment, not an overwrite of the recorded seed-42 run.

## B08D dependencies

B08D requires the completed B05/B06 checkpoints and matching test-evaluation
records in Drive. The original B08D notebook verifies their hashes. It does not
train models and does not rerun classification on test images. Restoring a trained
model and profiling synthetic forward passes is not a fresh confirmatory test.

Its source catalog can embed filesystem paths and checkpoint hashes. Do not edit
locked artifacts merely to simplify publication: preserve original copies and
document any public sanitization plus an original-to-public hash mapping.

## What to add before a research release

- The actual final notebooks, with secrets and unintended output disclosures removed.
- Sanitized aggregate results with stage, dataset, model, teacher, pipeline,
  training seed, source report, and artifact/campaign hash.
- Runtime-specific dependency/environment records and data preparation instructions.
- A manifest mapping published notebooks/tables to the original audited artifacts.
- Completed code provenance, license notices, and release citation metadata.

No numerical reproduction or full pipeline execution has been performed on this
documentation-only starter.
