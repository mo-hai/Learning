##Neural Networks Learning
**1**
Feedforward and cost function  
首先是完成前向传播算法，计算出h，由于h是向量形式，y是标量的形式，需要将y变成与h同形的矩阵，再进行函数的计算。  
```
y_temp = zeros(m, num_labels);

for row = 1:size(y,1)
    y_temp(row,y(row)) = 1;
end

%转换成同型矩阵

%Feedforward 
X_temp = [ones(m,1) X];
a1 = Theta1 * X_temp';
z1 =[ones(1,size(a1,2)) ; sigmoid(a1)];

a2 = Theta2 * z1;
h = sigmoid(a2);


J = -1 / m * sum(sum(y_temp .* log(h') + (1 - y_temp).*log(1-h')));
%计算没有正则话的代价函数
r = lambda / (2 * m) * (sum(sum(Theta1(:,2:end).^2)) + sum(sum(Theta2(:, 2:end).^2)));
%计算正则化带来的影响
J = J + r;
```

**2**
Backpropagation  
sigmoid的导数为：g = sigmoid(z) .* (1 - sigmoid(z));  
初始化theta矩阵  
```
epsilon_init = 0.12;
W = rand(L_out, 1 + L_in) * 2 * epsilon_init − epsilon_init;
```
其中epsilon_init的值被证明为取sqrt(6)/(sqrt(L_in + L_out))会比较好。  


**3**
梯度下降  
1. 计算结果的误差：error_3 = h - y_temp';  
2. 根据之前的theta矩阵计算每一层均摊的误差  
3. 根据误差公式，计算出每个theta的梯度，其中向量化是难点。


