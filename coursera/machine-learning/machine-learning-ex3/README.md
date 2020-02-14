## Vectorizing Logistic Regression
**1**
将代价函数向量化  
J = 1/m * (-y' * log(sigmoid(X * theta)) - (1-y)' * log(1 - sigmoid(X * theta))) + lambda / (2 * m) * sum(theta(2:end).^2);  
**2**
将梯度向量话，注意的是grad(1)需要另外计算  
grad(1) = 1/m * (X(:,1)' * (sigmoid(X * theta) - y));  
grad(2:end) = 1/m * (X(:,2:end)' * (sigmoid(X * theta) - y)) + lambda / m * theta(2:end);  
**3**
分别对每个分类的情况进行训练，使用for循环  
```

for c = 1 : num_labels
    initial_theta = zeros(n+1, 1);
    options = optimset('GradObj', 'on', 'MaxIter' ,100);
    [theta] =  fmincg(@(t)lrCostFunction(t, X,(y==c),lambda), initial_theta, options);
    all_theta(c,:) = theta;
end

```
**4**
预测，all_theta是一个矩阵，矩阵的每一行分别保存一个分类的参数情况，使用数据特征分别乘以每一个特征向量，分别得到每个分类的概率，取概率最大的那个数。  
[val, pos]= max(all_theta * X');  


## Neural Networks
在这里只需要实现一个前向网络就可以了。  
需要注意的是theta矩阵的行数代表的是下一层的单元数。列数是上一层的单元数
```
x_temp = [ones(m,1) X];


layer1 = sigmoid(Theta1 * x_temp');
layer1 = [ones(1,size(layer1,2));layer1];

layer2 = sigmoid(Theta2 * layer1);

[val, pos] = max(layer2);
```

