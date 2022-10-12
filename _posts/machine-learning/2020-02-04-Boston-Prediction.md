---
title: "Boston House Price Prediction"
categories:
  - machine-learning
tags:
  - machine-learning
use_math: true
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

Regression is basically a process which predicts the relationship between x and y based on features.
This time we are going to practice Linear Regression with Boston House Price Data that are already embedded in scikit-learn datasets.

**Useful functions**
- sklearn.metrics.mean_squared_error: famous evaluation method (MSE)

- np.sqrt(x): square root of tensor x

- linear_model.coef_ : get `Regression coefficient` of the fitted linear model


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import sklearn.datasets as datasets
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
```



```python
BOSTON_DATA = datasets.load_boston()
```

## Simple EDA


```python
# Load both boston data and target, and convert it as dataframe.
def add_target_to_data(dataset):
    # make the raw dataset cleaner and easier to process -> use dataframe
    df = pd.DataFrame(dataset.data, columns=dataset.feature_names)
    # put the target data (price) to the dataframe we just made.
    print("Before adding target: ", df.shape)
    df['PRICE'] = dataset.target
    print("After adding target: {} \n {}\n".format(df.shape, df.head(2)))
    return df
```


```python
"""
10 features as default.
Why didn't I put all the 13 features? Because n_row=2 and n_col=5 as default.
It will create 10 graphs for each features.
"""
def plotting_graph(df, features, n_row=2, n_col=5):
    fig, axes = plt.subplots(n_row, n_col, figsize=(16, 8))

    assert len(features) == n_row * n_col

    # Draw a regression graph using seaborn's regplot
    for i, feature in enumerate(features):
        row = int(i / n_col)
        col = i % n_col
        sns.regplot(x=feature, y='PRICE', data=df, ax=axes[row][col])

    plt.show()
```


```python
def split_dataframe(df):
    label_data = df['PRICE']
    # others without PRICE
    # axis!! --> Whether to drop labels from the index (0 or ‘index’) or columns (1 or ‘columns’).
    input_data = df.drop(['PRICE'], axis=1)

    # split! Set random_state if you want consistently same result
    input_train, input_eval, label_train, label_eval = train_test_split(input_data, label_data, test_size=0.3,
                                                                        random_state=42)

    return input_train, input_eval, label_train, label_eval
