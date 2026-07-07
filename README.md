# _Hackathon Session: Business Analytics - Data Preparation, Training, and Deployment Guide_
Welcome to the Business Analytics Hackathon! This session guides you through a comprehensive workflow starting with data preparation, followed by model training and deployment, using R-based scripts. We will apply the following steps to prepare, train, and deploy models on the TRAIN and TEST datasets (dt and dts).


#_Data Preparation Steps_
### Data preparation is a critical first step to ensure consistency, quality, and reliability of the datasets, enabling robust model training and accurate predictions. By addressing inconsistencies, missing values, and outliers, we create a solid foundation for advanced analytics and machine learning tasks.

    1. Install Required Packages
    Install necessary R libraries (e.g., caret, ROSE, e1071, nnet) to support data handling, modeling, and evaluation.

    2. Load the Datasets
    Load the TRAIN (dt) and TEST (dts) datasets into your R environment for processing.

    3. Explore the Data (TRAIN & TEST)

    Analyze both datasets to understand their structure:
        a. Check for Missing Values (NAs): Identify columns with missing data.
        b. Search for Empty Strings (""): Address empty strings in the data.
        c. Identify Data Types: Determine if columns are factor (categorical) or numeric, noting any new features or noise.

    4. Address Inconsistencies (TRAIN & TEST)
    Resolve data inconsistencies:
        a. Handle Empty Strings (""): Replace or remove empty strings.
        b. Standardize Column Order: Ensure consistent column order across datasets.
        c. Process Character Data: Treat character-type data appropriately.
        d. Split Columns if Necessary: Break down columns into meaningful features.
        e. Remove Duplicates: Eliminate duplicate rows to reduce bias.

    5. Convert Variable Types (TRAIN & TEST)
    Convert columns to the correct data types (e.g., character to factor, numeric to numeric or integer).
    Relevel the target variable (positive class should be the second)

    6. Handle Outliers Using Box Plots (TRAIN)
    Identify and treat outliers:
        a. No Action: Retain outliers if meaningful.
        b. Replace with NA, Then Fill All NAs (Option 1): Replace outliers with NA and fill all missing values.
        c. Remove Problematic Rows (Option 2): Remove rows with outliers that we find problematic.
        d. Remove all outliers.

    7. Treat Missing Values Using KNN (TRAIN)
        a.No Action: Retain missing values.Good to be tested on Decision Trees.
      Impute missing values with K-Nearest Neighbors (KNN):
        b. Test Different K Values: Experiment with k=5, k=7, and k=9.

    8. Check for Correlations (TRAIN)
    Analyze correlations:
        a. Do Not Remove Correlated Columns: Keep all columns initially. Good to be tested on Decision Trees (not affected by NAs and correlated columns)
        b. Remove Highly Correlated Columns (Set a Criterion): Remove correlated columns (e.g., correlation > 0.8) in both TRAIN and TEST.

    9. Check if the Dataset´is Unbalanced (TRAIN)
        a. This will give us UNBALANCED if at least one class represents less than 30% of the data. If it is unbalanced, we have to take this into account when doing the models (undersampling the majority class).

    10. Treat Missing Values in TEST Using KNN (TEST)
    Impute missing values:
        a. Use TRAIN Neighbors: Use TRAIN dataset neighbors; Use the Same K as in TRAIN: Apply the selected k value.

    11. Send the script to the Models' PC

---

_Model Training and Deployment_
Model training and deployment are crucial to build predictive models and apply them to new data. This step leverages a modular R-based framework to train diverse models, evaluate their performance, and generate predictions for deployment, ensuring robustness and reproducibility.

1. Framework Overview
The modeling pipeline uses two scripts:
    model_functions.R: Contains core functions for training, testing, and predicting.
    run_model.R: Orchestrates the workflow, from training to prediction.

