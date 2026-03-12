# 🔥 Gastopka Analytics: Optimalizace prodeje a logistiky pro rodinný podnik

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)

## 📌 O projektu
Krátká předmluva: toto je pouze mé portfolio pro pozici Junior Data Analyst. Jeho zvláštnost spočívá v poskytnuté mi příležitosti analyzovat reálná data vzatá z reálné firmy. Nebyl to freelance, protože firma patří mému otci a počet jejích kmenových zaměstnanců se nedávno zvýšil na 4. Jsou jí teprve 3 roky, na trhu s plynovým infračerveným vybavením se teprve upevnila, ale vykazuje stabilní plynulý růst.

Protože jsem projevil iniciativu k uspořádání dat pro analýzu posledního, a nejúspěšnějšího, roku firmy, tak jsem si úkoly zčásti vymýšlel sám a zčásti dostával dotazy od zaměstnanců. To byla i moje první úloha – najít slabá místa, zákonitosti, predispozice. Poté byla provedena velká ETL práce, protože, jak se v malých firmách nezřídka stává, nesystematizují data „ideálně“. Potom přišlo na řadu SQL, které ukázalo první výsledky. Celý proces uzavírám tvorbou interaktivních reportů v Power BI.

[![Gastopka Logo](Logo.png)](https://gastopka.ru/)

## 🎯 Můj cíl
V tomto projektu sleduji dva cíle. Prvním je pro mě, jakožto začínajícího analytika, získání praxe s reálnými daty a také s prezentací výsledků technickým specialistům i manažerovi.

Druhým cílem je pak konkrétní požadavek firmy na rozšíření skladových prostor pro uskladnění vybavení. Aktuálně má firma jediný sklad na severu velkého města. Mým úkolem je prozkoumat podíl zákazníků z jižní části a sestavit seznam nejprodávanějších modelů ohřívačů. Jde o to, že v prvním skladu nedostatek místa způsobují i položky, na které se jen práší, a firma se chce podobnému scénáři u druhého skladu vyhnout.

---

## 🛑 Problémy 

* **„Špinavá“ data a časově náročné ETL:**
    * Největší část práce (a času) zabral proces ETL, aby bylo možné SQL dotazy spouštět nad validními fakty.

* **Absence segmentace:**
    * Firma sice tušila, že jih má potenciál, ale chyběly tvrdé důkazy.
    * Bylo nutné pomocí SQL vypreparovat fakta o prodejích, identifikovat top produkty a vypočítat reálný tržní podíl jižních zákazníků oproti severu.

* **Hrozba neefektivního skladu:**
    * Centrální sklad je zaplněn položkami, které se téměř neprodávají a blokují místo.
    * Pro rozšiření na jih je kritické aby se nové prostory neproměnily v odkladiště prachu.

---

## 🛠️ Tech Stack & Workflow

Celý proces od surových dat až po finální report jsem rozdělil do tří logických fází, aby byla zajištěna maximální přesnost výsledků.

### 1. Čištění a transformace (Power Query & Anaconda Prompt)
* **Power Query:** Použil jsem jako prvotní vrstvu pro vyčištění surových exportů z 1C, sjednocení datových typů a odstranění duplicit.
* **Python (Anaconda Prompt):** Pomohl mi s automatizací rutinní práce. Např. vytvořil jsem skript pro generování dimenzionální tabulky `D_DATE`.
* **Standardizace dat (ETL):** Před nahráním do databáze jsem musel sjednotit všechny CSV soubory, které měly odlišnou strukturu, někde se lišily názvy sloupců (třeba "Datum" vs. "Date"), jinde byly odlišné formáty dat nebo čísel. Vše jsem upravil do jednoho standardu, aby import proběhl bez chyb a analýza vycházela z přesných a porovnatelných údajů.

### 2. Datové modelování (PostgreSQL & pgAdmin 4)
* **Galaxy Schema:** Implementoval jsem architekturu s více tabulkami faktů, které sdílejí společné dimenze (`D_PRODUCT`, `D_GEOGRAPHY`, `D_DATE`). Tento přístup umožňuje analyzovat firmu z různých úhlů pohledu.
* **Struktura faktových tabulek:**
    * `FACT_SALES_SERVICE`: Sleduje reálné tržby a výkonnost prodeje.
    * `FACT_INVENTORY`: Slouží k monitoringu skladových zásob a identifikaci "mrtvých" položek.
    * `FACT_PROJECTS`: Pro přehled statusu projektu.

### 3. Business Intelligence (Power BI)
* **Produktové a obchodní metriky:**
    * **CR (Conversion Rate):** Sledování průchodnosti Sales Funnel z tabulky `FACT_PROJECTS` napříč fázemi *Calculation* $\rightarrow$ *Technical Project* $\rightarrow$ *Closed Won*.
    * **AOV (Average Order Value):** Detailní srovnání průměrné hodnoty objednávky mezi aktuálním trhem a plánovaným jižním regionem.
    * **Sales Cycle Length:** Výpočet průměrné doby od prvního výpočtu po finální prodej (využití časových razítek `start_date` a `end_date`). Ukazuje, jak rychle se firmě vrací investovaný čas obchodníků.
    * **Lost Opportunity Cost (CAC proxy):** Kvantifikace utopených nákladů (čas inženýrů a obchoďáků) u zakázek, které skončily jako `Closed Lost`. Slouží jako argument, kolik peněz firmu stojí chybějící logistická infrastruktura na jihu města.

* **Statistická analýza a distribuce dat:**
    * **Pravidlo 20/80 (Pareto):** Dynamický DAX výpočet pro identifikaci skupiny produktů generujících 80 % tržeb na jihu, aby se zamezilo vzniku mrtvých zásob na novém skladu.
    * **Gaussovo rozdělení a popisná statistika:** Analýza distribuce cen zakázek (AOV) s využitím **modu** (nejčastější hodnota), **mediánu** a **průměru**. Pomáhá identifikovat "typického" regionálního klienta očištěného o extrémně drahé průmyslové instalace. 
    * **p-value:** T-test, zda je rozdíl v nákupním chování mezi severem a jihem statisticky významný ($p < 0,05$).

* **UI/UX Design:** Dashboard je navržen tak, aby technicky nesmýšlejícímu vedení poskytl okamžitý a srozumitelný přehled o regionální poptávce, úzkých hrdlech v prodejním procesu a prioritním zboží pro naskladnění nového hubu.

---
## 🛠️ Datové modelování a SQL (PostgreSQL)

Základem analytického modelu je čistá databázová architektura. Stojí na třech hlavních tabulkách faktů, které přes sdílené dimenze spojují všechny obchodní procesy do jednoho funkčního celku.

![ERD](Galaxy.png)

### 📐 Použité produktové a analytické metriky
Při vyhodnocování dat jsem analýzu postavil na těchto konkrétních výpočtech a statistických ukazatelích:

* **Produktové a obchodní metriky:**
    * **CR (Conversion Rate):** Sledování průchodnosti obchodního trychtýře z tabulky `FACT_PROJECTS`.
      $$CR = \frac{\text{Počet projektů 'Closed Won'}}{\text{Celkový počet projektů ('Calculation')}} \times 100$$
    * **AOV (Average Order Value):** Detailní srovnání průměrné hodnoty objednávky (Sever vs. Jih).
      $$AOV = \frac{\sum \text{Total Revenue}}{\text{Počet unikátních objednávek}}$$
    * **Sales Cycle Length:** Ukazuje, jak rychle se firmě vrací investovaný čas obchodníků.
      $$\text{Cycle Length} = \frac{\sum (\text{End Date} - \text{Start Date})}{\text{Počet projektů 'Closed Won'}}$$
    * **Lost Opportunity Cost (CAC proxy):** Kvantifikace utopených nákladů (čas inženýrů a obchoďáků), pokud chybí logistická infrastruktura.
      $$LOC = \sum \text{Actual Cost} \quad \text{[pro status = 'Closed Lost']}$$

* **Statistická analýza a distribuce dat:**
    * **Pravidlo 20/80 (Pareto pro Smart Stock):** Dynamický výpočet úzké skupiny produktů generujících většinu tržeb.
      $$\text{Pareto Ratio} = \frac{\sum \text{Tržby z Top 20 \% produktů}}{\text{Celkové tržby}} \approx 0,80$$
    * **Popisná statistika (Gaussovo rozdělení):** Analýza distribuce cen zakázek pro identifikaci typického regionálního klienta, očištěno o extrémní instalace.
      * Průměr: $\mu = \frac{1}{n}\sum x_i$
      * Medián: $\tilde{x}$ (prostřední hodnota seřazeného datasetu)
      * Modus: $\hat{x}$ (nejčastější hodnota)
    * **Ověření hypotéz (p-value):** Statistické potvrzení T-testem, zda je rozdíl v chování regionů významný, nebo jde o šum.
      $$t = \frac{\bar{x}_S - \bar{x}_J}{\sqrt{\frac{s_S^2}{n_S} + \frac{s_J^2}{n_J}}}, \quad \text{kde } p < 0,05$$

---

### 🔍 Praktické ukázky SQL dotazů

#### 1. Simulace návratnosti nového skladu
Mým hlavním úkolem bylo zjistit, jestli se firmě vyplatí otevřít sklad na jihu. Napsal jsem komplexní dotaz s využitím **CTE (Common Table Expressions)**, kde jsem porovnal náklady na současnou dopravu oproti variantě s lokálním skladem.

```sql
set search_path to gastopka_dw;


WITH Logistics_Costs as (
    SELECT
        3800 as cost_per_delivery_north_to_south,   -- náklady na současnou dopravu na jih ze skladu
        1900 as cost_per_delivery_from_south,       -- náklady na potenciální dopravu z jižního skladu
        1000000 as warehouse_setup_cost,            -- počáteční náklady na pronájem, rekonstrukci a vybavení
        84000 as monthly_op_cost                    -- měsíční nájemné a mzdové náklady na skladníka
),

Southern_Sales_Volume as (
    SELECT
        COUNT(fs.sales_id) as total_orders,
        SUM(fs.cost_amount) as total_revenue
    FROM "FACT_SALES_SERVICE" fs
    JOIN "D_GEOGRAPHY" dg ON fs.geography_key = dg.geography_key
    WHERE dg.city in ('Podolsk', 'Tula', 'Novomoskovsk', 'Skopin', 'Ryazan', 'Murom', 'Kasimov', 'Lyubertsy') -- města se zákazníky na jihu
)

-- výpočet logistických úspor (rozdíl mezi současnou a budoucí dopravou)
SELECT
    ssv.total_orders,
    ssv.total_revenue,
    ssv.total_orders * (lc.cost_per_delivery_north_to_south - lc.cost_per_delivery_from_south) as monthly_logistics_savings,
    lc.warehouse_setup_cost /
    ((ssv.total_orders * (lc.cost_per_delivery_north_to_south - lc.cost_per_delivery_from_south)) - lc.monthly_op_cost) -- vzorec pro dobu návratnosti (Payback Period)
    as payback_period_months
FROM Southern_Sales_Volume ssv, Logistics_Costs lc;
```

<p align="center">
<img src="PP_result.png" alt="PP_results.png" width="600">
</p>

💡 Výsledek:
Analýza ukázala, že při současném objemu objednávek (83 za sledované období) by měsíční úspora na logistice činila cca 157 700. Po odečtení provozních nákladů nám vychází doba návratnosti investice na 13 měsíců. To je pro vedení signál, že projekt je bezpečný a 
životaschopný.

---

2. Identifikace TOP produktů pro velkoobchod
Dále jsem potřeboval zjistit, které produkty tvoří základ našeho obratu a jak se nakupují. Cílem bylo vytipovat modely vhodné pro objemové slevy u dodavatelů.

```SQL
set search_path to gastopka_dw;

SELECT
    p.product_name,
    p.product_category,
    -- celkové množství prodaných kusů
    SUM(f.quantity_sold) as total_units_sold,
    -- celkové hrubé tržby za daný model
    ROUND(SUM(f.total_revenue), 2) as total_gross_revenue,
    -- průměrný počet kusů v jedné objednávce
    ROUND(AVG(f.quantity_sold), 1) as avg_units_per_order
FROM "FACT_SALES_SERVICE" f
JOIN "D_PRODUCT" p ON f.product_key = p.product_key
GROUP BY p.product_name, p.product_category
ORDER BY total_units_sold DESC
LIMIT 10;
```

<p align="center">
<img src="Top_10_items.png" alt="Top 10 Products SQL Result" width="800">
</p>

💡 Výsledek:
Dotaz odhalil nejen bestsellery podle tržeb, ale díky sloupci avg_units_per_order i nákupní chování. Tyto data slouží k lepšímu vyjednávání s výrobcem.

---
🚀 Závěr SQL části
V této fázi jsem úspěšně transformoval surová data do strukturované podoby a pomocí SQL ověřil klíčové business hypotézy. Máme tvrdá data, která potvrzují návratnost skladu i potenciál produktů.

Dál jsem připravená data napojil na Power BI.

---

## 📷 Dashboard Screenshots

### Executive Overview
![Overview](Sales&Margin.png)

### Product Matrix
![Product Matrix](Product_Matrix.png)

### Regional Analysis
![Regions](Customer_analysis.png)

---
*Note: Client names and specific financial figures have been anonymized for privacy.*