```


```python
boston_df = add_target_to_data(BOSTON_DATA)
features = ['RM', 'ZN', 'INDUS', 'NOX', 'AGE', 'PTRATIO', 'LSTAT', 'RAD', 'CRIM', 'B']
plotting_graph(boston_df, features, n_row=2, n_col=5)
```

    Before adding target:  (506, 13)
    After adding target: (506, 14) 
           CRIM    ZN  INDUS  CHAS    NOX     RM   AGE     DIS  RAD    TAX  \
    0  0.00632  18.0   2.31   0.0  0.538  6.575  65.2  4.0900  1.0  296.0   
    1  0.02731   0.0   7.07   0.0  0.469  6.421  78.9  4.9671  2.0  242.0   
    
       PTRATIO      B  LSTAT  PRICE  
    0     15.3  396.9   4.98   24.0  
    1     17.8  396.9   9.14   21.6  
    



![png]({{ site.url }}{{ site.baseurl }}/images/boston/Session02-BostonHousePrice-Solution_7_1.png)



```python
'''
The correlation coefficient ranges from -1 to 1. 
If the value is close to 1, it means that there is a strong positive correlation between the two variables.
When it is close to -1, the variables have a strong negative correlation.
'''
correlation_matrix = boston_df.corr().round(2)
```
Output: 



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>PRICE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CRIM</th>
      <td>1.00</td>
      <td>-0.20</td>
      <td>0.41</td>
      <td>-0.06</td>
      <td>0.42</td>
      <td>-0.22</td>
      <td>0.35</td>
      <td>-0.38</td>
      <td>0.63</td>
      <td>0.58</td>
      <td>0.29</td>
      <td>-0.39</td>
      <td>0.46</td>
      <td>-0.39</td>
    </tr>
    <tr>
      <th>ZN</th>
      <td>-0.20</td>
      <td>1.00</td>
      <td>-0.53</td>
      <td>-0.04</td>
      <td>-0.52</td>
      <td>0.31</td>
      <td>-0.57</td>
      <td>0.66</td>
      <td>-0.31</td>
      <td>-0.31</td>
      <td>-0.39</td>
      <td>0.18</td>
      <td>-0.41</td>
      <td>0.36</td>
    </tr>
    <tr>
      <th>INDUS</th>
      <td>0.41</td>
      <td>-0.53</td>
      <td>1.00</td>
      <td>0.06</td>
      <td>0.76</td>
      <td>-0.39</td>
      <td>0.64</td>
      <td>-0.71</td>
      <td>0.60</td>
      <td>0.72</td>
      <td>0.38</td>
      <td>-0.36</td>
      <td>0.60</td>
      <td>-0.48</td>
    </tr>
    <tr>
      <th>CHAS</th>
      <td>-0.06</td>
      <td>-0.04</td>
      <td>0.06</td>
      <td>1.00</td>
      <td>0.09</td>
      <td>0.09</td>
      <td>0.09</td>
      <td>-0.10</td>
      <td>-0.01</td>
      <td>-0.04</td>
      <td>-0.12</td>
      <td>0.05</td>
      <td>-0.05</td>
      <td>0.18</td>
    </tr>
    <tr>
      <th>NOX</th>
      <td>0.42</td>
      <td>-0.52</td>
      <td>0.76</td>
      <td>0.09</td>
      <td>1.00</td>
      <td>-0.30</td>
      <td>0.73</td>
      <td>-0.77</td>
      <td>0.61</td>
      <td>0.67</td>
      <td>0.19</td>
      <td>-0.38</td>
      <td>0.59</td>
      <td>-0.43</td>
    </tr>
    <tr>
      <th>RM</th>
      <td>-0.22</td>
      <td>0.31</td>
      <td>-0.39</td>
      <td>0.09</td>
      <td>-0.30</td>
      <td>1.00</td>
      <td>-0.24</td>
      <td>0.21</td>
      <td>-0.21</td>
      <td>-0.29</td>
      <td>-0.36</td>
      <td>0.13</td>
      <td>-0.61</td>
      <td>0.70</td>
    </tr>
    <tr>
      <th>AGE</th>
      <td>0.35</td>
      <td>-0.57</td>
      <td>0.64</td>
      <td>0.09</td>
      <td>0.73</td>
      <td>-0.24</td>
      <td>1.00</td>
      <td>-0.75</td>
      <td>0.46</td>
      <td>0.51</td>
      <td>0.26</td>
      <td>-0.27</td>
      <td>0.60</td>
      <td>-0.38</td>
    </tr>
    <tr>
      <th>DIS</th>
      <td>-0.38</td>
      <td>0.66</td>
      <td>-0.71</td>
      <td>-0.10</td>
      <td>-0.77</td>
      <td>0.21</td>
      <td>-0.75</td>
      <td>1.00</td>
      <td>-0.49</td>
      <td>-0.53</td>
      <td>-0.23</td>
      <td>0.29</td>
      <td>-0.50</td>
      <td>0.25</td>
    </tr>
    <tr>
      <th>RAD</th>
      <td>0.63</td>
      <td>-0.31</td>
      <td>0.60</td>
      <td>-0.01</td>
      <td>0.61</td>
      <td>-0.21</td>
      <td>0.46</td>
      <td>-0.49</td>
      <td>1.00</td>
      <td>0.91</td>
      <td>0.46</td>
      <td>-0.44</td>
      <td>0.49</td>
      <td>-0.38</td>
    </tr>
    <tr>
      <th>TAX</th>
      <td>0.58</td>
      <td>-0.31</td>
      <td>0.72</td>
      <td>-0.04</td>
      <td>0.67</td>
      <td>-0.29</td>
      <td>0.51</td>
      <td>-0.53</td>
      <td>0.91</td>
      <td>1.00</td>
      <td>0.46</td>
      <td>-0.44</td>
      <td>0.54</td>
      <td>-0.47</td>
    </tr>
    <tr>
      <th>PTRATIO</th>
      <td>0.29</td>
      <td>-0.39</td>
      <td>0.38</td>
      <td>-0.12</td>
      <td>0.19</td>
      <td>-0.36</td>
      <td>0.26</td>
      <td>-0.23</td>
      <td>0.46</td>
      <td>0.46</td>
      <td>1.00</td>
      <td>-0.18</td>
      <td>0.37</td>
      <td>-0.51</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.39</td>
      <td>0.18</td>
      <td>-0.36</td>
      <td>0.05</td>
      <td>-0.38</td>
      <td>0.13</td>
      <td>-0.27</td>
      <td>0.29</td>
      <td>-0.44</td>
      <td>-0.44</td>
      <td>-0.18</td>
      <td>1.00</td>
      <td>-0.37</td>
      <td>0.33</td>
    </tr>
    <tr>
      <th>LSTAT</th>
      <td>0.46</td>
      <td>-0.41</td>
      <td>0.60</td>
      <td>-0.05</td>
      <td>0.59</td>
      <td>-0.61</td>
      <td>0.60</td>
      <td>-0.50</td>
      <td>0.49</td>
      <td>0.54</td>
      <td>0.37</td>
      <td>-0.37</td>
      <td>1.00</td>
      <td>-0.74</td>
    </tr>
    <tr>
      <th>PRICE</th>
      <td>-0.39</td>
      <td>0.36</td>
      <td>-0.48</td>
      <td>0.18</td>
      <td>-0.43</td>
      <td>0.70</td>
      <td>-0.38</td>
      <td>0.25</td>
      <td>-0.38</td>
      <td>-0.47</td>
      <td>-0.51</td>
      <td>0.33</td>
      <td>-0.74</td>
      <td>1.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
sns.heatmap(correlation_matrix, cmap="YlGnBu")
plt.show()
```


