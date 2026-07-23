# Sahara Sevak AI — Abandonment Risk Clustering

Part of the Sahara Sevak AI (Samanvay Setu) project, which aims to reunite dislocated elderly individuals with their families.

This notebook uses K-Means clustering to identify high-risk zones for elder abandonment across Tamil Nadu districts, based on:
- **MPI Headcount Ratio** — poverty index
- **Elder Abuse Prevalence**
- **Transit Hub Score** — accessibility to transit hubs
- **Latitude / Longitude** — geographic location

## Data
`data/abandonment_risk_proxies.csv` — district-level proxy indicators for 8 Tamil Nadu districts (Chennai, Dharmapuri, Vellore, Coimbatore, Thiruvallur, Ariyalur, Tirunelveli, Madurai).

## Method
Uses the elbow method (WCSS across k = 1 to 5) to determine the optimal number of clusters, then applies K-Means to group districts by abandonment risk profile.

## Note
Originally developed in Google Colab. The dataset has been added to this repo (`data/`) so the notebook can run outside Colab using a relative file path.
