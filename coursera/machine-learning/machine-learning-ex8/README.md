## Anomaly detection
**1**
根据每个特征，求出该特征所有实例的均值与方差  

**2**
建立概率函数，以及寻找恰当的参数，使得F1的值尽量的高，这里用到了recall与precision两个概念  

## Recommender Systems
需要注意的几个小细节：  
Y矩阵每行代表的是一个电影，每列代表的是一个用户，其中每个元素是用户对电影的评分。  
R矩阵是Y矩阵对应的二进制，用户对电影有评分，则对应1，否者对应0。  
X矩阵是电影的特征矩阵，每行代表的是一个电影，与Y矩阵的行数相等，列代表的是电影的特征。  
theta矩阵是用户的特征矩阵，行代表的是用户，与Y矩阵的列相等，列代表的是特征，与X矩阵的列相等。  
X * theta' 表示的是电影的评分。
**1**
代价函数  
```
predict = (X * Theta').* R;
J = 1 / 2 * sum(sum((predict - Y).^2)) + lambda/2*sum(sum(Theta .^ 2))+lambda/2*sum(sum(X .^ 2));
```

predict选出只用用户评价过的电影才会计算到代价函数中。  
代价函数需要求出评价过电影的方差。  
正则话需要同时对theta与X两个需要求的参数进行正则化。  
**2**
电影特征的梯度
```
for i = 1:num_movies
    idx = find(R(i,:) == 1);
    theta_temp = Theta(idx, :);
    y_temp = Y(i, idx);
    X_grad(i, :) = (X(i, :) * theta_temp' - y_temp) * theta_temp + lambda*X(i,:);
end
```
for循环的个数肯定是电影的数量。  
1. 找出这个评价过该电影的用户。  
2. 找出theta矩阵中对应给出电影评价的用户的参数。  
3. 找出用对该电影的用户评分。  
4. 通过对代价函数的导数，计算出电影特征对应的梯度。 (X(i, :) * theta_temp' - y_temp)这是该对电影的评分。* theta_temp是求导后的结果。  

**3**
用户特征梯度
```
for i = 1:num_users
    idx = find(R(:, i) == 1);
    x_temp = X(idx, :);
    y_temp = Y(idx, i);
    Theta_grad(i , :) = (x_temp * Theta(i, :)' - y_temp)' * x_temp + lambda * Theta(i, :);
end

```
for循环是用户的个数。  
1. 找出该用户评价过的电影。  
2. 找出用户评价过的电影的特征向量。  
3. 找出对应电影的评分。  
4. 根据求导的结果计算出梯度。  
**3**
若是要预测的话，需要从新把一个用户的评价加入评价矩阵，之后再从新训练出新的theta与X计算出该用户对所有电影的评分，之后在对电影的评分进行排序，进行相应的推荐。  

**1**

**1**
