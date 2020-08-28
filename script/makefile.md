# Makefile

> make命令执行时，需要一个 Makefile 文件，以告诉make命令需要怎么样的去编译和链接程序。

- 格式

  ```makefile
  target ... : prerequisites ...
  	command
  	...
  	...
  ```

  > target可以是一个object file(目标文件)，也可以是一个执行文件，还可以是一个标签（label）。对于标签这种特性，在后续的“伪目标”章节中会有叙述。
  >
  > prerequisites就是，要生成那个target所需要的文件或是目标。
  >
  > command也就是make需要执行的命令。（任意的shell命令）

- 示例

  ```makefile
  edit : main.o kbd.o command.o display.o \
  		insert.o search.o files.o utils.o       /*注释:如果后面这些.o文件比edit可执行文件新,那么才会去执行下面这句命令*/
  	cc -o edit main.o kbd.o command.o display.o \
  		insert.o search.o files.o utils.o
  
  main.o : main.c defs.h
  	cc -c main.c
  kbd.o : kbd.c defs.h command.h
  	cc -c kbd.c
  command.o : command.c defs.h command.h
  	cc -c command.c
  display.o : display.c defs.h buffer.h
  	cc -c display.c
  insert.o : insert.c defs.h buffer.h
  	cc -c insert.c
  search.o : search.c defs.h buffer.h
  	cc -c search.c
  files.o : files.c defs.h buffer.h command.h
  	cc -c files.c
  utils.o : utils.c defs.h
  	cc -c utils.c
  clean :
  	rm edit main.o kbd.o command.o display.o \
  		insert.o search.o files.o utils.o
  ```

  > 目标文件（target）包含：执行文件edit和中间目标文件（*.o），依赖文件（prerequisites）就是冒号后面的那些 .c 文件和 .h文件。每一个 .o 文件都有一组依赖文件，而这些 .o 文件又是执行文件 edit 的依赖文件。依赖关系的实质上就是说明了目标文件是由哪些文件生成的，换言之，目标文件是哪些文件更新的。
  
  > make并不管命令是怎么工作的，他只管执行所定义的命令。make会比较targets文件和prerequisites文件的修改日期，如果prerequisites文件的日期要比targets文件的日期要新，或者target不存在的话，那么，make就会执行后续定义的命令。
  
  > clean不是一个文件，它只不过是一个动作名字，有点像c语言中的lable一样，其冒号后什么也没有，那么，make就不会自动去找它的依赖性，也就不会自动执行其后所定义的命令。要执行其后的命令（不仅用于clean，其他lable同样适用），就要在make命令后明显得指出这个lable的名字。这样的方法非常有用，我们可以在一个makefile中定义不用的编译或是和编译无关的命令，比如程序的打包，程序的备份，等等。

[跟我一起写Makefile:MakeFile介绍]: https://wiki.ubuntu.org.cn/%E8%B7%9F%E6%88%91%E4%B8%80%E8%B5%B7%E5%86%99Makefile:MakeFile%E4%BB%8B%E7%BB%8D

[如何系统地学习 Makefile 相关的知识（读/写）？]: https://www.zhihu.com/question/23792247

