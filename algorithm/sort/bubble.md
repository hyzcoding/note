## 算法描述



## 算法实现

```java
public int[] bubbleSort(int[] array) {
    for (int i = 0; i < array.length; i++) {
        for (int j = 0; j < array.length - 1 - i; j++) {
            if (array[j + 1] < array[i]) {
                int temp = array[j + 1];
                array[j + 1] = array[j];
                array[j] = temp;
            }
        }
    }
    return array;
}
```



