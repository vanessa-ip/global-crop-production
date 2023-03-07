# Global Crop Production

### Table of Contents
- [Preparing the data](/global-crop-production-analysis.md#preparing-the-data)
- [Definitions](/global-crop-production-analysis.md#definitions)
- [Data analysis](/global-crop-production-analysis.md#data-analysis)
  - [Segment by crop](/global-crop-production-analysis.md#segment-by-crop)
  - [Segment by country](/global-crop-production-analysis.md#segment-by-country)
- [Conclusion](/global-crop-production-analysis.md#conclusion)
- [Recommendations](/global-crop-production-analysis.md#recommendations)

## Definitions

- **Actual yield**: crop yields measured in tonnes per hectare.


- **Attainable yield**: estimates of feasible crop yields calculated from high-yielding areas of similar climate. They are more conservative than biophysical
‘potential yields’, but should be achievable using current technologies and management (e.g. fertilizers and irrigation). Attainable yields are based on
assessments for the year 2000. Attainable yield pre-2000 may be lower; and post-2000 may be higher than these values.


- **Attainable yield gap**: measures the difference between actual and attainable yields. Attainable yields are estimates of feasible crop yields calculated from high-yielding
areas of similar climate. They are more conservative than biophysical ‘potential yields’, but should be achievable using current technologies and management
(e.g. fertilizers and irrigation).

---

## Preparing the Data

1. Unpivot the original table to transform all the separate 'crop'_attainable and 'crop'_yield_gap columns into two columns: yield (FLOAT64) for the numeric values, and crop (STRING) for the different types of crops

The original table [crops.csv](csv/crops.csv)

|    | Entity  | Year | barley_attainable |    cassava_attainable |    cotton_attainable |    groundnut_attainable | maize_attainable |  ... |
|---:|:--------|-----:|------------------:|----------------------:|---------------------:|------------------------:|-----------------:|-----:|
|  0 | Belarus | 1992 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  1 | Belarus | 1993 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  2 | Belarus | 1994 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  3 | Belarus | 1995 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  4 | Belarus | 1996 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  ... | ...     | ...  |             ...   |                   ... |                  ... |                     ... |              ... |  ... |


Steps:
```SQL
-- unpivot 'crop'_yield_gap columns into the new crop column using yield values 
SELECT Entity, 
    Year, 
    yield, 
    crop
FROM `crop-yields-world-data.crop_yields.crops`
unpivot(
  yield for crop in (barley_yield_gap, rye_yield_gap, millet_yield_gap, sorghum_yield_gap, maize_yield_gap,
  cassava_yield_gap, soybeans_yield_gap, rapeseed_yield_gap, sugarbeet_yield_gap, potato_yield_gap, oilpalm_yield_gap,
  groundnut_yield_gap, rice_yield_gap, sunflower_yield_gap, cotton_yield_gap, sugarcane_yield_gap, wheat_yield_gap)
) sub

UNION ALL

-- unpivot 'crop'_attainable columns into the new crop column using yield values 
SELECT Entity, 
    Year, 
    yield, 
    crop
FROM `crop-yields-world-data.crop_yields.crops`
unpivot(
  yield for crop in (barley_attainable, rye_attainable, millet_attainable, sorghum_attainable, maize_attainable,
  cassava_attainable, soybean_attainable, rapeseed_attainable, sugarbeet_attainable, potato_attainable, oilpalm_attainable,
  groundnut_attainable, rice_attainable, sunflower_attainable, cotton_attainable, sugarcane_attainable, wheat_attainable)
) sub

UNION ALL 

-- calculate the actual yield (attainable yield - yield gap) for each crop below, and create a row for the actual yield per country per year 
SELECT Entity, Year, (barley_attainable - barley_yield_gap) as yield, "barley_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (rye_attainable - rye_yield_gap) as yield, "rye_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (millet_attainable - millet_yield_gap) as yield, "millet_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (sorghum_attainable - sorghum_yield_gap) as yield, "sorghum_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (maize_attainable - maize_yield_gap) as yield, "maize_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (cassava_attainable - cassava_yield_gap) as yield, "cassava_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (soybean_attainable - soybeans_yield_gap) as yield, "soybean_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (rapeseed_attainable - rapeseed_yield_gap) as yield, "rapeseed_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (sugarbeet_attainable - sugarbeet_yield_gap) as yield, "sugarbeet_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (potato_attainable - potato_yield_gap) as yield, "potato_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (oilpalm_attainable - oilpalm_yield_gap) as yield, "oilpalm_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (groundnut_attainable - groundnut_yield_gap) as yield, "groundnut_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (rice_attainable - rice_yield_gap) as yield, "rice_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (sunflower_attainable - sunflower_yield_gap) as yield, "sunflower_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (cotton_attainable - cotton_yield_gap) as yield, "cotton_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (sugarcane_attainable - sugarcane_yield_gap) as yield, "sugarcane_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`

UNION ALL

SELECT Entity, Year, (wheat_attainable - wheat_yield_gap) as yield, "wheat_yield" as crop
FROM `crop-yields-world-data.crop_yields.crops`
```

Result:

The transformed table [unpivot_crops.csv](csv/unpivot_crops.csv)

|     | Entity  | Year | yield | crop   |
|----:|:--------|-----:|------:|:-------|
|   0 | Belarus | 1992 |  3.06 | rye_yield |
|   1 | Belarus | 1993 |   2.8 | rye_yield |
|   2 | Belarus | 1994 |  2.25 | rye_yield |
|   3 | Belarus | 1995 |  2.21 | rye_yield |
|   4 | Belarus | 1996 |  2.07 | rye_yield |
| ... | ...     |  ... | ...   | ...    |

Both tables are used in the following analyses. 

---

## Data Analysis 

### Segment by Crop
<br>

**QUESTION 1** 

**In the most recent year of the dataset (2018), what is the total actual yield per crop type and their percentage share among all types?** 

Steps: 
```SQL
-- find the global actual yield of each crop type in 2018
SELECT crop, 
ROUND(SUM(yield),2) as global_yield,
ROUND((SUM(yield)/SUM(SUM(yield)) OVER()),2) as pct_share
FROM `crop-yields-world-data.crop_yields.unpivot_crops`
WHERE crop LIKE "%yield" AND Year = 2018 
GROUP BY crop ORDER BY global_yield DESC
```
Result:

In 2018, sugarcane is the highest crop yield in the world, followed by potato and sugar beet. 

|    | crop            |   global_yield |   pct_share |
|---:|:----------------|---------------:|------------:|
|  0 | sugarcane_yield |        4589.72 |        0.35 |
|  1 | potato_yield    |        2450.04 |        0.19 |
|  2 | sugarbeet_yield |        2442.07 |        0.19 |
|  3 | cassava_yield   |         690.99 |        0.05 |
|  4 | maize_yield     |         527.14 |        0.04 |
|  5 | oilpalm_yield   |         420.94 |        0.03 |
|  6 | rice_yield      |         387.82 |        0.03 |
|  7 | wheat_yield     |         328.48 |        0.03 |
|  8 | barley_yield    |         243.67 |        0.02 |
|  9 | sorghum_yield   |         172.18 |        0.01 |
| 10 | soybean_yield   |         149    |        0.01 |
| 11 | rye_yield       |         146.27 |        0.01 |
| 12 | groundnut_yield |         136.39 |        0.01 |
| 13 | rapeseed_yield  |         122.47 |        0.01 |
| 14 | cotton_yield    |         114.08 |        0.01 |
| 15 | sunflower_yield |          97.14 |        0.01 |
| 16 | millet_yield    |          83.76 |        0.01 |

<br>

**QUESTION 2**

**Expanding on question 1, for each crop type, what is the decade-to-decade growth rate of actual yield, and the total growth rate of actual yield from the decade of 1969-1978 to 2009-2018?**  

Steps: 
```SQL
WITH cte_decades as ( -- find the global actual yield of each crop type in each decade and combine results using UNION ALL 
  SELECT crop, -- find global actual yield of each crop between 2009 - 2018
  ROUND(SUM(yield),2) as global_yield,
  ROUND((SUM(yield)/SUM(SUM(yield)) OVER()),2) as pct_share_of_crops, 
  "2009 - 2018" AS decade
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE crop LIKE "%yield" AND Year BETWEEN 2009 AND 2018 
  GROUP BY crop

  UNION ALL

  SELECT crop,  -- find global actual yield of each crop between 1999 - 2008
  ROUND(SUM(yield),2) as global_yield,
  ROUND((SUM(yield)/SUM(SUM(yield)) OVER()),2) as pct_share_of_crops, 
  "1999 - 2008" AS decade
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE crop LIKE "%yield" AND Year BETWEEN 1999 AND 2008 
  GROUP BY crop 

  UNION ALL

  SELECT crop,  -- find global actual yield of each crop between 1989 - 1998
  ROUND(SUM(yield),2) as global_yield,
  ROUND((SUM(yield)/SUM(SUM(yield)) OVER()),2) as pct_share_of_crops, 
  "1989 - 1998" AS decade
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE crop LIKE "%yield" AND Year BETWEEN 1989 AND 1998 
  GROUP BY crop 

  UNION ALL

  SELECT crop,  -- find global actual yield of each crop between 1979 - 1988
  ROUND(SUM(yield),2) as global_yield,
  ROUND((SUM(yield)/SUM(SUM(yield)) OVER()),2) as pct_share_of_crops, 
  "1979 - 1988" AS decade
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE crop LIKE "%yield" AND Year BETWEEN 1979 AND 1988 
  GROUP BY crop 

  UNION ALL

  SELECT crop,  -- find global actual yield of each crop between 1969 - 1978
  ROUND(SUM(yield),2) as global_yield,
  ROUND((SUM(yield)/SUM(SUM(yield)) OVER()),2) as pct_share_of_crops, 
  "1969 - 1978" AS decade
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE crop LIKE "%yield" AND Year BETWEEN 1969 AND 1978 
  GROUP BY crop 
  ORDER BY crop
)
, lag_yields as ( -- create a new column to LAG the global actual yield per crop 
SELECT *, 
LAG(global_yield) OVER(PARTITION BY crop order by decade) as lag_global_yield, 
FROM cte_decades 
)
, yield_growth_overtime as ( -- calculate the decade-to-decade growth rate of actual yield per crop 
SELECT crop, 
global_yield, 
ROUND(((global_yield/lag_global_yield)-1),2) as yield_growth,
pct_share_of_crops, 
decade FROM lag_yields
)
-- calculate the decade-to-decade running total of yield growth to find the total yield growth per crop
SELECT crop, global_yield, yield_growth, 
ROUND((SUM(yield_growth) 
    OVER(PARTITION BY crop ORDER BY decade ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)),2) 
    as running_total_yield_growth,
pct_share_of_crops, decade FROM yield_growth_overtime ORDER BY crop
```                                                                   
Result:                                                                    
    

|    | crop            | global_yield | yield_growth |   running_total_yield_growth | pct_share_of_crops | decade          |
|---:|:----------------|-------------:|-------------:|-----------------------------:|-------------------:|:----------------|
|  0 | barley_yield    |       1135.8 |          nan |                       nan    |               0.01 | 1969 - 1978     |
|  1 | barley_yield    |      1373.11 |         0.21 |                         0.21 |               0.02 | 1979 - 1988     |
|  2 | barley_yield    |      1829.07 |         0.33 |                         0.54 |               0.02 | 1989 - 1998     |
|  3 | barley_yield    |      2168.74 |         0.19 |                         0.73 |               0.02 | 1999 - 2008     |
|  4 | **barley_yield**    |  **2418.67** |     **0.12** |                         **0.85** |           **0.02** | **2009 - 2018** |
|  5 | cassava_yield   |      4698.11 |          nan |                       nan    |               0.06 | 1969 - 1978     |
|  6 | cassava_yield   |      4988.96 |         0.06 |                         0.06 |               0.06 | 1979 - 1988     |
|  7 | cassava_yield   |      5372.35 |         0.08 |                         0.14 |               0.05 | 1989 - 1998     |
|  8 | cassava_yield   |      6257.07 |         0.16 |                         0.3  |               0.05 | 1999 - 2008     |
|  9 | **cassava_yield**   |      **6741.28** |         **0.08** |                         **0.38** |               **0.05** | **2009 - 2018**     |
| 10 | cotton_yield    |       731.15 |          nan |                       nan    |               0.01 | 1969 - 1978     |
| 11 | cotton_yield    |       844.79 |         0.16 |                         0.16 |               0.01 | 1979 - 1988     |
| 12 | cotton_yield    |      1001.16 |         0.19 |                         0.35 |               0.01 | 1989 - 1998     |
| 13 | cotton_yield    |       1104.2 |          0.1 |                         0.45 |               0.01 | 1999 - 2008     |
| 14 | **cotton_yield**    |      **1160.55** |         **0.05** |                         **0.5**  |               **0.01** | **2009 - 2018**     |
| 15 | groundnut_yield |       836.23 |          nan |                       nan    |               0.01 | 1969 - 1978     |
| 16 | groundnut_yield |       896.28 |         0.07 |                         0.07 |               0.01 | 1979 - 1988     |
| 17 | groundnut_yield |      1031.48 |         0.15 |                         0.22 |               0.01 | 1989 - 1998     |
| 18 | groundnut_yield |      1185.53 |         0.15 |                         0.37 |               0.01 | 1999 - 2008     |
| 19 | groundnut_yield |      1334.04 |         0.13 |                         0.5  |               0.01 | 2009 - 2018     |
| 20 | maize_yield     |       1908.2 |          nan |                       nan    |               0.02 | 1969 - 1978     |
| 21 | maize_yield     |       2344.4 |         0.23 |                         0.23 |               0.03 | 1979 - 1988     |
| 22 | maize_yield     |      3277.36 |          0.4 |                         0.63 |               0.03 | 1989 - 1998     |
| 23 | maize_yield     |      4272.31 |          0.3 |                         0.93 |               0.04 | 1999 - 2008     |
| 24 | maize_yield     |      4970.55 |         0.16 |                         1.09 |               0.04 | 2009 - 2018     |
| 25 | millet_yield    |       456.66 |          nan |                       nan    |               0.01 | 1969 - 1978     |
| 26 | millet_yield    |       465.99 |         0.02 |                         0.02 |               0.01 | 1979 - 1988     |
| 27 | millet_yield    |       542.34 |         0.16 |                         0.18 |               0.01 | 1989 - 1998     |
| 28 | millet_yield    |       690.84 |         0.27 |                         0.45 |               0.01 | 1999 - 2008     |
| 29 | millet_yield    |       787.65 |         0.14 |                         0.59 |               0.01 | 2009 - 2018     |
| 30 | oilpalm_yield   |      2924.93 |          nan |                       nan    |               0.04 | 1969 - 1978     |
| 31 | oilpalm_yield   |      3606.49 |         0.23 |                         0.23 |               0.04 | 1979 - 1988     |
| 32 | oilpalm_yield   |      4065.57 |         0.13 |                         0.36 |               0.04 | 1989 - 1998     |
| 33 | oilpalm_yield   |      4373.28 |         0.08 |                         0.44 |               0.04 | 1999 - 2008     |
| 34 | oilpalm_yield   |      4254.45 |        -0.03 |                         0.41 |               0.03 | 2009 - 2018     |
| 35 | potato_yield    |      11149.1 |          nan |                       nan    |               0.14 | 1969 - 1978     |
| 36 | potato_yield    |      13632.3 |         0.22 |                         0.22 |               0.15 | 1979 - 1988     |
| 37 | potato_yield    |      17563.6 |         0.29 |                         0.51 |               0.16 | 1989 - 1998     |
| 38 | potato_yield    |      21233.5 |         0.21 |                         0.72 |               0.18 | 1999 - 2008     |
| 39 | potato_yield    |      24046.2 |         0.13 |                         0.85 |               0.19 | 2009 - 2018     |
| 40 | rapeseed_yield  |       441.18 |          nan |                       nan    |               0.01 | 1969 - 1978     |
| 41 | rapeseed_yield  |       553.06 |         0.25 |                         0.25 |               0.01 | 1979 - 1988     |
| 42 | rapeseed_yield  |          776 |          0.4 |                         0.65 |               0.01 | 1989 - 1998     |
| 43 | rapeseed_yield  |      1041.05 |         0.34 |                         0.99 |               0.01 | 1999 - 2008     |
| 44 | rapeseed_yield  |       1208.5 |         0.16 |                         1.15 |               0.01 | 2009 - 2018     |
| 45 | rice_yield      |      2120.57 |          nan |                       nan    |               0.03 | 1969 - 1978     |
| 46 | rice_yield      |      2439.68 |         0.15 |                         0.15 |               0.03 | 1979 - 1988     |
| 47 | rice_yield      |      2832.72 |         0.16 |                         0.31 |               0.03 | 1989 - 1998     |
| 48 | rice_yield      |      3261.36 |         0.15 |                         0.46 |               0.03 | 1999 - 2008     |
| 49 | rice_yield      |      3783.39 |         0.16 |                         0.62 |               0.03 | 2009 - 2018     |
| 50 | rye_yield       |       594.94 |          nan |                       nan    |               0.01 | 1969 - 1978     |
| 51 | rye_yield       |       690.53 |         0.16 |                         0.16 |               0.01 | 1979 - 1988     |
| 52 | rye_yield       |      1024.76 |         0.48 |                         0.64 |               0.01 | 1989 - 1998     |
| 53 | rye_yield       |      1289.61 |         0.26 |                         0.9  |               0.01 | 1999 - 2008     |
| 54 | rye_yield       |      1451.67 |         0.13 |                         1.03 |               0.01 | 2009 - 2018     |
| 55 | sorghum_yield   |       851.03 |          nan |                       nan    |               0.01 | 1969 - 1978     |
| 56 | sorghum_yield   |      1016.73 |         0.19 |                         0.19 |               0.01 | 1979 - 1988     |
| 57 | sorghum_yield   |      1250.07 |         0.23 |                         0.42 |               0.01 | 1989 - 1998     |
| 58 | sorghum_yield   |      1444.99 |         0.16 |                         0.58 |               0.01 | 1999 - 2008     |
| 59 | sorghum_yield   |      1611.84 |         0.12 |                         0.7  |               0.01 | 2009 - 2018     |
| 60 | soybean_yield   |       531.06 |          nan |                       nan    |               0.01 | 1969 - 1978     |
| 61 | soybean_yield   |       734.35 |         0.38 |                         0.38 |               0.01 | 1979 - 1988     |
| 62 | soybean_yield   |      1119.12 |         0.52 |                         0.9  |               0.01 | 1989 - 1998     |
| 63 | soybean_yield   |      1287.11 |         0.15 |                         1.05 |               0.01 | 1999 - 2008     |
| 64 | soybean_yield   |      1403.28 |         0.09 |                         1.14 |               0.01 | 2009 - 2018     |
| 65 | sugarbeet_yield |      12211.7 |          nan |                       nan    |               0.15 | 1969 - 1978     |
| 66 | sugarbeet_yield |      13309.9 |         0.09 |                         0.09 |               0.15 | 1979 - 1988     |
| 67 | sugarbeet_yield |      18049.8 |         0.36 |                         0.45 |               0.17 | 1989 - 1998     |
| 68 | sugarbeet_yield |      21443.8 |         0.19 |                         0.64 |               0.18 | 1999 - 2008     |
| 69 | sugarbeet_yield |      23032.8 |         0.07 |                         0.71 |               0.18 | 2009 - 2018     |
| 70 | sugarcane_yield |      38011.7 |          nan |                       nan    |               0.47 | 1969 - 1978     |
| 71 | sugarcane_yield |      41050.5 |         0.08 |                         0.08 |               0.45 | 1979 - 1988     |
| 72 | sugarcane_yield |      43895.1 |         0.07 |                         0.15 |               0.41 | 1989 - 1998     |
| 73 | sugarcane_yield |      45210.2 |         0.03 |                         0.18 |               0.38 | 1999 - 2008     |
| 74 | sugarcane_yield |      45074.5 |           -0 |                         0.18 |               0.35 | 2009 - 2018     |
| 75 | sunflower_yield |        389.7 |          nan |                       nan    |                  0 | 1969 - 1978     |
| 76 | sunflower_yield |       489.57 |         0.26 |                         0.26 |               0.01 | 1979 - 1988     |
| 77 | sunflower_yield |       689.09 |         0.41 |                         0.67 |               0.01 | 1989 - 1998     |
| 78 | sunflower_yield |       829.22 |          0.2 |                         0.87 |               0.01 | 1999 - 2008     |
| 79 | sunflower_yield |       915.01 |          0.1 |                         0.97 |               0.01 | 2009 - 2018     |
| 80 | wheat_yield     |      1471.35 |          nan |                       nan    |               0.02 | 1969 - 1978     |
| 81 | wheat_yield     |      1881.85 |         0.28 |                         0.28 |               0.02 | 1979 - 1988     |
| 82 | wheat_yield     |      2549.04 |         0.35 |                         0.63 |               0.02 | 1989 - 1998     |
| 83 | wheat_yield     |      2978.57 |         0.17 |                         0.8  |               0.02 | 1999 - 2008     |
| 84 | wheat_yield     |      3271.08 |          0.1 |                         0.9  |               0.03 | 2009 - 2018     |


<br>
                                                               
**QUESTION 3**

**For each type of crop, count the number of countries that produce the crop. Compare the change in the number of countries producing a crop from the decade of 1969-1978 to 2009-2018.** 

Steps:
```SQL
WITH num_countries_2018 as ( -- count the number of countries that produce for each crop from 2009-2018
SELECT crop, count(DISTINCT Entity) as num_countries_2009_2018
FROM `crop-yields-world-data.crop_yields.unpivot_crops`
WHERE crop LIKE "%attainable" AND yield != 0 AND Year BETWEEN 2009 AND 2018
GROUP BY crop ORDER BY num_countries_2009_2018 DESC
)
, num_countries_1978 as ( -- count the number of countries that produce for each crop from 1969-1978
SELECT crop, count(DISTINCT Entity) as num_countries_1969_1978
FROM `crop-yields-world-data.crop_yields.unpivot_crops`
WHERE crop LIKE "%attainable" AND yield != 0 AND Year BETWEEN 1969 AND 1978
GROUP BY crop ORDER BY num_countries_1969_1978 DESC
)
-- find the percentage change in number of countries producing for each crop from 1969-1978 to 2009-2018
SELECT *, ROUND(((num_countries_2009_2018/num_countries_1969_1978)-1),2) as change_1969_to_2018
 FROM num_countries_2018 INNER JOIN num_countries_1978 USING(crop)
 ORDER BY num_countries_2009_2018 DESC
```

Result:

<br> 


**QUESTION 4**

**For each crop, what is the average yield efficiency in the decade of 2009-2018?** The yield efficiency is the ratio of actual yield / attainable yield.

Steps:

```SQL
WITH act_yields as ( -- filter for the actual yield values in 2009-2018
  SELECT Entity,
  Year,
  SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, -- edit the crop name for joining later
  yield as act_yield,
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE (Year BETWEEN 2009 AND 2018) AND (crop LIKE "%yield")
)
, att_yields as ( -- filter for attainable yield values
  SELECT distinct Entity, 
  SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, -- edit the crop name for joining later
  yield as att_yield
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE crop LIKE "%attainable"
  ORDER BY Entity
) 
, eff_ratio as ( -- join the actual and attainable yield cte's and calculate the efficiency ratio
  SELECT Entity, crop, Year, NULLIF(act_yield, 0)/NULLIF(att_yield, 0) as ratio,
  FROM act_yields
  INNER JOIN att_yields 
  USING(Entity, crop)
)
-- find the average ratio per crop
SELECT crop, ROUND((avg(avg(ratio)) OVER(PARTITION BY crop)),2) as avg_ratio_per_crop 
FROM eff_ratio GROUP BY crop ORDER BY avg_ratio_per_crop DESC
```

Result:


**QUESTION 5**

**Expanding on question 4, for each crop, what is the change of the yield efficiency from the decade of 1969-1978 to 2009-2018?** 

Steps:

```SQL
WITH act_yields_2018 as ( -- filter for the actual yield values in 2009-2018
  SELECT Entity,
Year,
SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, -- edit the crop name for joining later
yield as act_yield,
FROM `crop-yields-world-data.crop_yields.unpivot_crops`
WHERE (Year BETWEEN 2009 AND 2018) AND (crop LIKE "%yield")
)
, att_yields as ( -- filter for attainable yield values 
  SELECT distinct Entity, 
SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, -- edit the crop name for joining later
yield as att_yield
FROM `crop-yields-world-data.crop_yields.unpivot_crops`
WHERE crop LIKE "%attainable"
ORDER BY Entity
) 
, eff_ratio_2018 as ( -- join the actual and attainable yield cte's and calculate the efficiency ratio for 2009-2018
  SELECT Entity, crop, Year, NULLIF(act_yield, 0)/NULLIF(att_yield, 0) as ratio,
FROM act_yields_2018
INNER JOIN att_yields 
USING(Entity, crop)
)
, avg_eff_ratio_2018 as ( -- find the average ratio per crop in 2009-2018
  SELECT crop, ROUND((avg(avg(ratio)) OVER(PARTITION BY crop)),2) as avg_ratio_per_crop_2018
FROM eff_ratio_2018 GROUP BY crop ORDER BY avg_ratio_per_crop_2018 DESC
)
, act_yields_1978 as ( -- filter for the actual yield values in 1969-1978
  SELECT Entity,
Year,
SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, -- edit crop name for joining later
yield as act_yield,
FROM `crop-yields-world-data.crop_yields.unpivot_crops`
WHERE (Year BETWEEN 1969 AND 1978) AND (crop LIKE "%yield")
) 
, eff_ratio_1978 as ( -- join the actual and attainble yield cte's and calculate the efficiency ratio for 1969-1978
  SELECT Entity, crop, Year, NULLIF(act_yield, 0)/NULLIF(att_yield, 0) as ratio,
FROM act_yields_1978
INNER JOIN att_yields
USING(Entity, crop)
)
, avg_eff_ratio_1978 as ( -- find the average ratio per crop in 1969-1978
  SELECT crop, ROUND((avg(avg(ratio)) OVER(PARTITION BY crop)),2) as avg_ratio_per_crop_1978 
FROM eff_ratio_1978 GROUP BY crop ORDER BY avg_ratio_per_crop_1978 DESC
)
-- join the avg_eff_ratio of 2009-2018 with 1969-1978 cte's, and calculate the change in ratio
SELECT crop, 
avg_ratio_per_crop_2018 as avg_ratio_per_crop_2009_2018,
avg_ratio_per_crop_1978 as avg_ratio_per_crop_1969_1978,
ROUND(((avg_ratio_per_crop_2018/avg_ratio_per_crop_1978)-1),2) as efficiency_ratio_growth
FROM avg_eff_ratio_2018 
INNER JOIN avg_eff_ratio_1978 
USING(crop) 
ORDER BY avg_ratio_per_crop_2018 DESC
```

Results:

---

### Segment by Country
<br>

**QUESTION 1** 

**QUESTION 2** 

**QUESTION 3** 

**QUESTION 4** 

**QUESTION 5** 






## Conclusion

## Recommendations
Policy makers and agricultural professionals would want to know about global crop yields for a variety of reasons:

Food security: Global crop yields are an important indicator of the world's food supply. Policy makers and agricultural professionals need to know about global crop yields to ensure that there is enough food to meet the needs of the world's population.

Economic development: Agriculture is a major sector of many countries' economies. Policy makers and agricultural professionals need to know about global crop yields to identify opportunities for economic development and to make informed decisions about investment in agriculture.

Climate change: Climate change is having a significant impact on global crop yields. Policy makers and agricultural professionals need to know about global crop yields to identify the impact of climate change on agriculture and to develop strategies to mitigate its effects.

Resource management: Agricultural production requires significant amounts of resources such as water and fertilizer. Policy makers and agricultural professionals need to know about

global crop yields to manage resources effectively and ensure sustainable agriculture practices.

Trade: Global crop yields are an important factor in global trade of agricultural commodities. Policy makers and agricultural professionals need to know about global crop yields to make informed decisions about trade policies and negotiations.

Research and development: Understanding global crop yields can provide valuable insights for research and development in agriculture. By identifying factors that affect crop yields, agricultural professionals can develop new technologies and practices to improve productivity and efficiency.
