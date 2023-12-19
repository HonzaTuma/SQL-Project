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

1. **Růst Mzd v Odvětvích:** Ve sloupci "pay_growth" můžeme pozorovat, že ve všech pozorovaných odvětvích mzdy v průběhu let rostou.

2. **Porovnání Cen Potravin:**
   - V roce 2006 byla jednotková cena za Chléb konzumní kmínový 16.12 Kč a za Mléko polotučné pasterované 14.44 Kč.
   - V roce 2018 byla jednotková cena za Chléb konzumní kmínový 24.24 Kč a za Mléko polotučné pasterované 19.82 Kč.
   
   Při průměrné mzdě 21,165 Kč v roce 2006 bychom si mohli koupit buď 1,312 bochníků chleba nebo 1,465 kartonů mléka. V roce 2018, s průměrnou mzdou 33,091 Kč, bychom si mohli koupit buď 1,365 bochníků chleba nebo 1,669 kartonů mléka.

3. **Nejpomaleji Zdražující Potraviny:** Z dat lze vyčíst, že nejpomaleji zdražující kategorií potravin je Cukr krystalový.

4. **Růst Cen Potravin a Mzdí:** V rámci sledovaných let nebyl zaznamenán nárůst cen potravin vyšší než 10 %.

5. **Vliv HDP na Mzdy a Ceny Potravin:** Z dat plyne, že výše HDP nemá přímý vliv na růst mezd ani cen potravin. Pokud míra HDP vzroste výrazněji v jednom roce, projeví se to výrazněji na růstu cen potravin i mezd v následujícím roce (X+1).
