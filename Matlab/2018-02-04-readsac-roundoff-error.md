---
title: 使用readsac函数时出现的舍入误差问题
date: 2018-02-04
author: seisboy
categories:
  - Matlab
tags:
  - Roundoff
  - Readsac()
slug: readsac-roundoff-error
---

通过Matlab处理地震数据时，`readsac`和`getsacdata`是经常用到的两个函数。使用这两个函数时需要格外小心Matlab浮点型数据的舍入误差问题。

## Demo
``` Matlab
>> R=readsac('Test.SAC.R');
>> [t,am]=getsacdata(R);
>> R.DELTA
ans =
    0.0100
```
此时我们预期的`t`矩阵应该是这样的：
```
>> t
ans =
      0.0000
      0.0100
      0.0200
       ...
     99.9900
    100.0000
```
如果不幸遇到舍入误差的问题，则`t`矩阵可能是这样的：
```
>> t
ans =
     0.0000
     0.0100
     0.0200
      ...
     49.4600
     49.4700
     49.4799
     49.4899
      ...
     99.9899
     99.9999
>> find(t==49.48)
ans =
  1×0 empty double row vector
```
让人郁闷的是，舍入误差这一问题的出现仿佛没有什么规律。

## 解决方案
1. 对数据进行四舍五入，根据数据的采样间隔保留到小数点后N位；
2. 参考"[解决使用find函数时的舍入误差问题](./2018-02-03-find-roundoff-error.md)"这篇文章中的做法。

---
## 参考资料
- [SAC_Docs_v3.6.pdf](http://seisman.info/sac-manual.html)

## 修订历史
- 2018-02-04：初稿；
