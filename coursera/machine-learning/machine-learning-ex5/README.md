## Regularized Linear Regression and Bias v.s.Variance
**1**  
实现逻辑回归并正则化  
```
J = 1/(2 * m) * sum((X * theta - y).^2) +  lambda / (2 * m) * sum(theta(2:end).^2);


grad(1) = 1 / m * (X(:,1)' * (X * theta - y));
grad(2:end) = 1 / m * (X(:,2:end)' * (X * theta - y)) + lambda / m * (theta(2:end));
```

**2**  
使用不同数量的训练数据，可视化偏差
```
for i = 1:m
    theta = trainLinearReg(X(1:i,:),y(1:i),lambda);
    error_train(i)=linearRegCostFunction(X(1:i,:),y(1:i),theta,0);
    error_val(i)=linearRegCostFunction(Xval,yval,theta,0);
end
```

**3**  
使用多项式回归，主要是实现特征的多样化  
```
for i = 1:p
    X_poly(:,i) = X.^i;
end

```

**4**  
使用不同的正则化参数，看那个分别看训练集与测试集的误差  
```
for i = 1:length(lambda_vec)
  lambda = lambda_vec(i);
  theta=trainLinearReg(X, y, lambda);
  error_train(i)=linearRegCostFunction(X,y,theta,0);
  error_val(i)=linearRegCostFunction(Xval,yval,theta,0);
end;
```
