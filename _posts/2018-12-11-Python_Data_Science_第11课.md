K-means clustering

1. K-Means clustering is unsupervised learning. means split data into K groups,
algorithm for k-means clustering:  
  + Randomly pick K centroids(k-means)
  + Assign each data point to the centroid it is closest to.
  + Recompute the centroids based on the average position of each centroid's points.
  + Iterate until points stop changing assignment to centroids.
  + Predict the cluster for new points.

The limitation of K-means clustering:  
  * Choosing K:  choose the right value of K, the principal way of choosing k is just start low and keep increasing the value of K depending on how many groups you want.
  * Avoiding local minima
  * Labeling the clusters. 

2. 实际例子，可以使用sklearn里面的KMeans函数。  
这个例子制造了一些人收入和年龄的数字，然后进行cluster.  

        In [1]: from numpy import random , array 

        In [2]: def createClusteredData(N, k):
           ...:     random.seed(10)
           ...:     pointsPerCluster = float(N) / k 
           ...:     x = []
           ...:     for i in range(k):
           ...:         incomeCentroid = random.uniform(20000.0, 200000.0)
           ...:         ageCentroid = random.uniform(20.0, 70.0)
           ...:         for j in range(int(pointsPerCluster)):
           ...:             x.append([random.normal(incomeCentroid, 10000.0), random.nor
           ...: mal(ageCentroid, 2.0)])
           ...:     x = array(x)
           ...:     return x
           ...: 

        In [3]: from sklearn.cluster import KMeans

        In [4]: import matplotlib.pyplot as plt

        In [5]: from sklearn.preprocessing import scale

        In [6]: from numpy import random, float

        In [7]: data = createClusteredData(100, 5)

        In [8]: model = KMeans(n_clusters=5)

        In [9]: model = model.fit(scale(data))

        In [10]: print(model.labels_)
        [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
         1 1 1 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 2 3 3 3 3 3 3 3 3 3 3 3 3 3 3
         3 3 3 3 3 3 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2]

        In [11]: plt.figure(figsize=(8, 6))
        Out[11]: <matplotlib.figure.Figure at 0x7f6377123a58>

        In [13]: plt.scatter(data[:, 0], data[:, 1], c=model.labels_.astype(float))
        Out[13]: <matplotlib.collections.PathCollection at 0x7f6375a1a908>

        In [14]: plt.show()


