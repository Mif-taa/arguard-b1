##  Nile Nexus @ ArGuard Shared Tasks 2026

### Leakage-Controlled MARBERTv2–SVM Ensemble for Arabic Harmful-Prompt Classification

**Authors:**
- Miftahul Jannat Rishta
- Sumaiya Zaman

**Venue:** Proceedings of The Fourth Arabic Natural Language Processing Conference (ArabicNLP 2026) – Volume 2: Shared Tasks

**DOI:** [10.18653/v1/2026.arabicnlp-sharedtasks.41](https://doi.org/10.18653/v1/2026.arabicnlp-sharedtasks.41/)

####  Overview

This work addresses Arabic harmful-prompt detection in the ArGuard 2026 Shared Task, specifically **Subtask B1**, which involves binary classification of Arabic prompts into **Safe** and **Unsafe** categories.

The main challenge was detecting harmful intent in Arabic prompts despite dialectal variation, implicit expressions, and context-dependent meanings.

Our main contribution is methodological: we identified evaluation leakage in an initial pipeline where the development set was reused for early stopping, threshold tuning, and ensemble optimization. To address this, we introduced a leakage-controlled evaluation protocol with separate fitting, checkpoint-selection, and tuning splits.

We further developed a hybrid classification system combining MARBERTv2 and TF-IDF-based SVM, supported by five rounds of error-driven data augmentation.

####  Architecture

- **Data Preprocessing:**
  - Arabic text normalization, including diacritic and tatweel removal.
  - Alef-form unification and alef maqsura normalization.
  - Hamza-carrier and ta marbuta normalization.
  - Duplicate ID and invalid-row removal.

- **Leakage-Controlled Data Splitting:**
  - **train_fit (11,781 rows)** — model training and fitting.
  - **train_earlystop (2,525 rows)** — checkpoint selection.
  - **train_tune (2,525 rows)** — threshold search and ensemble optimization.
  - **dev_with_label (2,405 rows)** — reserved for final evaluation.

- **Classification Models:**
  - **MARBERTv2** — fine-tuned Arabic transformer for binary safe/unsafe classification.
  - **TF-IDF + Linear SVM** — character-level n-gram baseline (2–5 grams, 50,000 maximum features).
  - **Stacking Ensemble** — combines SVM scores and MARBERTv2 unsafe-class probabilities using Logistic Regression.

- **Data Augmentation:**
  - Five rounds of targeted, manually designed augmentation.
  - Safe-topic examples targeting false positives involving religion, gender, business, creative writing, and other sensitive-sounding but harmless topics.
  - Hard-negative unsafe examples addressing indirectly phrased harmful prompts.
  - Augmentation performed without using the development or blind test sets.

- **Model Selection:**
  - Compared TF-IDF SVM, MARBERTv2, and the stacking ensemble.
  - Selected the best-performing approach using the tuning split.
  - Applied threshold search and validation-based ensemble optimization.

####  Result

**Official Blind Test Macro F1: 0.860**, securing **5th place** in ArGuard Shared Task 2026, Subtask B1.

| Metric | Score |
|---|---:|
| Accuracy | 0.861 |
| Precision | 0.878 |
| Recall | 0.862 |
| Macro F1 | **0.860** |

The selected SVM–MARBERTv2 ensemble achieved a macro F1 of **0.949 on the tuning split**.

Manual error analysis identified false positives on sensitive but non-harmful content as a major remaining challenge, highlighting the importance of distinguishing sensitive topics from actual harmful intent.

####  Code

🔗 [GitHub Repository – ArGuard B1](https://github.com/Mif-taa/arguard-b1)

####  Citation

```bibtex
@inproceedings{rishta-zaman-2026-nile,
    title = "Nile Nexus at {A}r{G}uard Shared Tasks: Leakage-Controlled {MARBERT}v2{--}{SVM} Ensemble for {A}rabic Harmful-Prompt Classification",
    author = "Rishta, Miftahul Jannat and
      Zaman, Sumaiya",
    editor = "Abdul-Mageed, Muhammad and
      Elsayed, Tamer and
      Bouamor, Houda and
      El-Beltagy, Samhaa and
      Al-Khalifa, Hend and
      Abdelali, Ahmed and
      Elmadany, AbdelRahim and
      El Mekki, Abdellah and
      Darwish, Kareem and
      Alkhereyf, Sakhar and
      Luqman, Hamzah and
      Khalifa, Salam and
      Magdy, Samar and
      Alyafeai, Zaid and
      Hamed, Injy and
      Zaghouani, Wajdi and
      Zitouni, Imed",
    booktitle = "Proceedings of The Fourth {A}rabic Natural Language Processing Conference: Shared Tasks",
    month = oct,
    year = "2026",
    address = "Budapest, Hungary",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2026.arabicnlp-sharedtasks.41/",
    pages = "332--337",
    ISBN = "979-8-89176-497-2"
}
```
