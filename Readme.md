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
The full enriched dataset and all source/output files are available on Figshare:

**DOI:** [10.6084/m9.figshare.31718764](https://doi.org/10.6084/m9.figshare.31718764)

### Source Files Required
Download these files from our Figshare repository: [10.6084/m9.figshare.31718764](https://doi.org/10.6084/m9.figshare.31718764)

**Files included:**
1. **Erasmus-KA2-Mobility-Data.xlsx** - Raw mobility data (EU Open Data Portal)
2. **eter-export-selected.xlsx** - ETER database export
3. **erasmus_with_nuts3.csv** - Geocoded with NUTS3 codes
4. **erasmus_eter_linkage.csv** - Institution mapping table
5. **erasmus_final_complete.csv** - Final enriched dataset ✅

**Additional files to download separately:**
- **NUTS3 Shapefile**: [Eurostat GISCO](https://ec.europa.eu/eurostat/web/gisco/geodata/reference-data/administrative-units-statistical-units/nuts) - Download `NUTS_RG_01M_2021_3035.shp` (+ .dbf, .shx, .prj)
- **ROR data**: Auto-downloaded by notebook 04
- **Eurostat datasets**: Auto-downloaded by notebook 05

## Usage

### Quick Start
1. Download source files from Figshare (links above)
2. Download NUTS3 shapefile from Eurostat GISCO
3. Update file paths in each notebook's configuration cell
4. Run notebooks sequentially (01 → 05)
5. Each notebook produces outputs needed by the next

**Estimated runtime:** 2-4 hours total (including Eurostat downloads)

### Configuration
Each notebook has a configuration cell at the top. Update these paths before running:
```python
# Example from notebook 01
ERASMUS_DATA_PATH = '/path/to/Erasmus-KA2-Mobility-Data.xlsx'
NUTS3_SHAPEFILE_PATH = '/path/to/NUTS_RG_01M_2021_3035.shp'
OUTPUT_DIR = '/path/to/output/directory/'
```

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

## Data Variables

### Core Variables (Original)
- Project ID, Mobility ID
- Sending/Receiving Organization, City, Country
- Participant Profile, Duration, Start Month
- Total Participants

### Added: Geographic (Notebooks 01-02)
- Sending/Receiving Latitude, Longitude
- Sending/Receiving NUTS3, NUTS2, NUTS0 codes
- Geocoding source, validation status

### Added: Institutional (Notebook 03)
- ETER ID, Name (English, National)
- Academic staff (FTE)
- Core revenues, Third-party funding (EUR)
- Publications
- Match distance, name similarity

### Added: Socioeconomic (Notebook 05)
- Year (extracted from mobility start month)
- GDP per capita (NUTS3, NUTS2, Country-PPS)
- Crime rate (recorded offences per 100k, NUTS2)
- Population (NUTS3)
- All indicators available for both sending and receiving locations

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

## Methodology Details

### Geocoding Strategy (Notebook 01)
- **Primary:** ArcGIS World Geocoding Service
- **Fallback:** Nominatim (OpenStreetMap)
- **Parallel processing:** 20 threads
- **Spatial matching:** Haversine distance to NUTS3 centroids, then spatial join for final assignment

### LLM Validation (Notebook 02)
- **Model:** deepseek-r1:1.5b via Ollama
- **Use case:** 228 locations with suspicious coordinates (>450 km from expected country)
- **Notable corrections:** Heraklion Egypt→Crete (450km), Vitoria Brazil→Spain (8600km), Kos Indonesia→Greece (9400km)
- **Manual corrections:** 525 hard-coded coordinate fixes for problematic locations
- **Capital fallback:** Pre-validated NUTS3 codes for capital cities

### ETER Matching (Notebook 03)
- **Distance threshold:** 50 km maximum
- **Name similarity:** Minimum 60% (token sort ratio)
- **Combined score:** 60% name similarity + 40% proximity
- **Quality metrics:** 81.4% avg similarity, 6.18 km avg distance

### ROR Validation (Notebook 04)
- **Purpose:** Independent validation of ETER matches
- **Coverage:** 107/1,986 confirmed (5.4%)
- **Criteria:** <5 km distance + >70% name similarity
- **Result:** Validates matching precision, demonstrates ability to distinguish nearby institutions

### Eurostat Integration (Notebook 05)
- **Data sources:** 5 Eurostat datasets via API
- **Matching logic:** NUTS region code + year
- **GDP units:** EUR_HAB (NUTS3, NUTS2), PPS (country-level for comparability)
- **Crime:** ICCS0101 total offences per 100k inhabitants
- **Population:** Total (all ages, both sexes)
- **Coverage period:** 2000-2024 (filtered to 2014-2023 for matching)

## Comparison to Prior Work

### Gadár et al. (2020) - Scientific Data
- **Coverage:** 2008-2014
- **Enhancements:** ETER + GRID institutional linkage
- **Our extension:** 2014-2023, NUTS3 geocoding, Eurostat indicators

### Väisänen et al. (2025) - Scientific Data
- **Coverage:** 2014-2022, 2.19M movements
- **Geocoding:** LAU + NUTS3 (Photon/Nominatim), 96% accuracy
- **Limitations:** Systematic diacritic failures, no institutional data, no economic indicators
- **Our advantages:** 99.94% geocoding accuracy, LLM validation, ETER linkage, pre-integrated Eurostat data

## Research Applications

This dataset enables analysis of:
- Regional economic factors in student mobility patterns
- Network analysis of inter-institutional collaborations
- Impact of regional development on educational exchange
- Temporal trends in mobility and socioeconomic correlations
- Institutional characteristics and internationalization patterns
- Crime and safety perceptions in mobility decisions
- GDP disparities and mobility direction flows

## Technical Notes

- **Coordinate system:** WGS84 (EPSG:4326)
- **NUTS version:** 2021 boundaries
- **Missing data:** Coded as NaN in pandas, preserved in CSV output
- **Year extraction:** From 'Mobility Start Month' field (YYYY-MM format)
- **Merge validation:** All merges preserve original 206,448 record count (no row explosion)

## Troubleshooting

### Common Issues

**"File not found" errors:**
- Update file paths in configuration cells
- Ensure all source files are downloaded

**Geocoding fails:**
- Check internet connection (ArcGIS and Nominatim require API access)
- Nominatim rate limit: 1 request/second (built into code)

**LLM validation not working:**
- Install Ollama: https://ollama.ai
- Pull model: `ollama pull deepseek-r1:1.5b`
- Alternative: Skip notebook 02 and use pre-corrected file from Figshare

**Eurostat download slow:**
- First run downloads ~500 MB of data
- Subsequent runs use cached CSV files
- Patience required (15-20 minutes)

**Memory errors:**
- Dataset requires ~4 GB RAM
- Close other applications
- Consider using chunked processing for very large operations

## Authors

**Dénes Kiss**  
University of Pannonia, Faculty of Business and Economics  
Veszprém, Hungary  
📧 kiss.denes@gtk.uni-pannon.hu

**Zsolt Tibor Kosztyán**  
University of Pannonia, Faculty of Business and Economics  
Veszprém, Hungary  
📧 kosztyan.zsolt@gtk.uni-pannon.hu

## License

- **Code:** MIT License (see LICENSE file)
- **Data:** CC BY 4.0 (available on Figshare)

## Acknowledgments

### Data Sources
- **European Commission** - Erasmus+ Programme mobility data
- **Eurostat GISCO** - NUTS3 2021 administrative boundaries
- **ETER Project** - European Tertiary Education Register
- **Eurostat** - Regional socioeconomic indicators
- **ROR** - Research Organization Registry (validation)

### Tools & Services
- **ArcGIS** - Primary geocoding service
- **Nominatim/OpenStreetMap** - Fallback geocoding
- **Ollama** - LLM inference for validation
- **deepseek-r1** - Open-source language model

### Funding
*[Add funding information if applicable]*

## Version History

- **v1.0** (2026-03-13): Initial release
  - 206,448 records
  - 70 variables
  - 99.94% NUTS3 coverage
  - Complete Eurostat integration

## Support

For questions, issues, or collaboration inquiries:

1. **GitHub Issues:** [Open an issue](https://github.com/yrv5wg/Erasmus-mobility-2014-2023/issues)
2. **Email:** kiss.denes@gtk.uni-pannon.hu
3. **Institution:** University of Pannonia, Faculty of Business and Economics

## Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

Areas for contribution:
- Additional validation methods
- Alternative geocoding providers
- Extended Eurostat indicators
- Network analysis examples
- Visualization tools

## Related Resources

- **Erasmus+ Programme:** https://erasmus-plus.ec.europa.eu/
- **ETER Portal:** https://www.eter-project.com/
- **Eurostat GISCO:** https://ec.europa.eu/eurostat/web/gisco
- **NUTS Classification:** https://ec.europa.eu/eurostat/web/nuts
- **ROR:** https://ror.org/

---

**Status:** ✅ Dataset published | 📝 Paper under review | 🔄 Code maintained

**Last updated:** 2026-03-13
