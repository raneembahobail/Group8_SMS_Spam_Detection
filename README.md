# Group 8 — SMS Spam Detection

**Course:** Natural Language Processing (NLP)  
**Section:** FA02 · **Group:** 8 · **Academic year:** 2026–2027  
**Members:** Raneem Bahobail and Alaa Bughararah

This repository contains the integrated implementation and evaluation for the Group 8 SMS spam detection project. The main submission artifact is the Jupyter notebook in `notebooks/`.

## Project overview

The project compares two approaches for classifying SMS messages as **Ham (not spam)** or **Spam**:

1. **TF-IDF + Logistic Regression** — sparse word and bigram features.
2. **Sentence-BERT + Logistic Regression** — a frozen pretrained sentence encoder followed by a Logistic Regression classifier.

The experiment also investigates decision-threshold adjustment. Each model's adjusted threshold is selected independently on the development set by minimizing the False Positive Rate (FPR), subject to a minimum development Spam Recall of **90%**.

**Important:** The 90% recall constraint applies to the *development set used for threshold selection*. Test-set recall is a held-out result and is not forced to be 90%.

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── Group8_SMS_Spam_Detection.ipynb
├── config/
│   ├── experiment_settings.json
│   └── environment_versions.json
├── models/
│   ├── tfidf_vectorizer.joblib
│   ├── tfidf_logistic_regression.joblib
│   ├── sbert_logistic_regression.joblib
│   └── README.md
├── results/
│   ├── development_comparison.csv
│   ├── test_comparison.csv
│   ├── test_predictions.csv
│   ├── test_error_analysis.csv
│   ├── challenge_subset_results.csv
│   ├── shared_split_assignments.csv
│   ├── original_labelled_examples.csv
│   ├── tfidf_development_threshold_search.csv
│   ├── tfidf_development_eligible_thresholds.csv
│   ├── sbert_development_threshold_search.csv
│   └── sbert_development_eligible_thresholds.csv
└── docs/
    ├── CONTRIBUTIONS.md
    ├── DATASET_AND_REFERENCES.md
    └── REPRODUCIBILITY.md
