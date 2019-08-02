# Breast-Cancer-Binary-Logistic-Regression
Using Excel, Solver, and Real-Statistics Add-In to predict breast cancer.


__Introduction__

Data is from Kaggle: https://www.kaggle.com/uciml/breast-cancer-wisconsin-data

Real-statistics add-in: http://www.real-statistics.com/free-download/real-statistics-resource-pack/

Excel version: Mac 2011


__Data:__

- 569 Observations (rows)
- 357 Benign observations
- 212 Malignant observations
- 150 Benign and 150 Malignant observations used for model building
- 207 Benign and 62 Malignant observations used for model testing


__Methodology:__

I usually see people using a 70/30 split for training. I originally tried this approach, however, I was worried about class imbalance. 
I kept ~70% of malignant observations (150 out of 212) and ~45% of benign observations (150 out of 357), totaling ~53% (300 out of 569) of all available data for model building. I will refer to the 300 observations used as the trial stage and the remaining 269 observations as the testing stage.

I made a PivotTable for the averages of the variables of all observations as a preliminary method to view the data and choose the appropriate variables for model building. I kept a note of the variables' averages that had a stark contrast between classes. I chose the 10 real value variables with the _mean_ suffix instead of using the se (standard error) or worst (max or min) variables. Furthermore, I made a correlation matrix to determine multicollinearity since multicollinearity will decrease increase the precision of the variables. I determined that there was 25 unique combinations (models) between the 10 real variables in the dataset with averages and weak correlation in mind. 

After building the binary logistic regression models using Solver and the Real-Statistics add-in, I created a model comparison chart that recorded the number of variables, sum of log likelihoods (LL), chi-square, p-values, and significance. I also ranked the models in terms of LL values. I compared the models step-wise. I kept the degrees of freedom (df) equal to one and compared each model to its lowest value predecessor. 

For example: The Texture and Smoothness model had a LL value of ~-144. I compared the LL of the Texture and Smoothness model with the LL of the Texture model since the Texture model had a lower LL value (~-165) than the Smoothness Model (~-190) and kept the df equal to one to test the significance of the inclusion of the Smoothness variable. 

The Real-Statistics add-in determined the ideal coefficients per model. I used the calculated coefficients retrieved from the trial stage and implemented them in the testing stage (269 total observations, 207 benign observations, and 62 malignant observations) to determine the logit values. Below is a list of the equations used in the trial stage.

The logit equation is defined as:
  - B0 + B1V1 + B2V2 ... + BnVn
  
The odds equation is defined as:
  - e^logit
  
The probability that Y=1 equation is defined as:
  - e^logit/(1+e^logit)
  
The probability that Y=Y equation is defined as:
  - if diagnosis = 1, e^logit
  - if not diagnosis = 1, 1 - e^logit
  
The log likelihood (LL) equation is defined as:
  - the natural logarithm of the probability that Y = Y
  
The sum of the log likelihood equation is defined as:
  - ΣLL

I chose to have two different probability columns because the probability equation, by definition, evaluates the probability a given record has the Diagnosis value of 1 (malignant). What I am after is the probability is if a given record has a probability of 1 or 0 (malignant or benign). The first probability column is used in an IF function to test the model's prediction (if probability is <0.5, then 0; if probability >0.5, then 1) while the second probability column is used for the log likelihood equation since the LL is used as a criterion for the best coefficients (Bn).


__Result:__

The Texture, Area, and Smoothness Model yielded the results in the trial stage.
- Trial: 
  - 92.67% Malignant correct prediction
  - 94.00% Benign correct prediction
  - 93.33% Total correct prediction
  - Sum of log likelihood: -50.15
  - AUC = 0.9827
- Test:
  - 87.92% Benign correct prediction
  - 98.39% Malignant correct prediction
  - 90.33% Total correct prediction
  - Sum of log likelihood: -54.92

Multiple models yielded the same or better results in terms of overall correct prediction percentages in the test stage, specifically:
  - Texture and Concave Points Model
  - Area and Smoothness Model
  - Texture, Area, and Fractal Dimension Model
  - Texture, Fractal Dimension, and Concave Points Model. 
  
However, only the Texture and Concave Points Model and the Texture, Fractal Dimension, and Concave Points Model yielded a lower LL value than the Texture, Area, and Smoothness Model.

Texture and Concave Points Model
- Test:
  - 89.86% Benign correct prediction
  - 96.77% Malignant correct prediction
  - 91.45% Total correct prediction
  - Sum of log likelihood: -51.95
  
Area and Smoothness Model
- Test:
  - 89.86% Benign correct prediction
  - 95.16% Malignant correct prediction
  - 91.08% Total correct prediction
  - Sum of log likelihood: -63.81
  
Texture, Area, and Fractal Dimension Model
- Test:
  - 87.92% Benign correct prediction
  - 98.39% Malignant correct prediction
  - 90.33% Total correct prediction
  - Sum of log likelihood: -60.28

Texture, Fractal Dimension, and Concave Points Model
- Test:
  - 90.34% Benign correct prediction
  - 95.16% Malignant correct prediction
  - 91.45% Total correct prediction
  - Sum of log likelihood: -44.02
  
__Conclusion and Closing Remarks__

It was really interesting to see that models with only two variables yielded the same or better predictive results as a model that was optimized with three variables. It was also interesting to see that the Texture, Fractal Dimension, and Concave Points Model yielded better results than the Texture, Area, and Smoothness Model in all categories in the test stage while performing worse in the trial stage. Another interesting point to notice was the fact that the models that contained either the Concave Points variable or the Area variable were present in the models that performed the best. The single variable Concave Points Model and the Area Model also had the lowest LL values (-84.43 and -96.06 respectively) in regards to the other single variable models and some two variable models.

I hypothesize that there would be a change in the predictive capabilities of the models if there were more malignant observations within the testing stage. 
