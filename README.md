# Student_Performance- Fictional Dataset 
## What trends are there in student exam performance?

<img align="right" width="450" height="425" src="https://www.the74million.org/wp-content/uploads/2023/03/student-test-performance.jpg">

## Ask
### Key stakeholders
* Educators – Use the analysis to assess teaching effectiveness, identify struggling students, and tailor instruction.
* Parents/Guardians – Interested in their child's academic progress and potential areas for improvement.
* Government and Education Boards – Use data to assess school performance, shape policies, and allocate funding.
* Employers and Higher Education Institutions – May use aggregated insights to understand student preparedness for the workforce or further studies.

## Prepare 

### Data source: 

Student Exam Performance Data available on [Kaggle](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams/code?datasetId=74977&sortBy=voteCount). The data contains survey responses from 1,000 students. This dataset contains input related to gender, ethnicity, parental level of education, whether the student is entitled to a free/reduced cost school lunch, exam preparation and math, reading and writing test scores. The data dictionary can be found [here](https://github.com/barrett203/Student_Performance/blob/main/Data%20dictionary%20.png).

# Process

## Applications
Excel will be used to load the data and initially look for any issues. SQL will then be used to transform and explore the data. Finally, Tableau will be used to construct data visualisations. 

## Transform and Explore 
All SQL code can be found [here](https://github.com/barrett203/Student_Performance/blob/main/SQL_Code).

1) Create a Project ID on Big Query
2) Create a dataset and table, naming everything appropriately
3) Check for missing values
```
SELECT 
    COUNT(*) - COUNT(gender) AS missing_gender,
    COUNT(*) - COUNT(ethnicity) AS missing_ethnicity,
    COUNT(*) - COUNT(parental_education) AS missing_parent_edu,
    COUNT(*) - COUNT(lunch) AS missing_lunch,
    COUNT(*) - COUNT(test_preparation) AS missing_test_prep,
    COUNT(*) - COUNT(math_score) AS missing_math,
    COUNT(*) - COUNT(reading_score) AS missing_reading,
    COUNT(*) - COUNT(writing_score) AS missing_writing
FROM `studentsystem-452900.StudentPerformance.Students`
```
4) Check for duplicates
```
SELECT *, COUNT(*) AS duplicate_count
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY gender, ethnicity, parental_education, lunch, 
         test_preparation, math_score, reading_score, writing_score
HAVING COUNT(*) > 1;
```
# Analyze 

## Select summary statistics and visualizations 

1)Showcase the count of each unique variable that may impact student test performance 
```
SELECT gender AS category, COUNT(*) AS count FROM `studentsystem-452900.StudentPerformance.Students` GROUP BY gender
UNION ALL
SELECT ethnicity AS category, COUNT(*) FROM `studentsystem-452900.StudentPerformance.Students` GROUP BY ethnicity
UNION ALL
SELECT parental_education AS category, COUNT(*) FROM `studentsystem-452900.StudentPerformance.Students` GROUP BY parental_education
UNION ALL
SELECT lunch AS category, COUNT(*) FROM `studentsystem-452900.StudentPerformance.Students` GROUP BY lunch
UNION ALL
SELECT test_preparation AS category, COUNT(*) FROM `studentsystem-452900.StudentPerformance.Students` GROUP BY test_preparation;
```

2)Showcase average test scores

```
SELECT
    MIN(math_score) AS min_math, MAX(math_score) AS max_math, AVG(math_score) AS avg_math,
    MIN(reading_score) AS min_reading, MAX(reading_score) AS max_reading, AVG(reading_score) AS avg_reading,
    MIN(writing_score) AS min_writing, MAX(writing_score) AS max_writing, AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
```
![Scores](https://github.com/barrett203/Student_Performance/blob/main/Test%20scores.png "Scores")
* The average student scored 66.1/100 on their math test, 69.2 on their reading test and 68.1 on their writing test.


