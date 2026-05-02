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

Visual analysis in Tableau revealed four variables that show a visible relationship with median exam score: attendance, hours studied, tutoring sessions, and parental involvement. Among these, attendance and hours studied demonstrate the strongest variation in median scores, while parental involvement and tutoring sessions show more modest differences. 

![exam score (median) by attendance](./images/exam_score_(median)_by_attendance.png)

*Figure 1: Chart displaying median exam score by attendance shows a positive correlation across all teaching quality levels*

![exam score (median) by hours studied](./images/exam_score_(median)_by_hours_studied.png)

*Figure 2: median exam score by hours studied*

Figure 1 and Figure 2 show a noticable correlation between factors and median exam score and a larger range in median scores signalling a potential cause and effect relationship. Based on these figures we try to fit a linear model with the focus on evaluating the significance of attendance and hours studied on a student's exam score.

## Tableau Dashboard
[View Dashboard](https://public.tableau.com/views/studentexamscoreexploration/Dashboard1?:language=en-US&:sid=&:redirect=auth&publish=yes&showOnboarding=true&:display_count=n&:origin=viz_share_link)

## Evaluating Linearity and Checking Model Assumptions

As expected, a scatterplot of Exam scores vs Attendance and Exam Scores vs Hours studied show a weak positive linear relationship for these variables with a large amount of noise which may be the influence of other variables.

![Scatterplot of Exam Score vs Attendance](./images/ES_Attendance_Scatterplot.png)


![Scatterplot of Exam Score vs Hours Studied](./images/ES_Hours_Studied_Scatterplot.png)

First I checked the for linearity and normality of residuals by plotting the residuals vs fitted values and a qq-plot of the residuals.

![Homoscedasticity check](./images/fitted_vs_resid.png)
![QQ-plot](./images/qq-plot.png)

These both indicate issues with our model.
