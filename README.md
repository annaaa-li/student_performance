# Evaluating Student Performance Factors Using Linear Regression
This project showcases data cleaning and analysis techniques to explore factors that could affect student performance. The dataset used in this project was sourced from Kaggle and is intended for educational and portfolio purposes; therefore, findings should be interpreted as illustrative rather than definitive.

## Key Skills
* Cleaning and Preparing Data
* Linear Regression

## Project Overview
As a former English teacher and current language student, I’ve spent a lot of time developing an understanding of how people learn. Through my experience in education, I’ve observed patterns that appear to influence student performance, which I aim to examine more systematically in this project.

This project evaluates and identifies variables that are statistically significant to student performance-measured by exam scores. It demonstrates a complete analytical workflow, including data preprocessing, model fitting, evaluation, and statistical testing.

## Dataset Description
The dataset contains a total of 6377 observations and 20 variables describing student profiles, including:
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

[Data cleaning process](student_performance/student_factors_cleaning.ipynb)

### Visualizing Data

Visual analysis in Tableau revealed four variables that show a visible relationship with median exam score: attendance, hours studied, tutoring sessions, and parental involvement. Among these, attendance and hours studied demonstrate the strongest variation in median scores, while parental involvement and tutoring sessions show more modest differences. 

<img src="https://github.com/annaaa-li/student_performance/blob/main/images/exam_score_(median)_by_attendance.png" width="700" height="500"/>

*Figure 1:  positive correlation across all teaching quality levels for median exam score and attendance*

<img src="https://github.com/annaaa-li/student_performance/blob/main/images/exam_score_(median)_by_hours_studied.png" width="700" height="500"/>

*Figure 2: positive correlation for median exam score and hours studied with a slight drop at right tail end*

Figure 1 and Figure 2 show a noticable correlation between factors and median exam score and a larger range in median scores signalling a potential cause and effect relationship. Based on these figures we try to fit a linear model with the focus on evaluating the significance of attendance and hours studied on a student's exam score.

[Dashboard](https://public.tableau.com/views/studentexamscoreexploration/Dashboard1?:language=en-US&:sid=&:redirect=auth&publish=yes&showOnboarding=true&:display_count=n&:origin=viz_share_link)

### Fitting the Model and Checking Model Assumptions
I started by running a linear regression with Exam Score as the response variable: Exam_Score ~ Attendance + Previous_Scores + Hours_Studied + Motivation_Level + Sleep_Hours + Tutoring_Sessions + Family_Income + Parental_Education_Level + Parental_Involvement + Teacher_Quality + School_Type + Learning_Disabilities + Gender.


*Not included: Access_to_Resources Extracurricular_Activities Internet_Access Peer_Influence Physical_Activity Distance_from_Home*

**The workflow for checking model assumptions for an LM is as follows:**
* Residuals vs fitted → linearity + homoscedasticity (visual)
* QQ plot → normality
* Breusch–Pagan → formal heteroskedasticity/homoscedasticity test
* VIF → multicollinearity
* Cook’s distance / leverage → influence


#### Linearity, Homoskedasticity and Normality
To check these assumptions I plotted the residuals vs fitted values and a qq-plot.

Fitted Values vs Residuals             |  QQ-plot
:-------------------------:|:-------------------------:
<img src="https://github.com/annaaa-li/student_performance/blob/main/images/fitted_vs_resid.png" width="400" height="300"/>  |  <img src="https://github.com/annaaa-li/student_performance/blob/main/images/qq-plot.png" width="400" height="300"/>
*Figure 3: Fitted values vs Residuals checks linearity and homoskedasticity* |  *Figure 4: QQ-plot checks normality of residuals*


These both indicate issues with our model. For our assumptions to be satisfied we would expect to see a random scatter on the fitted vs residuals plot and points forming a straight diagonal line on the qq-plot. These graphs show 2 distinct clusters. Further examination of the observations that resulted in large residual values found that they were all from high performing students (with exam scores >=78). This suggests our model does a poor job of predicting high performing students possibly due to key variables missing. 

54 observations from the dataset are high performing students (with exam scores >=78) with 49 out of 54 observations contributing to very large (abnormal) residual values. This does not appear to be an issue that can be fixed by adding a nonlinear or interaction term.

<img src="https://github.com/annaaa-li/student_performance/blob/main/images/hist_exam_scores.png" width="500" height="400"/> 

*Figure 5: Histogram of Exam Scores*

Looking at a histogram of the exam scores also pictures high scorers as a very small proportion of the overall sample population. As we are unable to accurately account for high performing students I decided to redefine my analysis to focus on exploring what factors influence exam scores among mid-performing students.

#### Removing High Performers and Refitting the Data

<img src="https://github.com/annaaa-li/student_performance/blob/main/images/qq-plot_avg_students.png" width="500" height="400"/> 
*Figure 6: QQ-plot of residuals for model fit with data from mid-performing students*


After dropping the observations of high performing students and refitting the model we get a much more reasonable qq-plot with very mild non-normality at the tails which we can ignore since our sample size is rather large.

<img src="https://github.com/annaaa-li/student_performance/blob/main/images/fitted_vs_resid_avg_students.png" width="500" height="400"/> 

*Figure 7: fitted values vs residuals for model fit with data from mid-performing students*


However the fitted values vs residuals plot shows diagonal banding as opposed to a random scatter. A random scatter is expected and checks the assumption of linearity and the assumption of homoskedasticity. Within each band the residuals appear to be randomly scattered around 0 and there is no curvature or systematic shape within the bands so we assume linearity holds.

Additionally, performing a Breusch-Pagan test shows there is no statistical evidence of heteroskedasticity and our model assumption of homoskedasticity hold.

#### Multicollinearity and Influential Points

Calculating Variance Inflation Factor (VIF) for the model results in values between 1-2 which implies very mild multicollinearity, so the model assumption is satisfied.

<img src="https://github.com/annaaa-li/student_performance/blob/main/images/cooks_distance.png" width="500" height="400"/> 
*Figure 8: Plot of Cook's Distance values and threshold line for identifying influential observations*

Last, we check influence using Cook's distance. Calculating and plotting Cook's distance results in 254 observations with values above the Cook's distance threshold (~0.00063) which is calculated as 4/n, where n is the total number of observations in the dataset. Since the dataset has over 6000 observations this threshold is very small. In this case all of the observations above the threshold are also very small (<<0.01) and are likely not to be a threat or significant influence on the model.


As a pre-caution I inspected the top 20 points with the largest Cook's distance values and refit the model excluding the points above the threshold. The coefficients, standard errors, and $$R^{2}$$ do not change substantially between the two models so it is reasonable to assume that the points are not that influential.

This is means that our model satisfies all the model assumptions.

## Results


### Conclusion
This project successfully developed a linear regression model that predicts student exam score for average performing students (with scores between 50%-78%) with 91% variance explained (R² = 0.910) and an average prediction error of approximately ___ (14.4% relative error).

### Libraries Used:
* **Data manipulation:** pandas, numpy
* **Visualization:** matplotlib
* **Modelling and Evaluation:** statsmodels

### Author
Anna Li


