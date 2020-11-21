---
title: 用tensorboardX可视化神经网络
cover: https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201115115708.png
categories: PyTorch
tags:
  - PyTorch
  - 工具
keywords: 'PyTorch,tensorboardX,可视化'
---



# 用tensorboardX可视化神经网络

安装：`pip install tensorboardX`


```python
from tensorboardX import SummaryWriter
writer = SummaryWriter('runs/scalar_example')
for i in range(10):
    writer.add_scalar('quadratic', i**2, global_step=i)
    writer.add_scalar('exponential', 2**i, global_step=i)
```


```python
writer = SummaryWriter('runs/another_scalar_example')
for i in range(10):
    writer.add_scalar('quadratic', i**3, global_step=i)
    writer.add_scalar('exponential', 3**i, global_step=i)

```

## 1.启动tensorboard服务

- cd到runs目录所在的同级目录，在命令行输入如下命令，logdir等式右边可以是相对路径或绝对路径。
``` tensorboard --logdir=runs --port 6006 ```

## 2.web展示
- http://localhost:6006/

![image-20201115115702543](https://gitee.com/Asimok/picgo/raw/master/img/MacBookPro/20201115115708.png)