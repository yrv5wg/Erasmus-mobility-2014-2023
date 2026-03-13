# Erasmus+ Mobility Data 2014-2023: Enhanced Dataset

[![DOI](https://img.shields.io/badge/DOI-10.6084%2Fm9.figshare.31718764-blue)](https://doi.org/10.6084/m9.figshare.31718764)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Data License: CC BY 4.0](https://img.shields.io/badge/Data%20License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

Complete reproducible pipeline for enriching Erasmus+ KA2 mobility data (2014-2023) with NUTS3 regional codes (99.94% coverage), ETER institutional characteristics, and Eurostat socioeconomic indicators.

## Overview

This repository contains five Jupyter notebooks that transform raw Erasmus+ mobility data into a research-ready dataset with validated geographic codes, institutional metadata, and regional socioeconomic indicators.

**Final dataset:** 206,448 mobility records × 70 variables covering 45,488 institutions across 1,514 NUTS3 regions (2014-2023).

## Pipeline Architecture
```
Raw Erasmus Data (206K records)
    ↓
[01] Geocoding & NUTS3 Matching ──→ 99.94% coverage
    ↓
[02] LLM Validation & Corrections ──→ 525 manual fixes
    ↓
[03] ETER Institutional Linkage ──→ 1,986 matched institutions
    ↓
[04] ROR Validation ──→ Quality assessment
    ↓
[05] Eurostat Regional Enrichment ──→ GDP, crime, population
    ↓
Final Complete Dataset (70 variables)
```

## Notebooks

### 01_geocoding_and_nuts3_matching.ipynb
- Dual-provider geocoding (ArcGIS + Nominatim fallback)
- Parallel processing (20 threads)
- Spatial join to NUTS3 2021 boundaries
- **Output:** `erasmus_with_nuts3.csv`

### 02_llm_validation_and_manual_corrections.ipynb
- LLM-assisted validation of 228 problematic locations
- 525 manual coordinate corrections
- Capital city fallback with pre-validated NUTS3 codes
- **Output:** `erasmus_with_nuts3_corrected.csv`

### 03_eter_institutional_linkage.ipynb
- Fuzzy matching to European Tertiary Education Register
- Haversine distance + name similarity scoring
- 1,986 institutions matched (4.4% of records)
- **Outputs:** `erasmus_eter_linkage.csv`, `erasmus_with_eter.csv`

### 04_ror_validation.ipynb
- Cross-validation against Research Organization Registry
- 107 confirmed matches (5.4%), avg 0.91 km distance
- Quality assessment report
- **Output:** `eter_ror_validation.csv`

### 05_eurostat_regional_enrichment.ipynb
- Automated Eurostat data download (5 datasets)
- GDP per capita (NUTS3, NUTS2, Country)
- Crime rates (NUTS2), Population (NUTS3)
- Annual data 2014-2023 matched by year
- **Output:** `erasmus_final_complete.csv` ✅ **FINAL DATASET**

## Installation

### Requirements
- Python 3.8+
- Jupyter Notebook or JupyterLab

### Setup
```bash
# Clone repository
git clone https://github.com/yrv5wg/Erasmus-mobility-2014-2023.git
cd Erasmus-mobility-2014-2023

# Install dependencies
pip install -r requirements.txt

# For LLM validation (optional)
# Install Ollama: https://ollama.ai
# Pull model: ollama pull deepseek-r1:1.5b
```

## Data Availability

### Complete Dataset (Figshare)
The full enriched dataset and all intermediate outputs are available on Figshare:

**DOI:** [10.6084/m9.figshare.31718764](https://doi.org/10.6084/m9.figshare.31718764)

Files included:
- `erasmus_final_complete.csv` - Final enriched dataset (206,448 × 70)
- `erasmus_with_nuts3.csv` - Geocoded with NUTS3 codes
- `erasmus_eter_linkage.csv` - Institution-ETER mapping table
- `eter_ror_validation.csv` - ROR validation report
- `Erasmus-KA2-Mobility-Data.xlsx` - Original source data

### Source Files Required
To run the pipeline, download these files separately:

1. **Erasmus Raw Data** (included in Figshare)
   - File: `Erasmus-KA2-Mobility-Data.xlsx`
   - Original source: [EU Open Data Portal](https://data.europa.eu)

2. **NUTS3 Shapefile** (200 MB)
   - Download: [Eurostat GISCO](https://ec.europa.eu/eurostat/web/gisco/geodata/reference-data/administrative-units-statistical-units/nuts)
   - File: `NUTS_RG_01M_2021_3035.shp` (+ .dbf, .shx, .prj)

3. **ETER Database** (requires registration)
   - Download: [ETER Portal](https://www.eter-project.com/)
   - File: `eter-export-selected-[timestamp].xlsx`

4. **Eurostat Datasets** (auto-downloaded by notebook 05)
   - GDP NUTS3: `nama_10r_3gdp`
   - GDP NUTS2: `nama_10r_2gdp`
   - GDP Country: `nama_10_gdp`
   - Crime NUTS2: `crim_gen_reg`
   - Population NUTS3: `demo_r_pjangrp3`

## Usage

1. Download source files (NUTS3 shapefile, ETER database)
2. Update file paths in each notebook's configuration cell
3. Run notebooks sequentially (01 → 05)
4. Each notebook produces outputs needed by the next

**Estimated runtime:** 2-4 hours total (including Eurostat downloads)

## Key Statistics

| Metric | Value |
|--------|-------|
| **Records** | 206,448 |
| **Variables** | 70 |
| **Institutions** | 45,488 unique |
| **NUTS3 Regions** | 1,514 |
| **Countries** | 33 |
| **Timespan** | 2014-2023 |
| **NUTS3 Coverage** | 99.94% |
| **ETER Matches** | 1,986 (4.4%) |
| **GDP Coverage** | 88.2% (NUTS3), 98.7% (Country) |
| **Crime Coverage** | 79.8% (NUTS2) |
| **Population Coverage** | 98.9% (NUTS3) |

## Validation & Quality

- **Geocoding:** Dual-provider with LLM validation, 99.94% success rate
- **ETER Matching:** 81.4% avg name similarity, 6.18 km avg distance
- **ROR Validation:** 5.4% confirmed matches (107/1,986)
- **Eurostat:** Annual time-series 2014-2023, matched by region × year

## Citation

### Dataset
```bibtex
@dataset{kiss_2026_erasmus,
  author       = {Kiss, Dénes and Kosztyán, Zsolt Tibor},
  title        = {{Erasmus+ KA2 Mobility Data 2014-2023: Enhanced 
                   with NUTS3 Regional Codes, ETER Institutional 
                   Characteristics, and Eurostat Socioeconomic Indicators}},
  year         = 2026,
  publisher    = {figshare},
  doi          = {10.6084/m9.figshare.31718764},
  url          = {https://doi.org/10.6084/m9.figshare.31718764}
}
```

### Code Repository
```bibtex
@misc{kiss_2026_github,
  author       = {Kiss, Dénes and Kosztyán, Zsolt Tibor},
  title        = {Erasmus-mobility-2014-2023: Data Enrichment Pipeline},
  year         = 2026,
  publisher    = {GitHub},
  url          = {https://github.com/yrv5wg/Erasmus-mobility-2014-2023}
}
```

### Associated Publication
*[To be added upon Scientific Data publication]*

## Authors

**Dénes Kiss**  
University of Pannonia, Faculty of Business and Economics  
📧 kiss.denes@gtk.uni-pannon.hu

**Zsolt Tibor Kosztyán**  
University of Pannonia, Faculty of Business and Economics  
📧 kosztyan.zsolt@gtk.uni-pannon.hu

## License

- **Code:** MIT License
- **Data:** CC BY 4.0 (available on Figshare)

## Acknowledgments

- **Data Sources:**
  - European Commission - Erasmus+ Programme
  - Eurostat GISCO - NUTS3 boundaries
  - ETER Project - Institutional data
  - Eurostat - Regional indicators
  - ROR - Validation data

- **Tools:**
  - ArcGIS Geocoding Service
  - Nominatim (OpenStreetMap)
  - Ollama / deepseek-r1 for LLM validation

## Related Work

This work extends:
- Gadár et al. (2020) - *Scientific Data* - Erasmus 2008-2014
- Väisänen et al. (2025) - *Scientific Data* - Erasmus 2014-2022 (geocoding only)

Our dataset adds institutional and economic dimensions not available in prior work.

## Support

For questions or issues:
1. Open a GitHub issue
2. Contact: kiss.denes@gtk.uni-pannon.hu

---

**Status:** ✅ Dataset published | 📝 Paper under review | 🔄 Code maintained
