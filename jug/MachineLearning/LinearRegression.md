# Linear Regression

[toc]



## Overview

里捏, 哥rai身, Linear Regression, 线性回归, 线性衰退

统计学和机器学习中最基础算法, 

### definition of linear regression

线性回归的定义: 

线性回归是统计学和机器学习中最基础算法, 我们就是在通过这个方法, 来讨论能否使用一条直线描述出一个事物的大致分布, 再用得到的这个分布去进一步估算未知事物的可能结果. 

通过已有站队, 找到大致分布是一个什么情况, 从而再用这个已知分布去估算新同学应该有的站位.



Linear Regression

y = ax + b

a: slope, 斜率

b: intercept, 截距

用数学公式表达一条直线, 这里我们可以通过一组合适的 a 和 b 去描述 x 和 y 之间的对应关系. 线性回归就是在这个公式中做文章. 

1. 已知一组对应关系, 

   x = [2, 3]; y = [5, 7]

   得到: 

   5 = 2a + b

   7 = 3a + b

   得到: (找到a, b)

   a = 2

   b = 1

   得到: 

   NEW x = 4

   NEW y = 2x4 + 1 = 9

2. 通过找到最合适的 a 和 b 来描述一组对应的 x 和 y 之间的分布

3. 然后再用求得得分布来描述未知事物的可能解

4. how to evaluate a linear regression? 如何评估线性回归的训练过程?

   在数学中我们会定义一个距离公式, 也就是每个点距离理想直线的误差到底有多大. 如果每个位置都很好, 那么这个距离加起来就会很低. 这个公式在机器学习里称为 loss function 或者 objective function

   L = sum(Y true - Y pred)^                     			loss function, 具体 Sum Square Residual, 平方残差和

   dis = Y true - Y pred = 4.6 - 4.5 = 0.1

   dis = Y true - Y pred = 3.4 - 3.5 = -0.1

   L = (-0.1)^ + (0.1)^ = 0.02									the smaller the better!, 越小越好

5. why we use linear regression? 为什么用线性回归?

   1. 通过 公式 我们描述了一组对应关系 X, Y
   2. 为了准确表达这组对应关系 X, Y. 我们引入了一些参数 a 和 b, 那么问题的本质就可以描述为只要我们构建了相对应的关系, 我们都可以用线性回归去找到这组对应关系的描述. 也就是解分布.
   3. 最后再反过来用求得得解和分布来评估未知数据的结果.

   ⚠️ X, Y 是一对儿对应关系; a, b 是这对儿对应关系的描述

6. any limitations? 任何限制? 缺点?

   不具备求解一个非线性分布的能力. 也就是说, 如果你的数据不是简单的分布在一条直线的周围, 那么你用线性回归求解的分布也会误差很大, 从而对未知测试数据的评估结果也是不准的

   L is very larger

   linear regression could not handle this case!!

### summary

⚠️ X, Y 是一对儿对应关系; a, b 是这对儿对应关系的描述

⚠️ 线性回归不能解决非线性分布问题!



### actually

事实上, 你可以构建更复杂的 Linear Regression 包含更多的属性X (也叫特征)

Y = a1x1 + a2x2 + ... + adxd + B

例如: 一个预测股价 Y 的任务, 属性 X 可能包含[时间, 股票类别, 上市时间, 等等], 在这种情况下, 所得到的 X, Y 对应关系将不再是一条直线, 而会是更复杂的超平面.



### Hyperplane, 超平面

In geometry a hyperplane is a subspace of one dimension less than its ambient space.

在几何中，超平面指的是比所处空间少一个维度的子空间。

百度百科的定义: 超平面是n维欧氏空间中余维度等于一的线性子空间，也就是必须是(n-1)维度.

用人话讲，超平面就是对某一维度空间（比如我们生活的三维空间）而言，更低一维度的子空间（比如面前的一个二维平面），这个超平面将空间分为两部分。

0维的点可以将1维的线分为两部分
1维的线可以将2维的面分为两部分
2维的面可以将3维的体分为两部分







## FAQ





## See Also

linear regression, 线性回归

y = ax + b

a: slope, 斜率

b: intercept, 截距

known samples, 已知样本

unknown samples, 未知样本

loss function, 损失(目标)函数

Sum Square Residual, 平方残差和

Hyperplane, [超平面](https://zhuanlan.zhihu.com/p/263604941)
