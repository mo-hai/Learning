## K-means Clustering

**1**
初始化出K个中心，然后遍历所有点，并把每个点分配给最接近的中心。  
```
m = size(X,1);
r = [];
for i =1:m
    for j = 1:K
       r(j) = (X(i,:) - centroids(j,:)) * (X(i,:) - centroids(j,:))';
    end
    [M,I] = min(r);
    idx(i) = I;
end
```


**2**
分配好点之后在重新计算中新，新的中心为所有点的均值。
```
for i = 1:K
    temp = find(idx == i);
    centroids(i,:) = sum(X(temp, :))/ size(X(temp,:), 1);
end
```


**3**
对图片进行像素的分类，将每个像素当成一个数据，然后进行K-means进行聚类，在对聚类后的点进行像素对换。  


## Principal Component Analysis
主成分分析法。  
主要作用是对数据进行降维。与数据压缩。  
**1**
求出sigma矩阵sigma = 1/m * X' * X;  
sigma矩阵是一个n*n的方阵，n为特征的个数。  


**2**
对sigma矩阵进行奇异值分解。  
[U, S, V] = svd(sigma);  
其中U是用于降维的矩阵。  


**3**
降维：  
取矩阵U的前K列将原矩阵X降为K维度
```
U_reduce = U(:, 1:K);
Z = X * U_reduce;
```

**4**
数据恢复，将原来降维的数据中K维重新变为n维度。  
```
X_rec = Z * U(:, 1:K)';
```


**5** 
找出降维后的数据与原来数据之间的差距  
求S矩阵对角线元素的前K个值与所有值的和的商。  

