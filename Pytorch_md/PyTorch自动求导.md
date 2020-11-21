---
title: PyTorch自动求导
cover: https://gitee.com/Asimok/picgo/raw/master/img/night1/20201113165119.png
categories: PyTorch
tags:
  - PyTorch
keywords: 'PyTorch,自动求导'
---

# 标量反向传播

> 当目标张量为标量时，backward()无需传入参数。

- 例子：假设$w,x,b$都是标量，$z=wx+b$ ，对标量$z$调用backward()方法。

## 自动求导的主要步骤


```python
import torch
```

### 1.定义叶子结点，算子节点

> 如果需要对Tensor求导，requires_grad要设置为True。


```python
# 定义输入张量x
x = torch.Tensor([2])
# 初始化权重参数w,偏置b,#设置requires_grad为True，使用自动求导
w = torch.randn(1,requires_grad=True)
b = torch.randn(1,requires_grad=True)
# 设置前向传播
y = torch.mul(w,x)
z = torch.add(y,b)
# 查看requires_grad属性
print(x.requires_grad)
print(y.requires_grad)
# 因为与w,b具有y依赖关系，所以x,y的requires_grad也是True。
```

    False
    True


### 2.查看叶子结点，非叶子结点的其他属性
- grad_fn:表示梯度函数
> 通过运算创建的Tensor(非叶子结点)会自动被赋予grad_fn属性。<br>叶子结点的grad_fn为None。


```python
# 查看非叶子结点y,z的requires_grad属性。
print(y.requires_grad)
# 查看各节点是不是叶子节点
print(x.is_leaf)
print(y.is_leaf)
# 叶子结点：x,w,b
# 非叶子结点：y,z
# 查看叶子结点的grad_fn属性
print("x的grad_fn属性：",x.grad_fn)
# 查看非叶子结点的grad_fn属性
print("y的grad_fn属性：",y.grad_fn)
```

    True
    True
    False
    x的grad_fn属性： None
    y的grad_fn属性： <MulBackward0 object at 0x7fe83935dbb0>


### 3.自动求导，实现梯度反向传播
> 非叶子节点的梯度调用backward()之后，梯度将被清空。


```python
# 基于z对张量进行反向传播，执行backward之后计算图会清空。
z.backward()
# 如果需要多次backward()需要设置参数retain_graph为True。此时梯度是累加的。
# z.backward(retain_graph=True)

# 查看叶子结点的梯度。
# 因为x未设置requires_grad属性,默认为False，不求导，所以grad为None。
print("x的梯度是：",x.grad)
print("w的梯度是：",w.grad)

# 查看非叶子结点的梯度
# 非叶子节点的梯度调用backward()之后，梯度将被清空。故y,z此时没有梯度。
# print("y的梯度是：",y.grad)
# print("z的梯度是：",z.grad)

```

    x的梯度是： None
    w的梯度是： tensor([2.])


# 非标量反向传播
> Pytorch只允许标量对张量进行求导

## 步骤

### 1.定义叶子结点，计算结点


```python
import torch
```


```python
# 定义叶子张量x，形状为1x2
x = torch.tensor([[2,3]],dtype=torch.float,requires_grad=True)
print(x)
# 初始化雅可比矩阵
J = torch.zeros(2,2)
print(J[0])
# 初始化目标张量，形状为1x2
y = torch.zeros(1,2)
# 定义y与x之间的映射关系
# y1 = x1**2+3*x2
# y2 = x2**2+2*x1
y[0,0]=x[0,0]**2+3*x[0,1]
y[0,1]=x[0,1]**2+2*x[0,0]
print(y)
```

    tensor([[2., 3.]], requires_grad=True)
    tensor([0., 0.])
    tensor([[13., 13.]], grad_fn=<CopySlices>)


### 2.调用backward()获取y对x的梯度
> 需要重复使用backward()时,retain_graph=True


```python
# 生成y1对x的梯度
y.backward(torch.Tensor([[1,0]]),retain_graph=True)
J[0]=x.grad
# 因为梯度是累加的，所以需要清除对x的梯度
x.grad = torch.zeros_like(x.grad)
# 生成y2对x的梯度
y.backward(torch.Tensor([[0,1]]))
J[1]=x.grad
# 雅可比矩阵的值
print(J)
```

    tensor([[4., 3.],
            [2., 6.]])

