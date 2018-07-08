#### multivariate regression. (multiple regression)

1. What if more than 1 variable influence the one you are interested in ?  
Example:  
  predicting a price for a car based on its many attributes(body style, brand, mileage, etc.)  
  例如有多个因素都会影响车辆的价格，车型，品牌，公里数，车门数。  

    still use least squares.  
We just end up with coefficients for each factor.  
  For example,  
  prices = a + b(mileage) + c(doors)  
  These coefficients imply how important each factor is (if the data is all normalized)  
  Get rid of ones that don't matter.  
Can still measure fit with r-squared.  
Need to assume the different factors are not themselves dependent on each other.  

    这些统计学的概念弄的我头大，好在敲代码不那么难：  
    [回归评价指标MSE、RMSE、MAE、R-Squared](https://www.jianshu.com/p/9ee85fdad150)  

2.  直接上代码了.  
使用pandas 来读一个excel. 有可能碰到ImportError  

        
        #statsmodel package
        #ImportError: Install xlrd >= 0.9.0 for Excel support , need xlrd.
        
        apt-get install python3-xlrd
        import pandas as pd
        df=pd.read_excel("http://cdn.sundog-soft.com/Udemy/DataScience/cars.xls")
        df.head()  

    第一种版本:

        import statsmodels.api as sm
        from sklearn.preprocessing import StandardScaler
    
        scale= StandardScaler()
        X=df[['Mileage','Cylinder', 'Doors', 'Model_ord']]
        y=df[['Price']]
    
        X[['Mileage', 'Cylinder', 'Doors']]=scale.fit_transform(X[['Mileage', 'Cylinder', 'Doors']].as_matrix())
        print(X)
        est=sm.OLS(y, X).fit()
        est.summary()   
    
    

    第二种版本：

        df['Model_ord'] = pd.Categorical(df.Model).codes
        X=df[['Mileage','Cylinder', 'Doors', 'Model_ord']]
        y=df[['Price']]
        X1=sm.add_constant(X)
        est=sm.OLS(y, X1).fit()
        
        est.summary()   


    当然得到的结果都是一样的
    

        """
                                    OLS Regression Results                            
        ==============================================================================
        Dep. Variable:                  Price   R-squared:                       0.425
        Model:                            OLS   Adj. R-squared:                  0.422
        Method:                 Least Squares   F-statistic:                     147.4
        Date:                Sun, 08 Jul 2018   Prob (F-statistic):           2.10e-94
        Time:                        20:51:27   Log-Likelihood:                -8313.9
        No. Observations:                 804   AIC:                         1.664e+04
        Df Residuals:                     799   BIC:                         1.666e+04
        Df Model:                           4                                         
        Covariance Type:            nonrobust                                         
        ==============================================================================
                         coef    std err          t      P>|t|      [0.025      0.975]
        ------------------------------------------------------------------------------
        const       1.037e+04   1669.695      6.211      0.000    7093.297    1.36e+04
        Mileage       -0.1608      0.032     -4.966      0.000      -0.224      -0.097
        Cylinder    4726.1696    204.906     23.065      0.000    4323.952    5128.387
        Doors      -1742.6007    312.188     -5.582      0.000   -2355.406   -1129.795
        Model_ord   -309.4669     32.666     -9.474      0.000    -373.587    -245.346
        ==============================================================================
        Omnibus:                      193.097   Durbin-Watson:                   0.081
        Prob(Omnibus):                  0.000   Jarque-Bera (JB):              440.016
        Skew:                           1.288   Prob(JB):                     2.83e-96
        Kurtosis:                       5.549   Cond. No.                     1.37e+05
        ==============================================================================
        
        Warnings:
        [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
        [2] The condition number is large, 1.37e+05. This might indicate that there are
        strong multicollinearity or other numerical problems.
        """

    注意这里的R-squared ，1是完美，0是极差，0.425, 算是不那么差。如果把使用的columns 减少一个，  
    例如：使用三列，这个R-squared 就会变得很比较糟， 0.042.

        `X=df[['Mileage','Cylinder', 'Doors' ]]`



        In [13]: df['Model_ord'] = pd.Categorical(df.Model).codes
            ...: X=df[['Mileage', 'Doors', 'Model_ord']]
            ...: y=df[['Price']]
            ...: X1=sm.add_constant(X)
            ...: est=sm.OLS(y, X1).fit()
            ...:
            ...: est.summary()
            ...:
        Out[13]:
        <class 'statsmodels.iolib.summary.Summary'>
        """
                                    OLS Regression Results
        ==============================================================================
        Dep. Variable: Price R-squared: 0.042
        Model: OLS Adj. R-squared: 0.038
        Method: Least Squares F-statistic: 11.57
        Date: Sun, 08 Jul 2018 Prob (F-statistic): 1.98e-07
        Time: 20:52:16 Log-Likelihood: -8519.1
        No. Observations: 804 AIC: 1.705e+04
        Df Residuals: 800 BIC: 1.706e+04
        Df Model: 3
        Covariance Type: nonrobust
        ==============================================================================
                         coef std err t P>|t| [0.025 0.975]
        ------------------------------------------------------------------------------
        const 3.125e+04 1809.549 17.272 0.000 2.77e+04 3.48e+04
        Mileage -0.1765 0.042 -4.227 0.000 -0.259 -0.095
        Doors -1652.9303 402.649 -4.105 0.000 -2443.303 -862.558
        Model_ord -39.0387 39.326 -0.993 0.321 -116.234 38.157
        ==============================================================================
        Omnibus: 206.410 Durbin-Watson: 0.080
        Prob(Omnibus): 0.000 Jarque-Bera (JB): 470.872
        Skew: 1.379 Prob(JB): 5.64e-103
        Kurtosis: 5.541 Cond. No. 1.15e+05
        ==============================================================================
    
        Warnings:
        [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
        [2] The condition number is large, 1.15e+05. This might indicate that there are
        strong multicollinearity or other numerical problems.
        """


3. 最后

    
        #The table of coefficients above give us the values to plug into an equation of form B0 + B1 mileage + B2 model_ord + B3 * doors
        #In this example it's pretty clear that the number of cylinders is more important than anything based on the coefficient.
        #Could we have figured that out outliers?
        y.groupby(df.Doors).mean()
        #surpringly , more doors does not mean a higher price, so it's not surprising that it's pretty useless as a         predictor here.
    
    