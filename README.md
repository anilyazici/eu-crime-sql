# 🔍 European Crime Statistics Analysis (2008–2021)

A SQL-based data analysis project exploring crime trends across 36 European 
countries using official Eurostat data.

---

## 📌 Project Overview

This project analyzes police-recorded crime statistics across Europe from 2008 
to 2021. Using SQL queries, it explores patterns in homicide, theft, robbery, 
rape, and burglary — and examines the impact of COVID-19 on crime rates.

---

## 📊 Dataset

- **Source:** Eurostat — European Commission
- **Coverage:** 36 countries | 2008–2021 | 2,452 records
- **Crime types:** Intentional Homicide, Rape, Robbery, Burglary, 
  Residential Burglary, Theft, Motor Vehicle Theft
- **Tables:** `crimes`, `countries` (EU membership, region, population)

---

## 🔍 Key Findings

1. **Türkiye** has the highest average intentional homicide count in Europe (2,024 per year)
2. **Liechtenstein** is the safest country with only 0.4 average homicides per year
3. **COVID-19** caused a dramatic drop in theft: average fell from 155,963 (pre-2020) to 81,151 (2020–2021) — a 48% reduction
4. **Theft** is the most common crime type across all European countries
5. **EU members** show consistently lower homicide rates compared to non-EU countries

---

## 🛠️ SQL Concepts Used

| Concept | Description |
|---|---|
| `SELECT`, `WHERE`, `ORDER BY` | Basic filtering and sorting |
| `GROUP BY`, `HAVING` | Aggregation and group filtering |
| `JOIN` | Combining crimes and countries tables |
| `CASE WHEN` | Classifying countries by risk level, periods |
| `Subquery` | Finding countries above European average |
| Window functions (`RANK OVER`) | Ranking crime types per country |

---

## 📁 Project Structure

```
eu-crime-sql/
│
├── eu_crime_analysis.sql     # All SQL queries
├── eu_crime.db               # SQLite database
├── CRIM_GEN_REG.csv          # Raw data (Eurostat)
├── ICCS.csv                  # Crime type reference
├── UNIT.csv                  # Unit reference
├── data_prep.ipynb           # Python script for data cleaning
└── README.md                 # This file
```

---

## 🚀 How to Run

**Option 1 — DB Browser for SQLite**
1. Download [DB Browser for SQLite](https://sqlitebrowser.org)
2. Open `eu_crime.db`
3. Go to "Execute SQL" tab
4. Run queries from `eu_crime_analysis.sql`

**Option 2 — Python**
```python
import sqlite3
import pandas as pd

conn = sqlite3.connect('eu_crime.db')
df = pd.read_sql("SELECT * FROM crimes", conn)
```

---

## 🔎 Sample Queries

```sql
-- Countries above European average for robbery
SELECT country, AVG(count) AS avg_robbery
FROM crimes
WHERE crime_type = 'Robbery'
GROUP BY country
HAVING AVG(count) > (
    SELECT AVG(count)
    FROM crimes
    WHERE crime_type = 'Robbery'
)
ORDER BY avg_robbery DESC;

-- COVID impact on theft by region
SELECT co.region,
       ROUND(AVG(CASE WHEN c.year < 2020 THEN c.count END), 0) AS avg_pre_covid,
       ROUND(AVG(CASE WHEN c.year >= 2020 THEN c.count END), 0) AS avg_post_covid
FROM crimes c
INNER JOIN countries co ON c.country_code = co.country_code
WHERE c.crime_type = 'Theft'
GROUP BY co.region
ORDER BY avg_pre_covid DESC;
```

---

## 👥 Authors

**Pelin Kimiz**  
MSc Data Science for Society and Business — Constructor University Bremen  
[GitHub](https://github.com/pelinkimiz) | [LinkedIn](https://linkedin.com/in/pelinkimiz)

**Anıl Yazıcı**  
[GitHub](https://github.com/anilyazici) | [LinkedIn](https://linkedin.com/in/anilyazici)
