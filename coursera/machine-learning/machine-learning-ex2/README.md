## ex2
**1**  
使用plot函数画图，主要是一些matlab的语法问题：  
```
pos = find(y==1); neg = find(y == 0);
% Plot Examples
plot(X(pos, 1), X(pos, 2), 'k+','LineWidth', 2, ...
'MarkerSize', 7);
plot(X(neg, 1), X(neg, 2), 'ko', 'MarkerFaceColor', 'y', ...
'MarkerSize', 7);
```

**2**  
实现sigmoid函数，难点是如果输入的是矩阵的话，需要对矩阵中的每一个数进行计算，这里使用的是一个"./"进行这样的操作具体为：  
```
1./(1 + exp(-z));
```

**3**
Cost function and gradient  
使用矩阵乘法，而不是一个一个数的进乘法运算  
```
J = 1/m * (-y' * log(sigmoid(X * theta)) - (1 - y)' * log(1 - sigmoid(X * theta)));
grad = 1/m * X' * (sigmoid(X * theta) - y);

```
在计算得到theta之后对值进行预测需要一个round函数进行四舍五入  
**4**
调用高级优化函数  
```matlab
options = optimset('GradObj', 'on', 'MaxIter', 400);
% 'GradObj', 'on' 为一组表示同时返回theta与cost，允许使用梯度下降的算法来最小化cost
% 'MaxIter', 400  为一组，表示的是最大的循环次数为400
[theta, cost] = fminunc(@(t)(costFunction(t, X, y)), initial theta, options);

% @(t)是一个别名而已
```

##ex2 reg
**1**  
Feature mapping，主要是为了构造更多的特征以及使用高阶项  
**2**  
计算cost function 以及gradient  
```
J = 1/m * (-y' * log(sigmoid(X * theta)) - (1-y)' * log(1 - sigmoid(X * theta))) + lambda / (2 * m) * sum(theta(2:end).^2);
grad(1) = 1/m * X(:,1)' * (sigmoid(X * theta) - y);
grad(2:end) = 1/m * (X(:,2:end)' * (sigmoid(X * theta) - y)) + lambda / m * (theta(2:end));
```
计算cost function时需要加上一个惩罚项目，注意的是不要对theta1惩罚  
计算梯度的时候，theta1 的梯度也需要单独进行计算，其他的梯度可以直接一起进行计算。  

