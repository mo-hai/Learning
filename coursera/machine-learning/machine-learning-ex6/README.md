## Support Vector Machines
此处f表示为选择不同的核函数（kernel）对应的特征参数。  
对于参数C（类似于正则化线性回归中的参数1/lambda）：  
当参数C很大时，相当于lambda很小，其正则化效果较差，拟合曲线趋于过拟合，其对样本的分类正确率非常高，处于高方差，低偏差状态；  
当参数C很小时，相当于lambda很大，其正则化效果很好，拟合曲线趋于欠拟合，处于高偏差，低方差状态。  

**1**  
实现核函数：  
```
sim = exp((x1 - x2)' * (x1 - x2) / (-2 * sigma^2));

```
上面是高斯核函数。  


**2**  
找到最好的高斯核参数sigma与支持支持向量机参数C，主要是使用遍历的思想，训练出新的参数，然后通过交叉验证集验证错误率。
```
test_val = [0.01; 0.03; 0.1; 0.3; 1; 3; 10; 30; 100; 300; 1000];
predictionErr = [];
for i = 1:length(test_val)
    for j = 1:length(test_val)
        C = test_val(i);
        sigma = test_val(j);
        model = svmTrain(X, y, C, @(x1, x2)gaussianKernel(x1, x2, sigma));
        predictions = svmPredict(model, Xval);
        predictionErr =[predictionErr mean(double(predictions ~= yval))];
    end
end
[M,I] = min(predictionErr);
j = rem(I,length(test_val));
i = (I-j)/length(test_val)+1;
C = test_val(i);
sigma = test_val(j);
```
其中mean函数为求均值的函数，这里由于数据只有0~1所以很自然的转变成了求误差。



##Spam Classification
主要是对邮件进行分类。

**1**  
邮件预处理：  
1. ower-casing:将整个邮件内容全部转换成小写；  
2. Stripping HTML:将邮件内容中HTML格式标记内容去除；  
3. Normalizing URLs:将邮件中链接转换成字符“httpaddr”；  
4. Normalizing Email Address:将邮件中含邮箱地主的内容转换成字符"emailaddr"；  
5. Normailzing Dollars:将邮件中美元符号“$”转换成字符"dollar"；  
6. Normailzing Numbers:将邮件中所有的数字转换成字符"number"；  
7. Word Stemming:将邮件中所有的单词转换成词根形式；  
8. Removal of non-words:去除非字符和标点符号。  
```
例如：
> Anyone knows how much it costs to host a web portal ?
>
Well, it depends on how many visitors you're expecting.
This can be anywhere from less than 10 bucks a month to a couple of $100. 
You should checkout http://www.rackspace.com/ or perhaps Amazon EC2 
if youre running something big..

To unsubscribe yourself from this mailing list, send an email to:
groupname-unsubscribe@egroups.com

修改后：
anyon know how much it cost to host a web portal well it depend on how mani 
visitor you re expect thi can be anywher from less than number buck a month 
to a coupl of dollarnumb you should checkout httpaddr or perhap amazon ecnumb 
if your run someth big to unsubscrib yourself from thi mail list send an 
email to emailaddr 

```

**2**  

从垃圾邮件中已经挑出了至少出现100次的词汇列表，共计1899个词汇，存储在vocab.txt中，将单词影射成自然数，方便操作。  
```
    for i=1:numel(vocabList)
        if strcmp(str, vocabList{i})==1
            word_indices=[word_indices;i]
        end
    end
```

**3**  
提取邮件的特征参数：  
并将需要输入的邮件的所有单词与词汇表中的单词进行对照，完成词汇向量。
```
for i =1:numel(word_indices)
    x(word_indices(i),1) = 1;
end
```


