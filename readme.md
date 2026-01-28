# U.S. Supreme Court Analysis Scripts

![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/case_count_per_area_over_time.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/case_disposition_by_area.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/division_by_lawType_IssueArea.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/majOpinWriter_workload_vs_year_heatmap.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/case_duration_distribution_by_area.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/case_duration_difference_posthoc_by_area.jpg)
![Alt text](https://github.com/zs-zhuang/Supreme-Court-Case-Analytics/raw/main/results/tbl_case_duration_correlation_interpretation.jpg)

These scripts explore U.S. Supreme Court behavior and trends between the year 1946 to 2024 using the Modern Supreme Court Database (SCDB) housed at Penn State University. 

Here is a detailed breakdown of what each script does

---

### **Script: download_SCDB.ipynb**  
**Data Used:** The CSV files associated with SCDB provided at [SCDB Data](https://scdb.la.psu.edu/data/)  
**Purpose:** This script will download SCDB as 8 separate CSV files. It includes both case- and justice-centered data. Each case includes data organized by citations, dockets, issues with and without split votes. Each CSV file is stored in a separate directory inside the `data` directory with self-explanatory file names.

---

### **Script: create_database_from_csv.ipynb**  
**Data Used:** All eight CSV files described above  
**Purpose:** *(Optional Step)* This script takes all 8 separate CSV files and writes the content into a database `.db` file using `sqlite3`. It is possible to skip this step and read CSV files directly into pandas in later scripts; users will need to make appropriate changes if so. The database file is stored inside the `data` directory as `legal_sc.db` and contains all eight tables for analysis.

---

### **Script: gen_ID_map.ipynb**  
**Data Used:** `legal_sc.db`  
**Purpose:** In the SCDB, many columns use numerical codes instead of actual words (e.g., `issueArea`: 2 = Civil Rights, 3 = First Amendment; `majOpinWriter`: Roberts, John = 111). For readability, this script converts these numerical codes back to English words using either a dictionary or the SCDB Online Codebook. The mapping is saved as separate modules in the `scdb_code_map` directory for use in future scripts.

---

### **Script: inspection.ipynb**  
**Data Used:** `legal_sc.db` (Case-centered, organized by citations, dockets, issues, and issues including split vote)  
**Purpose:** Preliminary data exploration:
- Check data format, column names, size, common values etc.  

---

### **Script: case_count_duration.ipynb**  
**Data Used:** `legal_sc.db` (Case-centered, organized by citations)  
**Purpose:** explores case popularity, issue areas, durations and general trends: 
- Examines the 14 issue areas to identify which have higher caseloads and how their popularity has changed over time 
- Computes average case durations since 1946 and plot the distributions (normality check)
- Tests whether average case duration differs across issue areas (Kruskal–Wallis test + post-hoc test + effect size) and identifies which areas drive delays
- Shows total counts of Supreme Court oral arguments by month, highlighting peak months (October–April) and summer recess

Optional SQL alternatives are included for case popularity analysis. Case duration analysis uses pandas for better date handling. Results are presented as tables, plots, and heat maps

**Additional Analysis:** Full correlation and group-comparison analysis examines relationships between case duration and various case characteristics:
- Numeric: `majVotes` (ρ = −0.283), `naturalCourt` (ρ = 0.256), `term` (ρ = 0.254)  
- Binary: `precedentAlteration` (r = −0.251). Other factors have smaller effects. 
- Categorical: `issue`, `decisionType`, `lawSupp` have small but significant effects on case duration. Other factors have smaller or negligible effects

Overall, case duration is influenced by several characteristics, with the strongest effects observed for the size of the majority, the natural court, the term, whether precedent was altered, and what specific issues were involved

---

### **Script: case_decision_trend.ipynb**  
**Data Used:** `legal_sc.db` (Case-centered, organized by citations)  
**Purpose:** Examines patterns and trends in Supreme Court decisions:
- Examines reversal rates of lower court decisions and variation across issue areas  
- Tracks ideological trends, comparing liberal and conservative leanings over time  
- Analyzes petitioner success rates, vote margins (unanimous vs. close), and decision types (Opinion of the Court, Decree, etc.)  

---

### **Script: case_majopinwriter.ipynb**  
**Data Used:** `legal_sc.db` (Case-centered, organized by citations)  
**Purpose:** Explores workload assignment for justices:
- Identifies which justice wrote the most majority opinions overall and by issue area  
- Tracks the number of opinions per justice per year, highlighting the most active years and changes over time  

---

### **Script: case_lawType.ipynb**  
**Data Used:** `legal_sc.db` (Case-centered, organized by citations)  
**Purpose:** Explore Supreme Court law citations and decision patterns:
- Identifies the most frequently cited laws
- Analyzes whether decisions involving the most cited laws have become more or less liberal over time
- Examines whether the Court leans conservative or liberal based on the primary legal authority cited in the case (e.g., statutory, judicial review, administrative)
- Investigates whether the Court is more ideologically divided (e.g., 5–4 or 6–3 votes) when interpreting Federal Statutes compared to the Constitution (also repeats the analysis for separate issue areas)
- Determines which laws are most likely to have their precedents altered
- Analyzes how lower court type and law category affect Supreme Court reversal rates






