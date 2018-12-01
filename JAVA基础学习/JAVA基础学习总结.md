### JAVA之初体验

首先，需要声明的是Java是一种**半编译半解释性**语言。。

我们都知道，JAVA基本不存在平台问题，那么原因是什么呢？

答案就是JAVA源程序存储文件后缀是.java，在运行程序时的编译语言是javac，此时是使用编译命令javac将源文件编译成后缀为.class文件，该文件为与平台无关的二进制文件，提供给JVM。再者，JAVA的解释语言为java，此时实际上是启动了JVM虚拟机进程来将二进制的class文件翻译成平台可读取的相关文件进行执行。故JAVA的跨平台性很强。

在了解java语言的特性之后，我们即将进入JAVA基础学习。

#### 1.主方法中字符串数组传值

```java
public class Day12_1{
    public static void main(String[] args){
        int i=0;
        for(;i<args.length;i++)//java中数组长度可直接表示为数组名.length
        {
            System.out.println(args[i]);//传值时需在java命令解释二进制class文件时传值
        }
    }
}
```

![1](C:\Users\14665\source\JAVA\JAVA基础学习\1.png)

其中Hello是args[0],World是args[1].





