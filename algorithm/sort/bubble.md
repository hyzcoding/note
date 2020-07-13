# 冒泡排序 

## 算法描述

1. 指定第一个元素
2. 比较后一个元素，如果比下一个大则交换
3. 指定第一个元素，循环直到已排序的下标之前

![](../../_media/algorithm/bubble.gif)



## 算法实现

```java
// 将最大值放到最后
public int[] bubbleSort(int[] array) {
    for (int i = 0; i < array.length; i++) {
        for (int j = 0; j < array.length - 1 - i; j++) {
            if (array[j + 1] < array[j]) {
                int temp = array[j + 1];
                array[j + 1] = array[j];
                array[j] = temp;
            }
        }
    }
    return array;
}
// 将最小值放到最前面
public int[] bubbleSort(int[] array){
    for(int i=0;i<array.length;i++){
        for(int j=array.length-1;j>=i;j--){
             if (array[j] < array[j-1]) {
                int temp = array[j-1];
                array[j -1] = array[j];
                array[j] = temp;
            }
        }
    }
}
```



## 复杂度

时间复杂度: ***O(N^2^)***
空间复杂度: ***O(1)***
稳定性：稳定