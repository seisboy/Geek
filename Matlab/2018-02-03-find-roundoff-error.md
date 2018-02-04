---
title: 解决使用find函数时遇到的舍入误差问题
date: 2018-02-03
author: seisboy
categories:
  - Matlab
tags:
  - Roundoff
  - find()
slug: find-floating-point-roundoff-error
---

为了找到数值对应的下标，通常会用到`find`函数。如果矩阵元素为整型，可以放心地使用；如果矩阵元素为浮点型，可能会产生由舍入误差导致的问题。

- 整型
首先我们测试一下整型的情况：
``` matlab
>> x=[1,10,2,7,12,9];
>> ind=find(x==12);
>> ind
ind =
     5
```
12在向量`x`中的下标为`5`，这是完全OK的！

- 浮点型
接下来测试一下浮点型会发生什么问题：
``` matlab
>> x=0:0.1:1;
>> ind=find(x==0.6)
ind =
     7
>> ind=find(x==0.3)
ind =
  1×0 empty double row vector
```
寻找`0.6`在向量`x`中的位置时非常顺利；寻找0.3在向量`x`中的位置时出现问题，此时输出的`ind`是一个1×0的空的行向量。该问题是由Matlab中浮点型数据的舍入误差引起的，即浮点型数据并不完全等同于我们看到的的数值，可能存在小数点后N位的扰动，从而导致在Matlab中判断两个浮点型数据是否相等时可能出现上述问题。

- 解决方案
```
>> x=0:0.1:1;
>> ind=find(abs(x-0.3)<0.000001)
ind =
     4
```

---
## 参考材料
- [Mathworks Documentation](http://cn.mathworks.com/help/matlab/ref/find.html)

## 修订历史
- 2018-02-03：初稿；
