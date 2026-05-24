# Where Should BU Students Live?
### A Data-Driven Neighborhood Recommendation System for Boston University Off-Campus Housing

---

![Neighborhood Map](outputs/Screenshot%2843%29.png)

## The Problem

Finding off-campus housing as a BU student is overwhelming. Apartments.com and Zillow show you hundreds of listings but give you no real guidance on where to actually live — they just show you what's available.

I built this project for my friends (and anyone else) who are moving off campus for the first time and don't know where to start. Instead of browsing endless listings, this system scores Boston neighborhoods based on what actually matters to a BU student.

---

## How It Works

Each neighborhood is scored across four metrics:

- **Proximity to BU** (30%) — distance in miles from neighborhood center to BU's main campus
- **Affordability** (25%) — average monthly rent from Zillow's 2026 rental index
- **Transit Access** (20%) — number of MBTA rapid transit stops within 1 mile
- **Safety** (15%) — crime incident density from Boston Police Department data
- **Amenities** (10%) — number of restaurants, cafes, and grocery stores nearby

Each metric is normalized to a 0-1 scale and combined into a final neighborhood score.

---

## Key Findings

After analyzing 236,000+ real crime incidents, MBTA transit data, Zillow rent prices, and OpenStreetMap amenity data, the top neighborhoods for BU students are:

| Neighborhood | Score | Avg Rent | Distance to BU |
|---|---|---|---|
| Fenway | 0.86 | $3,764/mo | 0.62 miles |
| Allston | 0.79 | $3,654/mo | 1.31 miles |
| Brookline | 0.76 | $4,362/mo | 1.53 miles |
| Brighton | 0.70 | $3,032/mo | 2.54 miles |

**Fenway** wins overall — closest to BU and best transit, worth the price if budget allows. 

**Allston** is the sweet  spot — close, affordable, and packed with student amenities. 

**Brighton** is the best budget pick — $600/month cheaper on average than Fenway with a manageable commute. 

**Brookline** is a quieter, slightly pricier alternative with excellent transit along the Green Line.
---

## Data Sources

| Dataset | Source | Records |
|---|---|---|
| Crime Incidents | Boston Police Department (data.boston.gov) | 236,000+ incidents (2023-2026) |
| Rent Prices | Zillow Research (ZORI Index) | 25 Boston ZIP codes |
| MBTA Stations | MassGIS | 170 rapid transit stops |
| Neighborhood Boundaries | BPDA (data.boston.gov) | 26 neighborhoods |
| Amenities | OpenStreetMap (osmnx) | 1,199 restaurants, cafes, groceries |

---

## Project Structure
```
bu-neighborhood-recommender/
├── data/
│   ├── raw/          ← original downloaded datasets
│   └── processed/    ← cleaned and merged outputs
├── notebooks/
│   ├── 01_data_collection.ipynb    ← loading and cleaning datasets
│   ├── 02_spatial_join.ipynb       ← GeoPandas spatial analysis
│   ├── 03_scoring_model.ipynb      ← feature engineering and scoring
│   └── 04_visualizations.ipynb     ← maps and charts
├── outputs/
│   ├── neighborhood_map.html       ← interactive Folium map
│   ├── neighborhood_comparison.png ← bar chart comparisons
│   └── radar_chart.png             ← neighborhood profile radar chart
└── requirements.txt
```

---

## How to Run
```bash
git clone https://github.com/Kpop-Smoke/bu-neighborhood-recommender.git
cd bu-neighborhood-recommender
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

Then download the following datasets and place them in `data/raw/`:
- Crime data: [Boston Police Department](https://data.boston.gov/dataset/crime-incident-reports-august-2015-to-date-source-new-system)
- Neighborhood boundaries: [BPDA](https://data.boston.gov/dataset/bpda-neighborhood-boundaries)
- MBTA stations: [MassGIS](https://www.mass.gov/info-details/massgis-data-mbta-rapid-transit)
- Rent data: [Zillow Research](https://www.zillow.com/research/data/)

Run notebooks in order: 01 → 02 → 03 → 04

---

## Limitations & Notes

- **Brookline crime data** was estimated using the average crime count of Allston, Fenway, and Brighton since Brookline has its own police department separate from Boston PD and no open dataset was available
- **Rent data** for 6 neighborhoods (Harbor Islands, Longwood, Bay Village, Leather District, Downtown, Mattapan) was unavailable from Zillow and excluded from the model — these neighborhoods are not typical BU student housing areas anyway
- **Neighborhood scoring weights** are subjective and reflect priorities typical of a BU student on a budget — someone who prioritizes safety over rent would get different results
- **Fixed BU campus point** — all distance calculations use BU's main campus center (Commonwealth Ave). Students with classes on East Campus or at the medical campus on the South End may get different results in practice
- **Neighborhood centroids vs boundaries** — transit scores use full neighborhood boundary shapes rather than centroids to account for large neighborhoods like Dorchester having T stops near their edges but not their center
- **Static snapshot** — rent prices reflect January 2026 Zillow data and crime data runs through early 2026. Rankings may shift as the city changes


## Future Work

- **Personalized commute scoring** — let users select their specific class building (CAS, Questrom, Engineering, Medical Campus) and recalculate BU distance based  on their actual schedule rather than a fixed campus center point
  
- **Adjustable weight sliders** — build a Streamlit web app where students input their priorities (e.g. "I care most about rent") and get a personalized neighborhood ranking instantly
  
- **Roommate cost splitting** — add a "split by X roommates" calculator so students can see per-person monthly cost rather than total apartment rent
  
- **Apartment-level recommendations** — instead of neighborhood scores, integrate with a listings API to rank individual apartments on the market right now
  
- **Brookline real crime data** — scrape or request Brookline PD's crime data directly to replace the current estimate with real figures
  
- **Seasonal rent trends** — use Zillow's full time series data to show how rent fluctuates by month so students know the best time to sign a lease

---

## Tools & Libraries

Python, pandas, GeoPandas, scikit-learn, Folium, osmnx, matplotlib, requests

---

*Built by a BU sophomore who watched too many friends stress about where to live.*
