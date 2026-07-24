# 🏠 Sahara Sevak AI — Samanvay Setu

A social impact project aimed at reuniting dislocated elderly individuals with their families, using two AI-driven components: geospatial risk prediction and voice-based regional origin identification.

---

## 📋 Overview

Elderly individuals who go missing or are abandoned are often found far from home, unable to communicate their origin clearly due to age, distress, or health conditions. Sahara Sevak AI supports NGO and welfare staff with two tools:

1. **Predicting where abandonment is most likely to occur**, so outreach resources can be deployed proactively.
2. **Identifying a found individual's likely regional origin from their voice**, to speed up reconnecting them with family — alongside a quick health/emotional triage.

---

## 🧩 Component 1: Abandonment Risk Clustering

Uses **K-Means clustering** to identify high-risk zones for elder abandonment across Tamil Nadu districts.

**Features used:**
- MPI Headcount Ratio (poverty index)
- Elder Abuse Prevalence
- Transit Hub Score (accessibility to transport hubs — abandonment often occurs near stations/terminals)
- Latitude / Longitude

**Method:** The elbow method determines the optimal number of clusters (k), then K-Means groups districts into **High / Medium / Low risk** based on poverty and transit-accessibility scores.

**Output:** A geospatial risk map and a ranked table of districts, so pilot resources can be focused on the highest-risk areas first.

**Data:** `data/abandonment_risk_proxies.csv`

---

## 🎙️ Component 2: Dialect / Regional Origin Prediction

Uses an **SVM classifier** (with automatic fallback to KNN for very small datasets) to predict a found individual's likely regional origin from a voice recording.

**Features extracted (via `librosa`):**
- MFCCs, delta-MFCCs, and delta-delta MFCCs (voice texture/accent)
- Pitch mean & standard deviation
- RMS energy (voice strength)
- Spectral contrast
- Chroma (tonal content)

**Health & Emotional Triage:** A simple rule-based check flags:
- 🔴 **High priority** — very low voice energy (possible non-responsiveness)
- 🟡 **Medium priority** — flat/monotone pitch (possible depression/distress)
- 🟢 **Stable** — normal range

**Interface:** A Gradio-based intake portal lets NGO staff record or upload a voice sample and instantly get a predicted dialect + triage recommendation.

**Current performance:** ~80% cross-validation accuracy on the initial 30-sample dataset (10 each: Hindi, Tamil, Bengali). Accuracy will improve as more labeled samples are added per language.

**Data:** `data/real_dialect_mfccs.csv` (extracted features), `data/audio/{hindi,tamil,bengali}/` (raw training audio)

---

---

## 🚀 Running the Project

This was originally developed in Google Colab, but all file paths are relative — it runs identically locally, in GitHub Codespaces, or in any Jupyter environment. No Google Drive mounting is needed.

### 1. Install dependencies
```bash
pip install -r requirements.txt
```

### 2. Run the notebook
Open `Sahara_Sevak_AI.ipynb` and run all cells in order. This will:
1. Generate the geospatial risk clustering plots and district risk table
2. Train (or load, if already trained) the dialect classifier and print a cross-validation accuracy report
3. Launch a Gradio demo with a temporary public link for testing the voice intake portal

### 3. Adding more training data
To improve dialect prediction accuracy, add more `.wav`/`.mp3`/`.m4a` files into the relevant `data/audio/{language}/` folder, delete `data/real_dialect_mfccs.csv`, and re-run the notebook — it will automatically re-extract features and retrain.

---

## ⚠️ Disclaimer

This is a prototype developed for research/pilot purposes. Predictions (risk levels, dialect estimates, triage status) are probabilistic and should be used to *assist*, not replace, human judgment and professional care decisions.
