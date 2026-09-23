# Phase 1 baseline evaluation

Generated 2026-09-23 11:29 UTC by `python -m evaluation.run`. Model: InsightFace `buffalo_l` (RetinaFace detector + ArcFace R50), CPU. Scores are cosine similarity clamped to [0, 1].

**Operating threshold:** 0.2589. It's calibrated on FG-NET so that FAR = 1%.

## PRD §2 targets

| Target | Result | Goal | Met |
|---|---|---|---|
| Age gap: TAR on FG-NET @ FAR=1% | 85.42% | ≥ 90% | **no** |
| Pose: TAR on CFP-FP at the operating threshold | 96.29% | ≥ 85% | yes |
| Pose: FAR on CFP-FP at the operating threshold | 0.00% | ≤ 1% | yes |
| Latency per match (CPU, median / p95) | 1.20s / 1.56s | < 2s | yes |

## FG-NET (age gap, all pairs)

- Pairs: 5808 genuine, 495693 impostor
- Faces not found: 0 of 1002 images. Images with several faces: 0 (the largest face was used)
- AUC 0.9826, EER 5.89%

| Age gap (years) | Genuine pairs | TAR at operating threshold |
|---|---|---|
| 0-4 | 1655 | 97.64% |
| 5-9 | 1590 | 94.97% |
| 10-19 | 1700 | 76.35% |
| 20-29 | 575 | 66.96% |
| 30+ | 288 | 52.78% |

## CFP (pose, official 10-fold protocol)

| Protocol | 10-fold accuracy | AUC | EER | TAR @ FAR=1% (own threshold) | TAR / FAR at operating threshold |
|---|---|---|---|---|---|
| FF | 99.66% ± 0.23% | 0.9958 | 0.34% | 99.34% | 99.23% / 0.00% |
| FP | 98.56% ± 0.39% | 0.9855 | 2.07% | 97.77% | 96.29% / 0.00% |

Faces not found: 11 of 5000 frontal images and 33 of 2000 profile images.

## Caveats

- The threshold is calibrated and tested on the same FG-NET pairs. That makes the FG-NET FAR exactly 1% by construction. CFP is the independent check of how well the threshold transfers.
- A failed detection scores 0, which counts as a rejection. This is conservative for genuine pairs.
- Neither dataset has demographic labels, so this report has no per-slice bias check (PRD §8).

![ROC](roc.png)