![png]({{ site.url }}{{ site.baseurl }}/images/boston/Session02-BostonHousePrice-Solution_9_0.png)


## Prediction with Linear Regression


```python
X_train, X_test, Y_train, Y_test = split_dataframe(boston_df)

# Load your machine learning model
model = LinearRegression()
# Train!
model.fit(X_train, Y_train)
# make prediction with unseen data!
pred = model.predict(X_test)
expectation = Y_test
# what is mse between the answer and your prediction?
lr_mse = mean_squared_error(expectation, pred)
lr_rmse = np.sqrt(lr_mse)

print('LR_MSE: {0:.3f}, LR_RMSE: {1:.3F}'.format(lr_mse, lr_rmse))
# Regression Coefficient
print('Regression Coefficients:', np.round(model.coef_, 1))
# sort from the biggest
coeff = pd.Series(data=model.coef_, index=X_train.columns).sort_values(ascending=False)
print(coeff)
```
Output:  

    LR_MSE: 21.517, LR_RMSE: 4.639
    Regression Coefficients: [ -0.1   0.    0.    3.1 -15.4   4.1  -0.   -1.4   0.2  -0.   -0.9   0.
      -0.5]
    RM          4.057199
    CHAS        3.119835
    RAD         0.242727
    INDUS       0.049523
    ZN          0.035809
    B           0.011794
    TAX        -0.008702
    AGE        -0.010821
    CRIM       -0.133470
    LSTAT      -0.547113
    PTRATIO    -0.910685
    DIS        -1.385998
    NOX       -15.417061
    dtype: float64



```python
plt.scatter(expectation, pred)
plt.plot([0, 50], [0, 50], '--k')
plt.xlabel('Expected price')
plt.ylabel('Predicted price')
plt.tight_layout()
```


![png]({{ site.url }}{{ site.baseurl }}/images/boston/Session02-BostonHousePrice-Solution_12_0.png)


## Prediction with other Regression methods

- **Ridge, Lasso and ElasticNet**
- **Gradient Boosting Regressor**
- **XG Boost**
- **SGD Regressor**
According to sklearn's official documentation,  

"SGDRegressor is well suited for regression problems with a large number of training samples (> 10,000), for other problems we recommend Ridge, Lasso, or ElasticNet."


```python
from sklearn.linear_model import Ridge, Lasso, ElasticNet, SGDRegressor
from sklearn.ensemble import GradientBoostingRegressor
from xgboost import XGBRegressor
```


```python
# Try tuning the hyper-parameters
models = {
    "Ridge" : Ridge(),
    "Lasso" : Lasso(),
    "ElasticNet" : ElasticNet(),
    "Gradient Boosting" : GradientBoostingRegressor(),
    "SGD" : SGDRegressor(max_iter=1000, tol=1e-3), 
    "XGB" : XGBRegressor(objective ='reg:linear', colsample_bytree = 0.3, learning_rate = 0.1,
                max_depth = 5, alpha = 10, n_estimators = 10)
}

pred_record = {}
```


