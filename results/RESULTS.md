# Review of saved execution

Evidence: uploaded `DeepLure_Saree_Recognition.ipynb`, containing 23 executed code cells and no saved error outputs. This review does not independently rerun training or establish ground-truth label correctness.

## Data and protocol

- 165 handloom images, all readable; 165 source representatives.
- Only handloom and empty normal_sarees archives were uploaded in the execution shown. The Indian Fabric Patterns archive was not used for training/evaluation.
- Source-instance split: train 117, validation 24, test 24.
- Test gallery 24 original photographs; 24 synthetic recolored queries from those source photographs.
- Training: CPU, PyTorch 2.11.0+cpu, five epochs, checkpoint selected from epoch 2.
- No eligible real cross-colorway test queries.

## Measured test comparison

| Model | Recall@1 | Recall@5 | mAP | ROC-AUC | F1 |
|---|---:|---:|---:|---:|---:|
| Pretrained RGB | 1.000000 | 1.000000 | 1.000000 | 0.994188 | 0.790698 |
| Pretrained grayscale | 1.000000 | 1.000000 | 1.000000 | 0.999396 | 0.956522 |
| Fine-tuned | 1.000000 | 1.000000 | 1.000000 | 0.993584 | 0.690909 |

Comparison values above are copied at the printed precision of the notebook summary table. Full-precision fine-tuned values are in finetuned_test_metrics.json. Full baseline test JSON is not embedded in the uploaded notebook; obtain the runtime metrics.json for those values.

The fine-tuned model does not improve retrieval on this saturated synthetic test and has worse verification F1 than both baselines. Grayscale is strongest on this observed verification comparison. Do not switch models or thresholds using this test set and then present the same test as untouched evaluation; future selection should use validation and, after repeated development, a fresh test set.

## Fine-tuned verification

- Threshold: 0.8899458050727844 (validation selected).
- Positive pairs: 24; negative pairs: 552.
- True positives: 19; false negatives: 5.
- True negatives: 540; false positives: 12.
- F1: 0.6909090909090909.
- False acceptance rate: 0.021739130434782608.
- False rejection rate: 0.20833333333333334.

Retrieval asks which gallery image ranks first. Verification asks whether each pair clears one global threshold. Therefore 100% top-1 retrieval can coexist with rejected positive pairs and false accepts.

## Efficiency

- Parameters: 11,242,176; trainable parameters: 8,459,392.
- Embedding: 128 float32 values, 512 bytes before metadata.
- CPU forward median: 96.02840749994357 ms; p95: 304.8718403999601 ms.
- Gallery search median: 0.006705999908263038 ms, gallery size 24.
- Forward measurement excludes image decoding/preprocessing and transfer. CPU model was not recorded.
- Checkpoint reload consistency check passed in the saved run.

## Remaining scientific work

The proxy benchmark compares the same photograph before/after recoloring. It does not establish retrieval between independent photographs, real colorways or verified different designs with similar palettes. The hard-negative cell was skipped. Category-level labels cannot fill this gap. Review exact design identities, keep photograph derivatives together, create real design-disjoint splits, then run validation/test evaluation.

The search demos used images from the fabric archive against a handloom-only gallery. A query's exact design may be absent. Nearest-neighbor search always returns a match; a high ranking alone cannot establish correctness for these uploads.
