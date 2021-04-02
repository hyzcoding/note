# 选择排序

## 算法描述

时间复杂度*O(n^2^)*

## 算法实现

```java
public int[] selectionSort(int[] array) {
    for (int i = 0; i < array.length; i++) {
        int minIndex = i;
        for (int j = i; j < array.length; j++) {
            if (array[j] < array[minIndex]) minIndex = j;
        }
        int temp = array[minIndex];
        array[minIndex] = array[i];
        array[i] = temp;
    }
    return array;
}
```

