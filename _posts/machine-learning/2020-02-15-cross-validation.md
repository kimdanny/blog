---
title: "Cross Validation Techniques"
categories:
  - machine-learning
tags:
  - machine-learning
  - cross validation
use_math: true
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

In this post, we will use scikit-learn's iris data set to understand various types of cross validation techniques thoroughly.  
But First of all, 

## Why do we use sklearn??

1. **Example Datasets**  
    sklearn.datasets : Provides example datasets  

2. **Feature Engineering**  
    sklearn.preprocessing : Variable functions as to data preprocessing  
    sklearn.feature_selection : Help selecting primary components in datasets  
    sklearn.feature_extraction : Vectorised feature extraction  
    sklearn.decomposition : Algorithms regarding Dimensionality Reduction  

3. **Data split and Parameter Tuning**  
    sklearn.model_selection : 'Train Test Split' for cross validation, Parameter tuning with GridSearch  

4. **Evaluation**  
    sklearn.metrics : accuracy score, ROC curve, F1 score, etc.  

5. **ML Algorithms**  
    sklearn.ensemble : Ensemble, etc.  
    sklearn.linear_model : Linear Regression, Logistic Regression, etc.  
    sklearn.naive_bayes : Gaussian Naive Bayes classification, etc.  
    sklearn.neighbors : Nearest Centroid classification, etc.  
    sklearn.svm : Support Vector Machine  
    sklearn.tree : DecisionTreeClassifier, etc.  
    sklearn.cluster : Clustering (Unsupervised Learning)  
    
6. **Utilities**  
    sklearn.pipeline: pipeline of (feature engineering -> ML Algorithms -> Prediction)  
    
7. **Train and Predict**  
    fit()  
    predict()  






## 1. Train Test Split

Let's import what we need.
```python
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split
import pandas as pd
import numpy as np
```

```python
# load iris data (famous classification dataset), get data and label(target)
iris = load_iris()

print("iris data: \n", iris.data[:5])
print("iris target: \n", iris.target)

print("data length: ", len(iris.data))
print("target length: ", len(iris.target))

print("feature names: \n", iris.feature_names)
print("target names: \n", iris.target_names)
```

    iris data: 
     [[5.1 3.5 1.4 0.2]
     [4.9 3.  1.4 0.2]
     [4.7 3.2 1.3 0.2]
     [4.6 3.1 1.5 0.2]
     [5.  3.6 1.4 0.2]]
    iris target: 
     [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2
     2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
     2 2]
    data length:  150
    target length:  150
    feature names: 
     ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']
    target names: 
     ['setosa' 'versicolor' 'virginica']



```python
data = pd.DataFrame(iris.data, columns=iris.feature_names)
print(data.head(10), '\n')
print(data.describe(), '\n')
print(data.info())
```

       sepal length (cm)  sepal width (cm)  petal length (cm)  petal width (cm)
    0                5.1               3.5                1.4               0.2
    1                4.9               3.0                1.4               0.2
    2                4.7               3.2                1.3               0.2
    3                4.6               3.1                1.5               0.2
    4                5.0               3.6                1.4               0.2
    5                5.4               3.9                1.7               0.4
    6                4.6               3.4                1.4               0.3
    7                5.0               3.4                1.5               0.2
    8                4.4               2.9                1.4               0.2
    9                4.9               3.1                1.5               0.1 
    
           sepal length (cm)  sepal width (cm)  petal length (cm)  \
    count         150.000000        150.000000         150.000000   
    mean            5.843333          3.057333           3.758000   
    std             0.828066          0.435866           1.765298   
    min             4.300000          2.000000           1.000000   
    25%             5.100000          2.800000           1.600000   
    50%             5.800000          3.000000           4.350000   
    75%             6.400000          3.300000           5.100000   
    max             7.900000          4.400000           6.900000   
    
           petal width (cm)  
    count        150.000000  
    mean           1.199333  
    std            0.762238  
    min            0.100000  
    25%            0.300000  
    50%            1.300000  
    75%            1.800000  
    max            2.500000   
    
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 150 entries, 0 to 149
    Data columns (total 4 columns):
    sepal length (cm)    150 non-null float64
    sepal width (cm)     150 non-null float64
    petal length (cm)    150 non-null float64
    petal width (cm)     150 non-null float64
    dtypes: float64(4)
    memory usage: 4.8 KB
    None



