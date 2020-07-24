# 枚举

### 简介

基本数据类型，用于声明一组命名的常数，当一个变量有几种可能的取值时，可以将它定义为枚举类型。

## 定义

```java
// 继承自 Enum类
public enum Day{
      MON=1, TUE, WED, THU, FRI, SAT, SUN;
    // function
    public static void main(String[] args){
        ...
    }
}
public enum Unit {
    YUAN(1d,"元"),FEN(0.01d,"分");
    public double value;
    public String desc;

    Unit() {
    }
	// 构造器默认都是private修饰，而且只能是private
    Unit(double value, String desc) {
        this.value = value;
        this.desc = desc;
    }

    public double getValue() {
        return value;
    }

    public void setValue(double value) {
        this.value = value;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }
}

import static Day.*

 // 由编译器添加的static方法
Day[] vals=Day.values();
// 下一个
MON.next();
// 随机
Day.random();
```

## EnumSet



## EnumMap

