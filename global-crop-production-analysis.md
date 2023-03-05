# Global Crop Production

### Table of Contents
- [Preparing the data](/global-crop-production-analysis.md#preparing-the-data)
- [Definitions](/global-crop-production-analysis.md#definitions)
- [Data analysis](/global-crop-production-analysis.md#data-analysis)
  - [Question 1](/global-crop-production-analysis.md#question-1)
  - [Question 2](/global-crop-production-analysis.md#question-2)
  - [Question 3](/global-crop-production-analysis.md#question-3)
  - [Question 4](/global-crop-production-analysis.md#question-4)
  - [Question 5](/global-crop-production-analysis.md#question-5)
  - [Question 6](/global-crop-production-analysis.md#question-6)
  - [Question 7](/global-crop-production-analysis.md#question-7)
  - [Question 8](/global-crop-production-analysis.md#question-8) 
  - [Question 9](/global-crop-production-analysis.md#question-9)
  - [Question 10](/global-crop-production-analysis.md#question-10)
- [Conclusion](/global-crop-production-analysis.md#conclusion)

## Definitions

- **Actual yield**: crop yields measured in tonnes per hectare.


- **Attainable yield**: estimates of feasible crop yields calculated from high-yielding areas of similar climate. They are more conservative than biophysical
‘potential yields’, but should be achievable using current technologies and management (e.g. fertilizers and irrigation). Attainable yields are based on
assessments for the year 2000. Attainable yield pre-2000 may be lower; and post-2000 may be higher than these values.


- **Attainable yield gap**: measures the difference between actual and attainable yields. Attainable yields are estimates of feasible crop yields calculated from high-yielding
areas of similar climate. They are more conservative than biophysical ‘potential yields’, but should be achievable using current technologies and management
(e.g. fertilizers and irrigation).

## Preparing the Data

1. Unpivot the original table to transform all the separate 'crop'_attainable and 'crop'_yield_gap columns into two columns: yield (FLOAT64) for the numeric values, and crop (STRING) for the different types of crops

```SQL
SELECT Entity, Year, yield, crop
FROM `crop-yields-world-data.crop_yields.crops`
unpivot(
  yield for crop in (barley_yield_gap, rye_yield_gap, millet_yield_gap, sorghum_yield_gap, maize_yield_gap,
  cassava_yield_gap, soybeans_yield_gap, rapeseed_yield_gap, sugarbeet_yield_gap, potato_yield_gap, oilpalm_yield_gap,
  groundnut_yield_gap, rice_yield_gap, sunflower_yield_gap, cotton_yield_gap, sugarcane_yield_gap, wheat_yield_gap)
) sub
UNION ALL
SELECT Entity, Year, yield, crop
FROM `crop-yields-world-data.crop_yields.crops`
unpivot(
  yield for crop in (barley_attainable, rye_attainable, millet_attainable, sorghum_attainable, maize_attainable,
  cassava_attainable, soybean_attainable, rapeseed_attainable, sugarbeet_attainable, potato_attainable, oilpalm_attainable,
  groundnut_attainable, rice_attainable, sunflower_attainable, cotton_attainable, sugarcane_attainable, wheat_attainable)
) sub
UNION ALL
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
<br>

The original table [crops.csv](csv/crops.csv)

|    | Entity  | Year | barley_attainable |    cassava_attainable |    cotton_attainable |    groundnut_attainable | maize_attainable |  ... |
|---:|:--------|-----:|------------------:|----------------------:|---------------------:|------------------------:|-----------------:|-----:|
|  0 | Belarus | 1992 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  1 | Belarus | 1993 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  2 | Belarus | 1994 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  3 | Belarus | 1995 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  4 | Belarus | 1996 |              5.44 |                     0 |                    0 |                       0 |             9.25 |  ... |
|  ... | ...     | ...  |             ...   |                   ... |                  ... |                     ... |              ... |  ... |

<br>

The transformed table [unpivot_crops.csv](csv/unpivot_crops.csv)

|     | Entity  | Year | yield | crop   |
|----:|:--------|-----:|------:|:-------|
|   0 | Belarus | 1992 |  3.06 | rye_yield |
|   1 | Belarus | 1993 |   2.8 | rye_yield |
|   2 | Belarus | 1994 |  2.25 | rye_yield |
|   3 | Belarus | 1995 |  2.21 | rye_yield |
|   4 | Belarus | 1996 |  2.07 | rye_yield |
| ... | ...     |  ... | ...   | ...    |

## Data Analysis

### Question 1

**In 2018, for each crop type, which country produced the highest actual yield and which country 
produced the lowest actual yield?** 

(Do not include countries with zero crop yield because they did not participate in production. Show the countries that have the same actual yield. Order by the type of crop, then max yield.)

This question assesses the spread of yield volume per crop across all the countries. This helps us understand which countries are the highest and lower producers for each crop in 2018.

```SQL
WITH high_crop_yield as (
  SELECT Entity, round(yield,2) as actual_yield, crop, dense_rank() over(partition by crop order by yield DESC) as r 
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE Year = 2018 AND crop LIKE "%yield"),

r_high as (
  SELECT * FROM high_crop_yield WHERE r = 1),

low_crop_yield as (
  SELECT Entity, round(yield,2) as actual_yield, crop, dense_rank() over(partition by crop order by yield) as r 
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE Year = 2018 AND crop LIKE "%yield" AND yield != 0),

r_low as (
  SELECT * FROM low_crop_yield WHERE r = 1)

SELECT Entity, actual_yield, crop FROM r_high
UNION ALL
SELECT Entity, actual_yield, crop FROM r_low 
ORDER BY crop, actual_yield DESC
```

**Answer 1:** 

|    | Entity         |   actual_yield | crop            |
|---:|:---------------|---------------:|:----------------|
|  0 | Netherlands    |           7.03 | barley_yield    |
|  1 | Libya          |           0.51 | barley_yield    |
|  2 | Niger          |          23.46 | cassava_yield   |
|  3 | Guatemala      |           1.02 | cassava_yield   |
|  4 | Australia      |           4.48 | cotton_yield    |
|  5 | Indonesia      |           0.08 | cotton_yield    |
|  6 | Israel         |           4.19 | groundnut_yield |
|  7 | Mozambique     |           0.26 | groundnut_yield |
|  8 | Jordan         |          11    | maize_yield     |
|  9 | Zimbabwe       |           0.61 | maize_yield     |
| 10 | Croatia        |           2.01 | millet_yield    |
| 11 | Greece         |           2.01 | millet_yield    |
| 12 | Turkey         |           2.01 | millet_yield    |
| 13 | Austria        |           2.01 | millet_yield    |
| 14 | Bulgaria       |           2.01 | millet_yield    |
| 15 | France         |           2.01 | millet_yield    |
| 16 | Zimbabwe       |           0.17 | millet_yield    |
| 17 | Colombia       |          19.79 | oilpalm_yield   |
| 18 | Suriname       |           2.43 | oilpalm_yield   |
| 19 | United States  |          44.22 | potato_yield    |
| 20 | Eritrea        |           1.21 | potato_yield    |
| 21 | United Kingdom |           3.45 | rapeseed_yield  |
| 22 | Tajikistan     |           0.54 | rapeseed_yield  |
| 23 | Australia      |           9.38 | rice_yield      |
| 24 | Mozambique     |           0.59 | rice_yield      |
| 25 | Congo          |           0.59 | rice_yield      |
| 26 | Luxembourg     |           5.59 | rye_yield       |
| 27 | Ecuador        |           0.63 | rye_yield       |
| 28 | Italy          |           5.28 | sorghum_yield   |
| 29 | Namibia        |           0.19 | sorghum_yield   |
| 30 | Italy          |           3.24 | soybean_yield   |
| 31 | Tajikistan     |           0.34 | soybean_yield   |
| 32 | France         |          75.17 | sugarbeet_yield |
| 33 | Ecuador        |           6.2  | sugarbeet_yield |
| 34 | Egypt          |         111.33 | sugarcane_yield |
| 35 | Cameroon       |           9.44 | sugarcane_yield |
| 36 | Switzerland    |           2.62 | sunflower_yield |
| 37 | Algeria        |           0.45 | sunflower_yield |
| 38 | Netherlands    |           8.5  | wheat_yield     |
| 39 | Ireland        |           8.5  | wheat_yield     |
| 40 | Honduras       |           0.56 | wheat_yield     |

### Question 2

**In 2018, for each crop type, which country had the highest yield ratio, and which country had the lowest yield ratio?**

(Yield ratio = yield / attainable yield. Order by type of crop, then highest ratio.)

The question assesses the spread of crop yield efficiency, in other words, how successful or how efficient was the production of a crop compared to its attainable yield measure. This helps us understand which countries have the highest and lowest yield efficiency in 2018.

```SQL
WITH high_crop_yield as (
  SELECT Entity, round(yield,2) as actual_yield, 
  SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, 
  dense_rank() over(partition by crop order by yield DESC) as r 
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE Year = 2018 AND crop LIKE "%yield"
  )
, r_high as (
  SELECT * FROM high_crop_yield WHERE r = 1
)
, low_crop_yield as (
  SELECT Entity, round(yield,2) as actual_yield, 
  SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, dense_rank() over(partition by crop order by yield) as r 
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE Year = 2018 AND crop LIKE "%yield" AND yield != 0
)
, r_low as (
  SELECT * FROM low_crop_yield WHERE r = 1
)
, att_yields as (
  SELECT DISTINCT Entity, round(yield,2) as att_yield, 
  SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE crop like "%attainable" ORDER BY Entity, crop
)
SELECT Entity, actual_yield, att_yield, round((actual_yield/att_yield),2) as ratio, crop FROM r_high
LEFT JOIN att_yields USING(Entity, crop)
UNION ALL
SELECT Entity, actual_yield, att_yield, round((actual_yield/att_yield),2) as ratio, crop FROM r_low 
LEFT JOIN att_yields USING(Entity, crop)
ORDER BY crop, actual_yield DESC
```
**Answer 2:**

|    | Entity         |   actual_yield |   att_yield |   ratio | crop      |
|---:|:---------------|---------------:|------------:|--------:|:----------|
|  0 | Netherlands    |           7.03 |        7.03 |    1    | barley    |
|  1 | Libya          |           0.51 |        2.35 |    0.22 | barley    |
|  2 | Niger          |          23.46 |       32.26 |    0.73 | cassava   |
|  3 | Guatemala      |           1.02 |       18.14 |    0.06 | cassava   |
|  4 | Australia      |           4.48 |        4.48 |    1    | cotton    |
|  5 | Indonesia      |           0.08 |        2.03 |    0.04 | cotton    |
|  6 | Israel         |           4.19 |        4.19 |    1    | groundnut |
|  7 | Mozambique     |           0.26 |        2.1  |    0.12 | groundnut |
|  8 | Jordan         |          11    |       11    |    1    | maize     |
|  9 | Zimbabwe       |           0.61 |        5.06 |    0.12 | maize     |
| 10 | Croatia        |           2.01 |        2.01 |    1    | millet    |
| 11 | Greece         |           2.01 |        2.01 |    1    | millet    |
| 12 | Turkey         |           2.01 |        2.01 |    1    | millet    |
| 13 | Austria        |           2.01 |        2.01 |    1    | millet    |
| 14 | Bulgaria       |           2.01 |        2.01 |    1    | millet    |
| 15 | France         |           2.01 |        2.01 |    1    | millet    |
| 16 | Zimbabwe       |           0.17 |        1.99 |    0.09 | millet    |
| 17 | Colombia       |          19.79 |       19.79 |    1    | oilpalm   |
| 18 | Suriname       |           2.43 |       19.65 |    0.12 | oilpalm   |
| 19 | United States  |          44.22 |       44.22 |    1    | potato    |
| 20 | Eritrea        |           1.21 |       24.62 |    0.05 | potato    |
| 21 | United Kingdom |           3.45 |        3.48 |    0.99 | rapeseed  |
| 22 | Tajikistan     |           0.54 |        2.32 |    0.23 | rapeseed  |
| 23 | Australia      |           9.38 |        9.38 |    1    | rice      |
| 24 | Mozambique     |           0.59 |        5.32 |    0.11 | rice      |
| 25 | Congo          |           0.59 |        5.95 |    0.1  | rice      |
| 26 | Luxembourg     |           5.59 |        6.38 |    0.88 | rye       |
| 27 | Ecuador        |           0.63 |        5.14 |    0.12 | rye       |
| 28 | Italy          |           5.28 |        5.28 |    1    | sorghum   |
| 29 | Namibia        |           0.19 |        3.35 |    0.06 | sorghum   |
| 30 | Italy          |           3.24 |        3.24 |    1    | soybean   |
| 31 | Tajikistan     |           0.34 |        2.85 |    0.12 | soybean   |
| 32 | France         |          75.17 |       75.17 |    1    | sugarbeet |
| 33 | Ecuador        |           6.2  |       67.58 |    0.09 | sugarbeet |
| 34 | Egypt          |         111.33 |      116.28 |    0.96 | sugarcane |
| 35 | Cameroon       |           9.44 |       91.13 |    0.1  | sugarcane |
| 36 | Switzerland    |           2.62 |        2.62 |    1    | sunflower |
| 37 | Algeria        |           0.45 |        1.87 |    0.24 | sunflower |
| 38 | Netherlands    |           8.5  |        8.5  |    1    | wheat     |
| 39 | Ireland        |           8.5  |        8.5  |    1    | wheat     |
| 40 | Honduras       |           0.56 |        3.64 |    0.15 | wheat     |

### Question 3

**For each country, what is the average yield ratio of each crop in the decade of 2008 - 2018?** 

(Order by crop, then the average yield ratio.)

This question assesses a country's yield efficiency per crop in a decade. 

```SQL
WITH act_yields as (
  SELECT Entity, Year, SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, yield as actual_yield
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE (Year BETWEEN 2008 AND 2018) AND (crop LIKE "%yield")
  ORDER BY Entity, crop, Year
  )
, att_yields as (
  SELECT distinct Entity, SUBSTR(crop, 0, (STRPOS(crop, '_')-1)) as crop, yield as attainable_yield
  FROM `crop-yields-world-data.crop_yields.unpivot_crops`
  WHERE crop LIKE "%attainable"
  ORDER BY Entity
)
, yearly_ratios as (
  SELECT Entity, Year, crop, actual_yield, attainable_yield,
  ROUND(( NULLIF(actual_yield,0) / NULLIF(attainable_yield,0) ),2) as ratio
  FROM act_yields 
  LEFT JOIN att_yields 
  Using(Entity, crop) ORDER BY Entity
)
SELECT Entity, crop, 
round((avg(avg(ratio)) OVER(PARTITION BY crop, Entity)),2) as avg_act_to_att_yield,
--round((SUM(SUM(actual_yield)) OVER(PARTITION BY crop, Entity)),2) as sum_actual_yield
FROM yearly_ratios 
GROUP BY Entity, crop ORDER BY Entity, avg_act_to_att_yield DESC
```

**Answer 3:**

Refer to [table results](/csv/q3_avg_ratio_per_country_decade.csv)

Refer to interactive [Tableau dashboard]()

### Question 4
### Question 5
### Question 6
### Question 7
### Question 8
### Question 9
### Question 10
## Conclusion
