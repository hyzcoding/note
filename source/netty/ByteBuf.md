# ByteBuf

## 简介

- `Bytebuffter`

  缺点：

  1. 长度固定，容易发生索引越界异常。
  2. 只有一个标识位置的指针，读写时需要手动`flip()`和`rewind()`，容易导致程序处理失效。
  3. api功能有限。

- `Bytebuf`

  通过`readerIndex`和`writerIndex`协助读写操作，读操作使用`readerIndex`，写操作使用`writerIndex`。