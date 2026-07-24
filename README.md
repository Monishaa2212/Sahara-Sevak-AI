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

## 📁 Repository Structure
