# Saved model artifacts

- `tfidf_vectorizer.joblib`: fitted TF-IDF vectorizer.
- `tfidf_logistic_regression.joblib`: fitted TF-IDF Logistic Regression classifier.
- `sbert_logistic_regression.joblib`: fitted Logistic Regression classifier trained on Sentence-BERT embeddings.

The Sentence-BERT encoder itself is not stored here. The notebook downloads the public `sentence-transformers/all-MiniLM-L6-v2` checkpoint using the pinned revision in `config/experiment_settings.json`.

Load these files only from this trusted repository. Use a compatible Python/scikit-learn environment; see `config/environment_versions.json`.