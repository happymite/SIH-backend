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

---

## System Architecture

```text
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

                         ↓ Phase 3 ↓

                    Thermal Stress Engine
                         ↓
                    Risk Assessment
                         ↓
                    HeatSense Alerts
