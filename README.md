# Numpy中轴（Axis）的概念

> 作者：NathanHuXy
>
> 更新日期：2022.8.9

## 引言

Numpy最困难也是最重要的概念就是轴Axis。往往新手很难理解什么是轴，以及如何使用它。本文将对其进行介绍。



## 常见误解

新手往往将轴理解成“横向”和“纵向”，或者“行”和“列”。事实上，这样的理解并不完备，主要有两方面的严重缺陷：

1. 当数据维度增加后，无法用“行”和“列”简单理解。例如，图片数据包括了三个轴，分别是长、宽和颜色通道。再例如，视频数据包括了四个轴，分别是长、宽、颜色通道和时间；
2. 不同的函数对于轴的作用方向并不一样，至于是否是“横向”还是“纵向“取决于人的判断，并不客观，容易造成函数使用过程中的混淆；



因此，有必要明确——*轴Axis不可以理解成”横向“或者”纵向“*

## 轴的定义

类比二维平面，我们常说二维平面有两个轴，分别为X轴和Y轴。对于平面上的任何一个点，都可以用一个二维向量表示。例如，为了表示原点，使用$(0,0)$进行表示。类似地，为了选取`np.array`中的一个元素，需要使用列表作为索引，由外向内地进行选取，直到最后一层。

```python
import numpy as np
data = np.array([[1,2,3,4],[5,6,7,8]])
print("Out: {}".formt(data[1,2]))
# OUT: 7
```



在上面的例子中，索引为`[1,2]`。其中，第0轴为1，第1轴为2（从0开始编码），由于`data`只有2个维度，因此最多可以为2个轴赋值。



因此，我们给出轴的定义：当选取`array`中的元素是，从左到右的每个位置分别是第0轴，第1轴，以此类推。且轴数不超过其维度（`.ndim`）。



## 函数中的轴

在函数中经常使用关键字参数，如`axis=0`、`axis=1`。问题是，当我们向函数传入这一关键字参数，我们实际上传入了什么？



这一关键字表示函数作用的方向，其是沿着这一轴的坐标变化的方向。



例如，对于一个含有2个轴的数据`data`：

| $data(0,0)$ | $data(0,1)$ |
| :---------: | :---------: |
| $data(1,0)$ | $data(1,1)$ |

`axis=0`表示第一个轴变化的方向即从上到下，`axis=1`表示第二个轴变化的方向即从左到右。



再例如，对于一个含有4个轴的数据`data`，`axis=2`表示遍历$data_{i,j,k,w}$中的所有$k$的可能取值。下面看一个具体的例子。



首先，生成一个含有4个轴的数据结构：

```python
[In]:
import numpy as np

array = np.random.randint(0, 3, [3,2,4,5])
print(array)

[Out]:
[[[[1 0 0 2 1]
   [2 0 0 0 1]
   [2 0 1 2 2]
   [1 2 0 2 2]]

  [[2 2 0 1 0]
   [0 1 0 1 1]
   [2 2 1 0 1]
   [1 1 1 0 1]]]


 [[[1 1 2 1 2]
   [0 1 1 1 1]
   [1 0 1 0 2]
   [0 2 1 0 2]]

  [[1 1 2 2 1]
   [0 0 2 1 0]
   [1 2 2 0 2]
   [1 1 2 2 0]]]


 [[[1 0 2 2 2]
   [0 1 1 1 0]
   [2 2 2 0 0]
   [0 0 0 0 0]]

  [[2 2 1 1 2]
   [2 2 0 2 0]
   [0 0 1 1 2]
   [0 1 2 2 1]]]]
```

我们对变量`array`调用`np.sum()`函数，并且传入关键字参数`axis=2`，得到了：

```pyt
[In]:
print(np.sum(array,axis=2))

[Out]:
[[[6 2 1 6 6]
  [5 6 2 2 3]]

 [[2 4 5 2 7]
  [3 4 8 5 3]]

 [[3 3 5 3 2]
  [4 5 4 6 5]]]
```



成功了！



不出意外地，我们对原来结构为$[3,2,4,5]$再第2轴上调用了求和函数，得到的新的数据的结构为$[3,2,5]$。你可以理解成第2轴“坍塌”成了只需要1个数字就可以确定的了，那么也没必要对其所在的轴进行保留了。我们的数据从4维变成了3维。
