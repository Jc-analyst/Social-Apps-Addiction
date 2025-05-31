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

# UNDERSTANDING THE DATASET
```sql
SHOW TABLES;
Describe s_a;
SELECT * FROM s_a;
```

# QUALITY CHECKS 
## 1 Checking for duplicates
```sql
SELECT 
    Student_ID,  
    COUNT(*) AS duplicate_count
FROM s_a
GROUP BY Student_ID
HAVING COUNT(*) > 1;
```
### No duplicates, (not using having as its for agreagete fucntions)


## 2 How many distinc different values per Column
```sql
SELECT 
    COUNT(DISTINCT Student_ID) AS unique_ID,
    COUNT(DISTINCT Age) AS unique_Age,
    COUNT(DISTINCT Gender) AS unique_gender,
    COUNT(DISTINCT Academic_Level) AS unique_A_l,
    COUNT(DISTINCT Most_Used_Platform) AS unique_plat,
    COUNT(DISTINCT Affects_Academic_Performance) AS unique_A_P,
    COUNT(DISTINCT Relationship_Status) AS unique_R_s
FROM s_a;
```
### Distinc values are ok 


## 3 Checking for Missing Data (NULLs)
```sql
SELECT * 
FROM s_a 
WHERE 
Student_ID IS NULL OR Student_ID = ''
   OR Age IS NULL OR Age = ''
   OR Gender IS NULL OR Gender = ''
   OR Academic_Level IS NULL OR Academic_Level = ''
   OR Country IS NULL OR Country = ''
   OR Avg_Daily_Usage_Hours IS NULL OR Avg_Daily_Usage_Hours = ''
   OR Most_Used_Platform IS NULL OR Most_Used_Platform = ''
   OR Affects_Academic_Performance IS NULL OR Affects_Academic_Performance = ''
   OR Sleep_Hours_Per_Night IS NULL OR Sleep_Hours_Per_Night = ''
   OR Mental_Health_Score IS NULL OR Mental_Health_Score = ''
   OR Relationship_Status IS NULL OR Relationship_Status = ''
   OR Conflicts_Over_Social_Media IS NULL OR Conflicts_Over_Social_Media = ''
   ;
```
   ### OR 
```sql
SELECT * 
FROM s_a 
WHERE COALESCE(Student_ID, Age, Gender, Academic_Level, Country, 
               Avg_Daily_Usage_Hours, Most_Used_Platform, Affects_Academic_Performance, 
               Sleep_Hours_Per_Night, Mental_Health_Score, Relationship_Status, 
               Conflicts_Over_Social_Media) = NULL or ' ';
```
### There is non NULL or empty value

## 4 checking for missing data per columns
```sql
SELECT 
    SUM(CASE WHEN Student_ID IS NULL THEN 1 ELSE 0 END) AS missing_Student_ID,
    SUM(CASE WHEN Age IS NULL THEN 1 ELSE 0 END) AS missing_Age,
    SUM(CASE WHEN Gender IS NULL THEN 1 ELSE 0 END) AS missing_Gender,
    SUM(CASE WHEN Academic_Level IS NULL THEN 1 ELSE 0 END) AS missing_Academic_Level,
    SUM(CASE WHEN Country IS NULL THEN 1 ELSE 0 END) AS missing_Country,
    SUM(CASE WHEN Avg_Daily_Usage_Hours IS NULL THEN 1 ELSE 0 END) AS missing_Avg_Daily_Usage_Hours,
    SUM(CASE WHEN Most_Used_Platform IS NULL THEN 1 ELSE 0 END) AS missing_Most_Used_Platform,
    SUM(CASE WHEN Affects_Academic_Performance IS NULL THEN 1 ELSE 0 END) AS missing_Affects_Academic_Performance,
    SUM(CASE WHEN Sleep_Hours_Per_Night IS NULL THEN 1 ELSE 0 END) AS missing_Sleep_Hours_Per_Night,
    SUM(CASE WHEN Mental_Health_Score IS NULL THEN 1 ELSE 0 END) AS missing_Mental_Health_Score,
    SUM(CASE WHEN Relationship_Status IS NULL THEN 1 ELSE 0 END) AS missing_Relationship_Status,
    SUM(CASE WHEN Conflicts_Over_Social_Media IS NULL THEN 1 ELSE 0 END) AS missing_Conflicts_Over_Social_Media,
    SUM(CASE WHEN Addicted_Score IS NULL THEN 1 ELSE 0 END) AS missing_Addicted_Score
FROM s_a;
```
###There is non any null value