```python
# Load classifier model
model = DecisionTreeClassifier()
```


```python
# Train without splitting data
model.fit(iris.data, iris.target)

# Predict targets based on your x datasets
pred = model.predict(iris.data)

# Evaluate your prediction by comparing it with label you used for training
# You must get 100% Accuracy!
print("Accuracy : {}".format(accuracy_score(iris.target, pred)))
```

    Accuracy : 1.0


**Too Good to be true...**  
You are actually testing your model's prediction ability based on the data that you have already used for training. 
Unless you have an unused dataset explicitly for testing, you need to split the data into training purpose data and testing purpose data.  
This is where [**sklearn.model_selection.train_test_split**](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html) comes into play.  

train_test_split(arrays, test_size, train_size, random_state, shuffle, stratify)

- \*arrays : x and y data  
- test_size : Ratio of Test data (default = 0.25)  
- train_size : Ratio of Train data (default = 1 - 0.25)  
- random_state : seed value for shuffle. It is used to seed a new RandomState object. This is to check and validate the data when running the code multiple times  
- shuffle : shuffle or not? (default = True)  
- stratify : will discuss later on (default = None)  


```python
# Split your data into X_train, X_test, Y_train and Y_test
# What is optimal rate for the test_size?
X_train, X_test, Y_train, Y_test = train_test_split(iris.data, iris.target, test_size=0.25, random_state=np.random, shuffle=True)

print("Length of X_train: {}".format(len(X_train)))
print("Length of X_test: {}".format(len(X_test)))
print("Length of Y_train: {}".format(len(Y_train)))
print("Length of Y_test: {}".format(len(Y_test)))
print("train_test ratio: {0:.2f}%".format(
    len(X_test) / (len(X_train)+len(X_test))
))
```

    Length of X_train: 112
    Length of X_test: 38
    Length of Y_train: 112
    Length of Y_test: 38
    train_test ratio: 0.25%



```python
model = DecisionTreeClassifier()
# Train your model with your 'train data' not the whole data
model.fit(X_train, Y_train)

# Predict targets with your 'test data' not the whole target data
pred = model.predict(X_test)

# Evaluate your prediction. Which data should you compare your predicted data with?
print("Accuracy : {}".format(accuracy_score(Y_test, pred)))
```

    Accuracy : 0.918463


## 2. K-Fold Cross Validation

Visit [KFold official documentation](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.KFold.html).  

- Cross Validation is encouraged to use when dataset is not big enough.  
- Cross Validation is used to avoid overfitting.  
- Overfitting literally means 'data is overly fitted with the data' so that it does not perform well when new data are given.  
- K-fold Cross Validation makes K number of train and test data set.  

**KFold(n_splits)**  
```
n_splits : the number of folds (splits)  
KFold(n_splits=N).split(X)  
X : Data  
```


```python
from sklearn.model_selection import KFold

model = DecisionTreeClassifier()

n_iter = 1
kfold = KFold(n_splits = 5)
cv_accuracy = []

# type of idx => numpy ndarray
for train_idx, test_idx in kfold.split(iris.data):
    print("train index: \n", train_idx)
    print("train index shape: ", train_idx.shape)
    print("test index: \n", test_idx)
    print("test index shape: ", test_idx.shape)
    X_train, X_test = iris.data[train_idx], iris.data[test_idx]
    y_train, y_test = iris.target[train_idx], iris.target[test_idx]

    model.fit(X_train, y_train)
    pred = model.predict(X_test)

    accuracy = np.round(accuracy_score(y_test, pred), 3)
    train_size = X_train.shape[0]
    test_size = X_test.shape[0]

    print("Iteration : {}, Cross-Validation Accuracy : {}".format(n_iter, accuracy))

    n_iter += 1

    cv_accuracy.append(accuracy)
    print("\n")

print("Average accuracy : ", np.mean(cv_accuracy))
```

