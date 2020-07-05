---
title: Java代码风格之常量
categories: Java
tags:
  - Java
  - 开发规范
abbrlink: ddb1142c
date: 2020-06-26 00:37:15
---

### 常量命名规范

常量是作用域内保持不变的值。一般用`final`关键字修饰，根据作用域不同分为全局常量，类内常量，局部常量。

#### 1.全局常量

指类的公开静态属性，使用`public` static final 修饰。

#### 2.类内常量

指私有静态属性，使用`private` static final 修饰。

#### 3.局部常量

分为方法常量和参数常量。前者是方法或代码块内定义的常量，后者是在定义形参时增加 `final` 标识，表示此参数值不能被修改。

下面是这些不同类型变量的示例：

```java
public class Constant {
    // 全局常量，类内常量：变量名称全大写，单词之间下划线分割
    public static final String GLOBAL_CONSTANT = "shared in global";
    private static final String CLASS_CONSTANT = "shared in class";
    
    public void f (String a) {
        // 方法常量：变量名称小驼峰
        final String methodContant = "shared in method";
    }
    
    // 参数常量：b不可修改
    public void g (final int b) {
        // 编译出错，不允许对常量参数重新赋值
        b = 3;
    }
}
```

### 禁用魔法值

魔法值即共识层面上的常量，直接以具体的数值或字符出现在代码中。如下代码：

```java
String key = "Id#taobao_" + readeId;
cache.put(key, value);
```

这段代码是保存信息到缓存中的方法，即使用魔法值组装key。如果在key拼接过程中错误的将`"Id#taobao_"`写成`"Id#taobao"`少了下划线，这就会导致缓存没有命中而去访问数据库，一般在测试环境数据量不大情况下并不容易发现这个问题的严重性，但是在大促时缓存失效导致数据库瞬间压力急剧上升，导致查询变慢。

所以，即使类内常量和局部常量只用一次，也应该赋予一个有意义的名称，保证后期使用时*方便理解*和*值出同源*。

### Enum枚举类型

使用枚举类型定义全局常量都需要添加清晰的注释，比如业务相关信息或注意事项：

```java
// 定义交通灯枚举类
public enum ColorTypeEnum {
    
  RED(1, "停"),
  GREEN(2, "行"),
  YELLOW(3, "等");
  private int status;
  private String ledColor;

  ColorTypeEnum(int status, String ledColor) {
    this.status = status;
    this.ledColor = ledColor;
  }

  public int getStatus() {
    return status;
  }

  public String getLedColor() {
    return ledColor;
  }
}

// 测试类，获取红灯状态码
public class Test {
    
  public static void main(String[] args) {
    System.out.println(ColorTypeEnum.RED.getStatus());
  }
}
```

### 抽象类

使用不可实例化的抽象类的全局常量来表示交通灯状态和颜色：

```java
// 交通灯抽象类
public abstract class BaseColorStatus {

  public static final int RED = 1;
  public static final int GREEN = 2;
  public static final int YELLOW = 3;
}

// 测试类，获取红灯状态码
public class Test {
    
  public static void main(String[] args) {
    System.out.println(BaseColorStatus.RED);
  }
}
```



参考《码出高效Java开发手册》