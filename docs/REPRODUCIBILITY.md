# Reproducibility notes

1. Start from a fresh Google Colab runtime or compatible local Python environment.
2. Run the notebook cells in order.
3. The notebook downloads the UCI dataset, applies the recorded preprocessing and creates a stratified group-aware split using `RANDOM_STATE = 42`.
4. Identical normalized messages are kept together across the split.
5. TF-IDF is fitted on Train only. The Sentence-BERT encoder is frozen and used to encode each split; its classifier is trained on Train only.
6. Each model's adjusted threshold is selected independently on Development only by minimizing FPR while meeting at least 90% Spam Recall. Ties are resolved by higher Spam Recall and then higher threshold.
7. Thresholds are frozen before final Test evaluation.
8. The notebook saves comparison tables, test predictions, error review, threshold-search tables, split assignments, models, settings, and library versions.

## Recorded split

| Split | Rows | Ham | Spam |
|---|---:|---:|---:|
| Train | 3,593 | 3,117 | 476 |
| Development | 887 | 767 | 120 |
| Test | 1,092 | 941 | 151 |

## Recorded thresholds

- TF-IDF adjusted threshold: `0.28659602561767433`
- Sentence-BERT adjusted threshold: `0.5910092549040262`
- Standard threshold: `0.5`

The exact settings and environment versions are stored in `config/`.