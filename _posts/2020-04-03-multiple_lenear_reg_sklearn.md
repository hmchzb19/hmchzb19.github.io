使用sklearn练习的multiple_linear_regression, sklearn没有现成计算p-value,adjusted-R-squared的方法。也没有`statsmodel`那样的summary，需要自己手动制作.

````
α: level of significance, 常取值　0.05, 0.01,
(1-α): confidence level 
if we have a α = 0.05, means we are 95% confidence the feature are significant
the aim is -- the p-values always less than α.
````

代码如下: 
````
# coding: utf-8
import statsmodels.api as sm
import statsmodels.formula.api as smf
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
sns.set()

data=pd.read_csv('data-analysis/python-jupyter/1.02. Multiple linear regression.csv')
#print(data.describe())
X=data[['SAT','Rand 1,2,3']]
y=data['GPA']

from sklearn.linear_model import LinearRegression
reg=LinearRegression()

reg.fit(X, y)

#compute r-sqaured ,adjusted-R-squared
r2=reg.score(X,y)
n,p=X.shape[0], X.shape[1]
adjusted_r2=1-(1-r2)*(n-1)/(n-p-1)

from sklearn.feature_selection import f_regression
'''
f_regression() =>return 2 array 
1st array: F  : shape=(n_features,), => F values of features, F-statistic
2nd array: p-value : shape=(n_features,),  => p-values of F-scores.
We always want the p-value to be less than 0.05

'''

#get p_values for these 2 features 
p_values=f_regression(X,y)[1]
p_values.round(3)

#reg_summary=pd.DataFrame(data=['SAT','Rand 1,2,3'], columns=['features'])
reg_summary=pd.DataFrame(data=X.columns, columns=['features'])
reg_summary['cofficients']=reg.coef_
reg_summary['p-values']=p_values.round(3)
print(reg_summary)

'''
p-value for feature "Rand 1,2,3" is 0.676, much bigger than 0.05. 
'''
````