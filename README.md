# SQL Projekt - Kurz Datové Akademie od Engeto

## Datové Sady

### Primární Tabulky:
- **czechia_payroll:** Tato tabulka obsahuje informace o mzdách v různých odvětvích za několikaleté období. Datová sada pochází z Portálu otevřených dat České republiky.
- **czechia_payroll_calculation:** Číselník kalkulací v tabulce mezd.
- **czechia_payroll_industry_branch:** Číselník odvětví v tabulce mezd.
- **czechia_payroll_unit:** Číselník jednotek hodnot v tabulce mezd.
- **czechia_payroll_value_type:** Číselník typů hodnot v tabulce mezd.
- **czechia_price:** Tato tabulka obsahuje informace o cenách vybraných potravin za několikaleté období. Datová sada pochází z Portálu otevřených dat České republiky.
- **czechia_price_category:** Číselník kategorií potravin, které se vyskytují v našem přehledu.

### Číselníky Sdílených Informací o ČR:
- **czechia_region:** Číselník krajů České republiky dle normy CZ-NUTS 2.
- **czechia_district:** Číselník okresů České republiky dle normy LAU.

### Dodatečné Tabulky:
- **countries:** Tato tabulka obsahuje různé informace o zemích na světě, například hlavní město, měna, národní jídlo nebo průměrnou výšku populace.
- **economies:** Tato tabulka obsahuje informace o HDP, GINI indexu, daňové zátěži atd. pro různé státy a roky.

## Výzkumné Otázky

1. Rostou mzdy ve všech odvětvích v průběhu let, nebo v některých klesají?
2. Kolik litrů mléka a kilogramů chleba si můžeme koupit za první a poslední srovnatelné období v dostupných datech o cenách a mezdách?
3. Která kategorie potravin zdražuje nejpomaleji (s nejnižším percentuálním meziročním nárůstem)?
4. Existuje rok, ve kterém byl meziroční nárůst cen potravin výrazně vyšší než růst mezd (větší než 10 %)?
5. Má výška HDP vliv na změny v mzdách a cenách potravin? Jinými slovy, pokud HDP vzroste výrazněji v jednom roce, projeví se to na cenách potravin či mzdách ve stejném nebo následujícím roce výraznějším růstem?

## Odpovědi na Výzkumné Otázky
1. Ve sloupci "pay_growth" můžeme pozorovat, že ve všech pozorovaných odvětvích mzda v průběhu let roste.
2. Ve vytvořeném přehledu zjistíme, že v prvním srovnatelném období (2006) byla jednotková cena 16.12 Kč za Chléb konzumní kmínový a 14.44 Kč za Mléko polotučné pasterované a v posledním srovnatelném období (2018) byla jednotková cena 24.24 Kč za Chléb konzumní kmínový a 19.82 Kč za Mléko polotučné pasterované.
  - V roce 2006 bychom si při průměrné mzdě 21 165 Kč počítané z 19 odvětví koupili buď 1 312 bochníků chleba nebo 1 465 kartonů mléka.
  - V roce 2018 bychom si při průměrné mzdě 33 091 Kč počítané z 19 odvětví koupili buď 1 365 bochníků chleba nebo 1 669 kartonů mléka.
3. Z dat lze zjistit, že nejpomaleji zdražující kategorie potravin je Cukr krystalový.
4. V rámci sledovaných let nebyl pozorován nárůst cen potravin vyšší než 10%.
5. Z dat plyne, že výše HDP nemá přímý vliv na růst mezd nebo cen potravin. Pokud míra HDP vzroste výrazněji v roce X, pak se projeví výrazněji na růstu ceny potravin i růstu mezd v roce X+1. 
