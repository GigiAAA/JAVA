### Java异常详解

几乎所有的代码均会出现异常，为了保证程序在出现异常之后可正常执行完毕，就需要进行异常处理。

异常分类：

**非受查异常：所有Error以及RuntimeException直接子类，不强制进行异常处理。**

**受查异常：所有其他异常，必须强制用户进行异常处理（（1）try/catch（2）向上级抛出）。**

![1](C:\Users\14665\source\JAVA学习\学习总结Javaz异常与捕获\1.png)

所有的异常均是由Throwable继承而来。

它的两个子类Error描述的是Java运行时内部错误和资源耗尽错误，应用不抛出此类异常，这种错误一旦出现，系统只能提醒用户并终止程序，这种情况很少出现；Exception及其子类是程序中出现错误，由于程序错误导致的属于RuntimeException（运行错误，比如classCastException(强转异常)，NullPointerException(空指针异常),NumberFormatException(数组越界异常)），而程序本身没有问题，但出现像I/O错误（打开一个不存在的文件）这类问题导致的异常属于IOException。

#### 异常影响

```java
//无异常程序运行
public class Test{
    public static void main(String[] args) {
        System.out.println("1.简单数学运算开始；");
        System.out.println("2.数学运算中"+10/2);
        System.out.println("3.简单数学运算结束。");
    }
}
```

```java
//存在异常程序运行
public class Test{
    public static void main(String[] args) {
        System.out.println("1.简单数学运算开始；");
        System.out.println("2.数学运算中"+10/0);//出现除零异常
        System.out.println("3.简单数学运算结束。");
    }
}
```

![2](C:\Users\14665\source\JAVA学习\学习总结Javaz异常与捕获\2.png)

由以上一场运行我们可以看出，当不做任何处理时程序会正常运行异常前的代码，之后的代码不会运行（因程序在异常处会中止程序运行）。但我们要是想不受异常的影响运行完程序该怎么办呢？

```java
//异常处理后的程序运行
public class Test{
    public static void main(String[] args) {
        System.out.println("1.简单数学运算开始；");
        try{
            System.out.println("2.数学运算中"+10/0);
        }catch(Exception e){
            e.printStackTrace();
        }finally {
            System.out.println("异常处理出口");
        }
        System.out.println("3.简单数学运算结束。");
    }
}

```

![3](C:\Users\14665\source\JAVA学习\学习总结Javaz异常与捕获\3.png)

#### 异常处理格式：

**异常处理机制的作用时出现异常时还可执行完程序。**

```java
try{//try代码块中放置可能出现异常语句
    
}catch(Exception e){//异常信息获取，若有异常存在时执行catch语句，否则不执行
    e.printStackTrace();
}finally{//finally为异常运行出口，无论是否出现异常均会执行
    
}
```

对于以上关键字可出现组合为try...catch、try...finally、try...catch...finally

```java
public class Test{
    public static void main(String[] args) {
        System.out.println("1.简单数学运算开始；");
        System.out.println(print());
        System.out.println("3.简单数学运算结束。");
    }
    public static int print(){
        try{
            System.out.println("2.数学运算中"+10/0);
            return 98;
        }catch(Exception e){
            e.printStackTrace();
            return 99;
        }finally {
            System.out.println("4.异常处理出口");
            return 100;
        }
    }
}
```

![3](C:\Users\14665\source\JAVA学习\学习总结Javaz异常与捕获\3.png)

程序运行分析：

![4](C:\Users\14665\source\JAVA学习\学习总结Javaz异常与捕获\4.png)

由以上代码我们可以看到，若try、catch块中有return语句，则在return语句执行之前一定会执行finally代码块，故返回100。

