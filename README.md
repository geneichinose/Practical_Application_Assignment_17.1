# Practical_Application_Assignment_17.1


**Business Objectives:**

Direct marketing campaigns are an inexpensive means to increase sales; however, the performance of direct phone marketing is largely unknown.

In addition to performance, more information is need for improvement in efficiency of direct phone marketing. The goal is to examine the peformance of multiple classifier methods and find the one that returns the best metrics.

In this situation, there is a low cost or penalty for type-I errors i.e., false alarms. In this case we're targeting customers with phone calls who are likely to already purchase products.  This is inefficient and maximizing precision will reduce costs and being a nuisance to buying customers.  Type-II errors or missed detections is more detremental due to missing sales oppurtunities that would help. Here we wish to maximize recall score. Since there is a large class imbalance, accuracy score is not the best metric given that most of the feature space is of the class 0 or 'no sale'. We therefore optimize for maximum f1-score (combination of recall and precision) but utimately also want to pick methods that optimize recall.

Besides comparing the performance of the classifiers, we also wish to analize the imporatnce of the features and provide some interpretation so to improve the efficiency for future direct markinging methods. 

**Data Exploration and Feature Engineering:**

Correlation analysis indicates that duration, pdays, previous, emp.var.rate, euribor3m, and nr.employment correlates highest with target variable. 

![PCA.png](./PCA.png)

Cumlative Variance Explained versus principal components. 

Given that there are 53 features we use PCA do see if we can reduce the number of dimensions. The features are scaled by the mean and standard deviations. Based on the cummlative explain variance for all 53 principal components, we decided to grid search over 24, 32, 36 number of principal components to capture at least 75% to 95% of all the variance in the features. The significance, here is that we reduce the run time for training the classification models by using at least 17 fewer features.  As a test using some of the faster classifier models (not SVM), we also consider all original 53 untransformed features.

We hold out 30% of the dataset for later testing/scoring and train with 70%. We test the LogisticRegression, KNeighborsClassifier, SVM, and DecisionTreeClassifier. The hyperparameters for each of the classifiers were determined using GridSearchCV with 5-fold cross-validation. The scores are in the table below.

![SVM.png](./SVM.png)

Figure above shows the confusion matrix, ROC curve, and Precision-Recall curve with optimal SVM Classifier. 

<P>
<code>
                   CLF  PCA_ncomp  train_score  accuracy  precision  recall       f1  specificity    auc  fit_time
4                  SVC       36.0        0.825     0.896      0.544   0.443   0.489        0.953  0.866    60.412
6        DecisionTree2        NaN        0.590     0.907      0.641   0.390   0.485        0.972  0.865     0.088  
5         DecisionTree       36.0        0.752     0.889      0.509   0.434   0.469        0.947  0.766     0.878
1  LogisticRegression2        NaN        0.462     0.908      0.673   0.358   0.467        0.978  0.915     0.058   
0   LogisticRegression       36.0        0.459     0.907      0.662   0.348   0.456        0.977  0.914     0.175 
2           KNeighbors       32.0        0.453     0.899      0.615   0.273   0.378        0.978  0.831     0.141  
3          KNeighbors2        NaN        0.442     0.898      0.604   0.265   0.368        0.978  0.821     0.020 
</code>
</P>


   
**Findings:**

1. PCA reduced the number of features needed to train classifiers mainly reducing the number of non-important features by about 17 (53 to 36). The percent cumlative variance explaination was 95% with number of principal componets at 36. 

![Precision-Recall-Curve.png](Precision-Recall-Curve.png)

2. SVM and Decision Tree Classifier performed the best f1-score (see figure above and Table with scores). SVM took about 50 times slower than Decision Tree Classifier. 


3. We used the GridsearchCV for the best hyperparameters. These were used with the Decision Tree Classifier to estimate the Gini-feature Importance for the 53 features.  We find that the feature importance is the highest for the duration of the phone marketing calls, the number of days previous contact, and age. The months also factor in with March, October and June being most important. This is similar finding with the Moro and Laureano paper. 

![feature-importance.png](feature-importance.png)

Above is the DecisionTreeClassifier Gini-Feature importance as a function of the features. 

**Future work:**
1. The next step is to examine the thresholds which here are assumed to be probability of 0.5. 
2. We also did not use the social economic variables given that they would not be available for future predictions.  However, it would be interesting to included them to see if they give any improvement in the classifiers. 
