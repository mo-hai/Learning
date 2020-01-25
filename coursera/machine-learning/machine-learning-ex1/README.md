## ex1  
**1**  
在warmUpExercise函数中生成一个对角线元素为1的5*5矩阵：  
A = eye(5);  

**2**  
在plotData.m中完成绘制出数据。  
figure 生成一个画布。
plot(x轴元素,y轴元素,color and shape,'MarkerSize', 10 )：  
plot(x,y,'rx','MarkerSize', 10 )  
xlabel， ylabel 坐标轴名称  
**3**  
computeCost.m中完成对代价函数的计算。  
J = sum((X * theta - y).^2) / (2 * m); 使用向量化的方法可以更块的计算出结果。  
**4**  
gradientDescent.m计算梯度下降  
theta = theta - alpha / m * (X' * (X * theta - y)) ;  在每一轮的迭代中同时跟新theta的值。


## ex1_multi
**1**  
featureNormalize.m中完成归一化，其中mean计算出矩阵列的均值，std据算出矩阵列的标准差。

**2**  
computeCost.m中完成对代价函数的计算。  
J = sum((X * theta - y).^2) / (2 * m); 使用向量化的方法可以更块的计算出结果。  
**3**  
gradientDescent.m计算梯度下降  
theta = theta - alpha / m * (X' * (X * theta - y)) ;  在每一轮的迭代中同时跟新theta的值。

