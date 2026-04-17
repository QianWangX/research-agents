# PROJECT_CONTEXT
# Shared memory for all research agents. Fill this in before calling any agent.
# Keep it updated as your project evolves — new best results, architecture
# changes, key decisions made. Agents read this file before doing any work.

---

## Research goal
<!-- What are you trying to show or build? One or two sentences. -->


---

## Model
- Architecture: <!-- e.g. ViT-B/16, custom transformer, ResNet-50 -->
- Parameters: <!-- e.g. 86M -->
- Input shape: <!-- e.g. (B, 3, 224, 224) -->
- Output shape: <!-- e.g. (B, 1000) -->

---

## Dataset
- Name / source: <!-- e.g. ImageNet-1k, custom collected -->
- Size: <!-- e.g. 1.2M train, 50k val -->
- Split: <!-- how train/val/test is divided -->
- Preprocessing: <!-- normalization, augmentation strategy -->

---

## Evaluation
- Primary metric: <!-- e.g. top-1 accuracy, BLEU, FID, perplexity -->
- Baseline to beat: <!-- e.g. ResNet-50 at 76.1% top-1 -->

---

## Current best result
- Metric value: <!-- e.g. val accuracy 73.2% -->
- Epoch / step: <!-- e.g. epoch 42, step 84000 -->
- Config: <!-- path to config file, e.g. configs/baseline.yaml -->
- Log: <!-- path to log file, e.g. execution_logs/20260401_120000.log -->

---

## Active experiment
- Goal: <!-- what this experiment is testing -->
- Branch: <!-- current git branch -->
- Status: <!-- e.g. running, paused, done -->

---

## Environment
- Framework: <!-- e.g. PyTorch 2.3, JAX 0.4 -->
- Python: <!-- e.g. 3.11 -->
- CUDA: <!-- e.g. 12.1 -->
- GPU: <!-- e.g. A100 80GB, RTX 4090 -->

---

## Key decisions
<!-- Add entries as the project evolves. Most recent first.          -->
<!-- Format: - YYYY-MM-DD: tried X, result Y, conclusion Z           -->


---

## Notes
<!-- Anything else agents should know: data quirks, known issues,    -->
<!-- constraints, things not to touch.                               -->