2. Theoretical Foundations
    #### System Architecture and Logic
        The system follows a modular design, separating core algorithmic logic (in model_functions.R) from execution and orchestration (in run_model.R). The pipeline assumes preprocessing is done externally, with resulting datasets stored as dt1 (training) and dts1 (test).

        model_functions.R: Core Methodology ModuleProvides reusable functions:
            - Training multiple classification models via cross-validation (train_models)
            - Testing model robustness (robustness_test)
            - Predicting on new data (predict_new_data)
            - Analyzing feature importance via model-specific strategies


        run_model.R: Workflow ControllerOrchestrates the pipeline by sourcing core methods, assigning data and parameters, training models, performing robustness checks, and generating predictions. It supports repeatable experimentation.


    #### Methodological Foundations
    Model Diversity and Bias-Variance Trade-offThe train_models function implements a suite of classification algorithms:
        - Linear: Logistic Regression, Naive Bayes
        - Tree-based: Decision Trees, Bagged Trees
        - Instance-based: k-Nearest Neighbors
        - Kernel-based: Support Vector Machines
        - Neural Networks, OneR
        - Ensemble: Majority votingThis diversity addresses the bias-variance trade-off by incorporating models of different complexity and assumptions.


    #### Cross-ValidationUses 
    (k)-fold cross-validation (default (k=10)) to estimate generalization error. Different (k) values (e.g., 5, 10, 15) will be tested to assess stability and performance.

    Hyperparameter Tuning
    Grid search is performed for key hyperparameters:
        - Decision Trees: cp, minbucket
        - SVMs: cost, gamma
        - Neural Networks: size, decay
        - KNN: number of neighbors (k)Grid search is robust, interpretable, and easy to control, though not exhaustive like Bayesian optimization.


    #### Class Imbalance 
    Treatment If the minority class is less than 30% of the training data, undersampling is applied using the ROSE package. This approximates equal class priors, improving F1 score and recall, and reduces bias when paired with ensemble models.

    #### Performance Metrics
    Computed metrics include:
        - Accuracy: General success rate (misleading in imbalanced data)
        - F1 Score: Harmonic mean of precision and recall (primary metric)
        - AUC: Ranking ability across thresholds
        - Brier Score: Probability calibration for probabilistic decisions


    ####Feature Importance
    A dedicated function computes and visualizes feature importance:
        - Coefficients (linear models)
        - Variable importance (decision trees)
        - Permutation scores (KNN, SVM)
        - OneR rule strengthPlots guide feature engineering.


    #### Ensemble Learning
    Majority vote ensembling improves robustness by combining diverse base models. Per Breiman (1996), ensembles reduce variance without increasing bias if models are uncorrelated. Probabilistic metrics (AUC, Brier) use average predicted probabilities.

    ####Robustness Testing 
    The robustness_test() function retrains the best model under varying conditions (e.g., random seeds, samples) to ensure performance consistency.

    Experimental Runs and Iterative DesignMultiple runs will vary:
        - (k) in cross-validation
        - Feature subsets (guided by importance)
        - Preprocessing strategies (scaling, encoding, transformations)Results are logged and compared for optimization.


    #### Preliminary Runs
    Conducted with minimally processed data to establish baselines and diagnose issues.


3. Workflow Overview

