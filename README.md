# Multi-View Clickbait Detection

A comparative NLP study that investigates clickbait headline detection using three complementary perspectives:

* Lexical View (what words are used)
* Syntactic View (how the headline is structured)
* Semantic View (what the headline means)

The project evaluates each approach independently and analyzes their strengths, limitations, interpretability, and cross-dataset robustness using benchmark Webis Clickbait datasets.

---

## Motivation

Most clickbait detection systems focus only on classification performance. However, understanding *why* a headline is classified as clickbait is equally important.

This project explores clickbait detection from multiple linguistic perspectives and compares how different feature representations influence model performance and generalization.

---

## Dataset

### Webis Clickbait Corpus 2017

Used for training, validation, and in-distribution testing.

### Webis Clickbait Corpus 2016

Used for cross-dataset evaluation to assess robustness against domain shift and vocabulary changes.

---

## Dataset Instructions

The benchmark datasets used in this project are not included in the repository due to their size.

### Webis Clickbait Corpus 2017
Download from:
https://zenodo.org/records/5530410

### Webis Clickbait Corpus 2016
Download from:
https://zenodo.org/records/3251557

After downloading, place the extracted dataset files in a suitable location and update the dataset paths in the notebooks if necessary.

---

## Project Architecture

### 1. Lexical View

Captures surface-level textual patterns.

Features:

* TF-IDF Word N-grams (1–3)
* Character N-grams (3–5)
* Handcrafted clickbait indicators

  * Headline length
  * Question marks
  * Exclamation marks
  * Second-person pronouns
  * Emotional words
  * Clickbait phrase lexicon
  * Superlatives

Models:

* Logistic Regression
* Linear SVM
* SGD Classifier
* Naive Bayes

Final Model:

* Logistic Regression
* Chi-Square Feature Selection
* Balanced Class Weights
* Threshold Optimization

---

### 2. Syntactic View

Focuses on grammatical structure while ignoring vocabulary.

Features:

* POS tag counts
* Dependency relation counts
* Structural headline features
* POS-tag TF-IDF bigrams and trigrams

Example:

"You won't believe this"

↓

PRON AUX VERB DET

Models:

* Linear SVM

Variants:

1. Handcrafted syntactic features
2. POS-TF-IDF features
3. Combined syntactic model

---

### 3. Semantic View

Captures contextual meaning using transformer embeddings.

Sentence Encoders:

* all-MiniLM-L6-v2
* paraphrase-mpnet-base-v2

Models:

* SVM
* MLP
* XGBoost

Optimization:

* Class imbalance handling
* Balanced class weights
* GridSearchCV
* Kernel selection

Final Model:

* MPNet embeddings + RBF SVM

---

### 4. Feature Fusion

Combines all feature representations into a single vector space:

Fusion = Lexical + Syntactic + Semantic

The goal is to evaluate whether complementary information from multiple views improves clickbait detection performance.

---

## Results

### In-Distribution Performance (Webis 2017)

| Approach  | Accuracy | F1 Score |
| --------- | -------- | -------- |
| Lexical   | 0.852    | 0.699    |
| Syntactic | 0.800    | 0.810    |
| Semantic  | 0.850    | 0.840    |
| Fusion    | 0.804    | 0.806    |

### Cross-Dataset Performance (Webis 2016)

| Approach  | Accuracy | F1 Score |
| --------- | -------- | -------- |
| Lexical   | 0.802    | 0.466    |
| Syntactic | 0.790    | 0.770    |
| Fusion    | 0.805    | 0.820    |

---

## Key Findings

### Lexical Models

* Strong in-domain performance
* Highly interpretable
* Sensitive to vocabulary drift

### Syntactic Models

* Vocabulary-independent
* Better cross-dataset generalization
* Capture stylistic clickbait patterns effectively

### Semantic Models

* Best overall performance
* Robust to paraphrasing
* Capture contextual meaning beyond keywords

### Fusion Models

* Improve recall significantly
* Combine complementary linguistic signals
* Reduce false negatives compared to individual views

---

## Technologies Used

* Python
* Scikit-learn
* spaCy
* Sentence Transformers
* Hugging Face Transformers
* XGBoost
* NumPy
* Pandas

---

## Repository Structure

```text
.
├── lexical.ipynb
├── syntactic.ipynb
├── semantic.ipynb
├── fusion.ipynb
├── Report_Clickbait_detection.pdf
├── Project Proposal.pdf
└── README.md
```

---

## Future Work

* Fine-tune transformer models directly for clickbait classification
* Explore focal loss and advanced imbalance handling
* Incorporate article content and metadata
* Investigate attention-based interpretability methods
* Evaluate on newer clickbait datasets

---

## Authors

* Abhijit Dibbidi
* Vishnu Balla
* Marudi Praneeth Reddy
* Adwait Priyadarshan