```

## Dataset

The project uses the **UCI SMS Spam Collection**. The notebook downloads the dataset directly from the UCI Machine Learning Repository; the raw source archive is not duplicated in this repository.

- Original messages: **5,572**
- Unique normalized message groups: **5,157**
- Repeated normalized groups: **290**
- Conflicting duplicate-label groups: **0**

Duplicate handling is group-aware: all rows with the same normalized message key are assigned to one split so identical messages do not cross training, development, and test partitions.

### Data split

| Split | Ham | Spam | Total |
|---|---:|---:|---:|
| Train | 3117 | 476 | 3593 |
| Development | 767 | 120 | 887 |
| Test | 941 | 151 | 1092 |
| **Total** | **4825** | **747** | **5572** |

## Methods

### Method 1 — TF-IDF + Logistic Regression

- TF-IDF is fitted on training messages only.
- Word unigrams and bigrams are included (`ngram_range=(1, 2)`).
- `min_df=1`, `sublinear_tf=True`, and lowercase text handling are used.
- Logistic Regression is trained on the TF-IDF training features.
- The TF-IDF adjusted threshold is selected using development probabilities only.

### Method 2 — Sentence-BERT + Logistic Regression

- Encoder: `sentence-transformers/all-MiniLM-L6-v2`.
- The encoder is loaded at the pinned revision recorded in `config/experiment_settings.json`.
- Sentence embeddings are normalized and cached for the matching model revision, package versions, and input text.
- Logistic Regression is fitted on training embeddings.
- The SBERT adjusted threshold is selected independently using development probabilities only.

## Development-set threshold selection

| Model | Adjusted threshold | Development FPR | Development Spam Recall | Accuracy | Macro F1 |
|---|---:|---:|---:|---:|---:|
| TF-IDF | 0.286596 | 0.13% | 93.33% | 98.99% | 97.78% |
| Sentence-BERT | 0.591009 | 0.00% | 90.00% | 98.65% | 96.98% |

Both adjusted thresholds meet the pre-set minimum development Spam Recall of 90%.

## Held-out test results

The table below is exported from `results/test_comparison.csv`.

| Model | Condition | Threshold | FPR | Spam Recall | Accuracy | Macro F1 | FP | FN |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| TF-IDF | Default | 0.500000 | 0.00% | 65.56% | 95.24% | 88.26% | 0 | 52 |
| TF-IDF | Adjusted | 0.286596 | 0.64% | 90.73% | 98.17% | 96.07% | 6 | 14 |
| Sentence-BERT | Default | 0.500000 | 0.11% | 86.75% | 98.08% | 95.74% | 1 | 20 |
| Sentence-BERT | Adjusted | 0.591009 | 0.00% | 82.78% | 97.62% | 94.61% | 0 | 26 |

### Interpretation

- Under the default 0.5 threshold, Sentence-BERT has higher Spam Recall and Macro F1 than TF-IDF on this test split.
- Under the adjusted thresholds, TF-IDF detects more Spam messages (higher Spam Recall), while Sentence-BERT produces no false positives on this test split.
- The adjusted thresholds therefore show a trade-off between missed spam messages and false alarms. Neither model dominates every metric.
- The adjusted SBERT test Spam Recall is below 90%. This does **not** mean the threshold was selected using test data or that the development constraint failed: the 90% constraint was applied only to development data.

## Challenge-subset evaluation

The confirmed challenge keywords are: `free`, `win`, `urgent`, `cash`, `offer`, and `reply`.

The challenge subset consists of legitimate (Ham) test messages containing at least one of these keywords. The exported results identify **30** matching messages.

| Model | Condition | Ham examples | False positives | Challenge FPR |
|---|---|---:|---:|---:|
| TF-IDF | Default | 30 | 0 | 0.00% |
| TF-IDF | Adjusted | 30 | 3 | 10.00% |
| Sentence-BERT | Default | 30 | 0 | 0.00% |
| Sentence-BERT | Adjusted | 30 | 0 | 0.00% |

## Adjusted-threshold error counts

| Model | Error type | Count |
|---|---|---:|
| Sentence-BERT | False Negative | 26 |
| TF-IDF | False Negative | 14 |
| TF-IDF | False Positive | 6 |

Full message-level error records, probabilities, and thresholds are in `results/test_error_analysis.csv`.

## Run the project

### Option A — Google Colab (recommended)

1. Open `notebooks/Group8_SMS_Spam_Detection.ipynb` in Google Colab.
2. Run the notebook from top to bottom (`Runtime → Run all`).
3. The setup cell installs the notebook dependencies.
4. The data cell downloads the UCI SMS Spam Collection.
5. The notebook builds the shared split, trains both models, selects thresholds on Development, evaluates on Test, and exports results.
6. In the interactive **Try it** cell, enter a message when prompted. Type `exit` to leave the interactive loop.

The Sentence-BERT model is downloaded from Hugging Face at the pinned revision specified in the notebook. An access token is not required for this public checkpoint in ordinary usage, although unauthenticated downloads may be rate-limited.

### Option B — Local Jupyter environment

Use Python 3.10+ and install the packages listed in `requirements.txt`:

```bash
python -m venv .venv
# Windows:
.venv\\Scripts\\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter notebook notebooks/Group8_SMS_Spam_Detection.ipynb
```

PyTorch is installed as a platform-specific dependency of Sentence-Transformers. The exact versions recorded during the notebook run are in `config/environment_versions.json`.

## Saved model artifacts

The `models/` folder contains the fitted TF-IDF vectorizer, TF-IDF Logistic Regression classifier, and SBERT Logistic Regression classifier from the recorded run. The pretrained Sentence-BERT encoder weights are not duplicated in the repository; the notebook retrieves the pinned public checkpoint.

These `.joblib` files should only be loaded from this trusted project repository and with compatible package versions.

## Individual contributions

- **Raneem Bahobail:** UCI data loading and validation, duplicate-aware splitting, TF-IDF feature extraction, TF-IDF Logistic Regression training, and TF-IDF threshold selection.
- **Alaa Bughararah:** Sentence-BERT encoder setup and embedding generation, Logistic Regression on sentence embeddings, SBERT threshold selection, and comparative evaluation/visualizations.

See `docs/CONTRIBUTIONS.md` for a fuller summary.

