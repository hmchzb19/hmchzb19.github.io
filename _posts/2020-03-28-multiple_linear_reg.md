首先是一些名词

Mean absolute error(MAE)  
Mean squared error(MSE)  
Root mean squared error (RMSE)  

sum of squares total (SST)  
sum of squares regression (SSR)  
sum of squares error (SSE)  

SST = SSR + SSE   
R-squared = SSR / SST   

R-squared always in (0-1),
normally ,the R-squared is between 0.2-0.9 depending on
your field. too high may means overfitting, too low may 
means your model are totally guessing. 

但是当引入多个变量的时候，应该看adjusted R-squared，当R-squared
的值上升而adjusted R-squared的值下降，说明引入的变量无意义.

代码：
````
import statsmodels.api as sm
import statsmodels.formula.api as smf
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
sns.set()
data=pd.read_csv('data-analysis/python-jupyter/1.02. Multiple linear regression.csv')
data.describe()
y=data['GPA']
X1=data[['SAT','Rand 1,2,3']]
x=sm.add_constant(X1)
results=sm.OLS(y,x).fit()
print(results.summary())
'''

F-statistic: used for testing the overal significance of the model
Prob (F-statistic): P-value for F-statistic
The lower the F-statistic, the closer to a non-signifiant model.


'''
````

我尝试使用R-style formula来重写,但是还不知道到怎么处理这里面的空格
````
f='GPA~ SAT + Rand 1,2,3'
model = smf.ols(formula=f, data=data)
````