# Urban Displacement Index — London

**Authors:** Yam Ben-Tov, Naama Inbar, Elad Rubani  
**Course:** Data Collection & Management Lab (00940290) — Technion

## Overview

This project develops an **Urban Displacement Index** for London that quantifies how tourist infrastructure displaces essential local services at a neighborhood level. By comparing 11 types of tourist facilities (museums, hotels, attractions, etc.) against 11 types of local services (schools, pharmacies, supermarkets, etc.), we identify areas where tourism is eroding residential livability.

The tool produces four interactive maps and a personalized livability scorer that adapts to different user profiles (families, young professionals, investors, retirees).

## How to Run

This project runs as a **Databricks notebook**. Follow these steps:

1. **Open Databricks** and navigate to your workspace.
2. **Import the notebook**: Upload `Displacement_Index_Project.ipynb` into your Databricks workspace.
3. **Attach a cluster**: Select or create a cluster for the notebook (Python 3.x, any standard runtime).
4. **Configure SAS tokens**: The first two cells are the **SAS Tokens Configuration** section. Before running anything, you need to fill in the three SAS tokens:
   - `booking_sas_token` — for accessing the Booking.com dataset
   - `submissions_sas_token` — for accessing the OSM data in the submissions container
   - `airbnb_sas_token` — for accessing the Airbnb dataset
5. **Run the notebook**: Click **Run All** at the top of the notebook.
   - The notebook will automatically install required libraries (`h3`, `folium`, `branca`).
   - Each section runs sequentially — no manual input is required until the final **Livability Tool** cell, where you select a user profile (1–4).
5. **View outputs**: Each map section renders an interactive Folium map directly in the notebook output.

> **Note:** The notebook requires access to the course Azure storage account (`lab94290`) with valid SAS tokens. Data files should not be run locally outside of Databricks.

## Project Structure

| Section | Cells | Description |
|---|---|---|
| SAS Tokens Configuration | 1–2 | Fill in the Booking, Submissions, and Airbnb SAS tokens before running |
| Booking Data Handling | 3–4 | Loads and filters 21,649 London listings from Booking.com |
| OSM Data Handling | 5–6 | Loads pre-scraped OpenStreetMap GeoJSON (4,615 tourist + 16,335 local POIs) |
| Data Combining | 7–8 | Merges Booking.com hotels into OSM infrastructure framework |
| Data Cleaning | 9–12 | Consolidates 86 raw categories into 22 meaningful ones via synonym merging and domain filtering |
| Geographical Scoring | 13–15 | H3 hexagonal grid (resolution 8), IDF + PCA weighting, displacement score calculation, spatial smoothing |
| Airbnb Handling | 16–17 | Loads and filters 15,093 London Airbnb listings |
| Airbnb Saturation | 18–19 | Calculates Airbnb density score per hexagon |
| Visualizations | 20–30 | Four interactive Folium maps (Displacement Index, Airbnb Saturation, Divergence Patterns, Validation) |
| Livability Tool | 31–33 | Personalized suitability map based on user profile selection |

## Dependencies

The following libraries are installed automatically when the notebook runs:

- `h3` — Hexagonal spatial indexing
- `folium` — Interactive map rendering
- `branca` — Colormaps for Folium

Pre-installed on Databricks (no action needed):

- `pyspark` — Distributed data processing
- `pandas`, `numpy` — Data manipulation
- `scikit-learn` — PCA and StandardScaler
- `matplotlib` — Plotting

## Data Sources

| Source | Description |
|---|---|
| Booking.com (Azure) | Short-term accommodation listings |
| Airbnb (Azure) | Short-term rental listings |
| OpenStreetMap (Azure) | Tourist and local infrastructure POIs |

> **Important:** Do not upload any data files to this repository. All data is stored in Azure Blob Storage and accessed via SAS tokens at runtime.
