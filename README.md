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


## 3. Data Analysis
### 3.1. Which products contribute the most to carbon emissions?
```sql
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