Take a close look at the output!!

    train index: 
     [ 30  31  32  33  34  35  36  37  38  39  40  41  42  43  44  45  46  47
      48  49  50  51  52  53  54  55  56  57  58  59  60  61  62  63  64  65
      66  67  68  69  70  71  72  73  74  75  76  77  78  79  80  81  82  83
      84  85  86  87  88  89  90  91  92  93  94  95  96  97  98  99 100 101
     102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119
     120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
     138 139 140 141 142 143 144 145 146 147 148 149]
    train index shape:  (120,)
    test index: 
     [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
     24 25 26 27 28 29]
    test index shape:  (30,)
    Iteration : 1, Cross-Validation Accuracy : 1.0
    
    
    train index: 
     [  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
      18  19  20  21  22  23  24  25  26  27  28  29  60  61  62  63  64  65
      66  67  68  69  70  71  72  73  74  75  76  77  78  79  80  81  82  83
      84  85  86  87  88  89  90  91  92  93  94  95  96  97  98  99 100 101
     102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119
     120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
     138 139 140 141 142 143 144 145 146 147 148 149]
    train index shape:  (120,)
    test index: 
     [30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53
     54 55 56 57 58 59]
    test index shape:  (30,)
    Iteration : 2, Cross-Validation Accuracy : 0.967
    
    
    train index: 
     [  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
      18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35
      36  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53
      54  55  56  57  58  59  90  91  92  93  94  95  96  97  98  99 100 101
     102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119
     120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
     138 139 140 141 142 143 144 145 146 147 148 149]
    train index shape:  (120,)
    test index: 
     [60 61 62 63 64 65 66 67 68 69 70 71 72 73 74 75 76 77 78 79 80 81 82 83
     84 85 86 87 88 89]
    test index shape:  (30,)
    Iteration : 3, Cross-Validation Accuracy : 0.9
    
    
    train index: 
     [  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
      18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35
      36  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53
      54  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71
      72  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89
     120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
     138 139 140 141 142 143 144 145 146 147 148 149]
    train index shape:  (120,)
    test index: 
     [ 90  91  92  93  94  95  96  97  98  99 100 101 102 103 104 105 106 107
     108 109 110 111 112 113 114 115 116 117 118 119]
    test index shape:  (30,)
    Iteration : 4, Cross-Validation Accuracy : 0.933
    
    
    train index: 
     [  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17
      18  19  20  21  22  23  24  25  26  27  28  29  30  31  32  33  34  35
      36  37  38  39  40  41  42  43  44  45  46  47  48  49  50  51  52  53
      54  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71
      72  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89
      90  91  92  93  94  95  96  97  98  99 100 101 102 103 104 105 106 107
     108 109 110 111 112 113 114 115 116 117 118 119]
    train index shape:  (120,)
    test index: 
     [120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137
     138 139 140 141 142 143 144 145 146 147 148 149]
    test index shape:  (30,)
    Iteration : 5, Cross-Validation Accuracy : 0.733
    
    
    Average accuracy :  0.9065999999999999


## 3. Stratified K-Fold Cross Validation
Have you tried KFold(n_splits=3) from the above K-Fold example? What happend? Why does that happen?  
Plus, when you do KFold(n_splits=5), how can the accuracy reach 1.0 at the first iteration?
**The reason why is because the distribution of the data is UNBALANCED**  
**This is where Stratified K-Fold CV comes into play**  

Helpful URL to understand this concept:
1. [sklearn document](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html)  

**code from sciket-learn doc**
```
import numpy as np
from sklearn.model_selection import StratifiedKFold
X = np.array([[1, 2], [3, 4], [1, 2], [3, 4]])  -->
y = np.array([0, 0, 1, 1])                      --> Here you can see that the distribution of the data is unbalanced, where normal KFold is not desirable
skf = StratifiedKFold(n_splits=2)
skf.get_n_splits(X, y)

for train_index, test_index in skf.split(X, y):
   print("TRAIN:", train_index, "TEST:", test_index)
   X_train, X_test = X[train_index], X[test_index]
   y_train, y_test = y[train_index], y[test_index]
```

