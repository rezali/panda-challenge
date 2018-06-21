# panda-challenge
homework option #2 on school dataset

Observations
•	Smaller sized schools have higher average scores in math and reading.
•	Charter schools have higher average scores in math and reading than district schools.
•	Per student costs tend to be lower overall among the charter schools than the district schools.

```python import os import csv import pandas as pd ``` ```python #reference the file school_csv_path = "raw_data/schools_complete.csv" student_csv_path = "raw_data/students_complete.csv" ``` ```python #import the data into a panda dataframe school_df = pd.read_csv(school_csv_path) student_df = pd.read_csv(student_csv_path) ``` ```python # collect variables: total schools, students, budget,average math,average reading,%passing math,%passing read, %overall passing #district summary school_total = school_df["School ID"].count() student_total = school_df["size"].sum() student_total_formatted = '{:,}'.format(student_total) budget_total = school_df["budget"].sum() budget_total_formatted = '${:,.2f}'.format(budget_total) average_math = student_df["math_score"].mean() average_read = student_df["reading_score"].mean() passed_math = len(student_df.loc[student_df['math_score'] > 69])/len(student_df)*100 passed_math_formatted = '{:,.2f}%'.format(passed_math) passed_read = len(student_df.loc[student_df['reading_score'] > 69])/len(student_df)*100 passed_read_formatted = '{:,.2f}%'.format(passed_read) overall_passing = ((passed_math) + (passed_read))/2 overall_passing_formatted = '{:,.2f}%'.format(overall_passing) #school summary variable per_student_budget = school_df['budget']/school_df['size'] ``` ```python #create district summary table district_summary = pd.DataFrame({"Total Schools": [school_total], "Total Students": [student_total_formatted], "Total Budget": [budget_total_formatted], "Average Math Score": [average_math], "Average Reading Score": [average_read], "% Passing Math": [passed_math_formatted], "% Passing Reading": [passed_read_formatted], "% Overall Passing Rate": [overall_passing_formatted] }) ``` ```python #re-organize the columns organized_ds = district_summary[["Total Schools","Total Students","Total Budget", "Average Math Score","Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate" ]] organized_ds ``` 
	Total Schools	Total Students	Total Budget	Average Math Score	Average Reading Score	% Passing Math	% Passing Reading	% Overall Passing Rate
0	15	39,170	$24,649,428.00	78.985371	81.87784	74.98%	85.81%	80.39%
```python per_student_budget = school_df['budget']/school_df['size'] school_df.loc[:,'Per Student Budget'] = per_student_budget del school_df['School ID'] renamed_school_df = school_df.rename(columns={"name":"School Name"}) ``` ```python renamed_student_df = student_df.rename(columns={"school":"School Name"}) student_group = renamed_student_df[["School Name", "reading_score", "math_score"]] ``` ```python school_group = student_group.groupby(["School Name"], as_index=False) averages = school_group.mean() ``` ```python school_group = student_group.groupby(['School Name']) ``` ```python #combine the datasets. merge_table = pd.merge(renamed_school_df, averages, on="School Name") merge_table ``` 
	School Name	type	size	budget	Per Student Budget	reading_score	math_score
0	Huang High School	District	2917	1910635	655.0	81.182722	76.629414
1	Figueroa High School	District	2949	1884411	639.0	81.158020	76.711767
2	Shelton High School	Charter	1761	1056600	600.0	83.725724	83.359455
3	Hernandez High School	District	4635	3022020	652.0	80.934412	77.289752
4	Griffin High School	Charter	1468	917500	625.0	83.816757	83.351499
5	Wilson High School	Charter	2283	1319574	578.0	83.989488	83.274201
6	Cabrera High School	Charter	1858	1081356	582.0	83.975780	83.061895
7	Bailey High School	District	4976	3124928	628.0	81.033963	77.048432
8	Holden High School	Charter	427	248087	581.0	83.814988	83.803279
9	Pena High School	Charter	962	585858	609.0	84.044699	83.839917
10	Wright High School	Charter	1800	1049400	583.0	83.955000	83.682222
11	Rodriguez High School	District	3999	2547363	637.0	80.744686	76.842711
12	Johnson High School	District	4761	3094650	650.0	80.966394	77.072464
13	Ford High School	District	2739	1763916	644.0	80.746258	77.102592
14	Thomas High School	Charter	1635	1043130	638.0	83.848930	83.418349


