# BEGUSARAI FLOOD MODEL

## Overview

This file contains the Begusarai Flood Model, a Python-based tool that extracts and processes flood depth data for various scenarios and return periods. The model integrates data from multiple sources, performs statistical analysis, and generates useful flood risk outputs.

---

## Setup Instructions

### 1. Install Dependencies

Ensure all required Python libraries are installed before running the scripts:

```bash
pip install numpy pandas geopandas rioxarray xarray openpyxl scipy pyproj
```

### 2. Mount Google Drive (If using Colab)

If running in Google Colab, mount your Google Drive to access required datasets:

```python
from google.colab import drive
drive.mount('/content/drive')
```

### 3. File Structure
#### Project Directory Structure
Organize files in the following structure before running the model:

```
📁 Pavan_GEE-Dygnify/
│
├── 📁 Begusarai_18Locations/
│   ├── 📝 InputLatLongs.xlsx  (🔹 Input: Location coords)
│   ├── 📝 Input_sheet_CFRS_Begusarai.xlsx  (🔹 Input: CFRS)
│   ├── 📝 FloodDepthsExtracted.xlsx  (🔹 Input: flood depth data)
│   ├── 📝 Begusarai_IndicesValuesDownload_09-02-2025.xlsx  (🔹 Input: Indices value)
│   ├── 📝 Updated_Begusarai_IndicesValuesDownload.xlsx  (🔹 Input: Updated Indices value)
│   ├── 📝 Updated_Flood_Model_Output_for_COGs_Begurasai.xlsx  (🔹 Output: Updated Flood Model)
│   ├── 📝 output_return_periods_Begurasai.xlsx  (🔹 Output: return-period flood depths)
|   ├── 📝 Hazard_score_Sheet_Begusarai.xlsx  (🔹 Output: Hazard score sheet)
|   ├── 📝 ProjAssetFinancialRiskScore_Begusarai.xlsx  (🔹 Output: Fianacial Risk Score)
│   ├── 📁 Output245_Fluvial/  (🔹 Output SSP245 Fluvial Data)
│   ├── 📁 Output585_Fluvial/  (🔹 Output SSP585 Fluvial Data)
│   ├── 📁 Output245_Heat/  (🔹Output SSP245 Heat Data)
│   ├── 📁 Output585_Heat/  (🔹Output SSP585 Heat Data)
│   ├── 📁 Output245_Drought/  (🔹 Output SSP245 Drought Data)
│   ├── 📁 Output585_Drought/  (🔹Output SSP585 Drought Data)
│
├── 📁 Extraction_FloodDepths/  (🔹 Climate Raster Files)
│   ├── 🌍 rcp60_mean_cog.tif  (🔹 SSP245)
│   ├── 🌍 rcp85_mean_cog.tif  (🔹 SSP585)
│
├── 📁HazardScore_Flood/  (🔹 Flood Risk Analysis)
│   ├── 📝 District_vul.xlsx  (🔹 Input: District vulnerability data)
│   ├── 📝 Flood_Model_input.xlsx  (🔹Input: Flood Model data )
│   ├── 📝 IndiaShapefileInWGS84.shp  (🔹Input: India Shape file)
|
├── 📁Netcdfs_HeatStress/  (🔹 Heat Risk Analysis)
│   ├── 🌍ERA5_Ta_Td_1981-2023_India.nc  (🔹Input: Historical Temperature data)
│   ├── 🌍 cmip6_ensemble_ssp245_hurs_india_final.nc  (🔹Input: Future Humidity Projections)
│   ├── 🌍 cmip6_ensemble_ssp245_tas_india_final.nc  (🔹Input: Future Temperature Projections, ssp245)
│   ├── 🌍 cmip6_ensemble_ssp245_tas_india_final.nc  (🔹Input: Future Temperature Projections, ssp245)
│   ├── 🌍 cmip6_ensemble_ssp585_tas_india_final.nc  (🔹Input: Future Temperature Projections, ssp585)
│   ├── 🌍 cmip6_ensemble_ssp585_tas_india_final.nc  (🔹Input: Future Temperature Projections, ssp585)
|
├── 📁 All_Hazards_CFRS/
│   ├── 📝 Drought_Model_Input.xlsx  (🔹 Input: Drought model data)
│   ├── 📝 Heat_Model_Input.xlsx  (🔹 Input: Heat model data)
│
├── 📁 notebooks/  (🔹 Jupyter Notebooks for data processing)
│   ├── 📄 Begusarai.ipynb  (🔹 Jupyter Notebook performing hazard extraction)
│
├── 📜 README.md  (🔹 Documentation & Standard Operating Procedure)
├── 📜 requirements.txt  (🔹 Python dependencies)
```

