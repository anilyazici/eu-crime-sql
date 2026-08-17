# European Crime Data Analysis | SQL & Python

## 📊 Project Overview

This project analyzes crime statistics across European countries using **SQL and Python**.

The analysis focuses on identifying crime trends, comparing countries, and exploring changes over time using publicly available European crime data.

The project demonstrates a complete data analysis workflow:

**Data Preparation → SQL Analysis → Exploratory Data Analysis → Visualization → Insights**

### Key Objectives

- Analyze crime trends across European countries
- Compare crime levels between countries
- Identify changes in crime patterns over time
- Explore the impact of the COVID-19 period
- Practice advanced SQL analysis techniques
- Extract meaningful insights from real-world data

### Dataset

The dataset contains crime statistics for **36 European countries covering 2008–2021**.

The data is based on publicly available European statistical data.

### Tools & Technologies

- **Python**
- **Pandas**
- **SQL**
- **SQLite**
- **Jupyter Notebook**
- **Data Visualization**

---

## 🎯 Business Questions

This analysis aims to answer the following questions:

1. Which European countries recorded the highest levels of reported crime?

2. How have crime levels changed across Europe between 2008 and 2021?

3. Which crime categories are the most common across European countries?

4. Which countries experienced the largest changes in crime levels over time?

5. How did reported crime change during the COVID-19 period?

6. How do crime patterns differ between EU and non-EU countries?

7. Which countries show the highest crime rates after accounting for population differences?

## Key Findings

1. **Türkiye** has the highest average intentional homicide count in Europe (2,024 per year)
2. **Liechtenstein** is the safest country with only 0.4 average homicides per year
3. **COVID-19** caused a dramatic drop in theft: average fell from 155,963 (pre-2020) to 81,151 (2020–2021) — a 48% reduction
4. **Theft** is the most common crime type across all European countries
5. **EU members** show consistently lower homicide rates compared to non-EU countries

---

## SQL Concepts Used

| Concept | Description |
|---|---|
| `SELECT`, `WHERE`, `ORDER BY` | Basic filtering and sorting |
| `GROUP BY`, `HAVING` | Aggregation and group filtering |
| `JOIN` | Combining crimes and countries tables |
| `CASE WHEN` | Classifying countries by risk level, periods |
| `Subquery` | Finding countries above European average |
| Window functions (`RANK OVER`) | Ranking crime types per country |

---

## Project Structure

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

## How to Run

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

## Sample Queries

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

## Authors

**Anıl Yazıcı**  
[GitHub](https://github.com/anilyazici) | [LinkedIn](https://linkedin.com/in/anıl-yazıcı-a7b0aa25b)

**Pelin Kimiz**  
[GitHub](https://github.com/pelinkimiz) | [LinkedIn](https://linkedin.com/in/pelin-kimiz-b58a30140)
