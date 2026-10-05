# Remaining work

## Scientific completion

1. The notebook is functional, but its saved run is proxy-only. The standalone MODE="real" cell is immediately overwritten by MODE="proxy" in the configuration cell.
2. Label true same-design images and colorways in annotations.csv. Keep crops/edits from one source photograph grouped. If genuine real-colorway examples do not exist in the supplied images, state that limitation or obtain authorized additional examples; do not invent labels.
3. Set MODE="real" in the configuration cell that also defines EPOCHS/BATCH_SIZE. Remove the redundant standalone mode assignment in a new experimental copy.
4. Rerun from split creation through training and evaluation. Confirm the printed mode is real, gallery/query photographs are independent, and real cross-color query count is nonzero. The minimum six eligible designs is an execution gate, not sufficient evidence for broad accuracy claims.
5. Evaluate different-design/similar-palette negatives and record errors. Report baseline comparisons honestly: fine-tuning underperformed both verification baselines in the saved proxy run.

## Recover artifacts from the existing run first

Before restarting Colab, download the private results ZIP created by the export cell. It includes the trained checkpoint, complete metrics.json, environment.txt and split CSVs. None of these runtime files is automatically embedded just because you download the notebook. The full-precision baseline metrics, exact environment and individual split manifests are missing from the present package; the report explicitly labels this.

Run in the existing Colab session:

```python
from pathlib import Path
from google.colab import files
import zipfile

run = Path('/content/deeplure/outputs/proxy')
assert run.exists(), 'Runtime outputs are missing; restore your downloaded results ZIP.'
archive = Path('/content/DeepLure_Runtime_Backup_Private.zip')
with zipfile.ZipFile(archive, 'w', zipfile.ZIP_DEFLATED) as z:
    for p in run.iterdir():
        if p.is_file() and p.name not in {'gallery_embeddings.npy', 'gallery_metadata.csv'}:
            z.write(p, p.name)
files.download(str(archive))
```

Keep this private ZIP as a backup. To update the repository report, copy numeric metrics/environment/split documentation from it, preserving the protocol labels. Do not publish the private ZIP or model weights without checking data-owner permissions.

## GitHub and submission

1. Extract DeepLure_Reviewed_Submission.zip.
2. Create a repository named deeplure-saree-recognition with reviewer access.
3. Upload the CONTENTS of deeplure-reviewed-submission, preserving notebooks/, results/ and docs/. Do not upload only the ZIP.
4. Read results/RESULTS.md, check the notebook opens, and optionally add your Colab link to README.md.
5. Ensure no source images or private output files are public.
6. Submit the repository link through the form in the brief.

The cleaned notebook preserves historical code and metrics. It strips image/rich outputs; it does not change the mode or claim to convert a proxy run into a real run. No GitHub publication has been performed.
