# Dataset and references

## Dataset

The project uses the UCI SMS Spam Collection:

https://archive.ics.uci.edu/dataset/228/sms+spam+collection

The notebook downloads the ZIP archive directly from the UCI dataset page and reads the `SMSSpamCollection` tab-separated file. Original labels are `ham` and `spam`; the notebook maps them to `0` and `1`, respectively.

The repository does not bundle the original raw archive separately. `results/shared_split_assignments.csv` contains row IDs, labels, message text, normalized duplicate keys, and split assignments for reproducibility and team comparison.

## Research references

[1] T. A. Almeida, J. M. G. Hidalgo, and A. Yamakami, “Contributions to the Study of SMS Spam Filtering: New Collection and Results,” *Proceedings of the 11th ACM Symposium on Document Engineering*, pp. 259–262, 2011. doi: 10.1145/2034691.2034742.

[2] N. Reimers and I. Gurevych, “Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks,” *Proceedings of the 2019 Conference on Empirical Methods in Natural Language Processing and the 9th International Joint Conference on Natural Language Processing (EMNLP-IJCNLP)*, pp. 3982–3992, 2019. doi: 10.18653/v1/D19-1410.

## Software and pretrained model

- Sentence-Transformers: https://www.sbert.net/
- Sentence-BERT model: https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2
- scikit-learn: https://scikit-learn.org/stable/