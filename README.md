🔄 NYC Crime Analytics: End-to-End Data Pipeline
This project builds a cloud-native analytics pipeline that turns raw crime records from New York City into a dynamic, insight-rich dashboard — using Google BigQuery for preprocessing and Looker Studio for visualization.

📥 1. Data Ingestion — From API to Warehouse
Using the NYC Open Data API, raw crime complaint records were fetched and seamlessly ingested into Google BigQuery. This direct cloud ingestion approach eliminates manual downloads and enables automated, scalable, and up-to-date data access for long-term reporting and dashboarding.

🧹 2. Data Preprocessing — Cleansing, Enriching, and Structuring for Analytics
The preprocessing stage is where raw data was refined and engineered into a format ready for high-impact visual analytics. Here’s a breakdown of all key transformations done using SQL in BigQuery:

✅ A. Data Cleaning
Removed nulls, blanks, and garbage values in vic_sex, susp_sex, vic_age_group, susp_age_group, and vic_race

Filtered out records with missing or invalid latitude/longitude

Standardized all categorical fields by mapping inconsistent or lowercase values

🧠 B. Feature Engineering (Calculated Fields)
Field Name	Description
year, month, weekday, hour	Extracted from cmplnt_fr_dt and cmplnt_fr_tm to support temporal analysis
time_of_day	Categorized hour into 6 bins: Midnight, Early Morning, Morning, Afternoon, Evening, Night
lat_lon	Combined lat/long into string for mapping (CAST(latitude AS STRING), etc.)
crime_status	Mapped crm_atpt_cptd_cd → Attempted or Completed
law_severity	Cleaned law_cat_cd to group as Felony, Misdemeanor, Violation
relationship_type	Inferred context from prem_typ_desc (e.g., Domestic, Stranger)
clean_vic_sex / clean_susp_sex	Mapped raw values (M, F, U) to readable gender labels
clean_vic_age_group / clean_susp_age_group	Standardized to known buckets: <18, 18–24, 25–44, 45–64, 65+
repeat_offender_flag	Flag for susp_age_group = '25-44' to highlight common age in offenses
elderly_victim_flag	Flag for vic_age_group = '65+' to track crimes against seniors

Each of these fields was created with modular CASE statements and designed for use in scorecards, filters, and detailed chart breakdowns.


📊 3. Visualization — From SQL to Interactive Stories
The processed table feeds into Looker Studio, where it powers a 3-page interactive dashboard built for both general audiences and policy decision-makers. Key visuals include:

Time Trends: Yearly, monthly, weekday, and hourly patterns
Demographics: Victim vs suspect gender, age, and race comparisons
Geo-mapping: Borough heatmaps and location-specific crime clusters
Transit Safety: Incident breakdown by district and time of day
Severity Analysis: Felony/Misdemeanor split by area and crime type


LookerStudio Dashboard: https://lookerstudio.google.com/reporting/851067c9-4604-4af0-8b9c-b0bea9d62a33
