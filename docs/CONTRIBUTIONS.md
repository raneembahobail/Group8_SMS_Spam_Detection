# Team contributions

## Raneem Bahobail

- Loaded the UCI SMS Spam Collection and performed data inspection and validation.
- Normalized message text for duplicate identification and checked for conflicting labels.
- Created the group-aware stratified Train/Development/Test split so duplicate normalized messages remain in one partition.
- Implemented TF-IDF feature extraction with unigrams and bigrams.
- Trained the TF-IDF Logistic Regression classifier.
- Selected the TF-IDF adjusted threshold using Development data only, minimizing FPR subject to the minimum development Spam Recall.
- Exported the shared split assignments and TF-IDF artifacts.

## Alaa Bughararah

- Implemented the frozen Sentence-BERT encoder workflow using `sentence-transformers/all-MiniLM-L6-v2` at a pinned revision.
- Generated normalized sentence embeddings and used a cache keyed by model revision, package versions, and ordered input texts.
- Trained Logistic Regression on training embeddings.
- Selected the SBERT adjusted threshold using Development data only under the shared selection rule.
- Contributed the comparative evaluation and visualizations.

## Shared work

- Agreed on the 90% minimum development Spam Recall rule and challenge keyword list.
- Compared both methods on the same held-out test messages.
- Reviewed adjusted-threshold errors and new-message inference behavior.
- Prepared the integrated notebook and submission evidence.

The notebook records the implementation details and execution outputs. Each member should retain dated evidence of their individual work for the course appendix.