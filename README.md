/* ----------------------------------------------------------------------*/

/* 1. Rostou v průběhu let mzdy ve všech odvětvích nebo v některých klesají? */

CREATE OR REPLACE VIEW v_cz_payroll AS
SELECT
	year_price,
	industry_branch,
	pay_value
FROM 
	t_jan_tuma_project_SQL_primary_final
GROUP BY 
	year_price,
	industry_branch
ORDER BY 
	industry_branch,
	year_price;
	
CREATE OR REPLACE VIEW v_payroll_growth_cz AS
SELECT 
	year_price,
	industry_branch,
	pay_value,
	LEAD (pay_value,1) OVER (PARTITION BY industry_branch ORDER BY industry_branch, year_price) next_pay_value,
	ROUND ((((LEAD (pay_value,1) OVER (PARTITION BY industry_branch ORDER BY industry_branch, year_price)) - pay_value) / pay_value) * 100,2) AS growth
FROM
	v_cz_payroll
ORDER BY industry_branch,year_price;

SELECT
	industry_branch,
	ROUND(AVG(growth),2) AS pay_growth
FROM v_payroll_growth_cz
WHERE growth IS NOT NULL
GROUP BY industry_branch;

-- Ve sloupci "pay_growth" můžeme pozorovat, že ve všech pozorovaných odvětvích mzda v průběhu let roste.

/* ----------------------------------------------------------------------*/

/* 2. Kolik je možné si koupit litrů mléka a kilogramů chleba za první a poslední srovnatelné období v dostupných datech cen a mezd? */

CREATE OR REPLACE VIEW v_year_category AS
SELECT
	AVG(price) AS avg_price,
	AVG(pay_value) AS avg_pay,
	category,
	quantity,
	unit,
	year_price
FROM
	t_jan_tuma_project_SQL_primary_final
WHERE
	year_price IN (2006,2018) AND 
	category IN ("Chléb konzumní kmínový","Mléko polotučné pasterované")
GROUP BY
	year_price, category;
	
SELECT
	avg_price,
	avg_pay,
 	category,
 	year_price,
	ROUND(avg_pay / avg_price,2) AS quantity_goods,
	unit
FROM
	v_year_category;

--  Výsledky lze zpozorovat ve vytvořeném přehledu ve sloupci quantity_goods

/* ----------------------------------------------------------------------*/

/* 3. Která kategorie potravin zdražuje nejpomaleji (je u ní nejnižší percentuální meziroční nárůst)? */

CREATE OR REPLACE VIEW v_price_difference_cz AS
SELECT
	year_price,
	category,
	price,
	LEAD (price,1) OVER (PARTITION BY category ORDER BY category, year_price) next_year_price,
	ROUND ((((LEAD (price,1) OVER (PARTITION BY category  ORDER BY category, year_price)) - price) / price) * 100,2) difference
FROM
	t_jan_tuma_project_sql_primary_final
GROUP BY
	category, year_price;
			
SELECT
	category,  
	ROUND(AVG (difference),2)
FROM
	v_price_difference_cz
GROUP BY
	category
ORDER BY
	AVG (difference);

-- Z dat lze zjistit, že nejpomaleji zdražující kategorie potravin je Cukr krystalový.

/* ----------------------------------------------------------------------*/

/* 4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)? */

CREATE OR REPLACE VIEW v_price_growth_cz AS
SELECT
	year_price, 
	ROUND(AVG(difference),2) AS difference 
FROM v_price_difference_cz 
GROUP BY year_price;

CREATE OR REPLACE VIEW v_payroll_growth_cz AS
SELECT
	year_price, 
	ROUND(AVG(growth),2) AS growth 
FROM v_payroll_growth_cz 
GROUP BY year_price;

SELECT
	v_cz_p.year_price,
	v_cz_p.difference AS price_growth,
	v_cz_pay.growth AS pay_growth,
	v_cz_p.difference - v_cz_pay.growth AS difference_price_pay
FROM
	v_price_growth_cz AS v_cz_p
LEFT JOIN v_payroll_growth_cz AS v_cz_pay
	ON v_cz_p.year_price = v_cz_pay.year_price
ORDER BY difference_price_pay;

-- V rámci sledovaných let nebyl pozorován nárůst cen potravin vyšší než 10%.

/* ----------------------------------------------------------------------*/

/* 5. Má výše HDP vliv na změny ve mzdách a cenách potravin? Neboli, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem? */

CREATE OR REPLACE VIEW v_gdp_growth AS
SELECT
	*,
	LEAD (GDP,1) OVER (PARTITION BY country ORDER BY country, year) next_year_GDP,
	ROUND((((LEAD (GDP,1) OVER (PARTITION BY country ORDER BY country, year))-GDP) / GDP ) * 100,2) GDP_growth
FROM
	t_jan_tuma_project_SQL_secondary_final
WHERE
	country = LOWER("czech republic");


CREATE OR REPLACE VIEW v_price_growth_cz AS
SELECT
	year_price, 
	ROUND(AVG(difference),2) AS difference 
FROM
	v_price_difference_cz 
GROUP BY
	year_price;

CREATE OR REPLACE VIEW v_payroll_growth_cz AS
SELECT
	year_price, 
	ROUND(AVG(growth),2) AS growth 
FROM
	v_payroll_growth_cz 
GROUP BY
	year_price;

SELECT
	v_gdp.year AS basic_year,
	v_gdp.year + 1 AS next_year,
	v_gdp.GDP_growth,
	v_cz_p.difference AS price_growth,
	v_cz_pay.growth AS pay_growth
FROM
	v_gdp_growth AS v_gdp
LEFT JOIN v_price_growth_cz AS v_cz_p
	ON v_gdp.year= v_cz_p.year_price
LEFT JOIN v_payroll_growth_cz AS v_cz_pay
	ON v_gdp.year= v_cz_pay.year_price
WHERE v_gdp.year >= 2006 AND v_gdp.year <= 2020 ;

-- Z dat plyne, že výše HDP nemá přímý vliv na růst mezd nebo cen potravin. Pokud míra HDP vzroste výrazněji v roce X, pak se projeví výrazněji na růstu ceny potravin i růstu mezd v roce X+1.
