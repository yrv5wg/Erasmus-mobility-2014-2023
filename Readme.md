# Erasmus+ Mobility Data 2014-2023: Enhanced Dataset

[![DOI](https://img.shields.io/badge/DOI-10.6084%2Fm9.figshare.31718764-blue)](https://doi.org/10.6084/m9.figshare.31718764)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Data License: CC BY 4.0](https://img.shields.io/badge/Data%20License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

Complete reproducible pipeline for enriching Erasmus+ KA2 mobility data (2014-2023) with NUTS3 regional codes (99.94% coverage), ETER institutional characteristics, and Eurostat socioeconomic indicators.

## Overview

This repository contains five Jupyter notebooks that transform raw Erasmus+ mobility data into a research-ready dataset with validated geographic codes, institutional metadata, and regional socioeconomic indicators.

**Final dataset:** 206,448 mobility records × 70 variables covering 45,488 institutions across 1,514 NUTS3 regions (2014-2023).

## 📊 Key Statistics

| Metric | Value |
|--------|-------|
| **Records** | 206,448 |
| **Variables** | 70 |
| **Institutions** | 45,488 unique |
| **NUTS3 Regions** | 1,514 |
| **Countries** | 33 |
| **Timespan** | 2014-2023 (9 years) |
| **NUTS3 Coverage** | 99.94% |
| **ETER Matches** | 1,986 institutions |
| **GDP Coverage** | 88.2% (NUTS3), 98.7% (Country) |
| **Crime Coverage** | 79.8% (NUTS2) |
| **Population Coverage** | 98.9% (NUTS3) |

## 🎯 Citation

If you use this dataset or code, please cite:

**Dataset:**
> Kiss, D., & Kosztyán, Z.T. (2026). A Multilayer and Spatial Description of the Erasmus Mobility Network 2014-2023: Enhanced with NUTS3 Regional Codes and Socioeconomic Indicators [Data set]. Figshare. https://doi.org/10.6084/m9.figshare.31718764

**Code:**
> Kiss, D., & Kosztyán, Z.T. (2026). Erasmus Mobility 2014-2023 Processing Code (v1.0.0) [Computer software]. GitHub. https://github.com/yrv5wg/Erasmus-mobility-2014-2023

Or use the **"Cite this repository"** button in the GitHub interface (powered by [CITATION.cff](CITATION.cff)).

### BibTeX

**Dataset:**
```bibtex
@dataset{kiss_2026_erasmus,
  author       = {Kiss, Dénes and Kosztyán, Zsolt Tibor},
  title        = {{A Multilayer and Spatial Description of the Erasmus 
                   Mobility Network 2014-2023: Enhanced with NUTS3 
                   Regional Codes and Socioeconomic Indicators}},
  year         = 2026,
  publisher    = {Figshare},
  doi          = {10.6084/m9.figshare.31718764},
  url          = {https://doi.org/10.6084/m9.figshare.31718764}
}
```

**Code:**
```bibtex
@software{kiss_2026_code,
  author       = {Kiss, Dénes and Kosztyán, Zsolt Tibor},
  title        = {Erasmus-mobility-2014-2023: Data Enrichment Pipeline},
  year         = 2026,
  publisher    = {GitHub},
  version      = {v1.0.0},
  url          = {https://github.com/yrv5wg/Erasmus-mobility-2014-2023}
}
```

### Associated Publication
*Manuscript submitted to Scientific Data (2026)*

## 📜 License

- **Data**: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (via Figshare)
- **Code**: [MIT License](LICENSE) (this repository)

## 🏗️ Pipeline Architecture
```
Raw Erasmus Data (206,448 records)
    ↓
[01] Geocoding & NUTS3 Matching ──→ 99.94% coverage
    ↓
[02] LLM Validation & Corrections ──→ 525 manual fixes + 49 LLM corrections
    ↓
[03] ETER Institutional Linkage ──→ 1,986 matched institutions
    ↓
[04] ROR Validation ──→ Independent quality assessment
    ↓
[05] Eurostat Regional Enrichment ──→ GDP, crime, population (2014-2023)
    ↓
Final Complete Dataset (70 variables) ✅
```

## 📓 Notebooks

### 01_geocoding_and_nuts3_matching.ipynb
**Purpose:** Assign geographic coordinates and NUTS3 regional codes

- Dual-provider geocoding (ArcGIS + Nominatim fallback)
- Parallel processing (20 threads, ~45 min runtime)
- Spatial join to NUTS3 2021 boundaries via point-in-polygon
- **Output:** `erasmus_with_nuts3.csv` (206,448 records, 99.7% geocoded)

### 02_llm_validation_and_manual_corrections.ipynb
**Purpose:** Validate and correct problematic geocoding results

- LLM-assisted validation of 228 suspicious locations (Llama 3.2)
- 525 pre-compiled manual coordinate corrections
- Capital city fallback strategy for missing locations (23,430 records)
- **Output:** `erasmus_with_nuts3_corrected.csv` (99.94% NUTS3 coverage)

### 03_eter_institutional_linkage.ipynb
**Purpose:** Link institutions to European Tertiary Education Register

- Combined haversine distance + fuzzy name matching
- 50 km distance threshold, 60% minimum name similarity
- 1,986 institutions matched (avg 81.4% similarity, 6.18 km distance)
- **Outputs:** 
  - `erasmus_eter_linkage.csv` (institution mapping table)
  - `erasmus_with_eter.csv` (enriched mobility data)

### 04_ror_validation.ipynb
**Purpose:** Independent validation against Research Organization Registry

- Cross-reference with ROR database (2,666 EU education institutions)
- 107 confirmed matches (5.4%), avg 0.91 km distance, 89% similarity
- Demonstrates sub-kilometer precision for nearby institutions
- **Output:** `eter_ror_validation_COMPLETE.csv`

### 05_eurostat_regional_enrichment.ipynb
**Purpose:** Add annual regional socioeconomic indicators

- Automated download from 5 Eurostat datasets
- GDP per capita at 3 levels (NUTS3, NUTS2, Country-PPS)
- Crime rates (NUTS2), Population (NUTS3)
- Matched by NUTS code × year (2014-2023)
- **Output:** `erasmus_FINAL_with_regional_data.csv` ✅ **FINAL DATASET**

## 🚀 Quick Start

### 1. Installation

**Requirements:**
- Python 3.8+
- 4+ GB RAM
- ~2 GB disk space (for Eurostat data cache)
```bash
# Clone repository
git clone https://github.com/yrv5wg/Erasmus-mobility-2014-2023.git
cd Erasmus-mobility-2014-2023

# Install dependencies
pip install -r requirements.txt
```

**For LLM validation (optional):**
```bash
# Install Ollama: https://ollama.ai
# Pull model
ollama pull llama3.2:1.5b
```

### 2. Download Data Files

**From Figshare** ([DOI: 10.6084/m9.figshare.31718764](https://doi.org/10.6084/m9.figshare.31718764)):
- `Erasmus-KA2-Mobility-Data.xlsx` (raw mobility data)
- `eter-export-selected.xlsx` (ETER database)
- `erasmus_with_nuts3.csv` (if skipping notebook 01)
- `erasmus_eter_linkage.csv` (if skipping notebook 03)
- `erasmus_FINAL_with_regional_data.csv` (final dataset)

**NUTS3 Shapefile** (required for notebook 01):
- Download from [Eurostat GISCO](https://ec.europa.eu/eurostat/web/gisco/geodata/reference-data/administrative-units-statistical-units/nuts)
- File: `NUTS_RG_01M_2021_3035.shp` (+ .dbf, .shx, .prj, .cpg)

**ROR & Eurostat data:**
- Auto-downloaded by notebooks 04 and 05

### 3. Configure & Run

**Update file paths** in each notebook's configuration cell:
```python
# Example from notebook 01
BASE_PATH = '/Users/yourname/Desktop/Erasmus-Data/'
ERASMUS_DATA = BASE_PATH + 'Erasmus-KA2-Mobility-Data.xlsx'
NUTS3_SHAPEFILE = BASE_PATH + 'NUTS_RG_01M_2021_3035.shp'
OUTPUT_DIR = BASE_PATH + 'outputs/'
```

**Run notebooks sequentially:**
```bash
jupyter notebook
# Open and run: 01 → 02 → 03 → 04 → 05
```

**Estimated runtime:** 2-4 hours total
- Notebook 01: ~45 min (geocoding)
- Notebook 02: ~15 min (validation)
- Notebook 03: ~10 min (ETER matching)
- Notebook 04: ~5 min (ROR validation)
- Notebook 05: ~60 min (Eurostat downloads, first run only)

## 📦 Data Variables

### Original Variables (14)
- Project ID, Mobility ID, Activity Type
- Sending/Receiving Organization, City, Country
- Participant Profile, Age, Gender
- Duration, Start Month, Total Participants

### Added: Geographic (Notebooks 01-02) — 10 variables
- `Sending_Lat`, `Sending_Lon` (WGS84)
- `Receiving_Lat`, `Receiving_Lon`
- `Sending_NUTS3`, `Sending_NUTS3_Name`
- `Receiving_NUTS3`, `Receiving_NUTS3_Name`
- Geocoding source, validation flags

### Added: Institutional (Notebook 03) — 16 variables
- `Sending_ETER_ID`, `Sending_ETER_Name`
- `Receiving_ETER_ID`, `Receiving_ETER_Name`
- Academic staff (FTE)
- Core revenues, Third-party funding (EUR)
- Total publications
- Match distance, name similarity scores
- *Repeated for both sending and receiving institutions*

### Added: Socioeconomic (Notebook 05) — 30 variables
- `Year` (extracted from mobility start month)
- **Sending region:**
  - `Sending_NUTS2`, `Sending_NUTS0` (derived codes)
  - `Sending_GDP_NUTS3` (EUR per capita)
  - `Sending_GDP_NUTS2` (EUR per capita)
  - `Sending_GDP_Country_PPS` (Purchasing Power Standard)
  - `Sending_Crime_rate` (offences per 100k, NUTS2)
  - `Sending_Population` (NUTS3)
- **Receiving region:** (same 15 variables)

**Total: 70 variables**

## ✅ Validation & Quality

### Geocoding Accuracy
- **Method:** Dual-provider (ArcGIS primary, Nominatim fallback)
- **Coverage:** 99.94% (206,319 / 206,448 records)
- **Precision:** 96% within 1 km (manual verification, n=50)
- **Mean error:** 347 meters
- **LLM corrections:** 49 major errors fixed (e.g., 9,400 km correction)

### ETER Matching Quality
- **Matches:** 1,986 institutions (4.4% of unique institutions)
- **Name similarity:** 81.4% average, 84.0% median
- **Geographic distance:** 6.18 km average, 2.11 km median
- **Methodology:** 60% name weight + 40% distance weight

### ROR Independent Validation
- **Confirmed:** 107 matches (5.4%)
- **Precision:** 0.91 km average distance, 89% name similarity
- **Capability:** Distinguishes institutions <100m apart (e.g., Vienna: 0.09 km, Klagenfurt: 0.00 km)
- **Interpretation:** Low match rate reflects ROR's focus on research institutes vs. ETER's comprehensive HEI coverage

### Eurostat Data Quality
- **Temporal coverage:** Complete 2014-2023 annual data
- **Missing values:** Non-EU countries, statistical suppression, boundary years
- **Hierarchical fallback:** NUTS3 → NUTS2 → Country ensures 98.7% minimum coverage
- **Validation:** All merges preserve 206,448 record count (no row duplication)

## 🔬 Research Applications

This dataset enables investigation of:

**Economic Determinants:**
- Gravity models with GDP differentials
- Regional economic convergence through mobility
- Impact of COVID-19 on mobility-economy relationships
- Brain drain/gain patterns relative to regional prosperity

**Institutional Dynamics:**
- Prestige × regional wealth interaction effects
- Bilateral partnership formation mechanisms
- Research intensity and mobility attractiveness
- Institutional size effects on sending/receiving patterns

**Spatial Patterns:**
- Core-periphery structures at NUTS3 granularity
- Network clustering by field of study
- Geographic distance decay with economic controls
- Community detection in multilayer networks

**Policy Evaluation:**
- Regional cohesion policy impacts
- Crime/safety perceptions in destination choice
- Demographic trends (gender, age) × regional factors
- Brexit effects on UK-EU mobility flows

**Temporal Dynamics:**
- Pre/during/post-COVID mobility evolution
- Annual economic shocks and mobility responses
- Programme cycle effects (2014-2020 vs. 2021-2027)

## 🔧 Methodology Details

### Geocoding Strategy (Notebook 01)
**Multi-stage approach:**
1. **Primary provider:** ArcGIS World Geocoding Service (no rate limit)
2. **Fallback provider:** Nominatim/OpenStreetMap (1 req/sec)
3. **Caching:** Deduplicate city-country pairs before geocoding
4. **Parallel processing:** ThreadPoolExecutor with 20 workers
5. **Spatial matching:** Point-in-polygon join to NUTS3 2021 boundaries

**Quality checks:**
- Country consistency validation
- Geographic bounds testing
- Distance to nearest city center

### LLM Validation (Notebook 02)
**Use case:** Correct systematic geocoding failures

**Model:** Llama 3.2 (1.5B parameters) via Ollama
**Prompt strategy:** JSON-structured responses with confidence scores
**Criteria for LLM review:**
- Coordinates >450 km from expected country
- City geocoded to wrong country
- Ambiguous names with multiple global matches

**Performance:**
- 178 validated correct (78%)
- 49 corrected (21%)
- 1 error/fallback (0.4%)

**Notable corrections:**
| Location | Original | Corrected | Distance |
|----------|----------|-----------|----------|
| Heraklion, Greece | Egypt (31.30°N, 30.12°E) | Crete (35.34°N, 25.14°E) | 450 km |
| Vitoria, Spain | Brazil (-20.32°S, -40.31°W) | Basque (42.85°N, -2.67°W) | 8,600 km |
| Kos, Greece | Indonesia (-7.89°S, 110.06°E) | Dodecanese (36.89°N, 27.29°E) | 9,400 km |

**Manual corrections:** 525 hard-coded fixes for recurring issues

**Capital fallback:** 23,430 records (11.3%) assigned capital coordinates when city name missing

### ETER Matching (Notebook 03)
**Algorithm:**
1. Filter ETER institutions within same country
2. Calculate haversine distance to all candidates
3. Keep institutions within 50 km
4. Compute fuzzy name similarity (token sort ratio)
5. Apply minimum 60% similarity threshold
6. Score: 0.6 × name_similarity + 0.4 × (1 - normalized_distance)
7. Select best match per institution

**Name matching features:**
- Compares both English and national language names
- Handles abbreviations (e.g., "Univ" ↔ "University")
- Ignores word order
- Case-insensitive

**Coverage notes:**
- 4.4% match rate reflects Erasmus composition (majority non-HEI)
- Secondary schools, government agencies, NGOs excluded from ETER
- Among universities, match rate >70% in major countries

### ROR Validation (Notebook 04)
**Purpose:** Independent verification of ETER matching quality

**Method:**
1. Download ROR database (2,666 EU education institutions)
2. For each ETER match, find nearest ROR institution in same country
3. Calculate distance and name similarity
4. Classify: Confirmed (<5 km, >70% similarity) vs. Different vs. No coverage

**Key finding:** 86.3% "different institutions" validates precision
- Example: Vienna — University of Vienna (ETER) vs. IMADEC University (ROR), 0.09 km apart
- Demonstrates algorithm distinguishes co-located institutions

### Eurostat Integration (Notebook 05)
**Data sources:**
| Dataset | Code | Indicator | Unit | Coverage |
|---------|------|-----------|------|----------|
| GDP NUTS3 | nama_10r_3gdp | GDP per capita | EUR_HAB | 2000-2024 |
| GDP NUTS2 | nama_10r_2gdp | GDP per capita | EUR_HAB | 2000-2024 |
| GDP Country | nama_10_gdp | GDP per capita | CP_MPPS_EU27_2020 | 2000-2024 |
| Crime NUTS2 | crim_gen_reg | Recorded offences | P_HTHAB (per 100k) | 2008-2023 |
| Population NUTS3 | demo_r_pjangrp3 | Total population | NR | 2014-2025 |

**Matching logic:**
- Extract year from `Mobility Start Month` (YYYY-MM format)
- Derive NUTS2 (first 4 chars) and NUTS0 (first 2 chars) from NUTS3
- Left join on (NUTS code, Year) for each indicator
- Separate columns for sending and receiving regions

**Data cleaning:**
- Deduplicate Eurostat extracts (single unit per indicator)
- Validate no row explosion during merge
- Handle missing values (non-EU countries, suppression)

**Hierarchical coverage:** If NUTS3 missing → NUTS2 → Country PPS

## 📊 Comparison to Prior Work

| Feature | Gadár et al. (2020) | Väisänen et al. (2025) | **This Work** |
|---------|---------------------|------------------------|---------------|
| **Temporal** | 2008-2014 | 2014-2022 | **2014-2023** |
| **Records** | ~180,000 | 2,191,857 | **206,448** |
| **NUTS3** | No | Yes (96%) | **Yes (99.94%)** |
| **LAU** | No | Yes | No |
| **ETER** | Yes (2014 data) | No | **Yes (2022 data)** |
| **GRID** | Yes | No | No |
| **ROR** | No | No | **Yes (validation)** |
| **Eurostat** | No | No | **Yes (5 indicators)** |
| **Validation** | Standard | Manual sampling | **LLM + Manual + ROR** |
| **Economic data** | No | No | **Yes (annual 2014-2023)** |

**Our contributions:**
- Highest NUTS3 coverage through multi-stage validation
- Only dataset with institutional characteristics AND regional economics
- Updated ETER data (2022 vs. 2014)
- Independent ROR validation
- Captures Brexit + COVID-19 period

## 🛠️ Technical Notes

**Software environment:**
- Python 3.12
- Key libraries: pandas 2.x, geopandas 0.14.x, geopy 2.x

**Coordinate systems:**
- Storage: WGS84 (EPSG:4326)
- Spatial operations: ETRS89-LAEA Europe (EPSG:3035)

**NUTS classification:**
- Version: 2021 boundaries
- Note: May not align with historical boundaries for 2014-2020 mobility

**Missing data handling:**
- Represented as `NaN` in pandas
- Preserved in CSV output (empty cells)
- No imputation performed

**Data integrity:**
- All merges validated to preserve 206,448 record count
- No many-to-many joins
- No row duplication

**Performance:**
- Dataset size: ~200 MB CSV
- RAM requirement: 4+ GB for full processing
- Chunking available for memory-constrained systems

## 🐛 Troubleshooting

### Installation Issues

**Problem:** `pip install -r requirements.txt` fails
```bash
# Solution: Upgrade pip
pip install --upgrade pip
pip install -r requirements.txt
```

**Problem:** `geopandas` installation fails
```bash
# Solution: Install system dependencies first (Ubuntu/Debian)
sudo apt-get install gdal-bin libgdal-dev
pip install geopandas
```

### Notebook Errors

**Problem:** `FileNotFoundError` when running notebooks
- **Cause:** File paths not updated in configuration cell
- **Solution:** Edit paths in first code cell of each notebook

**Problem:** Geocoding fails or returns empty results
- **Cause:** Internet connection issues or API rate limits
- **Solution:** 
  - Check internet connection
  - Nominatim has 1 req/sec limit (enforced in code)
  - ArcGIS may block excessive requests (use cache)

**Problem:** LLM validation notebook fails
- **Cause:** Ollama not installed or model not pulled
- **Solution:**
```bash
# Install Ollama: https://ollama.ai
ollama pull llama3.2:1.5b
# Restart notebook kernel
```
- **Alternative:** Skip notebook 02, use pre-corrected file from Figshare

**Problem:** Eurostat downloads fail or timeout
- **Cause:** Eurostat API temporary issues
- **Solution:** 
  - Retry after waiting 5-10 minutes
  - Check Eurostat server status: https://ec.europa.eu/eurostat
  - Use cached files from Figshare if available

**Problem:** `MemoryError` during processing
- **Cause:** Insufficient RAM (<4 GB)
- **Solution:**
  - Close other applications
  - Process data in chunks (modify notebook code)
  - Use a machine with more RAM

### Data Quality Issues

**Problem:** High number of missing NUTS3 codes
- **Expected:** 129 records (0.06%) due to non-EU countries
- **Investigation:** Check `missing_nuts3_records.csv` in outputs

**Problem:** Low ETER match rate
- **Expected:** 4.4% reflects Erasmus composition (many non-HEI participants)
- **Not an error:** Majority are schools, agencies, NGOs not in ETER

**Problem:** Missing Eurostat indicators
- **Expected:** Varies by country (non-EU, statistical suppression)
- **Mitigation:** Use hierarchical fallback (NUTS3 → NUTS2 → Country)

## 👥 Authors

**Dénes Kiss**  
PhD Candidate  
University of Pannonia, Faculty of Business and Economics  
Veszprém, Hungary  
📧 kiss.denes@gtk.uni-pannon.hu  
🔗 [ORCID](https://orcid.org/YOUR-ORCID-HERE)

**Zsolt Tibor Kosztyán**  
Associate Professor  
University of Pannonia, Faculty of Business and Economics  
Veszprém, Hungary  
📧 kosztyan.zsolt@gtk.uni-pannon.hu  
🔗 [ORCID](https://orcid.org/HIS-ORCID-HERE)

## 🙏 Acknowledgments

### Data Sources
- **European Commission** — Erasmus+ Programme mobility data via EU Open Data Portal
- **Eurostat** — NUTS3 2021 boundaries (GISCO) and regional socioeconomic indicators
- **ETER Project** — European Tertiary Education Register
- **ROR** — Research Organization Registry (validation dataset)

### Tools & Services
- **ArcGIS World Geocoding Service** — Primary geocoding provider
- **Nominatim/OpenStreetMap** — Fallback geocoding service
- **Ollama** — Local LLM inference platform
- **Llama 3.2** — Open-source language model for coordinate validation

### Funding
*[Add funding information if applicable]*

## 🤝 Contributing

Contributions are welcome! Areas for enhancement:

**Data enrichment:**
- Additional institutional databases (QS, THE rankings)
- Cultural distance variables (Hofstede dimensions)
- Extended Eurostat indicators (unemployment, R&D)
- Temporal extension to 2024-2027

**Methodology:**
- Alternative geocoding providers (Google, MapBox)
- Improved name matching algorithms
- Automated validation pipelines

**Applications:**
- Network analysis examples
- Visualization dashboards
- Statistical modeling templates
- Machine learning applications

**How to contribute:**
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📞 Support

**Questions or issues?**

1. **GitHub Issues:** [Open an issue](https://github.com/yrv5wg/Erasmus-mobility-2014-2023/issues)
2. **Email:** kiss.denes@gtk.uni-pannon.hu
3. **Institution:** University of Pannonia

**Before opening an issue:**
- Check existing issues (open and closed)
- Review [Troubleshooting](#-troubleshooting) section
- Include error messages, code snippets, and environment details

## 🔗 Related Resources

### Erasmus+ Programme
- **Official website:** https://erasmus-plus.ec.europa.eu/
- **EU Open Data Portal:** https://data.europa.eu/

### Data Infrastructures
- **ETER Portal:** https://www.eter-project.com/
- **Eurostat GISCO:** https://ec.europa.eu/eurostat/web/gisco
- **NUTS Classification:** https://ec.europa.eu/eurostat/web/nuts
- **ROR:** https://ror.org/

### Related Datasets
- **Gadár et al. (2020):** [Scientific Data](https://doi.org/10.1038/s41597-020-0382-1)
- **Väisänen et al. (2025):** [Scientific Data](https://doi.org/10.1038/s41597-025-04789-0)

### Tools
- **GeoPandas:** https://geopandas.org/
- **Ollama:** https://ollama.ai/
- **Eurostat Python package:** https://pypi.org/project/eurostat/

## 📅 Version History

### v1.0.0 (2026-03-20) — Initial Release
- 206,448 mobility records (2014-2023)
- 70 variables (14 original + 56 enriched)
- 99.94% NUTS3 coverage
- 1,986 ETER institutional matches
- Complete Eurostat integration (5 indicators, annual 2014-2023)
- 5 documented Jupyter notebooks
- ROR validation report

### Future Releases
- v1.1.0: Extended Eurostat indicators (planned)
- v1.2.0: Temporal extension to 2024 (when available)

---

**Repository Status:**  
✅ Dataset published on Figshare  
📝 Manuscript under review at Scientific Data  
🔄 Code actively maintained  
⭐ Star this repo to track updates

**Last updated:** 2026-03-20
