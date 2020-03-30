有些时候需要把categorical data转化成number,需要使用dummy variable.
我发现做这个事情的方法太多了，可以用`pd.Series.map`, 可以用`pd.DataFrame.applymap`,
也可以用`sklearn.preprocessing里面提供的一些Encoder`  
e.g  
`from sklearn.preprocessing import LabelEncoder, OneHotEncoder, OrdinalEncoder`

代码如下
````
#import package
import statsmodels.api as sm
import statsmodels.formula.api as smf
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from sklearn.preprocessing import LabelEncoder, OneHotEncoder, OrdinalEncoder

sns.set()

data=pd.read_csv('data-analysis/python-jupyter/1.03. Dummies.csv')
# %load 11 12 15 30
print(data.shape)
new_data=data.copy()
#new_data['Attendance']=new_data['Attendance'].map({'Yes':1, 'No':0})

y=new_data['GPA']
X1=new_data[['SAT','Attendance']]

'''
or do this
'''
label_encoder=LabelEncoder()
new_data['Attendance']=label_encoder.fit_transform(new_data['Attendance'])


results=smf.ols(formula='GPA ~ SAT + Attendance', data=new_data).fit()
'''
or do this 
x=sm.add_constant(X1)
results=sm.OLS(y,x).fit()
'''
print(results.summary())
# %load 23-28

plt.scatter(new_data['SAT'], y)
yhat_no=0.6439 + 0.0014 * new_data['SAT']
yhat_yes = 0.8665 + 0.0014*new_data['SAT']
fig = plt.plot(new_data['SAT'], yhat_no, lw=2, c='#006837')
fig2= plt.plot(new_data['SAT'], yhat_yes, lw=2, c='#050026')
plt.xlabel('SAT', fontsize=20)
plt.ylabel('GPA', fontsize=20)
plt.show()

````
