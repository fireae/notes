# Makefile笔记

## 1.Makefile的规则

```
目标 : 需要的条件（冒号两边有空格）
    命令 （命令前面用tab键开头）
```

1. 目标可以是一个或多个，可以是Object File，也可以是可执行文件，甚至是一个标签。
2. 需要的条件就是生成目标文件所需要的文件或目标
3. 命令就是生成目标所需要执行的脚本

举个简单的例子。如果一个工程有2个头文件和3个c文件，生成helloworld可执行文件。

```
helloworld ： main.o hello.o world.o

main.o : main.c 
    cc -c main.c
hello.o : hello.c hello.h
    cc -c hello.o
world.o : world.c world.h
    cc -c world.c
clean:
    rm helloworld main.o hello.o world.o
```

将上面的内容写入Makefile, 执行make可以编译，执行make clean可以删除所有文件。

clean后面没有条件，而clean本身也不是文件，它只是一个动作的名字。clean冒号后面什么也没有，那么make就不会自动去找文件的依赖性，也不会自动执行其后定义的命令。

## 2.make命令的工作方式

默认情况下，我们只需要输入make命令， 然后：

1. make会在当前目录下寻找名字为“Makefile”或“makefile”的文件。
2. 如果找到，它会找文件中的第一个目标文件（target），在上面的例子中，它会找“helloworld”这个文件，并把它作为最终的目标文件。
3. 如果helloworld文件不存在，或者helloworld所依赖的.o文件比helloworld要新，那么，它会执行后面所定义的命令来生成helloworld这个文件。
4. 如果helloworld所依赖的.o文件也不存在，那么make会在当前文件中寻找目标为.o的文件的依赖性，并根据.o的依赖规则生成.o文件。
5. 最后，会找到.c和.h文件，于是make会生成.o文件，然后再用.o生成helloworld文件。



