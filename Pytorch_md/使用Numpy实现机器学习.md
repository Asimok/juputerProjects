---
title: 使用Numpy实现机器学习
cover: https://gitee.com/Asimok/picgo/raw/master/img/night1/20201113165119.png
categories: PyTorch
tags:
  - PyTorch
  - 机器学习
  - Numpy
keywords: 'PyTorch,Numpy,机器学习'
---

表达式：$y=3x^2+2$

模型：$y=wx^2+b$

损失函数：$Loss=\frac{1}{2}\sum_{i=1}^{100}(wx^2_i+b-y_i)^2$

对损失函数求导：
$\frac{\partial Loss}{\partial w}=\sum_{i=1}^{100}(wx^2_i+b-y_i)^2x^2_i$

$\frac{\partial Loss}{\partial b}=\sum_{i=1}^{100}(wx^2_i+b-y_i)^2$

利用梯度下降法学习参数，学习率为:lr

$w_1-=lr*\frac{\partial Loss}{\partial w}$

$b_1-=lr*\frac{\partial Loss}{\partial b}$



```python
import numpy as np
from matplotlib import pyplot as plt
```

### 1.生成训练数据


```python

#设置随机种子，生成同一份数据
np.random.seed(100)
x = np.linspace(-1, 1, 100).reshape(100, 1)
# y在真实值上增加噪声
y = 3*np.power(x, 2)+2+0.2*np.random.rand(x.size).reshape(100, 1)
```

### 2.查看x,y分布


```python
plt.scatter(x, y)
plt.show()
```


![png](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201114102842.png)


### 3.初始化权重参数


```python
# 随即初始化参数
w1 = np.random.rand(1, 1)
b1 = np.random.rand(1, 1)
```

### 4.求解模型


```python
lr = 0.001
for i in range(800):
    # 前向传播
    y_pred = np.power(x, 2)*w1+b1
    # 定义损失函数
    loss = 0.5 * (y_pred-y)**2
    # print(loss)
    # 对各维度求和
    loss = loss.sum()
    # 计算梯度(求导)
    grad_w = np.sum((y_pred-y)*np.power(x, 2))
    grad_b = np.sum((y_pred-y))
    # 使用梯度下降法，使得loss最小
    w1 -= lr*grad_w
    b1 -= lr*grad_b
```

### 5.结果可视化


```python
plt.plot(x, y_pred, 'r-', label='predict')
plt.scatter(x, y, color='blue', marker='o', label='true')
plt.xlim(-1, 1)
plt.ylim(2, 6)
plt.legend()
plt.show()
# 预测值
print(w1, b1)
```


![png](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201114102850.png)


    [[2.98927619]] [[2.09818307]]

