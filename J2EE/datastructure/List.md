# 列表

## 简介



## 使用

```java
public void useList(){
        List<String> list = new LinkedList<>();
        List<String> list2 = new LinkedList<>();
        list.add("1");
        list2.add("2");
        list.addAll(list2);
        list.add("2");
        System.out.println(list);
        System.out.println(list.contains("1"));
        System.out.println(list.indexOf("2"));
        list.remove("1");
        // 交集 listA内容变为listA和listB都存在的对象	listB不变
        list.retainAll(list2);
        // 差集	listA中存在的listB的内容去重 listB不变
        list.removeAll(list2);
        list.add("3");
        System.out.println(list.lastIndexOf("3"));
        System.out.println(list);
}
```



## 比较









## 源码解析

### ArrayList

`ensureCapacity()`该方法的作用是预先设置Arraylist的大小，这样可以大大提高初始化速度。



### LinkedList


[【程序员】你知道 if(a&lt;b) 和 if (a - b &lt; 0) 的区别吗？]: https://zhuanlan.zhihu.com/p/100590364
[Difference between if (a - b < 0) and if (a< b)]: https://stackoverflow.com/questions/33147339/difference-between-if-a-b-0-and-if-a-b