## 5 checking for wrong data (age)
```sql
SELECT Age
FROM s_a
WHERE Age < 0;
```
### Age data looks good, 
    
### Data is checked and prepare to be analized 

# RUN SUMMARY QUERIES

```sql
SELECT * FROM s_a;
```
```sql
SELECT
    AVG(Age) AS average_age,
    MIN(Age) AS min_age,
    MAX(Age) AS max_age
FROM s_a;
```
```sql
SELECT COUNT(*) AS total_males
FROM s_a
WHERE Gender = 'Male';
```
```sql
SELECT 
	SUM(CASE WHEN Gender = 'Male' THEN 1 ELSE 0 END) AS total_males,
    SUM(CASE WHEN Gender = 'Female' THEN 1 ELSE 0 END) AS total_females,
    SUM(CASE WHEN Gender NOT IN ('Male', 'Female') THEN 1 ELSE 0 END) AS total_other_genders
FROM s_a;
```
## There is 352 Males and 353 Females

```sql
SELECT Country, COUNT(*) AS total_students
FROM s_a
GROUP BY Country
ORDER BY total_students DESC;
```
## It can be seen that the data is taking more from North America, Europeans countries and India
```sql
SELECT Most_Used_Platform, COUNT(*) AS total_s_a
FROM s_a
GROUP BY Most_Used_Platform
ORDER BY total_s_a DESC;
```
## The most popular Social media apps are Instagram, TikTok and Facebook
        

## 📊 Correlation Analysis

  ### 1. Correlation daily usage vs mental health 
  ```sql
SELECT Avg_Daily_Usage_Hours, Mental_Health_Score
FROM s_a
ORDER BY Mental_Health_Score DESC;
  ```
   There is a some evidene of the negative correlation but we need to prove it. 
  ```sql
SELECT 
    (SUM(Avg_Daily_Usage_Hours * Mental_Health_Score) - 
    COUNT(*) * AVG(Avg_Daily_Usage_Hours) * AVG(Mental_Health_Score))
    /
    (SQRT(
        (SUM(Avg_Daily_Usage_Hours * Avg_Daily_Usage_Hours) - COUNT(*) * POW(AVG(Avg_Daily_Usage_Hours), 2)) *
        (SUM(Mental_Health_Score * Mental_Health_Score) - COUNT(*) * POW(AVG(Mental_Health_Score), 2))
    )) AS correlation_coefficient
FROM s_a;
  ```
   #### The correlation coef is -0.8, which indicate a strong negative correlation, higher daily use lower the mental health score, but it doesnt mean causation
----
   ### 2. Correlation daily usage vs sleep
   ```sql
SELECT Avg_Daily_Usage_Hours, Sleep_Hours_Per_Night
FROM s_a
ORDER BY Sleep_Hours_Per_Night DESC;
  ```  
 There seems to be a negative correlation so we prove it
  ```sql
SELECT 
    (SUM(Avg_Daily_Usage_Hours * Sleep_Hours_Per_Night) - 
    COUNT(*) * AVG(Avg_Daily_Usage_Hours) * AVG(Sleep_Hours_Per_Night))
    /
    (SQRT(
        (SUM(Avg_Daily_Usage_Hours * Avg_Daily_Usage_Hours) - COUNT(*) * POW(AVG(Avg_Daily_Usage_Hours), 2)) *
        (SUM(Sleep_Hours_Per_Night * Sleep_Hours_Per_Night) - COUNT(*) * POW(AVG(Sleep_Hours_Per_Night), 2))
    )) AS correlation_coefficient
FROM s_a;
  ```  
  #### There is a clearly negative correlation of -0.7, Found strong negative correlation between social media usage and mental health. Discovered link between excessive app time and sleep deprivation.

## 📝 Conclusion
This analysis highlights the risks of social media addiction, showing measurable declines in mental well-being and sleep quality as usage increases.
