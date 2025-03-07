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

## Process

## Applications
Excel will be used to load the data and initially look for any issues. SQL will then be used to transform and explore the data. Finally, Tableau will be used to construct data visualisations. 

## Transform and Explore 
All SQL code can be found [here](https://github.com/barrett203/Student_Performance/blob/main/SQL_Code).

1) Create a Project ID on Big Query:
2) Create a dataset and table, naming everything appropriately:
3) Check for missing values:
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
4) Check for duplicates:
```
SELECT *, COUNT(*) AS duplicate_count
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY gender, ethnicity, parental_education, lunch, 
         test_preparation, math_score, reading_score, writing_score
HAVING COUNT(*) > 1;
```
## Analyze 

## Select summary statistics and visualizations 

1)Showcase the count of each unique variable that may impact student test performance:
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
![Summary](https://github.com/barrett203/Student_Performance/blob/main/Descriptive%20statistics%20.png "Summary")
* Students of other white backgrounds were the most represented by the data. Parents were most like likely to have obtaind some college qualifications or achieved an associate's degree. In addition, students were not likely to have completed some test preparation and were more frequently charged the standard fee for their school lunches. 

2)Showcase average test scores:

```
SELECT
    MIN(math_score) AS min_math, MAX(math_score) AS max_math, AVG(math_score) AS avg_math,
    MIN(reading_score) AS min_reading, MAX(reading_score) AS max_reading, AVG(reading_score) AS avg_reading,
    MIN(writing_score) AS min_writing, MAX(writing_score) AS max_writing, AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
```
![Scores](https://github.com/barrett203/Student_Performance/blob/main/Test%20scores.png "Scores")
* The average student scored 66.1/100 on their math test, 69.2 on their reading test and 68.1 on their writing test.

3)Standard deviation, variance and range of test scores:
```
SELECT 
    STDDEV(math_score) AS stddev_math, 
    VARIANCE(math_score) AS variance_math,
    MAX(math_score) - MIN(math_score) AS range_math,

    STDDEV(reading_score) AS stddev_reading, 
    VARIANCE(reading_score) AS variance_reading,
    MAX(reading_score) - MIN(reading_score) AS range_reading,

    STDDEV(writing_score) AS stddev_writing, 
    VARIANCE(writing_score) AS variance_writing,
    MAX(writing_score) - MIN(writing_score) AS range_writing
FROM `studentsystem-452900.StudentPerformance.Students`
```
![Spread](https://github.com/barrett203/Student_Performance/blob/main/Measures%20of%20dispersion%20.png "Spread")
* Math and Writing test scores have similar standard deviations and variances, indicating a similar spread of scores.
* Reading has the lowest standard deviation and variance, suggesting that students reading scores are more consistent than Math and Writing test scores.
* The range for Math is the highest, meaning it has the widest spread between the highest and lowest test scores.

4)Effect of gender:
```
SELECT gender,
       AVG(math_score) AS avg_math, 
       AVG(reading_score) AS avg_reading, 
       AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY gender;
```
![Spread](https://github.com/barrett203/Student_Performance/blob/main/GroupBy_Gender%20.png "Spread")
* On average female students scored higher on their reading and writing tests, whereas male students scored higher on their maths tests. 

5)Effect of ethnicity:
```
SELECT ethnicity,
       AVG(math_score) AS avg_math, 
       AVG(reading_score) AS avg_reading, 
       AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY ethnicity;
```
![Spread](https://github.com/barrett203/Student_Performance/blob/main/GroupBy-Ethnicity.png "Spread")
* On average Mixed White and Black Americans scored the highest on their tests, across all of Maths, Reading and Writing. 

6)Effect of parental education level:
```
SELECT parental_education,
       AVG(math_score) AS avg_math, 
       AVG(reading_score) AS avg_reading, 
       AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY parental_education;
```
![Spread](https://github.com/barrett203/Student_Performance/blob/main/GroupBy_ParentalEducation.png "Spread")
* On average students' parents who had obtained a Master's degree scored the highest on their tests, across all of Maths, Reading and Writing. 

7)Effect of lunch entitlement:
```
SELECT lunch,
       AVG(math_score) AS avg_math, 
       AVG(reading_score) AS avg_reading, 
       AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY lunch;
```
![Spread](https://github.com/barrett203/Student_Performance/blob/main/GroupBy_Lunch.png "Spread")
* On average students who were charged the standard fee for their school lunches scored the highest of their tests, across all of Maths, Reading and Writing. 

8)Effect of test preparation:
```
SELECT test_preparation,
       AVG(math_score) AS avg_math, 
       AVG(reading_score) AS avg_reading, 
       AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY test_preparation;
```
![Spread](https://github.com/barrett203/Student_Performance/blob/main/GroupBy_TestPreparation.png "Spread")
* On average students who took part in test preparation scored higher on their tests than those that did not, across all of Maths, Reading and Writing. 

9)Effect of gender and ethnicity on test scores:
```
SELECT gender, ethnicity,
       AVG(math_score) AS avg_math, 
       AVG(reading_score) AS avg_reading, 
       AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY gender, ethnicity;
```
![Spread](https://github.com/barrett203/Student_Performance/blob/main/Variables%201.png "Spread")

![Graph](https://github.com/barrett203/Student_Performance/blob/main/Variables%201%20graph.png "Graph")

10)Effect of parental education, lunch entitlement and test preparation:
```
SELECT test_preparation, lunch, parental_education,
       AVG(math_score) AS avg_math, 
       AVG(reading_score) AS avg_reading, 
       AVG(writing_score) AS avg_writing
FROM `studentsystem-452900.StudentPerformance.Students`
GROUP BY test_preparation, lunch, parental_education;
```
![Spread](https://github.com/barrett203/Student_Performance/blob/main/Variables%202.png "Spread")

![Graph](https://github.com/barrett203/Student_Performance/blob/main/Variables%202%20graph.png "Graph")

## Act
The results of the data analysis showcases that there are a variety of improvements that can be implemented by the educators to support students' test performance:
* Gender: Adjust teaching methods based on learning preferences (e.g., collaborative vs. competitive approaches). Encourage STEM participation if gender disparities are observed.
* Ethnicity: Provide culturally responsive teaching and inclusive curriculum materials. Offer language support programs if needed. Encourage mentorship programs with role models from similar backgrounds.
* Parental education level: Offer parent workshops to equip them with strategies to support learning at home. Implement after-school tutoring for students who may lack parental academic guidance. Provide homework help sessions or online resources for additional learning.
* Free lunch eligibility: Offer free tutoring, mentorship, and additional study resources. Ensure students have access to technology (laptops, internet) for remote learning. Provide breakfast programs to support focus and cognitive function.
* Test preparation: Make test prep programs more accessible (e.g., mandatory in-person classroom sessions, online modules). Provide incentives for completing test prep (e.g., extra credit, recognition). Use adaptive learning tools to personalize test preparation based on individual needs.