```python
for name, model in models.items():
    # Load your machine learning model
    curr_model = model
    # Train!
    curr_model.fit(X_train, Y_train)
    # make prediction with unseen data!
    pred = curr_model.predict(X_test)
    expectation = Y_test
    # what is mse between the answer and your prediction?
    mse = mean_squared_error(expectation, pred)
    rmse = np.sqrt(mse)

    print('{} MSE: {}, {} RMSE: {}'.format(name, mse, name, rmse))
    
    pred_record.update({name : pred})
```
Output:  

    Ridge MSE: 22.044053089861013, Ridge RMSE: 4.695109486461526
    Lasso MSE: 25.639502928043992, Lasso RMSE: 5.063546477326341
    ElasticNet MSE: 25.40519636428209, ElasticNet RMSE: 5.040356769543411
    Gradient Boosting MSE: 7.737869423986623, Gradient Boosting RMSE: 2.781702612427616
    SGD MSE: 1.5570858695910825e+28, SGD RMSE: 124783246855941.47
    [20:06:46] WARNING: src/objective/regression_obj.cu:152: reg:linear is now deprecated in favor of reg:squarederror.
    XGB MSE: 82.71679422016705, XGB RMSE: 9.094877361469315



```python
prediction = pred_record["SGD"]

plt.scatter(expectation, prediction)
plt.plot([0, 50], [0, 50], '--k')
plt.xlabel('Expected price')
plt.ylabel('Predicted price')
plt.tight_layout()
```


![png]({{ site.url }}{{ site.baseurl }}/images/boston/Session02-BostonHousePrice-Solution_18_0.png)



```python
prediction = pred_record["XGB"]

plt.scatter(expectation, prediction)
plt.plot([0, 50], [0, 50], '--k')
plt.xlabel('Expected price')
plt.ylabel('Predicted price')
plt.tight_layout()
```


![png]({{ site.url }}{{ site.baseurl }}/images/boston/Session02-BostonHousePrice-Solution_19_0.png)



```python
prediction = pred_record["Gradient Boosting"]

plt.scatter(expectation, prediction)
plt.plot([0, 50], [0, 50], '--k')
plt.xlabel('Expected price')
plt.ylabel('Predicted price')
plt.tight_layout()
```


![png]({{ site.url }}{{ site.baseurl }}/images/boston/Session02-BostonHousePrice-Solution_20_0.png)


## A Little Taster Session for Neural Network


```python
from tensorflow import keras
from tensorflow.keras.layers import add, Dense, Activation

def neural_net():
    model = keras.Sequential()
    model.add(Dense(512, input_dim=BOSTON_DATA.data.shape[1]))
    model.add(Activation('relu'))
    model.add(Dense(256))
    model.add(Activation('relu'))
    model.add(Dense(128))
    model.add(Activation('relu'))
    model.add(Dense(64))
    model.add(Activation('relu'))
    model.add(Dense(1))
    return model
```


```python
model = neural_net()
model.summary()
```
Output:  

    Model: "sequential_10"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    dense_31 (Dense)             (None, 512)               7168      
    _________________________________________________________________
    activation_24 (Activation)   (None, 512)               0         
    _________________________________________________________________
    dense_32 (Dense)             (None, 256)               131328    
    _________________________________________________________________
    activation_25 (Activation)   (None, 256)               0         
    _________________________________________________________________
    dense_33 (Dense)             (None, 128)               32896     
    _________________________________________________________________
    activation_26 (Activation)   (None, 128)               0         
    _________________________________________________________________
    dense_34 (Dense)             (None, 64)                8256      
    _________________________________________________________________
    activation_27 (Activation)   (None, 64)                0         
    _________________________________________________________________
    dense_35 (Dense)             (None, 1)                 65        
    =================================================================
    Total params: 179,713
    Trainable params: 179,713
    Non-trainable params: 0
    _________________________________________________________________



```python
model.compile(loss='mse', optimizer='adam', metrics=['accuracy'])
```


```python
history = model.fit(X_train, Y_train, epochs=100)

loss, test_acc = model.evaluate(X_test, Y_test)
print('Test Loss : {:.4f} | Test Accuracy : {}'.format(loss, test_acc))
```


```python
prediction = model.predict(X_test)

plt.scatter(expectation, prediction)
plt.plot([0, 50], [0, 50], '--k')
plt.xlabel('Expected price')
plt.ylabel('Predicted price')
plt.tight_layout()
```


![png]({{ site.url }}{{ site.baseurl }}/images/boston/Session02-BostonHousePrice-Solution_26_1.png)

