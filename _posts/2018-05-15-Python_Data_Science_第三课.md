#### 今天是percentiles 和moments.  

1. percentiles: in a data set, what's the point at which x% of the values are less than that value.  
例如90% percentile就是大于90%,50% percentile就是median.  
IQR: interquartile range  
when we talk about a distribution,IQR is the area in the middle of the distribution that contains 50% of the values.  

    
    
        #find percentiles
        vals = np.random.normal(0, 0.5, 10000)
        plt.hist(vals, 50)
        plt.show()
        np.percentile(vals, 50)        #median
        np.percentile(vals, 90)
        np.percentile(vals, 20)
    



2.  moments: Quantitative measures of the shape of a probability density function.  
the 1st moment is mean.  
the 2nd moment is variance.  
the 3rd moment is skew.    long tail on the left means negative skew, long tail on the right means positive skew.  
the 4th moment kurtosis.   how thick is the tail and how sharp is the peak , higher peak have higher kurtosis. For a normal distribution the kurtosis value should be close to 0.  
峰度系数（kurtosis）用来度量数据在中心聚集程度  
skew: [数学] 不对称的; [统计学] 歪斜，扭曲      
    
        In [4]: vals = np.random.normal(0, 0.5, 10000)
    
        In [5]: np.mean(vals)
        Out[5]: 0.0011295365153999981
    
        In [6]: np.var(vals)
        Out[6]: 0.25548593546863058
    
        In [7]: np.skew(vals)
        ---------------------------------------------------------------------------
        AttributeError Traceback (most recent call last)
        <ipython-input-7-b80cde84233b> in <module>()
        ----> 1 np.skew(vals)
    
        AttributeError: module 'numpy' has no attribute 'skew'
    
        In [8]: sp.skew(vals)
        Out[8]: 0.040095376387442
    
        In [9]: sp.kurtosis(vals)
        Out[9]: -0.038755363660934794
    
EOF
