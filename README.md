# Last.fm Music Recommendation Engine

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626.svg?style=for-the-badge&logo=Jupyter&logoColor=white)

## Executive Summary
This repository contains an end-to-end, content-based music recommendation system. The project demonstrates a complete applied data science lifecycle: from raw data ingestion and custom feature engineering to ensemble model training and the deployment of a functional Top-K recommendation algorithm.

By analyzing the semantic overlap of user-generated tags, the system predicts continuous track-to-track similarity scores and successfully surfaces highly relevant, niche music recommendations based on a given seed track.

## Architecture & Methodology

### 1. Data Engineering & Ingestion
* **Dataset:** Utilizes the heavily nested JSON files from the Last.fm subset of the Million Song Dataset.
* **Pipeline:** Engineered a custom data parsing script to recursively extract track metadata, user-generated tags, and baseline similarity scores. The pipeline is designed to be computationally flexible, utilizing an `EVALUATION_LIMIT` parameter to balance data throughput against available RAM.

### 2. Feature Engineering (Semantic Similarity)
To quantify the relationship between tracks, custom features were engineered based on the overlap of user-applied tags:
* **Jaccard Similarity:** Measures the unweighted intersection over union for tag sets between two tracks[cite: 6].
* **Cosine Similarity:** Computes the directional similarity of tag vectors, factoring in the specific application weights of overlapping tags[cite: 6].
* **Set Metrics:** Absolute counts of tag intersections, unions, and individual track tag volumes[cite: 6].

### 3. Ensemble Modeling & Evaluation
The dataset was split into training, validation, and test sets[cite: 6] to benchmark three distinct regression architectures on their ability to predict track similarity:
* **Linear Regression (Baseline):** Established a foundational performance metric[cite: 6].
* **Random Forest Regressor:** Achieved the highest training performance but demonstrated significant variance and overfitting on unseen data[cite: 6].
* **Gradient Boosting Regressor (GBM):** Emerged as the optimal architecture. By sequentially minimizing errors, the GBM achieved the highest Test Spearman correlation (~0.426) and the lowest Test MSE (~0.053), ensuring superior ranking integrity for the final recommendation output[cite: 6].

### 4. Top-K Recommendation Engine
The optimized Gradient Boosting model acts as the core inference engine for the `recomend_for_track` function[cite: 6]. Given a single seed track ID, the system dynamically generates feature vectors against a candidate pool, predicts similarity scores, and surfaces the top 10 most relevant recommendations[cite: 6].

## Repository Structure
```text
lastfm-music-recommender/
├── lastfm_recommender_system.ipynb     # Main execution notebook
└── README.md                           # Project documentation
```
(Note: The data directory is omitted from the repository structure as the datasets are massive and fetched dynamically during runtime).

## Execution & Reproducibility
Environment: Open lastfm_recommender_system.ipynb in Google Colab (Python 3).

## Data Fetching: 
The notebook is fully self-contained. The second execution cell uses wget to automatically download and unzip the required Last.fm training and testing datasets directly to the runtime.

## Execution: 
Run cells sequentially.

## Author: Carter Hand
