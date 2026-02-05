# Warsaw Clean Transport Zone (SCT) Air Quality Analysis

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

An in-depth statistical analysis investigating the relationship between road traffic intensity and air quality in Warsaw, Poland, with a focus on the **Clean Transport Zone (SCT - Strefa Czystego Transportu)** that came into effect on **July 1, 2024**.

![SCT Map](/warsaw_stations_map.html)

## Table of Contents

- [Research Questions](#research-questions)
- [Key Findings](#key-findings)
- [Data Sources](#data-sources)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Methodology](#methodology)
- [Results Summary](#results-summary)
- [Limitations](#limitations)
- [License](#license)

## Research Questions

1. Is there a statistically significant correlation between traffic volume and pollutant concentrations (NO₂, PM2.5, PM10)?
2. Did the introduction of the SCT measurably affect air quality in Warsaw?
3. What external factors (weather, seasonality) influence these relationships?
4. What is the optimal time lag between traffic changes and pollution levels?

## Key Findings

| Pollutant | Correlation with Traffic | Optimal Lag | Post-SCT Change |
|-----------|-------------------------|-------------|-----------------|
| **NO₂** | Moderate positive (r ≈ 0.32-0.37) | +1 hour | -2.6% |
| **PM2.5** | Weak/negligible | +2 hours | -4.0% |
| **PM10** | Weak/negligible | Variable | -6.0% |

### Main Conclusions

- **NO₂ shows moderate positive correlation** with traffic (r ≈ 0.32-0.37), strongest in winter months
- **PM2.5 and PM10 show weak/negligible correlations**, suggesting other dominant sources (residential heating, construction)
- **Optimal lag for NO₂ is +1 hour** (r = 0.391, p < 0.001), indicating direct local emission relationship
- **Post-SCT reductions observed** are statistically significant but require caution due to confounding variables (weather, seasonal trends, COVID recovery)

## Data Sources

| Source | Data Type | Period | Resolution |
|--------|-----------|--------|------------|
| [GIOŚ](https://powietrze.gios.gov.pl/) | Air quality (NO₂, PM2.5, PM10) | 2019-2024 | Hourly |
| [ZTM Warsaw](https://zdm.waw.pl/) | Traffic volume | 2019-2024 | Hourly |
| [Open-Meteo API](https://open-meteo.com/) | Weather conditions | 2019-2024 | Hourly |

### Air Quality Monitoring Stations

| Station Code | Location | Type |
|--------------|----------|------|
| MzWarAlNiepo | al. Niepodległości | Traffic/roadside |
| MzWarSolid | ul. Solidarności | Traffic/roadside |
| MzWarGroch | ul. Grochowska | Traffic/roadside |

### Traffic Measurement Clusters

| Cluster | Station IDs | Paired Air Station |
|---------|-------------|-------------------|
| Grochowska | 5305, 5306, 5307 | MzWarGroch |
| Al. Niepodległości | 5315, 5316, 5317, 5368 | MzWarAlNiepo |
| Solidarności | 5325, 5326, 5327 | MzWarSolid |

## Project Structure

```
warsaw-sct-analysis/
├── warsaw-sct-analysis.ipynb    # Main analysis notebook
├── mapa_stacji_warszawa.html          # Interactive map of stations
├── SCT.kmz                            # SCT boundary KML file, manually created
├── requirements.txt                   # Python dependencies
├── README.md                          # This file
│
├── data/
│   ├── raw_data/
│   │   ├── air_data/                  # GIOŚ CSV files (2019-2024)
│   │   ├── traffic_data/              # ZTM Excel files
│   │   └── weather_data/              # Weather data cache
│   └── interim_data/                  # Processed/cleaned data (not included, auto-creation during notebook execution)
│       ├── air_data/
│       ├── traffic_data/
│       └── weather_data/
```

## Installation

### Prerequisites

- Python 3.8 or higher
- Jupyter Notebook or JupyterLab

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/warsaw-sct-analysis.git
   cd warsaw-sct-analysis
   ```

2. **Create a virtual environment** (recommended)
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Launch Jupyter**
   ```bash
   jupyter notebook warsaw-sct-analysis.ipynb
   ```

## Usage

### Running the Full Analysis

Open `warsaw-sct-analysis.ipynb` in Jupyter and execute all cells sequentially. The notebook is organized into the following sections:

1. **Introduction** - Research objectives and hypotheses
2. **Helper Functions** - Data loading and processing utilities
3. **Data Preparation** - Load and clean all data sources
4. **Exploratory Data Analysis** - Visualize temporal patterns and validate station pairings
5. **Seasonal & Monthly Analysis** - Correlation breakdown by time period
6. **Advanced Analysis** - Lag analysis, weather-controlled correlations, SCT impact assessment

### Data Requirements

The analysis expects data in the following locations:

- **Air quality data**: `data/raw_data/air_data/{year}/` - CSV files from GIOŚ
- **Traffic data**: `data/raw_data/traffic_data/` - Excel files from ZDM
- **Weather data**: Automatically fetched from Open-Meteo API and cached

### Interactive Map

Open `warsaw_stations_map.html` in a browser to view an interactive Folium map showing:
- SCT boundary polygon
- Air quality monitoring stations
- Traffic measurement stations
- Station metadata on hover

## Methodology

### Statistical Methods

| Analysis | Method | Purpose |
|----------|--------|---------|
| Correlation | Pearson's r with p-values | Measure linear relationship strength |
| Trend Analysis | LOWESS smoothing | Identify long-term patterns |
| Lag Analysis | Cross-correlation (0-12 hours) | Find optimal traffic-pollution delay |
| Seasonal Decomposition | Month/season grouping | Account for temporal confounders |
| Before/After Comparison | Two-sample t-test | Assess SCT policy impact |

### WHO Air Quality Guidelines (Reference)

| Pollutant | WHO 24-hour Limit | Unit |
|-----------|-------------------|------|
| NO₂ | 25 | µg/m³ |
| PM2.5 | 15 | µg/m³ |
| PM10 | 45 | µg/m³ |

## Results Summary

### Correlation Analysis

<img width="1189" height="590" alt="image" src="https://github.com/user-attachments/assets/23b42f8b-7e03-441f-9406-d5e29e5fb90a" />


### SCT Impact Analysis

The before/after SCT comparison shows:

| Metric | Pre-SCT (Jan-Jun 2024) | Post-SCT (Jul+ 2024) | Change |
|--------|------------------------|----------------------|--------|
| Mean NO₂ | 38.74 µg/m³ | 37.75 µg/m³ | -2.6% |
| Mean PM2.5 | 16.02 µg/m³ | 15.38 µg/m³ | -4.0% |
| Mean PM10 | 30.86 µg/m³ | 29.01 µg/m³ | -6.0% |
| Traffic Volume | 5148 veh/h | 5105 veh/h | -0.8% |

**Caution**: These results should be interpreted carefully. The observed improvements may be influenced by:
- Seasonal weather patterns (summer typically has better dispersion)
- Year-over-year variation
- Post-COVID traffic recovery patterns
- Construction and other local activities

## Limitations

1. **Short post-SCT period** - Only ~6 months of data available after July 1, 2024
2. **Confounding variables** - Weather, seasonality, economic factors affect both traffic and pollution
3. **Spatial limitations** - Analysis limited to 3 air quality stations within SCT
4. **Historical traffic data gaps** - 2019-2023 traffic data is sparse (annual snapshots)
5. **No control group** - Cannot compare with areas outside SCT

## Future Work

- [ ] Incorporate additional air quality stations outside SCT as control group
- [ ] Apply difference-in-differences methodology
- [ ] Include vehicle fleet composition data
- [ ] Extend analysis as more post-SCT data becomes available
- [ ] Add source apportionment modeling for PM

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- **GIOŚ** (Chief Inspectorate of Environmental Protection) for air quality monitoring data
- **ZDM Warsaw** for traffic measurement data
- **Open-Meteo** for the free weather API
- **Warsaw City Hall** for SCT boundary information

## Author

**Norbek**

---

*This analysis was conducted as part of an academic project examining urban environmental policy effectiveness.*
