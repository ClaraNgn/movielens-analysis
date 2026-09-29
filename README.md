# MovieLens 100K Collaborative Filtering Analysis

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ClaraNgn/movielens-analysis/blob/main/movielens_analysis.ipynb)

This project compares user-based and item-based mean-centered KNN collaborative filtering on MovieLens 100K. The goal is to evaluate rating prediction under a common, leakage-controlled protocol rather than claim that one method is universally better.

## Research question

Under the five predefined GroupLens train/test folds, how do user-based and item-based KNN compare on RMSE and MAE when both use cosine similarity, `k=40`, `min_k=3`, and the same regularized bias fallback?

## Main results

Results are means across the five official folds. Standard deviations describe fold-to-fold variation.

| Model | RMSE | MAE | Neighborhood coverage |
| --- | ---: | ---: | ---: |
| Bias baseline | 0.9448 ± 0.0087 | 0.7484 ± 0.0068 | 100.0% |
| User KNN | 0.9361 ± 0.0069 | 0.7312 ± 0.0058 | 99.0% |
| Item KNN | **0.9341 ± 0.0099** | **0.7308 ± 0.0083** | 99.7% |

Item KNN produced 0.21% lower mean RMSE than User KNN under this specific protocol. The difference is small and is not presented as statistically significant. Both neighborhood models improved mean RMSE over the regularized bias baseline.

## What the notebook does

1. Downloads MovieLens 100K from GroupLens and verifies its MD5 checksum.
2. Checks missing values, duplicate user–movie pairs, rating bounds, and metadata keys.
3. Describes rating imbalance, user activity, item popularity, density, and sparsity.
4. Uses the predefined `u1`–`u5` train/test folds supplied with the dataset.
5. Fits all means, biases, and similarities using each training fold only.
6. Evaluates a regularized bias baseline, User KNN, and Item KNN with RMSE and MAE.
7. Reports neighborhood coverage and falls back to the bias model when fewer than three eligible neighbors exist.

## Repository structure

```text
.
├── README.md
├── movielens_analysis.ipynb
├── requirements.txt
└── .gitignore
```

The dataset is not stored in this repository. The notebook downloads it directly from the official source at runtime.

- No movie-frequency threshold is described as data cleaning. The full catalog is retained for evaluation.
- A regularized `global mean + user bias + item bias` predictor is computed on the same folds as both KNN models.
- KNN similarities use mean-centered training ratings. Test ratings do not influence similarity, eligibility, or fallback estimates.
- The five training folds overlap, so the project does not treat fold-level outcomes as independent observations for a significance claim.
- RMSE and MAE measure explicit rating prediction. They do not directly measure top-N recommendation quality.

## Limitations

- Only one value of `k`, one `min_k`, and cosine similarity are evaluated.
- Offline ratings are subject to exposure and selection bias.
- The data were collected in 1997–1998 and may not represent modern platforms.
- Ranking metrics, diversity, novelty, fairness, and popularity bias are outside the current scope.
- The referenced platform-design study is motivation, not a theory directly tested by this dataset.

## Next steps

- Tune hyperparameters inside each training fold with nested validation.
- Add Precision@K, Recall@K, and NDCG@K using a clearly defined relevance threshold.
- Compare against matrix factorization under the same evaluation protocol.
- Report performance by user activity level and item popularity.

## Data source and terms

MovieLens 100K contains 100,000 ratings from 943 users on 1,682 movies. GroupLens permits research use subject to its conditions and states that the data may not be redistributed without separate permission. See the [official README and usage terms](https://files.grouplens.org/datasets/movielens/ml-100k-README.txt).

## References

- Harper, F. M., & Konstan, J. A. (2015). The MovieLens Datasets: History and Context. *ACM Transactions on Interactive Intelligent Systems*, 5(4), Article 19. https://doi.org/10.1145/2827872
- Mohammadi Darani, M., & Aghaie, S. (2025). Recommender systems impact on Platform's content and outcomes: the role of providers and algorithm designs. *Journal of Research in Interactive Marketing*, 19(6), 917–935. https://doi.org/10.1108/JRIM-04-2024-0198
