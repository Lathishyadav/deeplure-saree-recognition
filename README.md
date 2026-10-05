# DeepLure — Saree Design Retrieval and Verification

**Current status: executed synthetic-color prototype. Real colorway design recognition is not yet validated.**

This repository contains the reviewed executed notebook, measured results recovered from its saved output, an approach note, and reproducibility instructions. No proprietary dataset or image output is included. The trained checkpoint and complete Colab environment are not embedded in an `.ipynb` file and are not available in this package.

## Observed results

The run used 165 handloom images and a 117/24/24 source split. All three models achieved 100% top-1 retrieval on 24 synthetic recolored queries. Verification F1 was 0.790698 (RGB), 0.956522 (grayscale), and 0.690909 (fine-tuned). These summary numbers are at the notebook's printed precision. Read [results/RESULTS.md](results/RESULTS.md) for the full interpretation, pair counts and efficiency.

## Implementation

ImageNet-pretrained ResNet18, 128-D normalized projection, aspect-preserving RGB padding to 224x224, ImageNet normalization, color/grayscale and mild geometric augmentation. Source/identity-balanced sampling selects positive views; triplet training uses hardest in-batch negatives with margin 0.2. Only layer4 and the head train, with BatchNorm statistics frozen. Cosine similarity ranks images and a validation-selected threshold verifies pairs. Pretrained RGB/grayscale baselines use 512-D pooled features.

## Run in Colab

1. Open `notebooks/DeepLure_Saree_Recognition.ipynb` at https://colab.research.google.com/.
2. Select a GPU if available; CPU execution is supported and was used for the recorded run.
3. Run cells in order and upload the authorized handloom ZIP. The normal_sarees ZIP supplied in this exercise is empty. The recorded run did not use the additional fabric archive.
4. The effective mode is set in the configuration cell that also defines EPOCHS and BATCH_SIZE. The earlier standalone MODE assignment is overridden; do not rely on it.
5. To reproduce the saved proxy protocol, keep MODE="proxy" in that configuration cell and supply the same handloom archive.
6. To perform real evaluation, review contact sheets and annotations first, set MODE="real" in the configuration cell, upload the completed annotation CSV, and rerun splitting, training and evaluation. Do not merely relabel existing proxy results.
7. Download the private results ZIP/checkpoint before the runtime is lost. See `docs/REMAINING_STEPS.md`.

Adding the fabric archive changes the experiment. It is optional under the brief's permission to combine/resplit datasets; including it does not create exact design labels automatically. Its Banarasi/Bandhani/Ikat/Pichwai folders are broad categories.

## Setup and reproducibility

The notebook installs missing utility packages and uses the runtime's existing torch/torchvision. The observed torch version is 2.11.0+cpu; the full installed package list must be recovered from the Colab `environment.txt`. requirements.txt lists package names, not a claimed tested version lock. Seeds are set to 42. Results may vary across hardware and library versions.

Use the 165-image handloom source, source/pixel grouping, and saved split logic to reproduce the protocol. Individual split manifests are in the Colab results ZIP; they were not embedded in this uploaded notebook. The selected fine-tuned checkpoint was epoch 2 of 5.

## Outputs

`search_saree(path, top_k=5)` returns ranked references. `verify_sarees(a,b)` returns cosine similarity, threshold and a prediction. The operational gallery contains only the 24 test reference images. Queries with absent identities still receive nearest matches; there is no validated unknown-design rejection. Cosine similarity is not a confidence percentage.

## Files

- `notebooks/DeepLure_Saree_Recognition.ipynb`: executed source; image/rich outputs removed, numeric outputs preserved.
- `results/RESULTS.md`: measured report and limitations.
- `results/finetuned_test_metrics.json`: full printed fine-tuned metrics.
- `results/baseline_comparison_printed.csv`: printed-precision baseline summary.
- `results/training_history.csv`: the five recorded epochs.
- `results/efficiency.json`: measured efficiency.
- `results/observed_run.json`: observed run details and provenance.
- `approach.txt`: exact 406-character approach note printed by this run.
- `docs/REMAINING_STEPS.md`: completion and submission checklist.

## Data use and disclosure

Data is proprietary to vendors: do not redistribute the corpus or contact sheets; delete copies after the exercise. Pretraining is torchvision ResNet18_Weights.IMAGENET1K_V1 (ImageNet). Images from the additional fabric archive were used in interactive demos but not in recorded training/test evaluation. Source code originated from the assistant-guided workflow; review, understand and disclose assistance according to the organizer's rules. No claim of independently authored work is made by this packaging step.

## Submission

Complete real-design evaluation if feasible; otherwise describe this as a prototype and preserve the limitations. Upload the contents of this folder to a reviewer-accessible repository. Add your Colab link if desired. Verify access and submit the repository URL at https://forms.gle/1fJaAFbuTS4FdhQn9 . The supplied deadline is October 5, 2026, 11:59 PM IST. No repository has been published by this package.
