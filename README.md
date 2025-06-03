# Carbon-Emission-Analysis-SQL

## 1. Introduction
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

![image](https://github.com/user-attachments/assets/e50232d7-f7d9-4990-ba44-fbc67708b562) 

## 1.1. Data model
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
Table 'product_emissions'

![image](https://github.com/user-attachments/assets/acb7d961-bf0a-4ce0-8325-ae9a512b054d)

## 1.2. Data Structure
Table 'product_emissions'
```SQL
SELECT *
FROM product_emissions pe
LIMIT 5;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|

## 2. Data exploration
A) Duplicate data
```sql
SELECT * ,
	COUNT(*) AS duplicate_count
FROM product_emissions pe
GROUP BY
	id,
	company_id,
	country_id,
	industry_group_id,
	year,
	product_name,
	weight_kg,
	carbon_footprint_pcf,
	upstream_percent_total_pcf,
	operations_percent_total_pcf,
	downstream_percent_total_pcf
HAVING COUNT(*) > 1
LIMIT 10;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|duplicate_count|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|---------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|2|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|2|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|2|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|2|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|2|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|2|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2014|85|28|11|2014|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2015|85|28|6|2015|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|

_*Note:_ There are duplicates in data (As I don't have full rights to the database, I'll contact the relevant department and then create a new 'cleaned_carbon_emissions' table to proceed with the analysis.)

B) Number of unique product:
```sql
SELECT 
	COUNT(product_name) AS total_number_of_all_product,
	COUNT(DISTINCT product_name) AS total_number_of_unique_product
FROM product_emissions pe;
```
|total_number_of_all_product|total_number_of_unique_product|
|---------------------------|------------------------------|
|1037|661|

## 3. Data Analysis
### 3.1. Which products contribute the most to carbon emissions?
```sql
-- top_10_product_by_average_carbon_emission
SELECT 
	product_name,
	ROUND(AVG(carbon_footprint_pcf),2) AS carbon_footprint_pcf_average
FROM 
	product_emissions pe 
GROUP BY product_name
ORDER BY carbon_footprint_pcf_average DESC
LIMIT 10;
```
|product_name|carbon_footprint_pcf_average|
|------------|----------------------------|
|Wind Turbine G128 5 Megawats|3718044.00|
|Wind Turbine G132 5 Megawats|3276187.00|
|Wind Turbine G114 2 Megawats|1532608.00|
|Wind Turbine G90 2 Megawats|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|TCDE|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Mercedes-Benz S-Class (S 500)|85000.00|
|Mercedes-Benz SL (SL 350)|72000.00|

#### • Discovery: 
- Size scaling: 5MW turbines aren't just 2.5x larger than 2MW - their footprint is 2-3x higher, suggesting manufacturing complexity increases non-linearly
- Manufacturing vs. operational impact: These figures represent production emissions, not lifetime operational impact where wind turbines would be net-negative

### 3.2. What are the industry groups of these products?
```sql
-- industry_groups_of_these_products
SELECT 
	ig.industry_group, 
	product_name,
	ROUND(AVG(carbon_footprint_pcf),2) AS carbon_footprint_pcf_average
FROM product_emissions pe 
JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY product_name
ORDER BY carbon_footprint_pcf_average DESC
LIMIT 10;
```
|industry_group|product_name|carbon_footprint_pcf_average|
|--------------|------------|----------------------------|
|Electrical Equipment and Machinery|Wind Turbine G128 5 Megawats|3718044.00|
|Electrical Equipment and Machinery|Wind Turbine G132 5 Megawats|3276187.00|
|Electrical Equipment and Machinery|Wind Turbine G114 2 Megawats|1532608.00|
|Electrical Equipment and Machinery|Wind Turbine G90 2 Megawats|1251625.00|
|Automobiles & Components|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687.00|
|Materials|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000.00|
|Materials|TCDE|99075.00|
|Automobiles & Components|Mercedes-Benz GLE (GLE 500 4MATIC)|91000.00|
|Automobiles & Components|Mercedes-Benz S-Class (S 500)|85000.00|
|Automobiles & Components|Mercedes-Benz SL (SL 350)|72000.00|

#### • Discovery: 
- Clean energy: Wind turbines have massive manufacturing footprints
- Vehicle type impact: Commercial/utility vehicles have higher manufacturing emissions than luxury passenger cars
- Manufacturing intensity: Complex, large-scale products (turbines, steel structures) dominate the footprint rankings

### 3.3. What are the industries with the highest contribution to carbon emissions?
```sql
-- top_10_industries_by_total_carbon_emission
SELECT 
	ig.industry_group, 
	ROUND(SUM(carbon_footprint_pcf),2) AS sum_carbon_footprint_pcf
FROM product_emissions pe 
JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY industry_group
ORDER BY sum_carbon_footprint_pcf DESC
LIMIT 10;
```
|industry_group|sum_carbon_footprint_pcf|
|--------------|------------------------|
|Electrical Equipment and Machinery|9801558.00|
|Automobiles & Components|2582264.00|
|Materials|577595.00|
|Technology Hardware & Equipment|363776.00|
|Capital Goods|258712.00|
|"Food, Beverage & Tobacco"|111131.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486.00|
|Chemicals|62369.00|
|Software & Services|46544.00|
|Media|23017.00|

#### • Discovery: 
- Scale of electrical equipment: The massive footprint suggests energy-intensive manufacturing processes for large-scale equipment
- Automotive consolidation: The 2.6M total likely represents the combined footprint of major auto manufacturers
- Technology paradox: Companies making hardware have high footprints, while software companies are relatively low

### 3.4. What are the companies with the highest contribution to carbon emissions?
```sql
-- top_10_companies_by_total_carbon_emission
SELECT 
	c.company_name, 
	ROUND(SUM(pe.carbon_footprint_pcf),2) AS sum_carbon_footprint_pcf
FROM product_emissions pe 
JOIN companies c ON pe.company_id = c.id
GROUP BY company_name
ORDER BY sum_carbon_footprint_pcf DESC
LIMIT 10;
```
|company_name|sum_carbon_footprint_pcf|
|------------|------------------------|
|"Gamesa Corporación Tecnológica, S.A."|9778464.00|
|Daimler AG|1594300.00|
|Volkswagen AG|655960.00|
|"Mitsubishi Gas Chemical Company, Inc."|212016.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|167007.00|
|Weg S/A|160655.00|
|General Motors Company|137007.00|
|"Lexmark International, Inc."|132012.00|
|"Daikin Industries, Ltd."|105600.00|

#### • Discovery: 
- The renewable energy paradox: Gamesa, which makes wind turbines (clean energy infrastructure), has by far the highest carbon footprint
- Scale matters: The top company's footprint is 6x larger than the second-largest
- German automotive dominance: Daimler and VW represent significant automotive manufacturing footprints

### 3.5. What are the countries with the highest contribution to carbon emissions?
```sql
-- top_10_countries_by_total_carbon_emission
SELECT 
	c.country_name, 
	ROUND(SUM(carbon_footprint_pcf),2) AS sum_carbon_footprint_pcf
FROM product_emissions pe 
JOIN countries c ON pe.company_id = c.id
GROUP BY c.country_name
ORDER BY sum_carbon_footprint_pcf DESC
LIMIT 10;
```
|country_name|sum_carbon_footprint_pcf|
|------------|------------------------|
|Germany|9778464.00|
|Lithuania|212016.00|
|Greece|191687.00|
|Japan|132012.00|
|Colombia|105600.00|
|South Africa|35505.00|
|France|21364.00|
|Italy|20000.00|
|Ireland|11160.00|
|India|9328.00|

#### • Discovery: 
- Germany dominates significantly with 9.78 million units - roughly 46 times larger than the second-highest country (Lithuania). This massive difference suggests Germany either has much larger industrial operations in this dataset, different measurement standards, or represents a different type of carbon accounting.

### 3.6. What is the trend of carbon footprints (PCFs) over the years?
```sql
-- industry_groups_carbon_emission_trend_over_years
SELECT 
	ig.industry_group, 
	ROUND(SUM(CASE WHEN pe.year = 2013 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2013 emission',
	ROUND(SUM(CASE WHEN pe.year = 2014 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2014 emission',
	ROUND(SUM(CASE WHEN pe.year = 2015 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2015 emission',
	ROUND(SUM(CASE WHEN pe.year = 2016 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2016 emission',
	ROUND(SUM(CASE WHEN pe.year = 2017 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2017 emission'
FROM product_emissions pe 
JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY 
	'2013 emission',
	'2014 emission',
	'2015 emission',
	'2016 emission',
	'2017 emission';
```
|industry_group|2013 emission|2014 emission|2015 emission|2016 emission|2017 emission|
|--------------|-------------|-------------|-------------|-------------|-------------|
|"Consumer Durables, Household and Personal Products"|0.00|0.00|931.00|0.00|0.00|
|"Food, Beverage & Tobacco"|4995.00|2685.00|0.00|100289.00|3162.00|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|0.00|0.00|8909.00|0.00|0.00|
|"Mining - Iron, Aluminum, Other Metals"|0.00|0.00|8181.00|0.00|0.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|32271.00|40215.00|0.00|0.00|0.00|
|"Textiles, Apparel, Footwear and Luxury Goods"|0.00|0.00|387.00|0.00|0.00|
|Automobiles & Components|130189.00|230015.00|817227.00|1404833.00|0.00|
|Capital Goods|60190.00|93699.00|3505.00|6369.00|94949.00|
|Chemicals|0.00|0.00|62369.00|0.00|0.00|
|Commercial & Professional Services|1157.00|477.00|0.00|2890.00|741.00|
|Consumer Durables & Apparel|2867.00|3280.00|0.00|1162.00|0.00|
|Containers & Packaging|0.00|0.00|2988.00|0.00|0.00|
|Electrical Equipment and Machinery|0.00|0.00|9801558.00|0.00|0.00|
|Energy|750.00|0.00|0.00|10024.00|0.00|
|Food & Beverage Processing|0.00|0.00|141.00|0.00|0.00|
|Food & Staples Retailing|0.00|773.00|706.00|2.00|0.00|
|Gas Utilities|0.00|0.00|122.00|0.00|0.00|
|Household & Personal Products|0.00|0.00|0.00|0.00|0.00|
|Materials|200513.00|75678.00|0.00|88267.00|213137.00|
|Media|9645.00|9645.00|1919.00|1808.00|0.00|
|Retailing|0.00|19.00|11.00|0.00|0.00|
|Semiconductors & Semiconductor Equipment|0.00|50.00|0.00|4.00|0.00|
|Semiconductors & Semiconductors Equipment|0.00|0.00|3.00|0.00|0.00|
|Software & Services|6.00|146.00|22856.00|22846.00|690.00|
|Technology Hardware & Equipment|61100.00|167361.00|106157.00|1566.00|27592.00|
|Telecommunication Services|52.00|183.00|183.00|0.00|0.00|
|Tires|0.00|0.00|2022.00|0.00|0.00|
|Tobacco|0.00|0.00|1.00|0.00|0.00|
|Trading Companies & Distributors and Commercial Services & Supplies|0.00|0.00|239.00|0.00|0.00|
|Utilities|122.00|0.00|0.00|122.00|0.00|

#### • Discovery:
- Heavy materials industries ("Materials" and "Automobiles & Components") are the groups contributing the most to the carbon pollution from 100,000 to over 200,000
- In the second position are "Technology Hardware & Equipment" and Capital Goods with around 60,000
- Third largest are "Pharmaceuticals, Biotechnology & Life Sciences" with precisely 32,271

### 3.7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
![image](https://github.com/user-attachments/assets/bd4cb733-e7ba-4008-94ba-c5c9280ec988)

#### • Discovery: 
- "Automobiles & Components" dominates the dataset with highest carbon emissions than any sectors from 2015 to 2016. Subsequently it drops to significantly to 0 in 2017.
- "Technology Hardware & Equipment" and "Pharmaceuticals, Biotechnology & Life Sciences" both skyrocket at 2014 but then simultaneously decrease in the next two years.
- "Food, Beverage & Tobacco" sees its peak in 2016 before reducing to its normal emission.

_*Note:_ there's a suspicious outliner in "Electrical Equipment and Machinery" row, so I won't include in the discovery. (In real-life, to solve this, I'll contact relevant department to clarify this case)

![image](https://github.com/user-attachments/assets/40e12c49-6ed7-4b55-8b55-44d5060c8e28)



