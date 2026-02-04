1) Objective

Using the Beneficiary Summary and Outpatient Claims datasets:

Calculate cost per member for each provider (AT_PHYSN_NPI) and chronic illness combination

Represent the distribution of provider costs for each chronic illness combination

Recalculate results after filtering out provider–illness groups with only 1 member

Identify providers that are consistently expensive across illnesses, accounting for different illness baselines (standardization)

2) Repository Contents

Optum_Data_Analyst.ipynb — Main notebook containing:

Data cleaning

Feature engineering (chronic combos, chronic counts, age)

Aggregations and benchmarking

Visualizations (boxplots, trends)

Provider outlier scoring

(Optional) README.md — This file

Note: The datasets are not included in the repository. Please place them locally as described in the “How to Run” section.

3) Dataset Requirements

Synthetic Medicare claims sample:

Required files (place in same folder as the notebook)

DE1_0_2009_Beneficiary_Summary_File_Sample_20.csv

DE1_0_2008_to_2010_Outpatient_Claims_Sample_20.csv

Dataset Notes

Beneficiary file includes demographics + chronic condition indicator flags

Outpatient claims file includes claim cost and provider NPI fields

4) Tools & Libraries Used

Python

pandas — data loading, joins, groupby aggregations

numpy — numerical operations

matplotlib — charts & formatting

Optional

Power BI dashboard (interactive visuals using engineered outputs)

5) Methodology
   
5.1 Data Preparation

Loaded Beneficiary and Outpatient data

Parsed date fields (BENE_BIRTH_DT, BENE_DEATH_DT, CLM_FROM_DT, CLM_THRU_DT)

Created Age from beneficiary birth year (using 2009 as reference year)

Cleaned provider NPI values (handled scientific notation using integer type)

Created a cost variable: cost = CLM_PMT_AMT

Removed zero-cost claims to keep cost metrics meaningful

Dropped missing AT_PHYSN_NPI only for provider benchmarking steps (required for provider-level analysis)

5.2 Feature Engineering

Built chronic condition combination chronic_combo by concatenating all chronic flags = 1

If none → None

If 3+ conditions → Multiple

Built chronic condition count chronic_count = number of chronic flags = 1 for each beneficiary

Merged claims + beneficiary fields using DESYNPUF_ID

5.3 Core Metrics

Total cost by chronic combo

Cost per member by chronic combo

cost_per_member = total_cost / unique_members

Provider × chronic combo benchmarking

groupby(chronic_combo, AT_PHYSN_NPI) → total cost, members, cost per member

5.4 Filtering Step (Noise Reduction)

Repeated distribution analysis after filtering out provider–combo groups with only 1 member

This reduces volatility from “single-patient providers” and stabilizes cost estimates.

5.5 Standardized Provider Benchmarking (Z-score)

To compare providers fairly across conditions with different baseline costs, computed:

𝑧 = (cost_per_member − mean(cost_per_member)) / std(cost_per_member)
   
             
This produces z_within_combo = standardized expensiveness relative to peer providers treating the same illness.

Then identified consistently expensive providers using:

combos_treated = number of unique combos treated

avg_z = average standardized expensiveness

share_high = share of combos where z > 1 (provider is high-cost vs peers)

6) Results (Objectives Reached)
   
 Objective 1 — Cost per member by Provider × Chronic Combo

Created prov_combo dataset:

total_cost, members, cost_per_member per (AT_PHYSN_NPI, chronic_combo)

 Objective 2 — Distribution of provider costs per chronic combo

Used:

Summary distribution table (mean, median, p90, p95)

Boxplots across top chronic combinations

 Objective 3 — Change after filtering provider–combo groups with only 1 member

Created prov_combo_2plus (members > 1)

Produced side-by-side comparison table for All vs 2+ Members

 Objective 4 — Identify consistently expensive providers across conditions

Computed z-score within each combo (z_within_combo)

Ranked providers by:

average z-score (avg_z)

share of high-cost occurrences (share_high)

breadth of conditions treated (combos_treated)

7) Additional EDA (Beyond Requirements)

To better explain cost drivers, also explored:

Chronic Count vs Average Cost (trend increases with higher disease burden)

Age Group vs Average Cost

Disease co-occurrence using conditional probabilities (optional enhancement)

8) Summary

This project demonstrates:

End-to-end EDA on healthcare claims data

Feature engineering for chronic conditions

Provider benchmarking with appropriate cleaning/filtering

Standardized comparisons across heterogeneous chronic illness cost baselines

Key takeaways:

Costs rise with greater chronic disease burden

“Multiple” comorbidities drive the largest share of total costs

Provider costs vary widely within the same condition

Filtering out single-member provider groups stabilizes distribution metrics

Z-score benchmarking highlights providers consistently expensive across conditions



10) References

CMS Synthetic Public Use Files (DE-SynPUF) documentation for variable meaning and coding conventions


