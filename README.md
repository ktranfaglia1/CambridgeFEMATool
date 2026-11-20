# Cambridge Flood Analysis Tool

**A Python desktop GIS application that visualizes flood and storm-surge scenarios for Cambridge, Maryland, and estimates their financial impact.**

<p align="center"><img src="AnalysisTool/Cambridge_logo.png" alt="City of Cambridge logo" width="140"></p>

I built this during a software engineering internship with the **City of Cambridge, Maryland** (February–April 2025). Cambridge is a low-lying waterfront city in Dorchester County that is highly exposed to hurricanes and storm surge. The city wanted a clearer, interactive way to explore FEMA flood-model predictions and what they would cost.

The tool combines LiDAR and digital elevation model (DEM) data with flood simulation and cost-benefit modeling in a single interface, aimed at local decision-makers rather than GIS specialists.

## Features

Navigation runs through a sidebar with four areas.

### Flood simulators
- **3D Elevation:** renders Dorchester County terrain from DEM rasters in 3D. Raise the water level by projected year and hurricane category (1–5) to see which areas flood. Includes preset camera angles and export of the 3D view.
- **3D Surface:** a LiDAR-based surface model with terrain, buildings and vegetation, using the same year and category flood controls.
- **Street View:** a street-level map of Cambridge with flood-scenario controls.
- **Interactive Map:** a Folium/Leaflet map of the city with flood-risk markers and a legend.

### Damage Estimator
- **Property damage:** for a given property value, estimates building, contents and infrastructure losses under each hurricane category, using vulnerability curves and per-category damage factors.
- **Economic impact:** estimates citywide effects such as displacement, debris and cleanup costs. Building mix and impact weights are adjustable.
- Results appear in tables and Matplotlib charts, and download as CSV.

### Insurance Projections
- Enter an address and home value.
- The address is checked with geocoding (geopy/Nominatim, on a background thread).
- The tool projects annual insurance costs up to 100 years out, combining a base rate, inflation, and hurricane frequency and intensity rising toward NOAA-based projections. It charts cost alongside expected Category 3 and 4 storms, and exports the table and graph.

## Tech stack

- **Python 3** with **PyQt6**, plus **QtWebEngine** for the embedded web maps
- **Geospatial:** rasterio (raster I/O, masking, reprojection), Shapely, Folium, geopy
- **Analysis and plotting:** NumPy, pandas, Matplotlib (2D charts and 3D surfaces)
- **Performance:** threaded data loading with on-disk caching (pickle), so large DEM tiles load once

## Running it

```bash
pip install PyQt6 PyQt6-WebEngine matplotlib numpy pandas folium rasterio shapely geopy
cd AnalysisTool
python AnalysisTool.py
```

The app uses relative paths, so start it from inside `AnalysisTool/`.

**Data:** the large LiDAR and DEM datasets aren't in the repository. To use real terrain, place them at:

```
Data/Dorchester_LiDAR/          # LiDAR raster tiles
Data/Dorchester_DEM/dorc2015_m/ # 2015 Dorchester County DEM
```

If no LiDAR tiles are found, the 3D Surface view falls back to synthetic terrain so the interface can still be explored. The Interactive Map's markers are illustrative and are not drawn from a dataset.

## Repository structure

```
├── AnalysisTool/
│   ├── AnalysisTool.py     # The application: simulators, damage estimator, insurance projections
│   └── Cambridge_logo.png
├── TestPrograms/           # Prototypes: DEM rendering, GeoPandas, Folium-in-PyQt, .mat/NetCDF model readers
└── Data/WorldCountries/    # Base-map shapefile
```

## Author

**[Kyle Tranfaglia](https://github.com/ktranfaglia1)**: Software Engineering Intern, City of Cambridge, MD (2025).
