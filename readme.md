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
**Purpose:** explores case issue areas and durations: 
- Examine the 14 issue areas: which have higher case loads, and how did popularity change over time?  
- Compute average case durations since 1946 and examine the distributions (normality check).  
- Test whether average case duration differs across issue areas (Kruskal–Wallis test + post-hoc test + effect size) and identify which areas drive delays.  

Optional SQL alternatives are included for case popularity analysis. Case duration analysis uses pandas for better date handling. Results are presented as tables, plots, and heat maps.

**Additional Analysis:** Full correlation and group-comparison analysis examines relationships between case duration and various case characteristics:
- Numeric: `majVotes` (ρ = −0.283), `naturalCourt` (ρ = 0.256), `term` (ρ = 0.254)  
- Binary: `precedentAlteration` (r = −0.251). Other factors have smaller effects. 
- Categorical: `issue`, `decisionType`, `lawSupp` have small but significant effects on case duration. Other factors have smaller or negligible effects.  

Overall, case duration is influenced by several procedural and case characteristics, with the strongest effects observed for the size of the majority, the natural court, the term, whether precedent was altered, and what specific issues were involved.

---

### **Script: case_decision_trend.ipynb**  
**Data Used:** `legal_sc.db` (Case-centered, organized by citations)  
**Purpose:** Examines patterns and trends in Supreme Court decisions:
- Reversal rate of lower court decisions and variation across issue areas  
- Liberal vs. conservative leanings and trends over time  
- Petitioner success rates, unanimous vs. close votes, and decision type (Opinion of the Court vs. Decree etc.)  

---

### **Script: case_majopinwriter.ipynb**  
**Data Used:** `legal_sc.db` (Case-centered, organized by citations)  
**Purpose:** Explores workload assignment for justices:
- Which justice wrote the most majority opinions overall and by issue area  
- Number of opinions per justice per year, most active years, and changes over time  

---

### **Script: case_lawType.ipynb**  
**Data Used:** `legal_sc.db` (Case-centered, organized by citations)  
**Purpose:** Explores most cited laws in supreme court cases:
- Which laws are cited most often?
- For the most cited laws, whether decisions have become more or less liberal over time
- Examines how the Supreme Court’s conservative decisions vary across different types of legal authority (e.g., statutory, judicial review, administrative)
- Investigates whether the Supreme Court is more ideologically divided when interpreting the Constitution compared to Federal Statutes across various issue areas.






