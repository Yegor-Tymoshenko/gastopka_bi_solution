# 🔥 Gastopka Analytics: Optimalizace prodeje a logistiky pro rodinný podnik

![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)

## 👋 O projektu
Tento projekt jsem realizoval pro menší rodinnou firmu s cílem transformovat jejich syrová data v jasné byznysové argumenty. Celý proces zahrnoval sběr dat z roztříštěných zdrojů a jejich následné zpracování pomocí Power Query, PostgreSQL a prostředí Anaconda. Výsledkem je vyčištěný a plně funkční datový model připravený k analýze.

Firma se specializuje na komplexní řešení v oblasti plynového infračerveného vytápění pro průmyslové objekty, haly a sklady. Jde o mladý, agilní podnik založený v roce 2021, který si zakládá na efektivitě, a funguje se třemi kmenovými specialisty a zbytek procesů řeší skrze prověřený outsourcing.

[![Gastopka Logo](Logo.png)](https://gastopka.ru/)

**Můj cíl:**

Logistika a nový sklad: firma řeší, jestli otevřít druhý sklad na jihu města. Pomocí dat jsem analyzoval, kde máme nejvíc zákazníků, a spočítal, jestli se nám nový sklad vyplatí. Hlavním cílem bylo zjistit, kolik ušetříme na dopravě a za jak dlouho ušetřené peníze pokryjí náklady na nájem a provoz.

Plánování nákupů: protože zboží dovážíme ze zahraničí, musíme vědět, co se reálně prodává. Sledoval jsem prodeje konkrétních modelů, abychom mohli dělat větší objednávky s předstihem. Díky přesným datům teď víme, co se vyplatí koupit ve velkém, abychom dostali lepší cenu a nákup byl efektivnější.


---

## 🛑 Problémy (Původní stav)

* **Datový chaos:** Exporty z 1C a Excelu měly nekonzistentní formáty, "špinavá" data a duplicity.
* **Logistické úzké hrdlo:** Jeden centrální sklad (sever) nestačil pokrývat rostoucí poptávku na jihu, což prodlužovalo doručení.
* **Mrtvé zásoby:** Chyběl přehled o produktech, které pouze zabíraly místo a vázaly kapitál bez generování zisku.

---

## 🛠️ Tech Stack & Workflow

### 1. Čištění a transformace (Power Query & Python)
* **Power Query:** Prvotní vrstva pro sjednocení datových typů a filtraci surových exportů z 1C.
* **Python (Anaconda):** Automatizace rutinní práce. Vytvořil jsem skript pro generování dimenzionální tabulky `D_DATE`, což zrychlilo přípravu celého modelu.
* **Standardizace:** Sjednocení struktury CSV souborů před importem do DB.

### 2. Datové modelování (PostgreSQL)
* **Architektura:** Implementace **Constellation Schema** (hvězdicové schéma se sdílenými dimenzemi) pro vysoký výkon dotazů.
* **Validace:** SQL dotazy pro ověření integrity dat a skladových zásob před vizualizací.

### 3. Business Intelligence (Power BI)
* **DAX:** Výpočet komplexních metrik jako **Year-over-Year (YoY) Growth** a **Profit Margin**.
* **UI/UX:** Návrh intuitivního dashboardu pro okamžitý přehled o regionální poptávce a ziskovosti.

---

## 📊 Klíčová zjištění a výsledky

### 📍 Strategie skladování
Analýza zákazníků potvrdila rostoucí trend poptávky v jižním segmentu. 
> **Výsledek:** Data podpořila rozhodnutí o otevření nového tranzitního uzlu, což sníží náklady na logistiku a zrychlí expedici.

### 💰 Optimalizace portfolia (BCG Matice)
Pomocí scatter plotu (Výnosy vs. Marže) byly identifikovány produkty s vysokým objemem prodejů, ale minimálním ziskem.
> **Výsledek:** Strategické přesměrování obchodního týmu na prodej produktů s vysokou marží místo prostého honění obratu.

---

### Jak projekt spustit
1. SQL skripty pro inicializaci PostgreSQL databáze najdete ve složce `/sql`.
2. Python skript pro generování kalendářní dimenze je v `/scripts`.
3. Power BI šablona (`.pbit`) je k dispozici v `/report`.

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
