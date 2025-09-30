# Carbon Emission Analysis
![](https://github.com/nguyenngoctu-swisscoding/carbon-emission-analysis/blob/main/carbon.jpg)
Source: https://geographical.co.uk/climate-change/geo-explainer-how-carbon-emissions-affect-climate

## Analysis Problem
Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## Data Source
ur dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure
![](https://github.com/nguyenngoctu-swisscoding/carbon-emission-analysis/blob/main/Database%20diagram.png)

## Data Exploration
### Table `'product_emissions'`
```sql
SELECT * FROM product_emissions LIMIT 5;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|

### Table `'companies'`
```sql
SELECT * FROM companies LIMIT 5;
```
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|

## Analysis
### Which products contribute the most to carbon emissions?
Top 10 Products with the highest PCF
```sql
SELECT
  product_name,
  SUM(carbon_footprint_pcf) AS 'Total PCF'
FROM product_emissions
GROUP BY
	product_name 
ORDER BY
  SUM(carbon_footprint_pcf) DESC
LIMIT 10;
```
|product_name|Total PCF|
|------------|---------|
|Wind Turbine G128 5 Megawats|3718044|
|Wind Turbine G132 5 Megawats|3276187|
|Wind Turbine G114 2 Megawats|1532608|
|Wind Turbine G90 2 Megawats|1251625|
|TCDE|198150|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000|
|Electric Motor|160655|
|Audi A6|111282|
|Average of all GM vehicles produced and used in the 10 year life-cycle.|100621|


### Which industry contribute the most to carbon emissions?
TOP 10 Product and the Indesutry of product that have the highest AVG PCF
```sql
SELECT
  pe.product_name,
  ig.industry_group,
  ROUND(AVG(pe.carbon_footprint_pcf), 2) AS 'Average PCF'
  
FROM product_emissions pe	
LEFT JOIN
	industry_groups ig ON pe.industry_group_id = ig.id 
GROUP BY
	pe.product_name 
ORDER BY
  AVG(pe.carbon_footprint_pcf) DESC
LIMIT 10;
```
|product_name|industry_group|Average PCF|
|------------|--------------|-----------|
|Wind Turbine G128 5 Megawats|Electrical Equipment and Machinery|3718044.00|
|Wind Turbine G132 5 Megawats|Electrical Equipment and Machinery|3276187.00|
|Wind Turbine G114 2 Megawats|Electrical Equipment and Machinery|1532608.00|
|Wind Turbine G90 2 Megawats|Electrical Equipment and Machinery|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|Automobiles & Components|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|Materials|167000.00|
|TCDE|Materials|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|Automobiles & Components|91000.00|
|Mercedes-Benz S-Class (S 500)|Automobiles & Components|85000.00|
|Mercedes-Benz SL (SL 350)|Automobiles & Components|72000.00|

TOP 10 Industry with the highest of Total PCF
```sql
SELECT
  ig.industry_group,
  ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total PCF'
FROM product_emissions pe	
LEFT JOIN
	industry_groups ig ON pe.industry_group_id = ig.id 
GROUP BY
	ig.industry_group 
ORDER BY 	
	SUM(pe.carbon_footprint_pcf) DESC 
LIMIT 10;
```
|industry_group|Total PCF|
|--------------|---------|
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

Top 3 Industry that have the most PCF is Electrical Equipment and Machinery, Automobiles & Components and Materials. Most of those industry is heavy industry.




