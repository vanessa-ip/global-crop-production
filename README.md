## Global Crop Production

![crop_map](images/rice_crop_map.png)
### Table of Contents
- [Background](https://github.com/vanessa-ip/global-crop-production#background)
- [Business task](https://github.com/vanessa-ip/global-crop-production#business-task)
- [Data source](https://github.com/vanessa-ip/global-crop-production#data-source)
- [Entity relationships](https://github.com/vanessa-ip/global-crop-production#entity-relationships)
- [Case study questions](https://github.com/vanessa-ip/global-crop-production#case-study-questions)
- [Solutions on Github](https://github.com/vanessa-ip/global-crop-production/blob/main/global-crop-production-analysis.md)
- [Visualization on Tableau](https://public.tableau.com/app/profile/vanessa1607/viz/GlobalCropYield/RiceYield_)

### Background

Improvements in crop yields are crucial for feeding a growing population, reducing global poverty rates, and minimizing the environmental impact of food production. With the world's population projected to exceed 9 billion by 2050 (United Nations - Department of Economic and Social Affairs, 2017, The World Population Prospects: The 2017 Revision), it is essential to optimize crop yield.

### Business Task
The descriptive analyses of global crop yield aims to identify current and past yield trends and patterns across all countries, which can provide valuable insights to policymakers, agricultural practitioners, and food companies. 

This analysis will help these stakeholders understand the availability of different crop types and learn from countries that are efficient at production, informing decisions and planning around resource utilization, sustainability practices, international trade, operations of the food supply chain, and technological innovation in reducing land use while intensifying yield.

### Data Source
- [Crop Yields (2022) - published online at OurWorldInData.org ](https://ourworldindata.org/crop-yields)
- [Land Use (2019) - published online at OurWorldInData.org](https://ourworldindata.org/land-use)

### Entity Relationships 

![entity relationships](images/entity_relationship.png)

### Case Study Questions

Segment by crop:

1. In the most recent year of the dataset (2018), what is the total actual yield per crop type and their percentage share among all types?
2. For each crop type, what is the decade-to-decade growth rate of actual yield, and the total growth rate of actual yield ?
3. For each type of crop, count the number of countries that produce the crop. Compare the change in the number of countries producing a crop from the decade of 1969-1978 to 2009-2018.
4. For each crop, what is the average yield efficiency in the decade of 2009-2018?
5. For each crop, what is the change of the yield efficiency from the decade of 1969-1978 to 2009-2018?

Segment by country:

1. For each country, what is the number of different types of crop it produces?
2. What is the percentage of countries that produce a certain percentage of different crop types?
3. In 2018, for each country, what is the yield per crop, the total yield of all crops, and the percentage share of total yield amongst all countries?
4. In 2009-2018, what is the average yield efficiency per country per crop?
5. In 2009-2018 what is the average yield efficiency per country?
6. What is percentage change of yield efficiency per country from 1969-1978 to 2009-2018?
7. For Brazil, Canada, China, India, and United States, compare the change in total crop yield and cropland use over time.

