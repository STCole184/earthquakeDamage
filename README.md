# earthquakeDamage

This is my solution to the DrivenData competition Richter's Predictor: Modeling Earthquake Damage. As of the writing of this notebook, my solution is ranked 389 of 2290 solutions, i.e. it is in the 83rd percentile.

I tried a number of solutions on my way to this solution. A summary of my experiments and their results are below.

My best solution ended up using all features except for the geo_level_3_id feature. This feature had too many values to efficiently one-hot encode and use in training my algorithms. It also used regression instead of categorization. The problem is one of ordinal classification, and so the regression model was appropriate. I then took the output of the regression model and manually found optimal thresholds between damage_levels 1 and 2, and 2 and 3. These thresholds were not at the 0.5 mark, and so this manual discovery of thresholds added value to the model.

As you can see below, I tried using class rebalancing techniques such as SMOTE and random undersampling of the dominant class of data since the classes of the data were fairly imbalanced. I did not find these techniques to improve my model however.

I found my model to over predict the dominant class (damage_level 2) in all of my experiments. Setting thresholds for regression scores between the classes helped to ameliorate this deficiency, but the deficiency remains in my model with recall scores for the minority classes at 47% and 59%. Even breaking the problem down into two binary classifiers to differentiate between damage_level 1 and the rest of the data points, and damage_level 3 and the rest of the data points did not improve my model's performance on this front, or at all.


**Experiments:**

Baseline (random forest classifier, no one-hot encoding of geo features which are technically numeric, even though those numbers are just codes and have no numeric meaning) F1 score: 0.5749697818537634; Conditions: all features, geo features not categorical

RandomForest classifier with only geo_1 one-hot encoded, F1 score: 0.626196734521594; Conditions: did not use geo_2 or geo_3 but made geo_1 categorical; Issues: predictions highly skewed to class 2

GradientBoost classifier with only geo_1: 0.684; Conditions: did not use geo_2 or geo_3 but made geo_1 categorical, used GradientBoost instead of RandomForest; Issues: predictions still skewed to class 2 but not as much

XGBoostClassifier with geo_1 and geo_2: 0.674; Conditions: did not use geo_3 but made geo_1 and geo_2 categorical, used XGBoost instead of RandomForest; Issues: predictions still skewed to class 2 but not as much

XGBoostRegressor with geo_1 and geo_2: 0.714; Conditions: did not use geo_3 but made geo_1 and geo_2 categorical, used XGBoost instead of RandomForest; Issues: predictions still skewed to class 2 but not as much

XGBoostRegressor with geo_1 and geo_2 and custom thresholds: 0.7175; Conditions: did not use geo_3 but made geo_1 and geo_2 categorical, used XGBoost instead of RandomForest; Issues: predictions still skewed to class 2 but not as much

XGBoostRegressor with geo_1 and geo_2 and custom thresholds and smote for balancing training classes: 0.695; Conditions: did not use geo_3 but made geo_1 and geo_2 categorical, used XGBoost instead of RandomForest; Issues: predictions still skewed to class 2 but not as much

XGBoostRegressor with geo_1 and geo_2 and custom thresholds and random undersampling for balancing training classes: 0.698; Conditions: did not use geo_3 but made geo_1 and geo_2 categorical, used XGBoost instead of RandomForest; Issues: predictions still skewed to class 2 but not as much

Two binary XGBoostClassifiers with geo_1 and geo_2 and custom thresholds to make the classification task easier by splitting it into two classifiers (one to identify just damage_level 1 and one to identify just damage_level 2): 0.674; Conditions: did not use geo_3 but made geo_1 and geo_2 categorical, used XGBoost instead of RandomForest; Issues: I could not get the individual classifier to perform better than the single classifier for all categories.
