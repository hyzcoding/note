

```markdown
# 这是一级标题
## 这是二级标题
```

# 这是一级标题
## 这是二级标题

```text```
* 列表1
    - 子列表
1. 编号
*斜体* _斜体文本_ **粗体文本** __粗体文本__ ***粗斜体文本*** ___粗斜体文本___
分隔线： *** * * * ***** - - - ----------
删除线： ~~BAIDU.COM~~
下划线： <u>带下划线文本</u>
脚注： [^要注明的文本] [^要注明的文本]: 啊啊啊
区块： > 区块引用
链接： 这是一个链接 [github](https://www.github.com)
高级链接: 高级链接Google [Google][1]
图片: ![alt 属性文本](图片地址) ![alt 属性文本](图片地址 "可选标题")
[1]: http://www.google.com/
` 单行代码串`
​```markdown 多行代码 ```
|表格|标题|表头|
|----|----|----|
|aa|bb|cc|

```mermaid
sequenceDiagram
    Note over Boot: 启动类
    Note over PDFThread: 线程类
    Note over PDFWorker: 执行类
  	Boot->>PDFThread: Boot类启动线程
    PDFThread->>PDFWorker: 线程类调用真正工作Worker
    loop 队列处理
        PDFThread->PDFThread: 考虑成功与失败情况处理方案
    end
    
    PDFWorker-->>PDFThread: Worker响应执行结果
	Note right of PDFWorker: 注意参数校验 <br>文件格式校验

```



* 列表1
    - 子列表
1. 编号
    *斜体* _斜体文本_ **粗体文本** __粗体文本__ ***粗斜体文本*** ___粗斜体文本___

----------

  ~~BAIDU.COM~~ <u>带下划线文本 </u>[^要注明的文本]
 > 区块引用

这是一个链接 [github](https://www.github.com) 

高级链接Google [Google][1]

 ` 单行代码串`

```markdown
多行代码 
```

| 表格 | 标题 | 表头 |
| ---- | ---- | ---- |
| aa   | bb   | cc   |

<h1>html 语法</h1>

[^要注明的文本]: 

[1]: http://www.google.com/
```

```