# Breast-Cancer-Binary-Logistic-Regression
Using Excel, Solver, and Real-Statistics Add-In to predict breast cancer.

Introduction
Data is from Kaggle: https://www.kaggle.com/uciml/breast-cancer-wisconsin-data

Real-statistics add-in: http://www.real-statistics.com/free-download/real-statistics-resource-pack/

Excel version: Mac 2011

Data:

- 569 Observations (rows)
- 357 Benign observations
- 212 Malignant observations
- 150 Benign and 150 Malignant observations used for model building
- 207 Benign and 62 Malignant observations used for model testing

Methodology:

I usually see people using a 70/30 split for training. I originally tried this approach, however, I was worried about class imbalance. 
I kept ~70% of malignant observations (150 out of 212) and ~45% of benign observations (150 out of 357), totaling ~53% (300 out of 569) of all available data for model building. 

I made a PivotTable for the averages of the variables. I chose to use the variables whose averages had stark differences between the classes (malignant vs benign).
The variables are:
  - Area
  - Texture
  - Compactness
  - Concavity
  
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

I made all possible models for differing degrees of freedom to compare the models and evaluate whether the addition of each new variable was significant by finding the differences between the sum of log likelihood (some sources refer to this as simply the log likelihood), finding the chi-square, and evaluating the p value. 

Throughout the variable evaluation phase, I followed the KISS philosophy (Keep It Simple, Stupid); the less complex the model is, the more reliable it is.

Result:

Intercept, Area, Compactness, Concavity, and Texture Model yielded the best predicition results: 
- Trial: 
- 90.7% Malignant correct prediction
- 94.0% Benign correct prediction
- 92.3% Total correct prediction
- Sum of log likelihood: -60.09897433

- Test:
- 90.8% Benign correct prediction
- 100.0% Malignant correct prediction
- 92.2% Total correct prediction
- Sum of log likelihood: -50.43476614

Intercept, Area, Compactness, and Texture model yielded the second best results:
- Trial: 
- 90.7% Malignant correct prediction
- 93.3% Benign correct prediction
- 92.0% Total correct prediction
- Sum of log likelihood: -61.60081906

- Test:
- 89.9% Benign correct prediction
- 100.0% Malignant correct prediction
- 91.5% Total correct prediction
- Sum of log likelihood: -55.03836556

However, when I compared the model to test the significance of the addition of Concavity variable, the p value of the model (0.22) was greater than the alpha cut-off (0.05 or outside the 95% confidence interval). 
The model with all four variables yielded better results than the model with the Area
