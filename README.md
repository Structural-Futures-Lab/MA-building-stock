# Massachusetts Building Analysis Dashboard

An interactive web-based visualization dashboard for analyzing Massachusetts building inventory data from the NSI-Enhanced USA Structures Dataset. This comprehensive tool provides multi-dimensional analysis of 1.68M+ buildings with advanced clustering, temporal patterns, and geospatial visualizations.

## Live Demo

[View Live Dashboard](https://structural-futures-lab.github.io/MA-building-stock/#overview)

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Data Pipeline](#data-pipeline)
- [Installation](#installation)
- [Usage Guide](#usage-guide)
- [Data Structure](#data-structure)
- [Methodology](#methodology)
- [Technologies](#technologies)
- [Support](#support)

## Overview

This dashboard visualizes and analyzes the complete Massachusetts building inventory, integrating data from multiple authoritative sources:
- **USA Structures**: 2,091,488 building footprints
- **National Structure Inventory (NSI)**: Detailed building characteristics (building materials, built year and foundation materials, etc.)
- **Web Soil Survey**: Soil-related details
- **Boston Permit Dataset**: Demolition records (Boston only)
- **MassGIS**: Year built records.
- **CLF (Carbon Leadership Forum)**: Building embodied carbon and structural data

The final dataset contains **72 columns** of building attributes

## Key Features

### 11 Interactive Analysis Sections

#### 1. **Overview Dashboard**
- Real-time statistics for 1.68M cleaned buildings
- Interactive occupancy distribution visualizations
- Construction timeline from 1630 to 2024
- Multi-level hierarchical Sankey diagrams
- A Sampled 75,000-point interactive map with dynamic filtering

#### 2. **Data Pipelines & Processing**
- Visual representation of multi-source data integration
- Enhanced data cleaning funnel with detailed step tracking:
  - Year filtering
  - Material/Foundation type filtering (removes unmatched USA Structure polygons)
  - Area filtering (Est GFA sqmeters)
  - Height filtering (HEIGHT and PRED_HEIGHT validation)
- Unclassified building reclassification algorithm using OCC_DICT voting
- NSI point-to-polygon spatial join visualization
- NSI data sources methodology diagram (Lightbox coverage analysis)
- Year source comparison charts (NSI vs MassGIS)

#### 3. **Simple Clustering Analysis**
- K-means clustering (K=5 to 9)
- Real pre-computed clusters on full dataset
- Elbow method optimization
- Interactive 3D scatter plots
- Cluster statistics and treemap visualizations
- Geographic distribution map of clusters

#### 4. **Temporal Distribution**
- Annual construction patterns analysis
- 4 visualization modes (Stacked, Line, Normalized, Cumulative)
- Building type filters (All/Residential/Non-Residential)
- Total floor area trends over time

#### 5. **Multi-Dimensional Occupancy Clustering**
- 4D to 6D dynamic clustering:
  - Base (4D): Year Built, Footprint Area (SQMETERS), Height (HEIGHT_USED), Occupancy Class
  - +Material Type (5D)
  - +Foundation Type (5D)
  - +Both (6D)
- Feature selection toggles for Material/Foundation
- Balanced vs Random sampling (up to 20,000 points)
- Pre-computed clustering for all feature combinations
- Real-time reclustering based on selected features

#### 6. **Materials & Foundation Analysis**
- Interactive correlation heatmaps (Count and GFA views)
- Click-through occupancy breakdowns
- Material usage evolution (1630-2024)
- Count and area-based visualizations
- Multiple breakdown chart types (Pie, Bar, Horizontal Bar)

#### 7. **Soil Properties & Risk Assessment**
- Drainage class analysis
- Water table depth distribution
- Engineering property evaluation
- Soil Component Name (compname) analysis with Top 20 coverage
- A sampled 75,000-point risk mapping
- High-risk building identification
- Risk overlay toggle for map visualization

#### 8. **Boston Historic Shoreline**
- Buildings on land reclaimed since 1630
- Interactive historic map overlay with 1630 shoreline
- Filled land construction patterns
- Material/foundation analysis on reclaimed areas
- Filtering by Occupancy, Material, and Foundation types
- Color-coded visualization options

#### 9. **Boston Foundation-Height Statistics**
- Comprehensive analysis of foundation types by building height
- Original Land vs Shoreline (Filled) Land comparison
- **Section 1**: Side-by-side comparison for selected height bin
- **Section 2**: Height bin comparison within same land type
- **Section 3**: Complete data overview (collapsible)
- **Section 4**: CLF Metadata height vs foundation analysis
- CLF Foundation Type collapse toggle for grouped view
- Height bins: <0 ft, 0-24 ft, 24-72 ft, 72-147 ft, 147+ ft

#### 10. **CLF Data Analysis**
- CLF (Carbon Leadership Forum) building data integration
- Data preprocessing documentation:
  - Building use mapping to NSI categories
  - Structural system mapping to single-letter codes
- Scatter plot explorer with configurable axes:
  - Est GFA sqmeters, mass_total, gwp_a_to_c
  - Log scale toggle
- 4 Heatmap variations:
  - Mapped Material vs. Foundation (Count)
  - Mapped Material vs. Foundation (GFA)
  - Structural System vs. Foundation (Count)
  - Structural System vs. Foundation (GFA)
- GFA distribution comparison: Main dataset vs CLF dataset (box plot overlay)

#### 11. **Interactive Data Explorer** 
- Custom visualization builder
- Flexible axis and color mapping
- Tips for effective data exploration

## Data Pipeline

### Processing Stages

```
Stage 1: Spatial Join Enhancement
├── Input: 2,091,488 USA Structures + 2,095,529 NSI Points
├── Process: Multi-stage intelligent matching
│   ├── Strategy 1: Single-family one-to-one matching
│   ├── Strategy 2: Multi-unit aggregation
│   └── Strategy 3: 5-meter buffer nearest neighbor
└── Output: 1,686,451 matched buildings (80.63% match rate)

... (Further stages can be read in Data Pipelines Section of the Dashboard)
```

## Installation

### Quick Start

1. **Clone the repository**
```bash
git clone https://github.com/your-username/MA-building-stock.git
cd MA-building-stock
```

2. **File Structure Required**
```
MA-building-stock/
├── index.html                              # Main dashboard
├── building_data.json                      # Core dataset
├── clf_data.json                           # CLF analysis data
├── building_data_samples_random_*.json     # Random samples (15 files)
├── building_data_samples_balanced_*.json   # Balanced samples (4 files)
├── historic_shoreline_buildings.json       # Boston shoreline data
├── boston_shoreline_1630.png              # Historic map image
└── README.md                               # This file
```

3. **Launch the Dashboard**

Direct website opening
```bash
# Open https://samueeelsiu.github.io/MA-building-stock/
```


## Usage Guide

### Navigation

1. Use the top navigation tabs to switch between analysis sections
2. Each section has its own control panel for filtering and customization
3. Hover over any data point for detailed information
4. Click on charts for interactive features

### Key Interactions

- **Map Controls**: Zoom with scroll, pan with drag, filter with dropdowns
- **3D Plots**: Rotate with mouse, zoom with scroll
- **Sankey Diagrams**: Click nodes for details, drag to reposition
- **Heatmaps**: Click cells for occupancy breakdown
- **Foundation Analysis**: Use radio buttons to compare different height bins and land types
- **CLF Heatmaps**: Toggle between Count and GFA metrics
- **Export**: Use export buttons for PNG/JSON downloads

## Data Structure

### Main Dataset Schema

```javascript
{
  "metadata": {
    "total_buildings": 1686451,
    "version": "...",
    "samples_files": [...],      
    "date_processed": ...
  },
  "summary_stats": {
    "total_buildings": 1686451,
    "avg_year_built": 1962,
    "avg_area_sqm": 350,
    "occupancy_classes": [...]
  },
  "hierarchical_distribution": {...},   // Sankey data (by_count, by_gfa, by_count_simplified)
  "year_occ_flow": {...},               // Year→Occ→Material→Foundation→Soil
  "temporal_data": [...],               // Time series
  "clustering": {...},                  // K-means results
  "occupancy_clusters_enhanced": {...}, // Multi-dimensional clustering results
  "soil_analysis": {
    "drainage_stats": {...},
    "flooding_freq_stats": {...},
    "water_table_stats": {...},
    "engineering_property_stats": {...},
    "compname_stats": {...},            // NEW: Soil component analysis
    "soil_by_occupancy": {...},
    "spatial_distribution": [...],
    "soil_risk_analysis": {...}
  },
  "data_flow_stats": {
    "cleaning_pipeline": {...},         // Detailed cleaning statistics
    "unclassified_resolution": {...}    // Reclassification results
  },
  "nsi_data_sources": {...},            // NSI methodology stats
  "boston_foundation_analysis": {...},  // NEW: Boston height-foundation data
  "occ_cls_to_occdict_sankey": {...}    // NEW: OCC_CLS → NSI occtype mapping
}
```

### CLF Data Schema 

```javascript
{
  "scatter_data": {
    "Est GFA sqmeters": [...],
    "mass_total": [...],
    "gwp_a_to_c": [...],
    "OCC_CLS": [...],
    "material_type": [...],
    "str_sys_summary": [...]
  },
  "heatmap_material_count": { "z": [...], "x": [...], "y": [...] },
  "heatmap_material_gfa": { "z": [...], "x": [...], "y": [...] },
  "heatmap_struct_count": { "z": [...], "x": [...], "y": [...] },
  "heatmap_struct_gfa": { "z": [...], "x": [...], "y": [...] }
}
```

### Building Attributes (Key Columns)

- **Identification**: BUILD_ID
- **Location**: LONGITUDE, LATITUDE, PROP_ADDR, PROP_CITY
- **Physical**: HEIGHT, HEIGHT_USED, SQMETERS, Est GFA sqmeters
- **Classification**: OCC_CLS, PRIM_OCC, MIX_SC
- **Construction**: year_built, material_type, foundation_type
- **Year Sources**: massgis_yr_built, nsi_yr_built, yr_built_belong
- **Soil**: drainagecl, wtdepannmin, flodfreqcl, eng_property, compname

## Technologies

### Frontend
- **HTML5/CSS3**: Responsive design with modern/professional themes
- **JavaScript ES6+**: Dynamic interactions and data processing
- **Plotly.js v2.27.0**: Advanced interactive visualizations

### Backend Processing
- **Python 3.x**
  - pandas: Data manipulation
  - scikit-learn: K-means clustering
  - geopandas: Spatial operations
  - numpy: Numerical computations
  - pyogrio + pyarrow: Fast GeoPackage I/O
  - PIL/Pillow: GeoTIFF processing for shoreline detection

### Data Formats
- **JSON**: Primary data exchange format
- **GeoPackage (.gpkg)**: Source geospatial data
- **CSV**: CLF and auxiliary data files
- **Excel (.xlsx)**: CLF metadata
- **GeoTIFF**: Historic shoreline raster

## Performance Notes

### Optimization Strategies

1. **Data Chunking**: Split into 30 files to handle GitHub's 25MB limit
2. **Pre-computed Clustering**: All clustering results pre-calculated for multiple K values and feature combinations
3. **Sampling**: Balanced and random samples for visualization
4. **Progressive Loading**: Lazy loading of sample chunks
5. **Fast I/O**: pyogrio with Arrow engine for instant GeoPackage loading

### Known Limitations

- Maximum 75,000 points displayed on maps simultaneously
- Soil data coverage: ~11,385 buildings lack soil information
- Real-time clustering limited to sample data
- CLF dataset limited to 16 MA projects after filtering

## Credits

### Development Team
- **Developer**: Lang (Samuel) Shao
- **Supervisor**: Prof. Demi Fang
- **Institution**: [Northeastern University](https://www.northeastern.edu/)
- **Lab**: [Structural Futures Lab](https://structural-futures.org/)

### Data Sources
- USA Structures Dataset
- National Structure Inventory (NSI)
- Web Soil Survey
- Boston Permits
- MassGIS
- Carbon Leadership Forum (CLF)

## Support

For issues, questions, or suggestions regarding this dashboard, please contact: shao.la@northeastern.edu
