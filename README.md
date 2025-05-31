# 📱 Social Media Addiction & Its Effects on Mental Health 😵‍💫

## 📌 Overview
This project investigates the impact of excessive social media use on mental health and sleep patterns. Using MySQL, I cleaned and analyzed a dataset to uncover patterns, including a **strong negative correlation** between time spent on social media and well-being.

## 🔍 Key Findings
- **Negative correlation** between app usage and mental health scores.
- **Increased screen time** linked to sleep disturbances.
- **Data cleaning** included handling duplicates, missing values, and correcting wrong entries.

## 🏗️ Project Structure
📂 social-media-impact/ 
├── 📄 README.md  # Project documentation 
├── 📄 dataset.csv  # Raw data source 
├── 📂 sql_queries/ # Folder for SQL scripts │ 
  ├── cleaning.sql  # Data cleaning queries │ 
  ├── analysis.sql  # Correlation studies 
├── 📂 results/ # Processed outputs and reports


## 🧹 Data Cleaning Steps
```sql
DELETE FROM users WHERE id IN (
    SELECT id FROM (
        SELECT id, COUNT(*) FROM users GROUP BY id HAVING COUNT(*) > 1
    ) AS duplicates
);
```

## 📊 Correlation Analysis
```sql
SELECT social_media_hours, mental_health_score,
       CORR(social_media_hours, mental_health_score) AS correlation
FROM user_data;
```
Found strong negative correlation between social media usage and mental health.
Discovered link between excessive app time and sleep deprivation.

## 📝 Conclusion
This analysis highlights the risks of social media addiction, showing measurable declines in mental well-being and sleep quality as usage increases.