---

## Workflow Execution

### Step 1: Extract Flood Depth Data

The `Flood Depths Extraction` cell loads input locations and extracts flood depths from COG files.

```python
python Scripts/flood_extraction.py
```

Output: `FloodDepthsExtracted.xlsx`

### Step 2: Compute Return Periods

Using statistical distribution fitting (gumbel_r fit), the model computes flood depths for various return periods and target yeras.

The `Extracting the Return Periods and Target Years` cell loads extracts flood depths for return periods and target years.

```python
python Scripts/return_period.py
```

Output: `output_return_periods.xlsx`

### Step 3: Create the Flood model output sheet for FLRF and FLSW

Creating a new sheet similar to Flood_Model_Input.xlsx, with output_return_periods_final.csv as input.
Defining FLRF and FLSW from MasterNomenclature, and Damage functions and Scores from Flood_Model_Input.xlsx sheets will be incorporated in new sheet.
New sheet contains FLRF_max values for every return period for each scenario.

The 'Flood model Output sheet for FLRF and FLSW' cell creates the new sheet which has Scenario wise and target year wise flood depths.

```python
python Scripts/Flood_model_Output_sheet.py
```
Output: `Flood_Model_Output_for_COGs_Begurasai.xlsx`

### Step 4: Flood risk analysis and Damage Assessment 

The  cells 'Fluvial 245, Fluvial 585, Pluviual 245 and Pluvial 585' performs flood risk analysis and damage assessment by extracting flood depths, estimating damages, and computing exposure, vulnerability, and risk scores fro each locations.

```python
python Scripts/Fluvial_245.py
python Scripts/Fluvial_585.py
python Scripts/Pluvial_245.py
python Scripts/Pluvial_245.py
```

Output: `Output245_Fluvial`
 `Output585_Fluvial`
 `Output245_Pluvial`
 `Output585_Pluvial`

### Step 5: Generate Hazard Score Sheet

The cell 'Final Flood Output sheet' integrates the outputs from 'Output245_Fluvial', 'Output585_Fluvial`, 'Output245_Pluvial` and 'Output585_Fluvial` and generates a Final; hazard Score sheet

```python
python Scripts/flood_output_sheet.py
```

Output : `Hazard_score_Sheet_Begusarai.xlsx`

### Step 6: Heat Risk Assessment

The cells 'Heat 245 and Heat 585' performs heat risk analysis by processing historical and future temperature data, computing work loss due to heat stress, and estimating vulnerability and exposure factors.
Load heat model lookup tables 'Heat_Model_Input.xlsx').

```python
python Scripts/Heat245.py
python Scripts/Heat585.py
```
Output : `Output245_Heat`
 `Output585_Heat`

### Step 7: Drought Risk Assessment

The cells 'Drought 245 and Drought 585' analyzes drought and water stress risk by integrating industry-specific vulnerabilities, social factors, and exposure to drought indicators.

```python
python Scripts/Heat245.py
python Scripts/Heat585.py
```
Output : `Output245_Drought`
 `Output585_Drought`

### Step 8: Processing Final Risk Scores

The cells 'Financial Risk Score' processes climate-related risk scores (drought, heat stress, flood, water stress) by extracting values from multiple Excel sheets generated and organizing them into a structured output.

```python
python Scripts/financial_risk_score.py
```
Output : `ProjAssetFinancialRiskScore_Begusarai.xlsx`

## Key Features

✅ Extracts flood depths for multiple locations from raster datasets.
✅ Computes flood risk using Generalized Extreme Value (GEV) distribution.
✅ Supports return periods of 20, 50, 100, 200, 500, and 1500 years.
✅ Estimates Flood Depths, Heat, Drought and Water Stress
✅ Generates Excel outputs for visualization and further analysis.

---


