# 复杂度
## 简介

用于评估算法效率的数值

- 时间复杂度：评估执行程序所需的时间。可以估算出程序对处理器的使用程度。

- 空间复杂度：评估执行程序所需的存储空间。可以估算出程序对计算机内存的使用程度。

## 时间复杂度

*T(n)=3n=O(n)*

```JAVA
for(int i=0;i<n;i++){ 
    System.out.println(i);
    System.out.println(i);
    System.out.println(i);
}
```

*T(n)=3log<sub>2</sub>n=O(logn)*

```java
for(int i=0;i<n;i*=2){
    System.out.println(i);
    System.out.println(i);
    System.out.println(i);
}
```

*T(n)=2=O(1)*

```java
 System.out.println(i);
 System.out.println(i);
```

*T(n)=0.5n^2^+0.5n=O(n^2^)*

```java
  for(int i=0; i<n; i++){
       for(int j=0; j<i; j++){
           System.out.println(j);
       }
       System.out.println(i);
   }
```



## 复杂度比较

*O(1)<O(logn)<O(n)<O(nlogn)<O(n²)<O(n³)<O(2ⁿ)<O(n!)*