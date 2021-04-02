# 异常

![Java异常类层次结构图](../_media/java/Java异常类层次结构图.png)

- [使用-try-with-resources-来代替try-catch-finally](./语法糖)

  ```java
  try (Scanner scanner = new Scanner(new File("test.txt"))) {
      while (scanner.hasNext()) {
          System.out.println(scanner.nextLine());
      }
  } catch (FileNotFoundException fnfe) {
      fnfe.printStackTrace();
  }
  ```

  