## Spatial Outlier Detection in Enugu State Election Results

### Overview

This project applies geospatial analytics and statistical methods to detect potential anomalies in polling unit-level election results across Enugu State, Nigeria.
The analysis aims to identify polling units whose voting results significantly deviate from their geographic neighbours, supporting the integrity and transparency of electoral data.

This work was conducted as part of the **HNG i13 Data Analytics Internship – Stage 2B Task: Outlier Detection in Election Data Using Geospatial Analysis**.

---

### Objectives

1. **Dataset Preparation**

   * Clean and validate the Enugu State election dataset.
   * Add missing geographic coordinates using geocoding techniques.

2. **Neighbour Identification**

   * Identify neighbouring polling units within a 1 km radius using geodesic distance.

3. **Outlier Score Calculation**

   * Compute outlier scores for major parties (APC, LP, PDP, NNPP) based on deviations from neighbouring units.

4. **Sorting and Reporting**

   * Rank polling units by outlier scores to highlight anomalies.
   * Produce interactive maps and a summarized analytical report.

---

### Methodology

#### 1. Data Cleaning and Geocoding

The dataset was obtained from the official HNG internship repository. Missing coordinates were added using the **OpenCage Geocoder API**, ensuring every polling unit had valid latitude and longitude values.

The dataset was cleaned by:

* Removing duplicates
* Standardizing column names
* Ensuring valid numeric values for vote counts

#### 2. Neighbour Identification

Neighbouring polling units were identified using the **Haversine formula** (geodesic distance) within a 1 km radius.

Example function used:

```python
from geopy.distance import distance

def find_neighbours(lat, lon, df, radius_km=1):
    neighbours = df.apply(
        lambda row: distance((lat, lon), (row['Latitude'], row['Longitude'])).km <= radius_km,
        axis=1
    )
    return df[neighbours]
```

#### 3. Outlier Score Computation

For each political party, an outlier score was calculated as a standardized Z-score:

[
\text{Outlier Score} = \frac{(x_i - \bar{x}*{neighbors})}{\sigma*{neighbors}}
]

Where:

* (x_i) = votes in the polling unit
* (\bar{x}_{neighbors}) = mean votes of neighbouring units
* (\sigma_{neighbors}) = standard deviation of neighbouring votes

Units with **high positive scores** indicate unusually high vote counts compared to their surrounding units.

#### 4. Sorting and Exporting

Each party’s polling units were sorted in descending order of their outlier scores and exported for review:

* `with_outlier_scores.csv` – complete dataset with computed scores
* `sorted_outliers.xlsx` – party-specific sorted sheets (APC, LP, PDP, NNPP)

---

### Visualization

Geospatial visualizations were developed using **Folium**.

Each map marks outlier polling units using **red circular markers**.
Popups display polling unit details such as:

* PU Name
* LGA
* Ward
* Outlier Score

Generated visualizations:

* `Top 10 APC_outliers`
* `Top 10 LP_outliers`
* `Top 10 PDP_outliers`
* `Top 10 NNPP_outliers`

---

### Results Summary

The spatial analysis revealed a number of polling units with voting results that deviated sharply from their neighbours:

| Party    | Observation                                                                                                                              |
| -------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **APC**  | Outliers appeared across both urban and rural LGAs, suggesting mixed regional voting behaviours.                                         |
| **LP**   | Concentrated high outlier scores in urban areas such as Enugu North and Nsukka, reflecting potential anomalies or voter density effects. |
| **PDP**  | Moderate deviations observed, with scattered anomalies across wards.                                                                     |
| **NNPP** | Few isolated outliers, possibly due to smaller party presence or inconsistencies in data entry.                                          |

All computed values are stored in `with_outlier_scores.csv`, while per-party rankings are available in `sorted_outliers.xlsx`.

---

### Tools and Libraries

* **Python**
  Libraries: `pandas`, `numpy`, `geopy`, `folium`
* **Jupyter Notebook** for analysis workflow
* **Excel / CSV** for exporting results

---

### Deliverables

1. `with_outlier_scores.csv` – Final dataset with computed outlier scores
2. `sorted_outliers.xlsx` – Sorted outlier sheets for APC, LP, PDP, NNPP
3. `[party]_outliers_map.html` – Interactive folium maps per party
4. `Spatial_Outlier_Detection_Enugu.docx` – Final analytical report

---

### Insights and Recommendations

* Polling units with extreme outlier scores should undergo **manual review** and **data validation**.
* LGAs containing multiple high outliers may warrant closer examination by electoral auditors.
* Continuous improvement in geographic data precision is necessary for future elections.
* Advanced methods such as **Local Outlier Factor (LOF)** or **Moran’s I spatial correlation** could enhance future analyses.

---

### Author

**Enoch Chukwuebuka**
Data Analytics Track – HNG i13 Internship (Stage 2B)
October 2025
