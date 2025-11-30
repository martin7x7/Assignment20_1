# Preparing predictions for package gross weight

**Author** Martins Berzins

#### Executive summary
The document contains an informative description of the work task, within which machine learning models must be used to prepare predictions of package weight based on package characteristics and other available information.

#### Rationale
A company that manufactures plywood requested the development of a model that calculates the weight of a single package based on the package’s characteristics, information about the recipient, as well as information about the time when the package was prepared. The problem is that the current accounting system calculates the package weight, but it may not be accurate. At times, the actual weight of the packages differs from what the system has calculated. When these packages are loaded onto a truck, the total weight can exceed the permitted limit, and it is no longer possible to reorganize the cargo. In such cases, there is a risk of receiving a fine for an overloaded shipment. The goal of the task is to find a way to identify the potential risk during cargo assembly that the truckload may end up heavier than allowed.

#### Research Question
The goal is to prepare a prediction of the package weight in kilograms, based on 39 package characteristics used for the calculation.

#### Data Sources
Two data files were provided for model development — dataset-train.csv and dataset-test.csv. The larger file, dataset-train.csv, contains 217,720 rows and 40 columns. Each row represents information about one package. The 40 columns include one column, ‘GrossActualKg’, which is the target variable for the calculation, as well as 39 feature columns, most of which are categorical, with only a few containing numeric values. The second file, dataset-test.csv, contains a subset of rows from the larger file — a total of 45,721 rows.

#### Methodology
The exploration consisted of two parts — automated model preparation using the PyCaret library, and feature exploration, analysis, and the preparation of one provisionally strong model.

The data source exploration included the following tasks:
- The data was loaded for processing, and basic information about the data was printed.
- The data was split into training and testing sets. A process was created to ensure that all values present in the test data also exist in the training data.
- A process was prepared to scale numerical columns using StandardScaler() and to encode categorical features using OneHotEncoder.
- The value distribution was printed for each categorical feature.
- A correlation matrix and histograms were prepared for the numerical features and the target variable.
- The best features were obtained using SelectFromModel() with a LinearRegression algorithm.
- The best features were obtained using SelectFromModel() with a DecisionTreeRegressor algorithm.
- A DecisionTreeRegressor() model was prepared with default parameters. Performance on the test data: R2 score: 0.9843, MSE score: 4538.806.
- The best features were obtained using permutation_importance() with a DecisionTreeRegressor algorithm.

Automated data processing with PyCaret included the following activities:
- Running a RegressionExperiment() process on the smaller dataset with the categorical encoding limitation removed (all features were used).
- Running a RegressionExperiment() process on the larger dataset with all default parameters — the dataset contains more rows, but effectively uses fewer features.

#### Results
The exploration revealed the following:
- The dataset contains many categorical features with a very large number of distinct values. As a result, encoding all features in the larger dataset produces 3,047 features.
- The distribution of feature values is uneven — for many features, one value appears in a large number of records, while other values appear only once.
- Searching for the best features using the SelectFromModel() function produced different results depending on whether LinearRegression or DecisionTreeRegressor models were used.
- Searching for the best features with permutation_importance() and DecisionTreeRegressor provided a clear understanding of which features are important before feature value encoding.
- The DecisionTreeRegressor model with default parameters produced reasonably good results and low error metrics. Overall, a Decision Tree–type model may be the most suitable option for this task.
- In parallel with the manual exploration, two PyCaret processes were executed, automatically performing feature processing and training different types of models. In both cases, the best-performing models were Extra Trees Regressor, Random Forest Regressor, and Decision Tree Regressor. The performance of these three models indicates that Decision Tree–type models may be the best choice for solving the task. The process also provided insights into which features are the most important for prediction.

#### Next steps
- The training and test datasets will need to be improved so that the training process is as fast as possible, contains all feature values, but does not include records that are not relevant for training.
- It is necessary to understand which categorical feature values should be discarded after encoding and which should be kept as important for model development, while maintaining model quality.
- It must be determined whether some features can be removed entirely, or only certain feature values.
- Various Decision Tree–type models will need to be tested in order to find the best one.

#### Outline of project
- Link to the notebook where the manual analysis was performed: https://github.com/martin7x7/Assignment20_1/blob/main/package_weight_eda.ipynb
- Link to the notebook where the automated analysis with PyCaret was performed: https://github.com/martin7x7/Assignment20_1/blob/main/package_weight_eda_PyCaret.ipynb

##### Contact and Further Information
Discord: martin7x7





