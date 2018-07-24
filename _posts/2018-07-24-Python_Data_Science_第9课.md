1. 今天才看到Machine Learning:  
Machine learning:  
Algorithms that can learn from observational data, and can make predictions based on it.  
Machine Learning分为unsupervised learning 和 supervised learning.  
区别在: supervised learning: 有“参考答案”，而unsupervised learning则没有"参考答案"  

2. Train /Test :  
<p>
Need to ensure both sets are large enough to contain representatives of all the variations and outliers in the data you care about.  
The data sets must be selected randomly.  
Train/Test is a great way to guard against overfitting.  
</p>
<p>
K-fold Cross validation:  
  One way to further protect against overfitting is k-fold cross validation.  
    - split your data into k randomly-assigned segments.
    - Reserve one segment as your test data.
    - Train on each of the remaining k-1 segments and measure their performance against the test set.
    - Take the average of the k-1 r-squared scores.
</p>


3. 下面就是实际代码  
Train /Test practice - prevent overfitting of a polynomial regression.  
<p>
首先是伪造数据
</p>
        np.random.seed(2)
        pageSpeeds = np.random.normal(3.0, 1.0, 100)
        purchaseAmount = np.random.normal(50.0, 30.0, 100) / pageSpeeds
        plt.scatter(pageSpeeds,purchaseAmount)
        plt.show()


        np.random.seed(2)
        pageSpeeds = np.random.normal(3.0, 1.0, 100)
        purchaseAmount = np.random.normal(50.0, 30.0, 100) / pageSpeeds
        plt.scatter(pageSpeeds,purchaseAmount)
        plt.show()
    
        #now we will split the data in two part 80% of the data used for training.
        #20% for testing, This way we avoid overfitting.
        #in real world, shuffle the data before split it.
        trainX = pageSpeeds[:80]
        testX= pageSpeeds[80:]
    
        trainY = purchaseAmount[:80]
        testY=purchaseAmount[80:]
    
        plt.scatter(trainX, trainY)
        plt.show()
        plt.scatter(testX, testY)
        plt.show()
    
        #we will try to fit an 8-degree polynomial to this data(centainly overfitting)
        x=np.array(trainX)
        y=np.array(trainY)
    
        p8=np.poly1d(np.polyfit(x, y, 8))
    
        #plot the polynomial against the training data
        xp=np.linspace(0,7,100)
        axes=plt.axes()
        axes.set_xlim([0,7])
        axes.set_ylim([0,200])
        plt.scatter(x,y)
        plt.plot(xp, p8(xp), c='r')
        plt.show()
    
        #plot the polynomial against the test data
        testx=np.array(testX)
        testy=np.array(testY)
        axes=plt.axes()
        axes.set_xlim([0,7])
        axes.set_ylim([0,200])
        plt.scatter(testx,testy)
        plt.plot(xp, p8(xp), c='r')
        plt.show()
<p>    
检查r-squared
</p>
    
    
        #r-squared score on the test data is terrible: 0.30 , even though it fits the training data.
        r2=r2_score(testy, p8(testx))
        print(r2)
    
        #r-squared score on the training data: 0.64, better
        r3=r2_score(y, p8(x))
        print(r3)
    


4. 写在最后, 经过实验  
使用6-degree polynomial 的时候，test data 的r-squared最大. 0.60.
    
    
        p8=np.poly1d(np.polyfit(x, y, 8))
        p7=np.poly1d(np.polyfit(x, y, 7))
        p6=np.poly1d(np.polyfit(x, y, 6))
        p5=np.poly1d(np.polyfit(x, y, 5))
        p4=np.poly1d(np.polyfit(x, y, 4))
    
        In [23]: p5=np.poly1d(np.polyfit(x, y, 5))
    
        In [24]: r2=r2_score(testy, p5(testx))
            ...: print(r2)
            ...:
        0.504072389719
    
        In [25]: r2=r2_score(testy, p6(testx))
            ...: print(r2)
            ...:
        0.605011947036
    
        In [26]: p7=np.poly1d(np.polyfit(x, y, 7))
    
        In [27]: r2=r2_score(testy, p7(testx))
            ...: print(r2)
            ...:
        0.546145145284
    
    