Preprocessing: Done externally; outputs dt1 and dts1
Execution: Begins in run_model.R
Training: Calls train_models(), evaluates via CV
Robustness: Applies robustness_test()
Prediction: Uses predict_new_data() on test set
Feature Importance: Computes and plots relevant variables
Multiple Runs: Iterates across (k), features, and preprocessing
Output: Saves results to `sample


---

# _RoadMap during Hackathon (testimony)_

Concepts:
- Step: The purpose
- HandsOn: the approach choosen to achieve the purpose
- Discussion/Conclusions: what was verify, that will conduct to new steps and HandsOn choices



During Hackathon our team followed the `CRISP-DM` workflow.

## `Data Understanding`

Step: It was analysed the data deport provided by Kaggle.
Discussion/Conclusion: 
The problem is a binary classification task predicting whether a candidate is actively looking for another job (target = 1) or not (target = 0)
Positive class: target=1 (more critical)
Class imbalance is confirmed 




## `Data Preparation` - Preprocessing 1
// Install Required Packages //
// Load the Datasets //

Step (3. from Data Pre-processing steps): Explore the Data (TRAIN & TEST)
HandsOn:
Used histograms and bar charts
Detected missing values and inconsistent formats
Inspect variable types
Discussion/Conclusions: Discovered missing values ("") and incorrect format in employer_size. Confirmed high cardinality and imbalance in the target.


Step (4. from Data Pre-processing steps): Address Inconsistencies (TRAIN & TEST)
HandsOn:
Replaced "" with NA
Fixed "Oct-49" → "10-49"
Dropped location_city due to mismatch (different levels in train vs test) and redundancy (means the same as city_dev_score)
Discussion/Conclusions: Improved consistency. Removed variables with alignment or redundancy issues

Step (5. from Data Pre-processing steps): Convert Variable Types (TRAIN & TEST)
HandsOn:
Converted character to factor
Converted numeric columns (only 2 numeric attributes)
Releveled target to make "1" the positive class
Discussion/Conclusions: Ensured correct data types for modeling

This preprocessing is useful for tree models (it still has NAs)

NOTE: Removed the same column in the test set



## `Data Preparation` - Preprocessing 2
// Install Required Packages //
// Load the Datasets //

#same as preprocessing 1 except:

Step (6. from Data Pre-processing steps): Handle Outliers Using Box Plots (TRAIN)
HandsOn:
Used boxplots to inspect hours_of_training (many outliers from > hour of training) and city_dev_score (1 city with low value)
Decided to keep all outliers
Discussion/Conclusions: Outliers were plausible and potentially informative

Step (7. from Data Pre-processing steps): Treat Missing Values Using KNN (TRAIN)
HandsOn:
k = 5
Dropped employer_type and employer_size (many NAs, much time running the KNN)
Used VIM::kNN to impute other columns
Discussion/Conclusions: Reduced missing data while making the running more quick

Step (8. from Data Pre-processing steps): Check for Correlations (TRAIN)
HandsOn: There were not correlated numeric columns
Discussion/Conclusions: Kept features


Step (10. from Data Pre-processing steps): Treat Missing Values in TEST Using KNN (TEST)
HandsOn:
Applied same KNN (k = 5) using TRAIN data (not TEST)
Discussion/Conclusions: Avoided data leakage


NOTE: Removed the same columns in the test set

## `Data Preparation` - Preprocessing 3
// Install Required Packages //
// Load the Datasets //

#same as preprocessing 2 except:

Step (7. from Data Pre-processing steps): Treat Missing Values Using KNN (TRAIN)
HandsOn:
k = 5
Dropped sex, employer_type, employer_size (the sex was the new attribute that was dropped)
Used VIM::kNN to impute other columns
Discussion/Conclusions: Reduced missing data while making the running more quick

NOTE: Removed the same columns in the test set

## `Data Preparation` - Preprocessing 4
// Install Required Packages //
// Load the Datasets //

#Same as preprocessing 1, so: columns dropped: location_city 

Extra:
Removed rows with more than 4 missing values (resulting in 15126 rows out of 15327)
Columns dropped: employer_size, employer_type, sex (as preprocessing 3)
Binned work_experience_years into categories:
  <1, 1-2, 3-5, 6-10, 11-15, >20, and Missing.
  Why: simplify a variable with high cardinality and sparsity, making it more usable in the models

Left all other NAs intact, making this version tree-ready (as Decision Trees in R can handle missing values)

NOTE: Applied same transformations to test set (except the removal of rows) 





## `Modeling Trial 1: Decision Tree (With Missing Values)`
Preprocessing used: Preprocessing_1.R Script

HandsOn:
  Used Separated_Models.R script and ran the decision tree model
  Performed hyperparameter tuning 
  Used 10-fold cross-validation
  
Results of Hyperparameter Tuning:
  Splitting Criterion: information gain
  Complexity Parameter (cp): 0.01
  Minimum number of objects per leaf (min_objects): 2

Evaluation:
  Mean Accuracy: 0.7103
  Mean F1 Score: 0.7954  
  
Discussion/Conclusions:
  This first trial served as a baseline for future model comparisons. The performance is reasonable, especially the F1 score, indicating a good balance between precision and recall. Future iterations could explore:
  Imputation strategies for missing values,
  Feature engineering,
  Alternative models and ensembles.

## `Modeling Trial 2: Automatic Code`
We encountered unexpected errors while executing the run_models function, which is connected to model_functions. 
  First Error: Imbalance Detection and Undersampling
  Second Error: F1 Score Calculation
We spent over an hour attempting to debug this issue. 
Pressed for time, we pivoted to our backup plan—running the models independently rather than through the automated pipeline. This allowed us to complete our submission.

## `Modeling Trial 3: Separated Models`
HandsOn:
  Used Separated_Models.R script and ran all models
  Performed hyperparameter tuning for Decision Tree and KNN
  Used 10-fold cross-validation for each model

Evaluation:
        Model         Accuracy      F1        Brier       AUC
4        OneR         0.7478323  0.8314045  0.1885838  0.6659499
1         KNN         0.7012465  0.7888759  0.2135512  0.7061108
3 Naive Bayes         0.7037902  0.7846115  0.2148834  0.7247684
2    Logistic         0.3421393  0.2412370  0.2036402  0.7362105
5 Decision Tree       0.703893   0.7849023

Given the problem, we had to code the deployment for OneR and KNN during Hackathon since we did not have it.

## `Final Submission Decision`
Due to time constraints, we were unable to complete and validate additional modeling runs that clearly surpassed this performance.
Submission 1 ( Decision Tree + Preprocessing 1)
Submission 2 ( KNN + Preprocessing 2)
Submission 3 ( OneR + Preprocessing 2)
Submission 4 ( Decision Tree + Preprocessing 2)
