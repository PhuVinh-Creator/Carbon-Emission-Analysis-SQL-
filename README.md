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
HAVING COUNT(*) > 1;
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
|10661-1-2016|85|28|11|2016|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-10-2014|85|28|11|2014|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-10-2015|85|28|6|2015|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-10-2016|85|28|11|2016|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-11-2014|85|28|11|2014|501® Original Jeans – Rinse Run|0.997|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-11-2015|85|28|6|2015|501® Original Jeans – Rinse Run|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-2-2014|85|28|11|2014|Slim Straight 514™ Jeans – Indigo Wash|0.68|9|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-2-2015|85|28|6|2015|Slim Straight 514™ Jeans – Indigo Wash|0.68|9|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-2-2016|85|28|11|2016|Slim Straight 514™ Jeans – Indigo Wash|0.88|9|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-3-2014|85|28|11|2014|Slim Straight 514™ Jeans – Rigid Tank|0.68|8|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-3-2015|85|28|6|2015|Slim Straight 514™ Jeans – Rigid Tank|0.68|8|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-3-2016|85|28|11|2016|Slim Straight 514™ Jeans – Rigid Tank|0.88|8|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-4-2014|85|28|11|2014|501® Original Jeans – Light Stonewash|0.997|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-4-2015|85|28|6|2015|501® Original Jeans – Light Stonewash|0.997|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-5-2014|85|28|11|2014|501® Original Jeans – Medium Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-5-2015|85|28|6|2015|501® Original Jeans – Medium Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-6-2014|85|28|11|2014|Slim Straight 514™ Jeans – Tumbled Rigid|0.68|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-6-2015|85|28|6|2015|Slim Straight 514™ Jeans – Tumbled Rigid|0.68|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-7-2014|85|28|11|2014|Regular Straight 505® Jeans – Range (Water<Less™)|0.7665|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-7-2015|85|28|6|2015|Regular Straight 505® Jeans – Range (Water<Less™)|0.7665|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-8-2014|85|28|11|2014|Regular Straight 505® Jeans – Range|0.7665|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-8-2015|85|28|6|2015|Regular Straight 505® Jeans – Range|0.7665|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-9-2014|85|28|11|2014|Regular Straight 505® Jeans – House Cat|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-9-2015|85|28|6|2015|Regular Straight 505® Jeans – House Cat|0.7665|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-1-2014|16|28|25|2014|CX310DN - Color Laser Printer|31.365912|2358|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-10-2014|16|28|25|2014|MS315DN - Mono Laser Printer|16.03449|1616|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-11-2014|16|28|25|2014|MS410DN - Mono Laser Printer|15.898413|1523|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-12-2014|16|28|25|2014|MS415DN - Mono Laser Printer|16.03449|1525|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-13-2014|16|28|25|2014|MS510DN - Mono Laser Printer|16.66952|1618|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-14-2014|16|28|25|2014|MS610DE - Mono Laser Printer|18.37049|1978|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-15-2014|16|28|25|2014|MS610DN - Mono Laser Printer|17.576704|1958|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-16-2014|16|28|25|2014|MS810DE - Mono Laser Printer|27.4877|2107|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-17-2014|16|28|25|2014|MS810DN - Mono Laser Printer|27.44234|2005|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-18-2014|16|28|25|2014|MS811DN - Mono Laser Printer|27.44234|2388|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-19-2014|16|28|25|2014|MS812DE - Mono Laser Printer|27.75985|2744|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-2-2014|16|28|25|2014|CX410DE - Color Laser Printer|31.365912|3236|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-20-2014|16|28|25|2014|MS812DN - Mono Laser Printer|27.44234|2747|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-21-2014|16|28|25|2014|MX310DN - Mono Laser Printer|22.45282|1140|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-22-2014|16|28|25|2014|MX410DE - Mono Laser Printer|23.019813|1499|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-23-2014|16|28|25|2014|MX511DE - Mono Laser Printer|25.4012|1679|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-24-2014|16|28|25|2014|MX611DE - Mono Laser Printer|28.03201|2132|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-25-2014|16|28|25|2014|MX710DE - Mono Laser Printer|57.1526|2923|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-26-2014|16|28|25|2014|MX711DE - Mono Laser Printer|57.1526|3127|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-27-2014|16|28|25|2014|MX810DFE - Mono Laser Printer|118.07009|2536|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-28-2014|16|28|25|2014|MX811DFE - Mono Laser Printer|118.07009|2973|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-29-2014|16|28|25|2014|MX812DFE - Mono Laser Printer|118.07009|3336|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-3-2014|16|28|25|2014|CX510DE - Color Laser Printer|32.6587|3129|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-4-2014|16|28|25|2014|CS310DN - Color Laser Printer|20.32094|1969|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-5-2014|16|28|25|2014|CS410DN - Color Laser Printer|20.50238|3143|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-6-2014|16|28|25|2014|CS510DE - Color Laser Printer|20.91061|3025|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-7-2014|16|28|25|2014|M5163 - Mono Laser Printer|23.700201|2597|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-8-2014|16|28|25|2014|MS310DN - Mono Laser Printer|15.807694|1505|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10666-9-2014|16|28|25|2014|MS312DN - Mono Laser Printer|15.807694|1490|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-1-2014|86|22|19|2014|Mobile Batteries|1000.0|5846|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-1-2015|86|22|9|2015|Mobile Batteries|1000.0|10245|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-2-2013|86|22|19|2013|"LG Chem’s automotive batteries are sustaining the undisputed No.1 position in the world based on more than 10 years of R&D capability. LG Chem is equipped with solutions for all types of electrical cars (Hybrid EV, Plugged-in EV, Battery EV) based on strong product development capability and manufacturing competitiveness via mass production experience. This enables LG Chem to supply battery cell, pack and BMS (Battery Management System) to global carmakers such as GM, Ford, Renault, Hyundai-Kia and Volvo. LG Chem expects the increased revenue from its automotive battery business by being awarded next-generation projects from existing customers while strengthening strategic partnerships, and securing new customers. Additionally, LG Chem plans to launch new products to meet customer needs in new applications such as SLI and garden tools."|1000.0|3197|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-2-2014|86|22|19|2014|Automotive Batteries|1000.0|3113|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-2-2015|86|22|9|2015|Automotive Batteries|1000.0|6309|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-2-2016|86|22|19|2016|Automotive Battery|1000.0|4286|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-3-2015|86|22|9|2015|ABS (Acrylonitrile Butadiene Styrene)|1000.0|876|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-3-2016|86|22|19|2016|ABS(Acrylonitrile Butadiene Styrene)|1000.0|396|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-4-2016|86|22|19|2016|BPA(Bisphenol-A)|1000.0|1088|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10667-5-2016|86|22|19|2016|IT&New Application Battery|1000.0|10607|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10670-1-2013|87|22|11|2013|LG-E970|0.1471|7|11.28|0.01|88.70|2|
|10834-1-2015|88|25|25|2015|"Mouse, M185"|0.136|4|4.63|60.42|34.95|2|
|1085-1-2013|35|27|2|2013|White crystalline sugar|1000.0|650|46.15|53.85|0.00|2|
|1085-1-2014|35|27|2|2014|Sugar|1000.0|650|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|1085-1-2016|35|27|2|2016|Sugar|1000.0|650|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|11152-1-2013|89|3|19|2013|grinding media cast.|1500.0|1750|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|11632-1-2016|92|26|22|2016|IC chips of Smartphone|0.1|2|72.39|27.61|0.00|2|
|1198-1-2013|1|28|24|2013|1 DVD Software Package|0.0907185|1|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|1198-1-2014|1|28|24|2014|DVD software|0.0907185|1|44.05|11.15|44.79|2|
|1198-1-2015|1|28|24|2015|DVD software (1 DVD)|0.0907185|1|44.05|11.15|44.80|2|
|1198-2-2013|1|28|24|2013|USB Software|0.0085|2|75.54|2.22|22.24|2|
|1198-2-2014|1|28|24|2014|USB software|0.0085|2|67.61|2.22|30.17|2|
|1198-2-2015|1|28|24|2015|USB software|0.0085|2|67.61|2.22|30.17|2|
|1198-4-2015|1|28|24|2015|DVD software (2 DVDs)|0.181437|2|43.72|17.13|39.15|2|
|12084-1-2016|95|23|19|2016|Filtro 26 WS|1.0|3|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12117-1-2013|96|27|10|2013|Dell Laptop|2.32|320|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12117-2-2013|96|27|10|2013|"Kimberley-Clark toilet tissue, 10,000 sheets"|4.2|20|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12134-1-2017|17|16|19|2017|Paraformaldehyde|500.0|200|56.50|29.93|13.57|2|
|12134-2-2017|17|16|19|2017|Methacrylic acid|20.0|71|64.44|34.13|1.43|2|
|12134-3-2017|17|16|19|2017|Hydrogen peroxide|20.0|4|65.22|34.54|0.24|2|
|12134-4-2017|17|16|19|2017|Amines|9.0|8|63.68|33.73|2.60|2|
|12134-5-2017|17|16|19|2017|Peroxides|25.0|42|63.82|33.80|2.38|2|
|12134-6-2017|17|16|19|2017|Super-pure hydrogen peroxide|17736.0|6469|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12134-7-2017|17|16|19|2017|ELM|160.0|139|7.13|3.77|89.10|2|
|12134-8-2017|17|16|19|2017|TCDE|12000.0|99075|64.95|34.40|0.66|2|
|12289-1-2017|18|16|8|2017|Zinc Oxide|20.0|6|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12348-1-2013|97|28|2|2013|500ml Coors Light brewed in the UK (steal can) CO2e expressed in CO2e/ hl|100.0|31|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12348-2-2013|97|28|2|2013|"Carling brewed in the UK, 24 440ml cans in 4 packs"|12.029|3|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12348-2-2014|97|28|2|2014|"Carling brewed in the UK, 24 440ml cans in 4 packs"|12.029|3|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12348-3-2013|97|28|2|2013|"Carling brewed in the UK, 24 568ml cans in 4 packs"|15.53|2|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12348-3-2014|97|28|2|2014|"Carling brewed in the UK, 24 568ml cans in 4 packs"|15.53|2|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12348-4-2014|97|28|2|2014|"Coors Light brewed in the UK, 24 500ml cans in 4 packs"|13.67|4|64.93|28.58|6.49|2|
|12903-1-2015|102|16|25|2015|Server|19.28|1600|5.40|2.06|92.54|2|
|12903-2-2015|102|16|25|2015|Broadband Routers|0.816|56|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12903-3-2015|102|16|25|2015|Mobile Network Equipment|19.28|10273|1.83|0.70|97.48|2|
|12942-1-2013|103|25|2|2013|"1 dl cup of spray dried soluble Nescafé coffee ready to be drunk, at the consumer’s home"|0.1|0|24.43|16.03|59.54|2|
|12942-1-2014|103|25|2|2014|Nescafé soluble coffee|0.1|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12942-1-2015|103|25|15|2015|Nescafé soluble coffee|0.1|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|12942-1-2016|103|25|2|2016|Nescafé soluble coffee|0.12|0|49.61|11.05|39.34|2|
|12942-2-2013|103|25|2|2013|"EcoShape  bottle of water (0.5 L): Serving of 500 ml of hydration to the consumer. The study examines the extraction and processing of all upstream raw materials to produce the products and packaging components; the manufacturing process; distribution logistics; product use and packaging disposal. The work has been conducted using the Ecoinvent database and SimaPro software.  Climate change impacts are measured in kg CO2 equivalents, following the IPCC’s most recent methodology).  Scenario evaluations have been used to examine the importance of several variables in the product comparison. These include a range of possible consumer behaviours regarding the use of the products, such as conditions of refrigeration, washing of glasses, and disposal routes for packaging, among others."|0.54167|0|45.88|4.92|49.20|2|
|12942-2-2014|103|25|2|2014|EcoShape  bottle of water (0.5 L)|0.54167|0|45.75|4.91|49.34|2|
|12942-2-2015|103|25|15|2015|EcoShape bottle of water (0.5 L)|0.54167|0|45.75|4.91|49.34|2|
|12942-2-2016|103|25|2|2016|EcoShape bottle of water (0.5 L)|0.54167|0|45.75|4.91|49.34|2|
|12942-3-2013|103|25|2|2013|"Nestlé Babyfood NaturNes packaging - plastic pot The good assessed:\" Packaging systems used to provide one baby food meal in France, Spain, and Germany\". The 200-g packaging size was selected as the basis for this study. (The total emissions in kg CO2-e per unit provided [in this report] is the figure for France as it is not possible to list all three data in one cell. Please find detail by country and life cycle stages in questiom SM3.2.b"|0.02|0|44.02|7.61|48.37|2|
|12942-4-2013|103|25|2|2013|"Nestlé Babyfood NaturNes packaging - plastic pot The good assessed:\" Packaging systems used to provide one baby food meal in France, Spain, and Germany\". The 200-g packaging size was selected as the basis for this study. (The total emissions in kg CO2-e per unit provided [in this report] is the figure for France as it is not possible to list all three data in one cell. Please find detail by country and life cycle stages in questiom SM3.2.b"|0.02|0|56.64|13.29|30.07|2|
|12942-5-2013|103|25|2|2013|"Nestlé Babyfood NaturNes packaging - plastic pot The good assessed:\" Packaging systems used to provide one baby food meal in France, Spain, and Germany\". The 200-g packaging size was selected as the basis for this study. (The total emissions in kg CO2-e per unit provided [in this report] is the figure for France as it is not possible to list all three data in one cell. Please find detail by country and life cycle stages in questiom SM3.2.b"|0.02|0|64.80|11.20|24.00|2|
|13360-1-2013|104|8|25|2013|Average mobile phone|0.218|12|54.17|4.17|41.67|2|
|13360-1-2014|104|8|25|2014|Mobile phone|0.218|12|54.17|4.17|41.67|2|
|13360-2-2013|104|8|25|2013|"Nokia Lumia 800. Nokia's high end smart phone, mass = 142 g including battery."|0.142|16|65.43|3.70|30.86|2|
|13360-3-2013|104|8|25|2013|"Nokia Asha 300. Nokia's low end device, mass = 85 g incluging battery."|0.085|8|37.83|7.09|55.08|2|
|13889-1-2014|105|16|25|2014|Blood Pressure Monitor HEM-7113|0.5448|28|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-10-2017|105|16|25|2017|ECU|1.06|8|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-11-2017|105|16|25|2017|Laser radar|1.34|86|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-12-2017|105|16|25|2017|Power window switch|0.45|9|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-13-2017|105|16|25|2017|Safety controller|0.42|2|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-14-2017|105|16|25|2017|Switch|0.25|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-2-2014|105|16|25|2014|Keyless Entry System for Automobiles(Transmitter + Receiver unit)|0.112|9|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-4-2014|105|16|25|2014|Signal Relay Type G6Zk-1F-TR|0.0028|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-5-2017|105|16|25|2017|Automotive Relay|0.034|3|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-6-2017|105|16|25|2017|Automotive Relay|0.034|1|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-7-2017|105|16|25|2017|Automotive Relay|0.034|1|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-8-2017|105|16|25|2017|Blood Pressure Monitor|0.25|28|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|13889-9-2017|105|16|25|2017|ECU|1.06|8|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14021-1-2013|19|16|30|2013|City gas|1.062|61|12.51|2.69|84.80|2|
|14021-1-2015|19|16|17|2015|City gas|1.062|61|12.51|2.69|84.80|2|
|14021-1-2016|19|16|30|2016|City gas|1.062|61|12.34|1.19|86.48|2|
|14605-1-2013|20|28|2|2013|Walkers Crisps 32.5 g sold in the United Kingdom|0.0325|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-1-2014|20|28|2|2014|Walkers Crisps 32.5 g sold in the United Kingdom|0.0325|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-1-2015|20|28|15|2015|Walkers Crisps 32.5 g sold in the United Kingdom|0.0325|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-2-2013|20|28|2|2013|Walkers Crisps 50g bag sold in the United Kingdom|0.05|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-2-2014|20|28|2|2014|Walkers Crisps 50g bag sold in the United Kingdom|0.05|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-2-2015|20|28|15|2015|Walkers Crisps 50g bag sold in the United Kingdom|0.05|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-3-2013|20|28|2|2013|Quaker Oats 1Kg box sold in the United Kingdom|0.04|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-3-2014|20|28|2|2014|Quaker Oats 1Kg box sold in the United Kingdom|0.04|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-3-2015|20|28|15|2015|Quaker Oats 1Kg box sold in the United Kingdom|0.04|0|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-4-2013|20|28|2|2013|Quaker Oats So Simple Original and Golden Syrup products|0.324|1|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-4-2014|20|28|2|2014|Quaker Oats So Simple Original and Golden Syrup products|0.324|1|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14605-4-2015|20|28|15|2015|Quaker Oats So Simple Original and Golden Syrup products|0.324|1|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|14697-1-2013|109|26|25|2013|HH DVD|0.0907185|26|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|149-1-2013|28|26|25|2013|power supply unit (MF6004-030L)|1.8|244|43.69|46.44|9.87|2|
|15391-1-2013|114|6|25|2013|CD-ROM|0.0907185|2|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|15391-1-2014|114|6|25|2014|CD-ROM|0.0907185|12|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|15398-1-2013|112|26|25|2013|"19” LCD Monitor  Weight: 2.4Kg  Diagonal size: 19\"  Power consumption: <16.25W (on mode), <0.72W (power saving mode), <0.53W (off mode) Excluded from the study of carbon footprint of product were CD、Manual and Quick Start Guide"|2.4|70|98.40|1.29|0.31|2|
|15398-10-2014|112|26|25|2014|LED Lighting|6.3|33|98.38|1.38|0.24|2|
|15398-11-2014|112|26|25|2014|Projector|2.8|89|87.70|12.06|0.24|2|
|15398-12-2014|112|26|25|2014|Mobile Phone|0.31|19|88.18|11.71|0.10|2|
|15398-13-2015|112|26|25|2015|Monitor|7.4|66|97.21|2.24|0.56|2|
|15398-13-2016|112|26|25|2016|Monitor|5.8|75|97.06|1.56|1.38|2|
|15398-14-2015|112|26|25|2015|Projector|3.4|83|98.41|1.27|0.31|2|
|15398-14-2016|112|26|25|2016|Projector|4.4|100|97.26|2.41|0.33|2|
|15398-15-2015|112|26|25|2015|Mobile Phone|0.35|53|99.40|0.06|0.54|2|
|15398-15-2016|112|26|25|2016|Mobile Phone|0.35|16|99.38|0.43|0.19|2|
|15398-16-2015|112|26|25|2015|LED Lighting|8.2|69|82.96|16.88|0.16|2|
|15398-16-2016|112|26|25|2016|LED Lighting|5.8|24|90.10|9.57|0.33|2|
|15398-17-2015|112|26|25|2015|Industrial Solutions|14.2|146|96.88|2.70|0.42|2|
|15398-18-2016|112|26|25|2016|Industrial Solutions|7.0|66|89.48|10.02|0.50|2|
|15398-2-2013|112|26|25|2013|Projector  Weight: 7.5Kg Lamp: 24 pcs laser with phosphor Excluded from the study of carbon footprint of product were CD、Manual and Quick Start Guide.|7.5|183|99.17|0.52|0.31|2|
|17659-2-2014|24|28|21|2014|Staples Sustainable Earth 38A toner cartridge|0.8172|8|78.45|5.26|16.29|2|

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

#### Discovery: 

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

#### Discovery: 

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

#### Discovery: 

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

#### Discovery: 

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

#### Discovery: 

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

#### Discovery: 

### 3.7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
![image](https://github.com/user-attachments/assets/bd4cb733-e7ba-4008-94ba-c5c9280ec988)

#### • Discovery: 

_*Note:_ there's a suspicious outliner in "Electrical Equipment and Machinery" row, so I won't include in the discovery. (In real-life, to solve this, I'll contact relevant department to clarify this case)

![image](https://github.com/user-attachments/assets/40e12c49-6ed7-4b55-8b55-44d5060c8e28)



