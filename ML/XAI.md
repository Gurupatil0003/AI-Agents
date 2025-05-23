# 🔍 Explainable AI (XAI) Techniques Comparison

This guide compares the most popular Explainable AI libraries and techniques:

- [x] SHAP
- [x] LIME
- [x] ELI5 (Permutation Importance)
- [x] PDP (Partial Dependence Plots)
- [x] ALE (Accumulated Local Effects)

---

## 📊 Comparison Table

| Feature / Tool      | **SHAP**                                                                 | **LIME**                                                          | **ELI5 (PI)**                                              | **PDP**                                               | **ALE**                                                |
|---------------------|--------------------------------------------------------------------------|--------------------------------------------------------------------|------------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------|
| 🔍 **Type**         | Local + Global                                                           | Local                                                              | Global                                                     | Global                                                | Global                                                 |
| 🧠 **Model Support**| Any model (tree-based optimized)                                         | Any black-box model                                                | Any sklearn-compatible model                              | Any sklearn-compatible model                         | Any model                                              |
| 📌 **Use Case**     | Feature importance (local/global), consistent math foundation            | Local explanation for a single prediction                          | Global feature importances via permutation                | Global avg effect of one/two features                | More accurate global effect for correlated features    |
| 📈 **Output**       | SHAP values (visual & numeric)                                           | Feature weights for one instance                                   | Weight for each feature (importance)                      | Plot showing avg prediction vs feature               | Similar plot, but unbiased in presence of correlation |
| ⚙️ **How it works** | Shapley values (game theory)                                             | Perturbs inputs and fits a local surrogate model                   | Permutes feature columns, measures change in score         | Varies one feature at a time                         | Accumulates effects, adjusted for correlation          |
| 🧮 **Mathematically Grounded**| ✅ Very strong                                             | ❌ Approximate, less stable                                        | ✅ Reasonable                                               | ❌ Limited interpretability                          | ✅ Better than PDP with correlations                   |
| 💬 **Interpretation**| ✅ Easy (e.g. “feature X increased prediction by Y”)                    | ✅ Intuitive local influence                                        | ✅ Global importance                                        | ✅ Plot based                                            | ✅ Similar, more accurate                              |
| 📉 **Handles Feature Correlation**| ❌ Not well for some types                         | ❌ No                                                              | ❌ No                                                      | ❌ No                                                 | ✅ Yes                                                 |
| 🚀 **Speed**        | ❌ Slow (especially for large datasets or non-tree models)               | ✅ Fast                                                             | ✅ Fast                                                     | ✅ Very Fast                                          | ✅ Medium                                              |
| 💡 **Visualization**| ✅ Rich plots: beeswarm, waterfall, force                                | ✅ Interactive tables                                               | ✅ HTML/text tables                                         | ✅ Line plots                                         | ✅ Line plots                                          |
| 📚 **Library**      | `shap`                                                                   | `lime`                                                             | `eli5`                                                     | `sklearn.inspection` (deprecated for PDP)            | `alibi`, `PyALE`                                      |

---

## ✅ When to Use What?

### 🔷 SHAP
> Use SHAP for **precise, consistent**, local + global explanations when you need math-backed reasoning.

- Best for: **production models, reports, research**
- Pros: Most **accurate**, consistent, global + local
- Cons: **Slow**, harder for large neural nets

---

### 🔶 LIME
> Use LIME when you need **fast**, understandable explanations for **individual predictions**.

- Best for: **debugging misclassifications**, presentations
- Pros: Easy to use, intuitive
- Cons: Not always **stable**, only local, approximate

---

### 🟡 ELI5 (Permutation Importance)
> Use when you want to understand **global feature importance** quickly.

- Best for: **Feature selection**, model debugging
- Pros: Fast, simple
- Cons: Doesn’t explain **why** features matter, just **that** they do

---

### 🟢 PDP (Partial Dependence Plot)
> Use PDP to see **average effect** of a feature across the dataset.

- Best for: **Understanding non-linear relationships**
- Pros: Visual, intuitive
- Cons: Misleading with correlated features

---

### 🔵 ALE (Accumulated Local Effects)
> Use ALE if your dataset has **correlated features**, and you want **trustworthy global explanations**.

- Best for: More accurate global insights
- Pros: Solves correlation bias
- Cons: Fewer visualization tools than SHAP

---

## 📦 Python Libraries

| Tool      | Install with                         |
|-----------|---------------------------------------|
| SHAP      | `pip install shap`                    |
| LIME      | `pip install lime`                    |
| ELI5      | `pip install eli5`                    |
| PDP       | `pip install scikit-learn` (deprecated) |
| ALE       | `pip install pyALE` or `pip install alibi` |

---

## 🛠 Example Use Cases

| Use Case                              | Recommended Tool |
|---------------------------------------|------------------|
| Explaining one prediction             | LIME / SHAP      |
| Showing global feature importance     | SHAP / ELI5      |
| Visualizing effect of a single feature| PDP / ALE        |
| Handling feature correlation          | ALE              |
| Production-grade trustworthy results  | SHAP             |
| Quick debugging                       | LIME / ELI5      |

---

## 🧠 Final Thoughts

| Want...                                    | Use This        |
|-------------------------------------------|-----------------|
| Accuracy + Consistency                    | SHAP            |
| Speed + Simplicity                        | LIME            |
| Feature importance (not explanation)      | ELI5            |
| Better global plots without correlation   | ALE             |

---

> ✅ All tools are **model-agnostic** and work with any classifier (SVM, RF, XGBoost, NN, etc.)


| Topic            | PDF Link                                                                                                                                     | Streamlit App                                                                                      | Colab Notebook                                                                                                                                           |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| XAI-ML      | <a href="" target="_parent"><img src="https://img.shields.io/badge/Open in PDF-%23FF0000.svg?style=flat-square&logo=adobe&logoColor=white"/></a> | <a href="https://ds-cheat-sheets-sklearn.streamlit.app/" target="_parent"><img src="https://static.streamlit.io/badges/streamlit_badge_black_white.svg"/></a> | <a href="https://colab.research.google.com/drive/1NZj_e49G0BaxWTle3fBQ7Vn43iLPSupj?usp=sharing" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> |
| Heaart-Broke    | <a href="" target="_parent"><img src="https://img.shields.io/badge/Open in PDF-%23FF0000.svg?style=flat-square&logo=adobe&logoColor=white"/></a> | <a href="" target="_parent"><img src="https://static.streamlit.io/badges/streamlit_badge_black_white.svg"/></a> | <a href="https://colab.research.google.com/drive/1lis-bUgOS9DSD_KWf4hfUsKygYlB8Ku9?usp=sharing" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a> |

