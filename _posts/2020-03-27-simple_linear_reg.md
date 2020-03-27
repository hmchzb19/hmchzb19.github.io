这一段时间比较闲，找了UDEMY上面的教程看，看了些DataScience.
记录一下，今天是Linear Regression.
按照Lazyprogrammer的教程上，我们应该把这些所谓的DataScience都当做
Geometry.

代码如下,需要提前安装`statsmodel`,`seaborn`,`pandas`,`matplotlib`
可以使用`ipython3`或者`jupyter`.  
csv文件是我从github上找的，乱搜索了一通`kaggle`,`google`.
最后直接clone了别人的一个repo  
`git clone https://github.com/timurista/data-analysis`

````
import statsmodels.api as sm
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
sns.set()

data=pd.read_csv('data-analysis/python-jupyter/1.01. Simple linear regression.csv')
data.describe()
y=data['GPA']
X=data['SAT']
plt.scatter(X,y)
plt.xlabel('SAT',fontsize=20)
plt.ylabel('GPA', fontsize=20)
plt.show()

x1=sm.add_constant(X)
results=sm.OLS(y,x1).fit()
'''
intercept=0.275
slope=0.0017 

coef=coefficient
t: t-statistic
P>|t|: p-value, less than 0.005 menas the variable is significant, so the best value we want is 0.000

'''
print(results.summary())
plt.scatter(X,y)

yhat=0.0017*X+0.275
fig=plt.plot(X, yhat, lw=4, c='orange', label='regression line')
plt.xlabel('SAT', fontsize=20)
plt.ylabel('GPA', fontsize=20)
plt.show()
````
