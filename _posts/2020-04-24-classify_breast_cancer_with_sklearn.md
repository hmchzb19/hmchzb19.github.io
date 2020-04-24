在sklearn里面有个数据集叫`load_breast_cancer`,569个sample,每个sample有30个feature.
我打算用`svm.SVC`来试试看.主要尝试的参数有`kernel和gamma`,kernel分别设置为`rbf`和`sigmoid`. gamma则设置为`auto`和`scale`.  
经过实验我的感觉如下 
`sigmoid`的score略低。而`rbf`稍微好一点.  
'gamma'设置为`auto`和`scale`最后的score都一样.  

我很奇怪，`sigmoid`的train score比test score还少了`0.03, 3%`为什么呢?

````
(569, 30)
(569,)
['malignant' 'benign']
X data shape:(569, 30); no. positive:357; no. negative:212
svm with kernel "rbf" train score and test score
train score: 0.984251968503937, test score: 0.9680851063829787

svm with kernel "sigmoid" train score and test score
train score: 0.931758530183727, test score: 0.9627659574468085
````

代码如下:

````

from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn import svm 
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import classification_report, accuracy_score

cancer = load_breast_cancer()
X, y = cancer.data, cancer.target
print(X.shape)
print(y.shape)
print(cancer.target_names)
print('X data shape:{0}; no. positive:{1}; no. negative:{2}'.format(X.shape, y[y==1].shape[0], y[y==0].shape[0]))

X_train,X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)

scaler=StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

svm_cf = svm.SVC(C=1, kernel='rbf',gamma='scale', tol=.001, random_state=42)
svm_cf.fit(X_train, y_train)
y_train_pred = svm_cf.predict(X_train)
y_test_pred = svm_cf.predict(X_test)
train_score = accuracy_score(y_train, y_train_pred)
test_score = accuracy_score(y_test, y_test_pred)

print('svm with kernel "rbf" train score and test score')
print('train score: {}, test score: {}'.format(train_score, test_score))

svm_sig_cf = svm.SVC(C=1, kernel='sigmoid', tol=.001, gamma='auto',random_state=42)
svm_sig_cf.fit(X_train, y_train)
y_train_pred = svm_sig_cf.predict(X_train)
y_test_pred = svm_sig_cf.predict(X_test)
train_score = accuracy_score(y_train, y_train_pred)
test_score = accuracy_score(y_test, y_test_pred)

print('\nsvm with kernel "sigmoid" train score and test score')
print('train score: {}, test score: {}'.format(train_score, test_score))

'''
print(classification_report(y_train, y_train_pred, target_names=cancer.target_names))

print()

print(classification_report(y_test, y_test_pred, target_names=cancer.target_names))
'''
````