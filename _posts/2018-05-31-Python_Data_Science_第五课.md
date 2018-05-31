conditional probability and Bayes' Theorem
这两个理论看的我头晕。

1. conditional probability  
if I have two events that depend on each other, what's the probability that both will occur.  
Notation:  
P(A,B) is the probability of A and B both occuring independent of each other.
P(B|A): probability of B given A has already occurred.

    we know:
    P(B|A) = P(A,B) / P(A)
    
    Example:
    I give my students two tests, 60% of my students passed both tests, but the first
    test was easier, -80% passwd that one. what percentage of students who passed
    the first test also passed the second ?
    A= passing the first
    B= passing the second
    so we are asking for P(B|A) - probability of B given A
    
    
    P(B|A) = P(A,B) / P(A) = 0.6 / 0.8 = 0.75  
    75% of (students who passed the first test) also passed the second.
    
    Question ,What about P(A|B), probability of A given B has already occurred.
    we calculate the students passed the second test also passed the first test.
    P(A|B) is NOT equal to P(B|A)
    

2.  Bayes' Theorem  
    Now that you understand conditional probability, you can understand Bayes' Theorem:

    P(A|B) = P(A)P(B|A) / P(B)   
    In english: The probability of A given B, is the Probability of A times the probability of B given A over the probability of B.
    
    The key insight is that the probability of something that depends on B depends very much on the base  probability of B and A.
    
    Drug testing is a common example. Even a "highly accurate" drug test can produce more false positives than true positives.  
    Let's say we have a drug test that can accurately identify users of a drug 99% of the time  
    and accurately has a negative result for 99% of non-users. But only 0.3% of the overall population actually uses this drug.   
      
    A= is a user of the drug  
    B = test positively for the drug
    
    P(A)=0.3% = 0.003  
    We can work out from that information that P(B) is 1.3% ( 0.99 * 0.03 + 0.01 * 0.997 )   
    The probability of testing positive if you do use, plus the probability of testing positive if you don't .
    
    P(A|B)= P(A) * P(B|A) / P(B) = 0.003 * 0.99 / 0.013 = 22.8%
    
    So the odds of someone being an actual user of the drug given that they test positive is only 22.8%.  
    Even though P(B|A) is 99%, P(A|B) is only 22.8%.
    

3. 看完Bayes' Theorem,  对很多所谓的统计数字能有些新的认识。
    参考web page .  
    [https://betterexplained.com/articles/an-intuitive-and-short-explanation-of-bayes-theorem/](URL)

    里面的例子是cancer test.
    %1的人有癌症， 99%没有  
    80%有癌症的人能得到正确的结果，20%人即使有cancer 可能也会miss.  
    没有癌症的人9.6%的几率会被误诊为癌症， 90.4%的概率会得到自己没有癌症的诊断。  
    
    如果一个人诊断结果是癌症，那么他真的有癌症的几率是多少呢？  
    our chance of cancer is .008/.10304 = 0.0776, or about 7.8%.  
    