今天是Predictive Model (预测模型)

Linear Regression: AKA likelihood estimation.  
Fit a line to a data set of observations.  
Use this line to predict unobserved values.  
其实Linear Regression 画出来就是一条直线。尽量靠近所有的数据点。  

Linear Regression 可以用多种算法来做。

1. Linear Regression use a technique called 'ordinary least squares': OLS
least squares(最小二乘法)
Minimize the squared-error between each point and the line.
Remember the slope-intercept equation of a line? y=mx+b
The slope is the correlation between the two variables times the standard deviation in Y, all divided by the standard deviation in X.
Neat how standard deviation how some real mathematical meaning ?
The intercept is the mean of Y minus the slope times the mean of X.
But Python will do all that for you.


2. Gradient Descent is an alternate method to least squares. (梯度下降)
Basically iterate to find the line that best follows the contours defined by the data.
Can make some sense when dealing with 3D data.
usually seen in higher dimensional data.
usually least squares is a perfectly good choice.

3. Measuring error with r-squared
How do we measure how well our line fits our data.
R-squared (AKA coefficient of determination) measures. (确定性系数)
how to calculate
1.0 - (sum of squared errors / sum of squared variation from mean )  

    R-squared 取值:Ranges from 0 - 1.0  
    0 is bad(none of the variance is captured), 1 is good(all of the variance is captured)

4. Compute Linear Regression and r-squared in Python  

        pageSpeeds = np.random.normal(3.0, 1.0, 1000)
        purchaseAmount = 100 - (pageSpeeds + np.random.normal(0, 0.1, 1000)) * 3
        plt.scatter(pageSpeeds, purchaseAmount)
        plt.show()
    
        #as we only have two features, we can keep in simple and just use scipy.state.linregress
        from scipy import stats
        slope, intercept, r_value, p_value, std_err = stats.linregress(pageSpeeds, purchaseAmount)
    
        #R-squared value shows a really good fit.
        r_value ** 2
    
        #use the slope and intercept we got from the regression to plot predicted values vs observed.
        def predict(x):
            return slope * x + intercept
    
        fitline=predict(pageSpeeds)
        plt.scatter(pageSpeeds, purchaseAmount)
        plt.plot(pageSpeeds, fitline, c='r')
        plt.show()
    

5. 5- 关于Gradient Descent， 请移步这里：  
[http://ozzieliu.com/2016/02/09/gradient-descent-tutorial/](http://ozzieliu.com/2016/02/09/gradient-descent-tutorial/)
