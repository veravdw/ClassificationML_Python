# Stroke Prediction

The aim of this project is to create a model to predict which people will be more at risk of getting a stroke, and to identify risk factors. 

## Format

The project uses python, in a jupyter notebook. 

## Dataset

The dataset for this analysis is a sample of people of which some got a stroke, consisting of a few qualitative and quantitative features. 
It comes from a website called Kaggle, to be found in csv format via this url: https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset
After some exploratory analysis we find the following: 
- The dataset consists of 5110 cases, of which none got taken out with data cleaning. 
- Our target feature: 'stroke', is binary (yes/no). Thus, we can train a binary classification model based on this dataset, and evaluate how well the model can predict stroke cases based on a hold-out set. If performing sufficiently accurate, it can be used to predict for future cases whether they will be likely to get a stroke or not.
- Only 5% of cases in this dataset suffers from a stroke, therefore we apply weights and use hyperparameters to improve model performance under the imbalance.
- The model performance metric used is F1 score, as accuracy will overrate performance. 
- There are 3 continuous features for bmi, age, and glucose levels, of which only bmi is normally distributed. 
- No correlations between continuous features are found.
- There are 6 categorical features for gender, smoking, marital status, hypertension, heart disease, residence type and type of work.

 ## Predictive Modelling

- First we run a single decision tree, in case no boosting method would outperform this one, we could still choose the single tree for the sake of speed, explainability and to save on computing cost. Using a weight parameter for sample balance, after one round of tuning we reached an F1 score of only 0.22.
- Then we ran several boosted tree models on the training data: xgboost, lightgbm, catboost and sklearn's GradientBoostingClassifier, and also randomforest, and compared their performance in terms of cross-validated (10-fold) F1 and AUC. Lightgbm had highest F1 score, however this type is more suitable for a larger dataset, therefore we went with the model types with second highest F1: XGBoost, and second highest in AUC: GradientBoostingClassifier, and tuned hyperparameters for both. 
- After tuning hyperparameters, the performance improved for both models, however remained poor, the best was XGBoost with an F1 score of 0.26, precision of 0.15 and recall of 0.84 on the holdout set. Thus, we had a large amount of false positives and a relatively low amount of false negatives. 

## Feature Importance 

Using .feature_importance, permutation importance and SHAP, we consistently find age to be the most important feature for predicting our target, with most other features having very low or no importance at all, differing slightly between methods. We created and tuned a model with only three features: age, bmi and glucose levels, which reaches an F1 score of 0.25, with a precision of 0.15 and recall of 0.80 for predicting class 1 cases. We deployed this one. 

## Limitations

The target feature classes are imbalanced, thus this makes it hard to get the model to be very accurate in its predictions. The main limitation of our model is that it misclassifies the vast majority of cases. Thus, this makes it not good enough for use in real life. 

A lot of features we initially used in the models, turn out to not have any predicitive value for estimating the risk of getting a stroke. 
The only feature that seemed to really be of influence in predicting stroke risk with this model is age. However, patients cannot change their age, thus this doesn't have much practical value in preventing strokes beyond the intuitive.  
The features that had at least some impact were coincidentally the continuous features. Perhaps this was not a coincidence, further investigation is needed. 
 
 

