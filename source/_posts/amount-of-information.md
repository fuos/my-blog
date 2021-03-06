---
title: 信息基础和计算方式
categories: 理论
tags:
  - 信息
  - 熵
abbrlink: c013043
date: 2020-06-28 15:59:25
---

### 相关概念

#### 熵

当一件事情有多种可能情况时，这件事情对某人而言具体是哪种情况的不确定性叫做**熵**。

#### 信息

能够消除该人对这件事情不确定性的事物叫做**信息**。信息是从多个可能状态中确定实际状态所需的**物理量**。

#### 两者关系

熵和信息的关系：数量相等，意义相反。获取信息 = 消除熵。

#### 噪音 和 数据

那些不能够消除某人对某件事情不确定性的事物被称为数据或噪音。噪音是干扰某人获取信息的事物，数据是噪音和信息的混合，需要用知识将其分离。

#### 概率 和 熵 的区别

概率是某件事情某个可能情况的确定性，熵是某件事情到底是哪种情况的不确定性。

### 信息的计算

信息是一个物理量，信息消除的是事件的不确定性。可以选择一个事件的不确定性作为参照，当想要测量其他事件的不确定性时，就看待测事件的不确定性相当于*多少个*参照事件的不确定性。这里的*多少个*就是信息量。

#### 参照事件

在这里*只考虑等概率事件*，即单个事件相互独立，概率相等。选择的参照事件不同，测量的信息量不同，对应的单位也不同，下面是常见的几种单位：

（Ⅰ）如果参照事件只有2种等概率情况，那么测得的信息量的单位被称作 bit。（比如：抛硬币，只有正反两种情况，且概率相等，50%）

（Ⅱ）如果参照事件有10种等概率情况，那么测得的信息量的单位被称作ban。（比如：抛一个正10面体🎲，有10种情况，且每种情况概率相等，10%）

（Ⅲ）如果参照事件有e种等概率情况，那么测得的信息量的单位被称作nat。（类推，有e种等概率发生情况，每种情况发生的概率为 1/e）

#### 如何计算

下面通过例子来分析如何计算上面提到的*多少个*问题，这个值就是信息量。分两种情况：

（Ⅰ）被测事件不确定性的所有可能情况*等概率*

（Ⅱ）被测事件不确定性的所有可能情况*非等概率*

#### 等概率

假如，有一道单项选择题，提供ABCD四个选项，在不知道正确答案的情况下，四个选项对我们而言就是*四种不确定性*，不确定是ABCD中的哪一项。如果我们以抛硬币作为参照事件（最终计算的信息量单位为bit），等概率的*四种不确定情况*相当于一次抛出**2**枚硬币，那么我们就可以说：我们对这道题答案是ABCD中哪一项的不确定性为2bits。

如果，这道单项选择题有ABCDEFGH八个选项，在不知道正确答案的情况下，他就有*八种不确定情况*，还是以抛硬币作为参照事件，那么相当于一次抛出**3**枚硬币（2^3=8），所以对这道题选项是哪一个的不确定性就是3bits。

从上面可以看出，待测事件不确定情况的个数（m） 与 参照事件不确定情况的个数（n） 是*指数关系*进行累积的，表示为：3 = log_2 8，我们用m和n表示，那么上面两种情况信息量计算公式为：n = log_2 m，这里n就是*参照*事件为两种等概率情况下不确定性的*个数*，**信息量**。

选择其他参照事件推演逻辑相同，计算所得信息量相同，只是单位不同而已，即实际客观存在的信息量是一样的（可以理解为1000g和1kg单位不同，实际客观存在的质量相同）。

#### 非等概率

非等概率情况，需要分别测量待测事件每种可能情况下的信息量，再乘以他们各自发生的概率，最后相加得到的值即为总信息量。

假如，有一道单选题，提供ABCD四个选项，当被告知C选项有50%概率是正确答案时，信息量的计算方式为：

1/6 * log_2 6/1 + 1/6 * log_2 6/1 + 1/2 * log_2 2/1 + 1/6 * log_2 1/6 = 1.79 bits



参考：[YJango](https://www.zhihu.com/people/YJango)	《学习观》 





