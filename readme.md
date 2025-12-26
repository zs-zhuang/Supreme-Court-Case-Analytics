Supreme-Court-Case-Analytics

![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/blob/main/results/case_count_per_area_over_time.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/case_disposition_by_area.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/majOpinWriter_workload_vs_year_heatmap.jpg)
![Alt text]([https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/blob/main/results/case_duration_difference_posthoc_by_area.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/tbl_case_duration_correlation_interpretation.jpeg)

These scripts explores U.S. Supreme Court behavior and trend between the year 1946 to 2024 using the Modern Supreme Court Database (SCDB) housed at Penn State University. 

Here is a detailed breakdown of what each script does

Script: download_SCDB.ipynb
Data Used: the csv files associated with SCDB provided at https://scdb.la.psu.edu/data/
Purpose: This script will download SCDB as 8 seperate csv files. It include both case and justice centered data. In each case, it includes data organized by citations, dockets, issues, and issues including split vote. Each CSV file is stored in a separate directory inside the data directory with self-explanatory file names. 

Script: create_database_from_csv.ipynb
Data Used: All eight csv file described above
Purpose: (Optional Step) This script takes all 8 separate csv files and write the content into a database .db file using sqlite3. It is possible to skip this step and read csv files directly into pandas in later scripts. Users will have to make appropriate changes to the scripts if that's the case. The database or db file created is inside the data directory under the name legal_sc.db. Since this db file contains all eight tables from the 8 csv files, this is the file that will be used for analysis.

Script: gen_ID_map.ipynb
Data Used: legal_sc.db
Purpose: In the Supreme Court Database, many columns uses a numerical code instead of actual words to describe its content/value. For instance, in the issueArea column, the value 2 corresponds to Civil Rights, the value 3 corresponds to First Amendment. Likewise, the name of the justice in columns like majOpinWriter is also coded, where Roberts, John is coded as 111. For readibility, it is often desirable that these numerical code is converted back to English words, especially when plotting or presenting the finding. This script will either take a dictionary directly where the index = numerical code, key = English words, or it will take a table provided by the Supreme Court Database Online Codebook (http://scdb.wustl.edu/documentation.php), convert to dictionary, then eventually save the mapping as seperate modules in the scdb_code_map directory so that they can be imported into future scripts. 

Script: case_count_duration.ipynb
Data Used: legal_sc.db (Case Centered, Organized by Citations, Dockets, Issues, Issues including Split Vote)
Purpose: Preliminary data exploration. Checking data format, column names, size etc. Check for the 14 issue areas, which areas have higher case load than others. Did the popularity of these areas change over time? On average, what's the case duration for all cases since 1946. Is the distribution of case duration a normal distribution? Do average case duration differ significantly across issue areas (Kruskal–Wallis test followed by post-hoc test), and which areas drive delays? An optional simple sql alternative is included for the case popularity by area analysis for those who prefer it over pandas. The case duration analysis is done using pandas due to its superior handling of date formats. Results are also presented as tables, plots, heat maps. 

In addition, a full correlation and group-comparison analysis was conducted to examine the relationship between case duration and various case characteristics. Among numeric variables, majority votes showed a small-to-moderate negative correlation with case duration (Spearman ρ = −0.283, p < 0.001), while natural court (ρ = 0.256, p < 0.001) and term (ρ = 0.254, p < 0.001) showed small-to-moderate positive correlations. For binary variables, precedent alteration was associated with shorter case durations (rank-biserial r = −0.251, p < 0.001), while other indicators such as vote unclear and three-judge FDC had smaller effects. Among categorical variables, issue, decision type, and law supplement showed small but statistically significant associations with case duration (ε² = 0.085, 0.077, 0.063, respectively; all p < 0.001), while petitioner, issue area, and law type had smaller effects (ε² ≈ 0.036–0.042). Effects of jurisdiction, decision direction, and law minor were negligible. Overall, the analysis indicates that case duration is influenced by several procedural and case characteristics, with the strongest effects observed for majority votes, natural court, term, precedent alteration, and issue-related factors, though most associations are small in magnitude.

Script: case_decision_trend.ipynb
Data Used: legal_sc.db (Case Centered, Organized by Citations)
Purpose: This script looks at the overall reversal rate or how frequently the supreme court would reverse or vacate the lower court’s decision instead of affirming it, and whether that rate changes significantly in different areas. It also looks at whether the supreme court decision lean toward liberal or conservative side and how that tendency changes over time. In addition, it asks which type of petitioner is most likely to get the supreme court to vote in their favor, or how often the supremet court reached unanmious vote instead of close vote like 5 vs. 4 or what the decision type is (such as Opinion of the Court vs. Decree) And lastly, are any of those decision trend highly correlated with the issue area?

Script: case_majopinwriter.ipynb
Data Used: legal_sc.db (Case Centered, Organized by Citations)
Purpose: This script explores the workload assignment for the justices (specifically who wrote the majority opinion). It looks at which justice wrote the highest number of opinions over all. But also which justice is the most active writer for a given issue area. In addition, it also explores how many opinions a particual justice wrote in a given year, what their most active years are, and how that changes over time. Results are displayed as tables, bar plots and heat maps. 








