---
title: Java代码风格之常量
date: 2020-06-26 00:37:15
categories: Java
tags: [Java,开发规范]
---

### 常量命名规范

常量是作用域内保持不变的值。一般用`final`关键字修饰，根据作用域不同分为全局常量，类内常量，局部常量。

1.全局常量

指类的公开静态属性，使用`public` static final 修饰；

2.类内常量

指私有静态属性，使用`private` static final 修饰；

3.局部常量

分为方法常量和参数常量。前者是方法或代码块内定义的常量，后者是在定义形参时增加 `final` 标识，表示此参数值不能被修改。

下面是这些不同类型变量的示例：

```java
public class Constant {
    // 全局常量，类内常量：变量名称全大写，单词之间下划线分割
    public static final String GLOBAL_CONSTANT = "shared in global";
    private static final String CLASS_CONSTANT = "shared in class";
    
    public void f (String a) {
        // 局部常量：变量名称小驼峰
        final String methodContant = "shared in method";
    }
}
```

### 禁用魔法值

即共识层面上的常量，直接以具体的数值或字符出现在代码中。如下代码：

```java
String key = "Id#taobao_" + readeId;
cache.put(key, value);
```

这段代码是保存信息到缓存中的方法，即使用魔法值组装key。如果在key拼接过程中错误的将`"Id#taobao_"`写成`"Id#taobao"`少了下划线，这就会导致缓存没有命中而去访问数据库，一般在测试环境数据量不大情况下并不容易发现这个问题的严重性，但是在大促时缓存失效导致数据库瞬间压力急剧上升，导致查询变慢。

所以，即使类内常量和局部常量只用一次，也应该赋予一个有意义的名称，保证后期使用时方便理解和值出同源。



参考《码出高效Java开发手册》