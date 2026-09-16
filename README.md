<div align="center">

# 🚦 Crash Hotspot Detection in Dubai Using DBSCAN and Severity Weighting

**Maryam Alblooshi** · **Najma Nour** · **Khalid Elgazzar**

Canadian University Dubai, United Arab Emirates

**2026 IEEE International Conference on Smart Mobility (SM2026)** · Al Alamein City, Egypt · 11–13 May 2026

[![Paper](https://img.shields.io/badge/IEEE%20Xplore-Paper-00629B?logo=ieee&logoColor=white)](https://ieeexplore.ieee.org/document/11614141)
[![DOI](https://img.shields.io/badge/DOI-10.1109%2FSM69703.2026.11614141-blue)](https://doi.org/10.1109/SM69703.2026.11614141)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Method](https://img.shields.io/badge/Method-DBSCAN-orange)](https://en.wikipedia.org/wiki/DBSCAN)
[![Domain](https://img.shields.io/badge/Domain-Smart%20Mobility-informational)](https://ieeesm.org/)
[![Live Demo](https://img.shields.io/badge/Live%20Demo-View%20Map-38bdf8)](https://claude.ai/artifact/UkbDgk7Hmk2jDv7stjDyFU)

</div>

---

## 🗺️ Live Demo

**[View the interactive hotspot map →](https://claude.ai/artifact/UkbDgk7Hmk2jDv7stjDyFU)**

Built from the actual Dubai Police traffic incident dataset (via [Data.Dubai](https://data.dubai)), filtered to 14–30 March 2026 — the same 16-day window used in the paper — and run through the full pipeline: severity weighting from the incident's own Arabic category text, DBSCAN clustering (ε=200m, minPts=4, Haversine distance), risk scoring, and peak time-window detection. **5,256 real incidents** were analyzed, producing **302 clusters**. The top hotspot, **Al Satwa** (86 incidents, risk score 102), peaks 20:00–22:00 — consistent with the paper's finding of an evening/late-night surge.

> Location names are nearest-landmark matches rather than a live reverse-geocoding call (Nominatim isn't reachable from the analysis environment used to build this demo) — see the map page for full methodology notes.

## 📝 Abstract

> Rapidly growing cities such as Dubai frequently experience a high number of traffic-related incidents. To reduce those incidents, transportation authorities must prioritize and classify locations with a high number of accidents. This research introduces a method for identifying high-crash areas, employing DBSCAN clustering and severity-weighted hotspot scoring. The methodology utilizes geographic coordinates, road classifications, and community designations to improve the clarity of results. The analysis showed that certain areas, particularly major roads, had a higher number of accidents. The model is proving to be quite useful, easily understood, and it fits right in with smart mobility applications.

## 🔑 Highlights

- 📍 **Density-based spatial clustering (DBSCAN)** identifies crash hotspots without needing to pre-specify the number of clusters
- ⚖️ **Severity-weighted risk scoring** — clusters with fewer but more severe incidents are ranked above clusters with many minor ones
- 🕒 **Temporal pattern analysis** — incidents are grouped into 2-hour windows to surface each hotspot's peak-risk period
- 🗺️ **Reverse geocoding** turns raw coordinate clusters into human-readable location labels (nearby roads, neighborhoods, districts)
- 🏙️ Built and validated on a real ~5,000-record traffic incident dataset from the Dubai open-data ecosystem

## 🧩 Method Overview

```
Traffic Incident Dataset
        │
        ▼
Data Preprocessing (cleaning, coordinate filtering)
        │
        ▼
Spatial Clustering — DBSCAN with Haversine distance
        │
        ▼
Severity-Weighted Risk Scoring   (Risk_c = Σ w_i, per cluster)
        │
        ▼
Temporal Analysis — peak time-window detection
        │
        ▼
Spatial Annotation & Cluster Visualization (reverse geocoding)
```

- **Dataset:** ~5,000 Dubai traffic incident records (14–30 March 2026), sourced from the Dubai Pulse / Dubai Police open-data platform. Records with missing/invalid coordinates were removed; timestamps parsed to datetime.
- **Severity weighting:** incidents classified via keyword matching on the incident-category text — minor = 1, moderate = 2, severe = 3 (default weight 1 where a category couldn't be determined).
- **Clustering:** DBSCAN with the Haversine distance metric (accounts for Earth's curvature), controlled by a neighborhood radius `ε` and minimum-points threshold `MinPts`.
- **Risk scoring:** each cluster's risk score is the sum of its incidents' severity weights, so a small cluster of severe crashes can outrank a larger cluster of minor ones.
- **Location labeling:** cluster centroids are reverse-geocoded via a map-based API to produce descriptive, real-world hotspot labels.

Full methodology, dataset description, results, and discussion are in the [paper](paper/).

## 📊 Results

- DBSCAN identified numerous statistically significant high-density crash zones that correspond to established hotspots in Dubai, while successfully excluding noise/outlier points.
- Several hotspots showed a clear surge in incidents during **evening and late-night hours**, consistent with heavier traffic and reduced visibility.
- Major roads and high-density community areas emerged as the most consistent hotspot locations.
- The severity-weighted scoring approach provided a more informative hotspot ranking than raw incident counts alone — prioritizing clusters with fewer but more serious crashes.

## ⚠️ Limitations

- Defining precise road segments within detected clusters is difficult without more detailed road-network data.
- Reverse geocoding via an external provider limits the precision of location labels.
- Incident severity is inferred from free-text category keywords rather than a dedicated severity field, which may introduce classification noise.
- The framework currently analyzes a static dataset snapshot rather than a real-time incident stream.

## 📁 Repository Structure

```
Crash-Hotspot-Detection-in-Dubai/
├── notebooks/
│   └── Crash_Hotspot_Detection_DBSCAN.ipynb   # Full pipeline: preprocessing → DBSCAN → risk scoring → temporal analysis → map
├── paper/
│   └── Crash_Hotspot_Detection_in_Dubai_IEEE_SM2026.pdf
├── presentation/
│   └── Dubai_Crash_Hotspot.pptx
├── requirements.txt
├── LICENSE
└── README.md
```

## 🚀 Quick Start

```bash
git clone https://github.com/Najma-Abdulkarim-Nor/Crash-Hotspot-Detection-in-Dubai-Using-Spatial-Clustering-and-Road-Level-Analysis.git
cd Crash-Hotspot-Detection-in-Dubai-Using-Spatial-Clustering-and-Road-Level-Analysis
pip install -r requirements.txt
```

1. Download the Traffic Incidents dataset from [Data.Dubai](https://data.dubai/en/l/469979) (Dubai Police, tagged "Open" — freely downloadable as CSV, no account required) and place the CSV where the notebook can find it.
2. Open `notebooks/Crash_Hotspot_Detection_DBSCAN.ipynb`, update the `DATA_PATH` and `COLUMN_MAPPING` in the loading cell to match your file, and run all cells.
3. The notebook will clean and filter the data, weight incidents by severity, run DBSCAN clustering with a Haversine distance metric, score and rank hotspots by risk, detect each hotspot's peak-risk time window, reverse-geocode cluster centroids into readable labels, and render an interactive Folium map — saving both a CSV summary and an HTML map at the end.

> 💻 **Note:** The keyword lists used for severity classification (`SEVERE_KEYWORDS`, `MODERATE_KEYWORDS`, `MINOR_KEYWORDS`) are a starting point based on the paper's description — refine them to match the exact vocabulary in your dataset's incident-description field for best results.

## 📖 Citation

If you use this work, please cite:

```bibtex
@inproceedings{alblooshi2026crash,
  author    = {Alblooshi, Maryam and Nour, Najma and Elgazzar, Khalid},
  title     = {Crash Hotspot Detection in Dubai using DBSCAN and Severity Weighting},
  booktitle = {2026 IEEE International Conference on Smart Mobility (SM)},
  year      = {2026},
  address   = {Al Alamein City, Egypt},
  doi       = {10.1109/SM69703.2026.11614141}
}
```

## 👥 Authors

- **Maryam Alblooshi** — Canadian University Dubai, UAE
- **Najma Nour** — Canadian University Dubai, UAE
- **Khalid Elgazzar** — IoT Research Laboratory, Ontario Tech University, Canada

## 📄 License

This project is licensed under the [MIT License](LICENSE).
