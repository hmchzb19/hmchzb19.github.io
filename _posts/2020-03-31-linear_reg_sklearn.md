这是sklearn的linear regression和前两天的statsmodel不一样，statsmodel如果用
`statsmodels.api`,不论是fit方法还是predict方法，
都需要用`sm.add_constant`方法增加一列`const`,
如果使用`statsmodels.formula.api`则不需要`add_constant`方法，
只需要传入R-style formula string就可以. 
使用sklearn的LinearRegression则可以直接fit,predict. 只是要注意传入参数的shape.


````
# coding: utf-8

import numpy as np
import statsmodels.api as sm
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
from sklearn.linear_model import LinearRegression
sns.set()

data=pd.read_csv('data-analysis/python-jupyter/1.01. Simple linear regression.csv')

y=data['GPA']
X=data['SAT']
print(X.shape)
print(y.shape)

reg=LinearRegression()
'''
run reg.fit(X,y) 
error message:
ValueError: Expected 2D array, got 1D array instead:

check X type
type(X) == Series, 

'''

#reshape
X_matrix=X.values.reshape(-1,1)
print(X_matrix.shape)

reg.fit(X_matrix, y)

'''
reg.score:  R-squared
reg.coef_: coefficient / slope
reg.intercept_: intercept
'''
print(reg.score(X_matrix, y))
print(reg.coef_)
print(reg.intercept_)


#make prediction
gen_data=np.linspace(1700,1800, num=10, dtype=int)
new_data=pd.DataFrame(data=gen_data, columns=['SAT'])
reg.predict(new_data)
new_data['Predicted_GPA']=reg.predict(new_data)
print(new_data)
````

下面是前两天的用statsmodel.api的predict.
````
#predict
gen_data=np.linspace(1700,1800, num=10, dtype=int)
new_data=pd.DataFrame(data=gen_data, columns=['SAT'])
new_x=sm.add_constant(new_data)

predicted_y=results.predict(new_x)

new_x['Predicted_GPA']=predicted_y
#drop const-column
new_x=new_x.drop(['const'], axis=1)
print(new_x)
````
