ReadMe  

# Credit Card Fraud

Authors: Kelly Epley, Adam Blomfield, and Helen Levy-Myers.

In this project, we trained a model to label transactions as fraudulent or valid. See the folder named "final" for notebooks containing full details on our data cleaning, exploratory data analysis, and modeling, and a slide deck describing our results. 

## Background

Credit card security is important to all concerned. Consumers are at risk for fraudulent transactions, merchants are often out of pocket for inventory and banks may be forced to credit vendors or consumers for losses. Investigating fraudulent and possible fraudulent transactions is an expense for the credit card issuers and a possible annoyance for consumers and merchants. Everyone has a vested interest in correctly identifying fraudulent credit card transactions so that they can be prevented.

The [data set]('https://www.kaggle.com/mlg-ulb/creditcardfraud') consisted of 284,807 European credit card transactions over two days in September 2013 with 492 identified as fraudulent, making the data set highly unbalanced. The data includes the following features: class (fraud or valid), amount, time of day in seconds  elapsed since the first transaction, and 28 other variables that are not identiified for confidentiality reasons. These masked variables have also been transformed using Principal Component Analysis. The type of currency in the amount column is also not identified. We have assumed it is Euros.

## Data Cleaning and Feature Engineering

This data set did not include any nulls or missing values. There were some duplicated rows, which were deleted. Time was converted to four time periods reflecting high transaction volume, low volume and shifting volume from low to high.

One possibility considered was to eliminate amounts below threshold. The logic is that if, for the sake of argument, it costs the bank €75 to investigate a potential fraudulent transaction, the bank should focus its efforts on finding fraudulent transactions above that amount becuase it could cost more money than it is worth to block smaller frauds. In the end, we opted not pursue this strategy for two reasons. One, the quantity of fraudulent transactions was so small that to lose just a few would potentially harm the model. 66 of the 492 identified frauds (or 13%) were less than €1, so we would have significantly fewer instances of fraud to work with. Two, it's possible that exluding frauds would create an opening for more frauds. Given that so many frauds were small amounts, it's plausible that fraudsters are testing the bank's algorithm to see if there is a floor under which transactions are monitored with less vigilance. 

## Exploratory Data Analysis

Time is one of the more interesting variables. Assuming that the inital transaction occured at midnight, there were some transactions until 1 am, then a significant lull until 7 am when again there is upward movement in the number of transactions. During the middle of the day the largest number of transactions occurred, then the amount declines until 1 am. Interestingly, although the overnight hours have fewer transactions, they have a higher ratio of fraud to valid transactions. 

Fraudulent transactions were skewed towards low amounts. Of the 492 fraudulent transactions, 25 had an amount of zero and an additional 41 had less than one Euro. 

A heat map of feature correlations were plotted revealed no high correlations, possibly because the entire data set had already been transformed with PCA.

## Models Explored 

We reviewed several sholarly articles on credit card fraud (The [data source]('https://www.kaggle.com/mlg-ulb/creditcardfraud') contains a list of recommended readings). We noticed that three machine learning models were frequently claimed to be most promising: logistic regression, SVM, and random forest. In our attempts, Random Forest produced the best results in terms of F1 scores.

Random Forests: Random forests produced the best results. The goal in building the model is to identify the greatest number of frauds, true positives, while keeping the misidentified frauds, false positives, which annoy merchants and credit card holders, to a minimum. Because of this, the focus is on the precision-recall curve. 

A grid search was used find out the optimal number of branches and sample splits to use. Because the data set is so unbalanced, class weighting with the target variable was used. Rather than simply oversampling with replacement of the fraud cases or undersampling the valid transactions to get a more balanced data set, SMOTE (Synthetic Minority Over-sampling Technique) is used. SMOTE creates new or synthetic data points for the smaller, minority part of the data set between the existing, real instances in the data set. It places these new data points on vectors between the existing minority instances. Cross validation is used to test the model. Results from the final confusion matrix showing the number of true positives (fraud cases, identified as fraud), true negatives (valid transactions, identified as valid), false positives (valid transactions identified as fraud) and false negative (fraud cases identified as valid transactions). The Precision-Recall curve plots these results. Precision is the ratio of true fraud cases to all the transactions identified as frauds (TP/(TP+FP)). Recall is the ratio of true fraud cases to all the true fraud cases TP/(TP+FN). 

The PR curve for this model shows how the precision and recall scores change as we adjust the probability threshold for a positive identification. The precision score dips when the number of false positives increases and the recall score dips when the number of false negatives increases. The more the curve hugs the upper right corner, the better the model does at correctly identifying true positives,  minimizing both false positives and false negatives.

![precision-recall-curve](reports/figures/pr_curve.png)

Our final model correctly identified 111 out of 142 frauds. It misidentified 31 frauds as valid transactions and 68 valid transactions as fraud, with an F1 score of 0.69. 



 



