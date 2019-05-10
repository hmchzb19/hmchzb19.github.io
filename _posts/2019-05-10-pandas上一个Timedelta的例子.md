该例子出自pandas-for-everyone一书.
使用了如下的csv文件.  
````
    In [80]: !cat ./gapminder/other_csv/scientists.csv
    Name,Born,Died,Age,Occupation
    Rosaline Franklin,1920-07-25,1958-04-16,37,Chemist
    William Gosset,1876-06-13,1937-10-16,61,Statistician
    Florence Nightingale,1820-05-12,1910-08-13,90,Nurse
    Marie Curie,1867-11-07,1934-07-04,66,Chemist
    Rachel Carson,1907-05-27,1964-04-14,56,Biologist
    John Snow,1813-03-15,1858-06-16,45,Physician
    Alan Turing,1912-06-23,1954-06-07,41,Computer Scientist
    Johann Gauss,1777-04-30,1855-02-23,77,Mathematician
````
读入csv文件,将其中的一列赋值给ages,然后shuffle这一列,会发现ages会跟着变化，所以ages只是指向这一列的指针而已  
````
    In [81]: import numpy as np

    In [82]: import pandas as pd

    In [83]: import matplotlib.pyplot as plt

    In [84]: scientists=pd.read_csv("./gapminder/other_csv/scientists.csv")

    In [85]: scientists.shape
    Out[85]: (8, 5)

    In [86]: scientists.columns
    Out[86]: Index(['Name', 'Born', 'Died', 'Age', 'Occupation'], dtype='object')

    In [87]: scientists.dtypes
    Out[87]:
    Name object
    Born object
    Died object
    Age int64
    Occupation object
    dtype: object

    In [88]: ages=scientists['Age']

    In [89]: ages
    Out[89]:
    0 37
    1 61
    2 90
    3 66
    4 56
    5 45
    6 41
    7 77
    Name: Age, dtype: int64

    In [90]: import random

    In [91]: random.seed(42)

    In [92]: random.shuffle(scientists['Age'])
    /usr/lib/python3.7/random.py:278: SettingWithCopyWarning:
    A value is trying to be set on a copy of a slice from a DataFrame

    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      x[i], x[j] = x[j], x[i] 
````
然后根据born和died两列创造出新的两列,　类型为datetime64  
````
    In [93]: born_datetime=pd.to_datetime(scientists['Born'], format="%Y-%m-%d")

    In [94]: died_datetime=pd.to_datetime(scientists['Died'], format='%Y-%m-%d')

    In [95]: scientists['Born_dt'], scientists['Died_dt']=(born_datetime, died_datetime)

    In [96]: scientists.dtypes
    Out[96]:
    Name object
    Born object
    Died object
    Age int64
    Occupation object
    Born_dt datetime64[ns]
    Died_dt datetime64[ns]
    dtype: object

    In [97]: scientists.shape
    Out[97]: (8, 7) 
````
最后使用得到的两列datetime64的类型做减法，得到timedelta64数据类型，然后将这个类型转化为int.  
````
    #下面两种方法都可以
    scientists['age_years_dt']=scientists['age_days_dt'].astype(pd.Timedelta).apply(lambda l: l.days //365)
    scientists['age_years_dt']=scientists['age_days_dt'].astype('timedelta64[D]').astype(int) // 365
    In [102]: scientists.dtypes
    Out[102]:
    Name                     object
    Born                     object
    Died                     object
    Age                       int64
    Occupation               object
    Born_dt          datetime64[ns]
    Died_dt          datetime64[ns]
    age_days_dt     timedelta64[ns]
    age_years_dt              int64
    dtype: object

    In [103]: scientists['age_years_dt']
    Out[103]:
    0    37
    1    61
    2    90
    3    66
    4    56
    5    45
    6    41
    7    77
    Name: age_years_dt, dtype: int64
````
如果跟ages做比较会发现不等.  
````
    In [106]: scientists['age_years_dt'].equals(ages)
    Out[106]: False

    In [107]: type(ages)
    Out[107]: pandas.core.series.Series

    In [108]: ages
    Out[108]:
    0 66
    1 56
    2 41
    3 77
    4 90
    5 45
    6 37
    7 61
    Name: Age, dtype: int64

    In [109]: scientists['age_years_dt']
    Out[109]:
    0 37
    1 61
    2 90
    3 66
    4 56
    5 45
    6 41
    7 77
    Name: age_years_dt, dtype: int64

    In [110]: scientists['age_years_dt']==ages
    Out[110]:
    0 False
    1 False
    2 False
    3 False
    4 False
    5 True
    6 False
    7 False
    dtype: bool
````
