# HeatSense — Hyperlocal Heat Stress Early Warning Backend

HeatSense is a FastAPI backend for a hyperlocal, ward-level heat stress early warning system focused on Haldia, West Bengal.

The current backend combines:

- Live weather data from Open-Meteo
- Haldia area coordinates
- GIS-verified mapping of H-areas to municipal wards
- Census of India 2011 ward-level population data
- GIS-derived ward areas
- Population-density indicators
- 72-hour weather forecasts

The backend is being developed as the data and API foundation for the HeatSense heat-stress risk engine.

---

## Current Development Status

### Phase 2 — Data & Weather Integration

**Status: Complete and validated**

The current backend supports:

1. H-area coordinate management
2. Live Open-Meteo weather ingestion
3. 26 Haldia municipal ward records
4. GIS-based H-area → ward mapping
5. Census 2011 ward population data
6. GIS-derived ward areas
7. Ward-level population density
8. Combined weather + demographic API responses
9. 72-hour weather forecasting

### Upcoming — Phase 3

The next development stage will add:

- Thermal-stress calculations
- Heat Index / WBGT-related metrics
- Ward-level risk scoring
- Risk categories
- Explainable alert drivers
- ML-based risk/prediction components where scientifically justified

These components are not yet part of the production API.

## System Architecture

                    ┌─────────────────────┐
                    │   Haldia Areas      │
                    │ haldia_areas.json   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ GIS Ward Mapping    │
                    │ H-area → Ward       │
                    └──────────┬──────────┘
                               │
              ┌────────────────┴────────────────┐
              │                                 │
              ▼                                 ▼
   ┌─────────────────────┐          ┌─────────────────────┐
   │ Census 2011 Data    │          │ Open-Meteo API      │
   │ Population / Wards  │          │ Temperature / RH    │
   └──────────┬──────────┘          │ Wind / Forecast     │
              │                     └──────────┬──────────┘
              ▼                                │
   ┌─────────────────────┐                     │
   │ Ward Density Data   │                     │
   └──────────┬──────────┘                     │
              │                                │
              └────────────────┬───────────────┘
                               ▼
                    ┌─────────────────────┐
                    │   FastAPI Backend   │
                    │       main.py       │
                    └──────────┬──────────┘
                               │
                               ▼
                    Weather + Ward + 
                    Demographic Response
Tech Stack
 Python 3.9+
 FastAPI
 Uvicorn
 HTTPX
 Open-Meteo
 JSON
 GIS / GeoJSON / KMZ boundary data
 Census of India 2011 data      


Currently used weather variables:

Air temperature
Relative humidity
Wind speed
Forecast time



Haldia Ward Boundaries

Municipal ward boundaries are derived from the Haldia Municipal Boundary 2012 KMZ dataset.

The GIS dataset contains 26 municipal wards.

Ward areas used by the backend are calculated from the GIS boundary polygons rather than manually entered estimates.



Running the Backend
1. Create the virtual environment
python -m venv venv
2. Activate the environment
.\venv\Scripts\activate
3. Install dependencies
pip install -r requirements.txt
4. Start FastAPI
python -m uvicorn main:app --reload

The development server will normally be available at:

http://127.0.0.1:8000
Testing the API
Get all areas
curl.exe "http://127.0.0.1:8000/api/areas"
Get weather for an area
curl.exe "http://127.0.0.1:8000/api/weather?area_id=H01"
Validation
Population-Density Validation

Run:

python validate_population_density.py

The validation checks:

Exactly 26 ward records
Ward numbers 1–26
No duplicate wards
Positive population values
Positive GIS areas
Correct population-density calculation
No missing required fields

Current result:

7 passed, 0 failed
Integration Validation

Run:

python validate_integration.py

The integration validation checks:

All 30 H-areas are present
Original latitude/longitude values are preserved
Ward numbers are valid
Ward 1 population and density
Ward 26 population and density
Total Census population

Current result:

8 passed, 0 failed
Important Data Integrity Notes
Census and GIS

Population values and ward boundaries come from separate source datasets:

Population: Census of India 2011
Boundaries/areas: Haldia Municipal Boundary 2012 GIS dataset

Ward population density is calculated by combining these datasets.

H-area Coordinates

The existing H-area coordinates are preserved because they are used by the Open-Meteo weather integration.

Synthetic Indicators

The file:

app/data/haldia_indicators.json

contains earlier demonstration/synthetic indicators.

These values should not be treated as measured or official Haldia data and should not be used as real-world training data without appropriate replacement or validation.

