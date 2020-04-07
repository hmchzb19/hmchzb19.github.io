记录下最近在看的两个博客  
[刘建平Pinard](https://www.cnblogs.com/pinard/)  
[Machine Learning Mastery](https://machinelearningmastery.com/)  
参考了[Feature Selection For Machine Learning in Python](https://machinelearningmastery.com/feature-selection-machine-learning-python/)  

数据文件仍然使用了上一篇中的[pima-indians-diabetes.csv](https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.csv)
才有了我这篇文章，在他的４种method之外，我多加了一个`RandomForestClassifier`,
但是`RandomForestClassifier`和`ExtraTreesClassifier`结果非常接近. 

代码如下：

````

# coding: utf-8
import pandas as pd, numpy as np
import statsmodels.api as sm
import statsmodels.formula.api as smf
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC
from sklearn.neighbors import KNeighborsClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import (accuracy_score, f1_score, confusion_matrix)
from sklearn.ensemble import (RandomForestClassifier, ExtraTreesClassifier)
sns.set()

#Handle data
col_names=['preg','plas','pres','skin','insu','mass','pedi','age','class']
data=pd.read_csv('data/pima-indians-diabetes.csv',header=None,names=col_names)
X=data.drop('class', axis=1)
y=data['class']
#pring X.shape and y.shape
print(X.shape, y.shape,'\n')

#using PCA -- Principal Component Analysis
def pca_select():
    from sklearn.decomposition import PCA

    pca=PCA()
    pca.fit(X)
    #sum of this list almost == 1
    print(sum(pca.explained_variance_ratio_))

    #print the features according to their importance
    print("Print the feature importance from biggest to smallest")
    for i in pca.explained_variance_ratio_:
        print('{:.6f}'.format(i))
    
    #or use map     
    '''
    for i in map('{:.6f}'.format , pca.explained_variance_ratio_):
        print(i,end='\t')
    '''

    print()
pca_select()


from sklearn.feature_selection import (f_classif,SelectKBest)
def univariate_select():
    
    test=SelectKBest(score_func=f_classif, k=4)
    fit=test.fit(X, y)
    
    print("feature scores: {}".format(fit.scores_))
    
    #a higher score means higher importance 
    for score, feature in sorted(zip(fit.scores_, list(X))):
        print('score {:<20} of feature {}'.format(score, feature))

    features=fit.transform(X)
    #print first 5 line of these 4 features 
    print(features[0:5, :])
    
    #confirm the 4 features being selected are "preg", "plas", "mass", "age", 
    assert np.array_equal(features, np.array(X[["preg", "plas", "mass", "age", ]]))
    
    

univariate_select()
    
from sklearn.feature_selection import RFE
from sklearn.linear_model import LogisticRegression
def RFE_select():
    model = LogisticRegression(solver='lbfgs')
    rfe=RFE(model, 4)
    fit=rfe.fit(X, y)
    
    print('\n')
    print("Num Features: {}".format(fit.n_features_))
    print("Selected Features: {}".format(fit.support_))
    print("Feature ranking, those with rank 1 will be choosen:\n{}".format(fit.ranking_))

    #print the 4 feature names to be choosen
    print("The 4 features to be choosen:")
    for idx,value in enumerate(fit.ranking_):
        if value==1:
            print(list(X)[idx], end='\t')
        
        
RFE_select()


def rlf_select():
    rnd_clf = RandomForestClassifier(random_state=0, n_estimators=100)
    rnd_clf.fit(X, y)
    rnd_name=rnd_clf.__class__.__name__
    
    feature_importances = rnd_clf.feature_importances_
    importance = sorted(zip(feature_importances, list(X)), reverse=True)
    
    print('\n\n{} most important features ( {} )'.format(4, rnd_name))  
    print("The 4 features to be choosen:") 
    [print(row) for i, row in enumerate(importance) if i < 4]

rlf_select()

def et_select():
    et_clf = ExtraTreesClassifier(random_state=0, n_estimators=100)
    et_clf.fit(X, y)
    et_name=et_clf.__class__.__name__
    
    feature_importances = et_clf.feature_importances_
    importance = sorted(zip(feature_importances, list(X)), reverse=True)
    
    print('\n\n{} most important features ( {} )'.format(4, et_name))  
    print("The 4 features to be choosen:") 
    [print(row) for i, row in enumerate(importance) if i < 4]

et_select()

````


