logistic_regression 说白了就是个binary classifier.遵从伯努利分布

代码如下:

````
# coding: utf-8

import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

raw_data=pd.read_csv('data-analysis/python-jupyter/2.01. Admittance.csv')

type(raw_data)
print(raw_data.columns)


data=raw_data.copy()
data['Admitted']=raw_data['Admitted'].map({'Yes':1, 'No':0})

X1=data['SAT']
y=data['Admitted']
print('X shape: {},y shape: {}'.format(X1.shape, y.shape))

#plot 
plt.scatter(X1, y, color='c0')
plt.xlabel('SAT', fontsize=20)
plt.ylabel('Admitted', fontsize=20)
plt.show()


import statsmodels.api as sm
import statsmodels.formula.api as smf

x=sm.add_constant(X1)
reg_log=sm.Logit(y, x)
results_log = reg_log.fit()
from scipy import stats

print(results_log.summary())
print()

from sklearn.linear_model import LogisticRegression
#Scikit-Learn LogisticRegression
reg=LogisticRegression(solver='lbfgs')
#reshape X
x_matrix = X1.values.reshape(168, 1)
reg.fit(x_matrix, y)

#cm_df: confustion matrix 
cm_df=pd.DataFrame(results_log.pred_table())
cm_df.columns=['Predicted 0', 'Predicted 1']
cm_df = cm_df.rename(index={0:'Acutal 0', 1:'Actual 1'})

#print the confusion-matrix
print(cm_df)

#calculate the accuracy
cm = np.array(cm_df)
accuracy_train = (cm[0,0] + cm[1,1]) / cm.sum()
print('accuracy: {}'.format(accuracy_train))

````