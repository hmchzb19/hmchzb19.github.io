####我主要看的书叫 Hands-On Data Science and Python Machine Learning.  作者Frank kane    

今天是PMF 和PDF, 其实就是Data Distribution. 单纯看代码比较不直观，但是一用matplotlib 把图绘出来，就会变的非常直观。   

Terminology difference: A probability density function is a solid curve that describes the probability of a range of values happening with continuous data. A probability mass
function is the probabilities of given discrete values occurring in a dataset  


    
    
        PDF=Probability Density Function
        PMF=Probility Mass function.
    

0. import packages   

        
        import numpy as np
        from scipy import stats
        import matplotlib.pyplot as plt
    

1. Uniform Distribution: Flat  

    
        value= np.random.uniform(-10.0, 10.0, 100000)
        plt.hist(value, 50)
        plt.show()
    

2. Normal / Gaussian Distribution:  


    
        from scipy.stats import norm
        x= np.arange(-3, 3, 0.001)
        plt.plot(x, norm.pdf(x))
    
3.  Exponential PDF / "Power Law" 

    
        from scipy.stats import expon
        x=np.arange(0, 10, 0.001)
        plt.plot(x, expon.pdf(x))
        plt.show()
    

4. Binomial Probability Mass function.  

    
        from scipy.stats import binom
        x=np.arange(0, 10, 0.001)
        plt.plot(x, binom.pmf(x, 10, 0.5))
        plt.show()
    

5.  Poisson Probability Mass Function:

    
        from scipy.stats import poisson
        mu=500
        x=np.arange(400, 600, 0.5)
        plt.plot(x, poisson.pmf(x, mu))
        plt.show()

<EOF>