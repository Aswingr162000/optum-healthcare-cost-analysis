Healthcare Cost & Provider Benchmarking Analysis
This project was completed as part of an Optum Data Analyst technical assessment.
The objective was to analyze Medicare outpatient claims data and evaluate provider cost performance across chronic illnesses using statistical benchmarking and exploratory data analysis.

Optum Task
Using the beneficiary and outpatient claims datasets:
For each provider (AT_PHYSN_NPI) and chronic illness, calculate cost per member
For each chronic illness, represent the distribution of provider costs
Evaluate how results change after filtering provider–illness groups with only 1 patient
Identify providers that are consistently expensive across conditions
Account for different illness baselines using a standardized comparison approach

Objectives
This analysis answers:

What conditions drive the highest spending?
How much variation exists across providers?
Which providers are outliers vs peers?
Does cost rise with chronic disease burden?
Do demographic factors like age affect costs?

Tools Used
Python
Pandas (data cleaning & aggregation)
NumPy (numerical operations)
Matplotlib (visualizations)
Jupyter Notebook


Dataset
Medicare synthetic claims sample:

DE1_0_2009_Beneficiary_Summary_File_Sample_20.csv
→ demographics + chronic condition flags

DE1_0_2008_to_2010_Outpatient_Claims_Sample_20.csv
→ outpatient claims + provider costs

Note: Dataset not included due to size and privacy considerations.
Please download the CMS synthetic claims dataset separately and place the CSV files in the project folder before running.

Methodology

1️⃣ Data Preparation

Loaded claims & beneficiary data
Converted date fields
Created age variable
Cleaned provider NPIs
Removed missing/zero costs

2️⃣ Feature Engineering
Created:

chronic_combo → illness combinations
chronic_count → number of conditions
Cost metric:
cost_per_member = total_cost / unique_members


3️⃣ Provider Benchmarking
Aggregation
Grouped by provider × illness:
total cost
unique members
cost per member

Distribution Analysis
Boxplots to visualize provider cost spread


Stability Filtering
Removed groups with only 1 patient

Standardization (fair comparison)
Used z-score normalization:
z = (cost − mean) / std

Provider Scoring
Identified consistently expensive providers using:
average z-score
share of high-cost occurrences
number of conditions treated



4️⃣ Exploratory Analysis
Additional insights:

Chronic count vs cost trend
Age group vs cost
Provider variation across illnesses
Demographic distributions


Key Insights

Costs increase with number of chronic conditions
Multiple comorbidities drive highest spending
Significant variation exists between providers treating the same illness
Removing single-patient providers stabilizes estimates
Several providers are consistently 3–5+ standard deviations above peers
Older age groups show higher average costs



Visualizations Included


Cost distribution by condition (boxplots)
Provider variation plots
Chronic count vs cost line chart
Age group vs cost bar chart
Provider outlier tables



▶️ How to Run

Step 1 – Clone repo
git clone <repo link>

Step 2 – Install dependencies
pip install pandas numpy matplotlib

Step 3 – Place CSVs in the same folder

Step 4 – Run notebook
jupyter notebook Optum_Data_Analyst.ipynb










