想要练下standardization,结果就有了这一篇.
理论上的Normalization vs. Standardization  
The terms normalization and standardization are sometimes used interchangeably, but they usually refer to different things. 
Normalization usually means to scale a variable to have a values between 0 and 1,   
while standardization transforms data to have a mean of zero and a standard deviation of 1.   
This standardization is called a z-score  

Standardized: (X-mean) / sd  
Normalized: (X - min(X)) / (max(X)-min(X))  

在sklearn里面standardization只要两行
````
scaler = StandardScaler().fit(X_train)
X_train_std, X_test_std = scaler.transform(X_train), scaler.transform(X_test)
````

今天使用的数据文件是一个[pima-indians-diabetes.csv](https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.csv),对这个文件的说明在这里，
[Dataset Detail](https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.names)

共有9 columns,col 9是target.
````
1. Number of times pregnant : 怀孕次数
2. Plasma glucose concentration a 2 hours in an oral glucose tolerance test  : 血浆葡萄糖浓度　－2小时口服葡萄糖耐量试验
3. Diastolic blood pressure (mm Hg) : 舒张压 
4. Triceps skin fold thickness (mm) : 三头肌皮褶厚度
5. 2-Hour serum insulin (mu U/ml) : 餐后2小时血清胰岛素
6. Body mass index (weight in kg/(height in m)^2) : 体重指数（体重（公斤）/ 身高（米）^2）
7. Diabetes pedigree function : 糖尿病家系作用
8. Age (years) : 年龄
9. Class variable (0 or 1) : target, 标签， 0表示不发病，1表示发病
````

这个代码是我参考一本书叫Hands-on Scikit-Learn for Machine Learning Applications上的代码修改而来的。
RandomForestClassifier和ExtraTreesClassifier对stardardized数据和未经standardized的数据几乎没有差别。  
后面的svm和knn我没有实验.
代码如下


````
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

def get_scores(model, xtrain, ytrain, xtest, ytest, scoring):
    ypred = model.predict(xtest)
    train = model.score(xtrain, ytrain)
    test = model.score(xtest, ytest)
    f1 = f1_score(ytest, ypred, average=scoring)
    return (train, test, f1)

def main():

    col_names=['preg','plas','pres','skin','insu','mass','pedi','age','class']
    data=pd.read_csv('data/pima-indians-diabetes.csv',header=None,names=col_names)
    print(data.describe())

    print(data['class'].unique())
    print(data['class'].value_counts())

    X=data.drop('class', axis=1)
    y=data['class']

    print('X shape: {}'.format(X.shape))
    print('y shape: {}'.format(y.shape))

    #split into train and test
    X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=42 )
    
    #Standardization
    scaler = StandardScaler().fit(X_train.astype(np.float64))
    X_train_std, X_test_std = scaler.transform(X_train.astype(np.float64)), scaler.transform(X_test.astype(np.float64))
    

    et = ExtraTreesClassifier(random_state=0, n_estimators=100)
    et.fit(X_train, y_train)
    et_scores = get_scores(et, X_train, y_train, X_test, y_test, 'micro')
    print('{} train, test, f1_score'.format(et.__class__.__name__))
    print(et_scores, '\n')

    
    et.fit(X_train_std, y_train)
    et_std_scores = get_scores(et, X_train_std, y_train, X_test_std, y_test, 'micro')
    print('{} with standardization (train, test, f1_score)'.format(et.__class__.__name__))
    print(et_std_scores, '\n')

    rf = RandomForestClassifier(random_state=42, n_estimators=100)
    rf.fit(X_train, y_train)
    rf_scores = get_scores(rf, X_train, y_train, X_test, y_test, 'micro')
    print('{} train, test, f1_score'.format(rf.__class__.__name__))
    print(rf_scores, '\n')

    rf.fit(X_train_std, y_train)
    rf_std_scores = get_scores(rf, X_train_std, y_train, X_test_std, y_test, 'micro')
    print('{} with standardization (train, test, f1_score)'.format(rf.__class__.__name__))
    print(rf_std_scores, '\n')

    knn = KNeighborsClassifier().fit(X_train, y_train)
    knn_scores = get_scores(knn, X_train, y_train, X_test, y_test, 'micro')

    print('{} train, test, f1_score'.format(knn.__class__.__name__))
    print(knn_scores, '\n')

    svm = SVC(random_state=0, gamma='scale')
    svm.fit(X_train_std, y_train)
    svm_scores =  get_scores(svm, X_train_std, y_train, X_test_std, y_test, 'micro')

    print('{} train, test, f1_score'.format(svm.__class__.__name__))
    print(svm_scores, '\n')

    knn_name, svm_name = knn.__class__.__name__, svm.__class__.__name__

    y_pred_knn = knn.predict(X_test)
    cm_knn = confusion_matrix(y_test, y_pred_knn)
    cm_knn_T = cm_knn.T

    y_pred_svm = svm.predict(X_test_std)
    cm_svm = confusion_matrix(y_test, y_pred_svm)
    cm_svm_T = cm_svm.T

    plt.figure(knn_name)
    ax= plt.axes()
    sns.heatmap(cm_knn_T, annot=True, fmt='d', cmap='gist_ncar_r', cbar=False)
    ax.set_title('{}  confustion matrix'.format(knn_name))
    plt.xlabel('true label')
    plt.ylabel('predicted label')

    plt.figure(svm_name)
    ax= plt.axes()
    sns.heatmap(cm_svm_T, annot=True, fmt='d', cmap='gist_ncar_r', cbar=False)
    ax.set_title('{}  confustion matrix'.format(svm_name))
    plt.xlabel('true label')
    plt.ylabel('predicted label')

    cnt_no, cnt_yes = 0, 0
    for i,row in enumerate(y_test):

        if row == 0: cnt_no += 1
        elif row == 1:
            cnt_yes += 1

    print('true => 0: {} , 1: {}\n'.format(cnt_no, cnt_yes))

    p_no, p_nox = cm_knn_T[0][0], cm_knn_T[0][1]
    p_yes, p_yesx = cm_knn_T[1][1], cm_knn_T[1][0]

    print('knn classification report')
    print('predited 0: {}, ( {} misclassified)'.format(p_no, p_nox))
    print('predicted 1: {}, ( {} misclassified)'.format(p_yes, p_yesx))

    print()

    p_no, p_nox = cm_svm_T[0][0], cm_svm_T[0][1]
    p_yes, p_yesx = cm_svm_T[1][1], cm_svm_T[1][0]

    print('svm classification report')
    print('predited 0: {}, ( {} misclassified)'.format(p_no, p_nox))
    print('predicted 1: {}, ( {} misclassified)'.format(p_yes, p_yesx))

    plt.show()

main()
````
