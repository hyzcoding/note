# 二分查找

## 应用场景

> 仅针对有序序列可以使用

## 思想简介

时间复杂度`O(logn)`

## 算法实现
```java
public int binarySearch(int[] array,int target){
    if (nums.length == 0) return -1;
    int left=0;
    int right=array.length-1;
    while(left<=right){
        int mid = left +(right-left)/2;
        // int mid = (left+right)/2; 会溢出
        if(array[mid]==target){
            return mid;
        }else  if(array[mid]<target){
            left = mid;
        }else if(array[mid]==target){
            right = mid;
        }
    }
    return -1;
}
```