polynomial regression

1. Not all relationships are linear.  
Linear formula: y=mx+b  
  This is a "first order" or "first degree" polynomial , as the power of x is 1.  
Second order polynomial: `y=ax**2 + bx + c`  
Third order polynomial: `y = ax**3 + bx**2 + cx + d`  
Higher orders polynomial produce more complex curves.  

2. beware overfitting:  
Don't use more degrees than you need.  
Visualize your data first to see how complex of a curve there might really be.  
Visualize the fit - is your curve going out of its way to accomodate outliers?  
A high r-squared simply means your curve fits your training data well, but it may not be a good predictor.  


3. Code  

        #fabricate data
        np.random.seed(2)
        pageSpeeds = np.random.normal(3.0, 1.0, 1000)
        purchaseAmount = np.random.normal(50.0, 10.0, 1000) / pageSpeeds
        plt.scatter(pageSpeeds, purchaseAmount)
        plt.show()
        #numpy has a handy polyfit function we can use, to let us construct an nth-degree polynomial model of our data that minimizes squared error. Let's try it with a 4th degree polynomial.
        x=np.array(pageSpeeds)
        y=np.array(purchaseAmount)
        p4=np.poly1d(np.polyfit(x,y, 4))
        
        #visualize
        xp=np.linspace(0, 7, 100)
        plt.scatter(x, y)
        plt.plot(xp, p4(xp), c='r')
        plt.show()
        
        #measure the r-squared error, 0 is bad, and 1 is good.
        from sklearn.metrics import r2_score
        r2=r2_score(y, p4(x))
        print(r2)
        #output will be ,pretty good
        0.82937663963
        
        #change the order to 8
        In [14]: p4=np.poly1d(np.polyfit(x,y, 8))
            ...:
        In [15]: xp=np.linspace(0, 7, 100)
            ...: plt.scatter(x, y)
            ...: plt.plot(xp, p4(xp), c='r')
            ...: plt.show()
            ...:
        
        In [16]: from sklearn.metrics import r2_score
            ...: r2=r2_score(y, p4(x))
            ...: print(r2)
            ...:
        #more accurate than order of 4
        0.881439566368
        
        #change the order to 1 , this will be linear regression.
        p4=np.poly1d(np.polyfit(x,y, 1))
        xp=np.linspace(0, 7, 100)
        plt.scatter(x, y)
        plt.plot(xp, p4(xp), c='r')
        plt.show()
        from sklearn.metrics import r2_score
        r2=r2_score(y, p4(x))
        print(r2)
        #r-squared is only 0.50
        0.502494130455