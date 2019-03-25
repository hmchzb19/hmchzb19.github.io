#### 今天是covariance and correlation.

0. 代码都是在ipython3里面敲的，所以prereq如下:

    
    
        ipython3
    
        In [1]: import numpy as np
    
        In [2]: import matplotlib.pyplot as plt
    

1. they give us a means of measuring just how tight these things are correlated  
covariance: Measures how two variables vary in tandem from their means.  
correlation: -1 negative(inverse) correlation,one value increases, the other decreases. vice versa. 0 no correlation, 1 positive correlation. these two attributes are moving in exactly the same way as you look at different data points.  


2. using following methods to calculate the covariance and correlation, these are self-written methods for calculate covariance and correlation.   
+. Think of the data sets for the two variables as high-dimensional vectors.  
+. Convert these to vectors of variances from the mean.  
+. Take the dot product(cosine of the angle between them)of the two vectors.  
+. Divide by the sample size.  
    
        def de_mean(x):
            xmean=np.mean(x)
            return [xi - xmean for xi in x]
    
        def covariance(x, y):
            n=len(x)
            return np.dot(de_mean(x), de_mean(y)) / (n-1)
    
        #compute correlation
        def correlation(x, y):
            #compute the standard deviation
            stddevx=np.std(x)
            stddevy=np.std(y)
            
            #check devide by 0 in this step
            return covariance(x,y) / stddevx /stddevy
        
        
        #Fabricate data:page speeds(how quickly a page renders on a website) and how much people spend.
        pagespeeds = np.random.normal(3.0, 1.0, 1000)
        #normal distribution : there is no real relationship between the two attributes
        purchaseAmount = np.random.normal(50.0, 10.0, 1000)
        plt.scatter(pagespeeds, purchaseAmount)
        covariance(pagespeeds, purchaseAmount)
        np.cov(pagespeeds,purchaseAmount)
        plt.show()
    
        #Fbricate these data with relations.
        pagespeeds2 = np.random.normal(3.0, 1.0, 1000)
        purchaseAmount2 = np.random.normal(50.0, 10.0, 1000) / pagespeeds2
        plt.scatter(pagespeeds2, purchaseAmount2)
        covariance(pagespeeds2, purchaseAmount2)
        np.cov(pagespeeds2,purchaseAmount2)
        plt.show()
    
        #close to 0
        correlation(pagespeeds, purchaseAmount)
        np.corrcoef(pagespeeds, purchaseAmount)
    
        #close to 1 or -1 ,means they have relationship
        correlation(pagespeeds2, purchaseAmount2)
        np.corrcoef(pagespeeds2, purchaseAmount2)
    
        #a perfect correlations ,close to -1.
        pagespeeds3 = np.random.normal(3.0, 1.0, 1000)
        purchaseAmount3 = 100 - pagespeeds3 * 3
        plt.scatter(pagespeeds3, purchaseAmount3)
        correlation(pagespeeds3, purchaseAmount3)
        plt.show()
    


3. numpy has a numpy.cov function that can compute covariance.
numpy.corrcoef, it returns a matrix of correlation coefficients between every combination of the arrays passed in. 