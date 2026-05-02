# Evaluating Student Performance Factors Using Linear Regression
This project showcases data cleaning and analysis techniques to explore factors that could affect student performance. The dataset used in this project was sourced from Kaggle and is intended for educational and portfolio purposes; therefore, findings should be interpreted as illustrative rather than definitive.

## Key Skills

## Project Overview
As a former English teacher and current language student, I’ve spent a lot of time developing an understanding of how people learn. Through my experience in education, I’ve observed patterns that appear to influence student performance, which I aim to examine more systematically in this project.

This project evaluates and identifies variables that are statistically significant to student performance-measured by exam scores. It demonstrates a complete analytical workflow, including data preprocessing, model fitting, evaluation, and statistical testing.

## Dataset Description
The dataset contains 20 variables describing student profiles, including:
* **Socioeconomic Factors:** School_Type, Access_to_Resources, Internet_Access, Tutoring_Sessions, Family_Income, Parental_Education_Level
* **Student Effort & Engagement:** Hours_Studied, Attendance, Motivation_Level
* **Academic Background:** Previous_Scores
* **School & Instructional Quality:** Teacher_Quality
* **Social & Environmental Influences:** Parental_Involvement, Peer_Influence
* **Lifestyle & Well-being:** Sleep_Hours, Physical_Activity, Extracurricular_Activities
* **Accessibility & Constraints:** Distance_from_Home, Learning_Disabilities
* **Demographics:** Gender
* **Target Variable:** Exam Score (integer 0-100)

## Methodology

### Data Cleaning and Preprocessing

The data cleaning process checked for and addressed the following:

1. Data Entry Errors: Checked for duplicate and impossible values (dropped 1 row with an exam score > 100%)
2. Standardization: Checked for standardization among categorical variables to ensure consistancy (e.g. variations in spelling and capitalization)
3. Null values: Identified and dropped rows with null values after performing MCAR test to evaluate any significant correlation to exam scores.
4. Outliers: Checked for extreme outliers

[Data cleaning process](student_performance/student_factors_cleaning.ipynb)

### Visualizing Data

Visualizing the data in Tableau revealed 4 factors that visibly show correlation with exam score, with attendance and hourt studied showing the largest change in exam performane and parental involvement and tutoring displaying a smaller change in score. 

![exam score (median) by attendance](student_performance/images/exam score (median) by attendance.png)

## Tableau Dashboard
[View Dashboard](https://public.tableau.com/views/studentexamscoreexploration/Dashboard1?:language=en-US&:sid=&:redirect=auth&publish=yes&showOnboarding=true&:display_count=n&:origin=viz_share_link)
