####Data Science 打算写一系列的笔记，记录下平时看书，看视频学到的知识.  

今天是第一课.  

1. Mean, Mode, Median.  


        Mean AKA Averate: sum/ number of samples
        Median: sort the values, and take the value at the midpoint, for even numbers
        then take the average of the midpoint 2.
        Mode: the most common value in a data set, which means this data occurs the most time.


    下面使用Python 代码来实地求出这些值  


        #import packages
        import numpy as np
        from scipy import stats
        import matplotlib.pyplot as plt
    
        #fabricate some data
        #use np.random.normal Draw random samples from a normal (Gaussian) distribution
        incomes = np.random.normal(27000,15000,10000)
        '''
            Parameters
            ----------
            loc : float or array_like of floats
                Mean ("centre") of the distribution.
            scale : float or array_like of floats
                Standard deviation (spread or "width") of the distribution.
            size : int or tuple of ints, optional
                Output shape. If the given shape is, e.g., ``(m, n, k)``, then
                ``m * n * k`` samples are drawn. If size is ``None`` (default),
                a single value is returned if ``loc`` and ``scale`` are both scalars.
                Otherwise, ``np.broadcast(loc, scale).size`` samples are drawn.
        '''
        np.mean(incomes) #average ,close to 27000
        plt.hist(incomes, 50)
        plt.show()
    
        #compute median
        np.median(incomes)
    
        #add one outlier, then the mean will change a lot, but the median will not change too much.
        incomes = np.append(incomes, [1000000000])
        In [26]: np.mean(incomes)
        Out[26]: 126837.27483313478
        In [27]: np.median(incomes)
        Out[27]: 26584.942499458524
    
        #If there is more than one such value, only the smallest is returned.
        lst=[1,1,2,2,3,3,4,4]
        In [20]: stats.mode(lst)
        Out[20]: ModeResult(mode=array([1]), count=array([2]))
        In [15]: lst=[1,2,3,2,2,2]
        In [16]: stats.mode(lst)
        Out[16]: ModeResult(mode=array([2]), count=array([4]))
        ages = np.random.randint(18,high=90, size=500)
        stats.mode(ages)
    

2.     standard deviation and variance:  

    variance: is simply the average of the squared differences from the mean.  
    Standard deviation is the squared root of the variance.  
    
    Example:  
    what is the variance of (1,4,5,4,8)  
    get mean: (4.4)  
    differences from the mean: (-3.4, -0.4, 0.6, -0.4, 3.6)  
    Squared differences: (11.56, 0.16, 0.36, 0.16, 12.96)  
    average of the squared differences:  5.04  
    Standard deviation : 2.24   


    下面是代码：

    
    
        #use numpy to calculate variance and standard deviation.
        In [30]: lst=[1,4,5,4,8]
        #standard deviation
        In [31]: np.std(lst)
        Out[31]: 2.2449944320643649
        #variance
        In [32]: np.var(lst)
        Out[32]: 5.04
    
    