2. [Quora](https://www.quora.com/What-is-difference-between-k-fold-and-stratified-k-fold-cross-validations)  
3. [StackExchange](https://stats.stackexchange.com/questions/49540/understanding-stratified-cross-validation)


Stratified KFold is encouraged to use when the distribution of the given data is unbalanced, where normal KFold creates high bias to your model.


StratifiedKFold(n_splits)  
n_splits : the number of splits i.e. folds

StratifiedKFold(n_splits=5).split(X, Y)  
X : Data  
Y : label  


```python
from sklearn.model_selection import StratifiedKFold

iris_df = pd.DataFrame(data=iris.data, columns=iris.feature_names)
iris_df['label'] = iris.target

# Classification model
model = DecisionTreeClassifier()

n_iter = 0      # this var is for tracking the iteration
SKF = StratifiedKFold(n_splits=3)
avg_acc = []

for train_idx, test_idx in SKF.split(iris.data, iris.target):

    n_iter += 1

    # split it and assign data to variables
    X_train, X_test = iris.data[train_idx], iris.data[test_idx]
    y_train, y_test = iris.target[train_idx], iris.target[test_idx]

    print("Iteration :", n_iter)
    print("--------------------")

    print("Check distribution of train data : \n",
          iris_df['label'].iloc[train_idx].value_counts())
    print("--------------------")
    
    print("Check distribution of test data : \n",
          iris_df['label'].iloc[test_idx].value_counts())
    print("--------------------")

    # train your model with train data
    model.fit(X_train, y_train)

    # make your model predict data with test data
    pred = model.predict(X_test)

    # Evaluate your model
    accuracy = np.round(accuracy_score(y_test, pred), 4)
    train_size = X_train.shape[0]
    test_size = X_test.shape[0]

    print("Iteration : {}, Accuracy : {}%, Size of Train data : {}, Size of Test data : {}\n"
          .format(n_iter, accuracy * 100, train_size, test_size))

    avg_acc.append(accuracy)

print("Average accuracy : {}".format(np.mean(avg_acc)))
```

    Iteration : 1
    --------------------
    Check distribution of train data : 
     2    33
    1    33
    0    33
    Name: label, dtype: int64
    --------------------
    Check distribution of test data : 
     2    17
    1    17
    0    17
    Name: label, dtype: int64
    --------------------
    Iteration : 1, Accuracy : 98.04%, Size of Train data : 99, Size of Test data : 51
    
    Iteration : 2
    --------------------
    Check distribution of train data : 
     2    33
    1    33
    0    33
    Name: label, dtype: int64
    --------------------
    Check distribution of test data : 
     2    17
    1    17
    0    17
    Name: label, dtype: int64
    --------------------
    Iteration : 2, Accuracy : 92.16%, Size of Train data : 99, Size of Test data : 51
    
    Iteration : 3
    --------------------
    Check distribution of train data : 
     2    34
    1    34
    0    34
    Name: label, dtype: int64
    --------------------
    Check distribution of test data : 
     2    16
    1    16
    0    16
    Name: label, dtype: int64
    --------------------
    Iteration : 3, Accuracy : 97.92%, Size of Train data : 102, Size of Test data : 48
    
    Average accuracy : 0.9604


## 4. Train, Validation and Test data set

[medium blog](https://towardsdatascience.com/train-validation-and-test-sets-72cb40cba9e7)

**Validation Set**  
The validation set is used to evaluate a given model, 
but this is for frequent evaluation. 
We as machine learning engineers use this data to fine-tune the model hyperparameters. 
Hence the model occasionally sees this data, but never does it “Learn” from this. 
We(mostly humans, at-least as of 2017 😛 ) use the validation set results and update higher level hyperparameters. 
So the validation set in a way affects a model, but indirectly. - from the blog-  

You split training set into train and validation sets (for tuning), and check its performance with test datasets.  


```python
X_train, X_test, y_train, y_test = train_test_split(iris.data, iris.target, test_size=0.3)

'''Option 1: Just split again'''
X_train, X_vali, y_train, y_vali = train_test_split(X_train, y_train, test_size=0.2)

'''Option 2-1: Split and CV(SKF)'''
# TODO: declare kfold or SKfold
STK = StratifiedKFold(n_splits=3)

# TODO: Split X_train into train and validation set
for train_idx, vali_idx in STK.split(X_train, y_train):
    X_train, X_vali = iris.data[train_idx], iris.data[vali_idx]
    y_train, y_vali = iris.target[train_idx], iris.target[vali_idx]
    
    
'''Option 2-2: Split and CV(KF)'''
# TODO: declare KFold
kfold = KFold(n_splits=3)

# TODO: Split X_train into train and validation set
for train_idx, vali_idx in kfold.split(X_train):
    X_train, X_vali = iris.data[train_idx], iris.data[vali_idx]
    y_train, y_vali = iris.target[train_idx], iris.target[vali_idx]
```
