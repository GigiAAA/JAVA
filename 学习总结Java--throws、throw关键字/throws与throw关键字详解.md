### throws与throw关键字详解

#### throws关键字

throws关键字用在方法声明上，明确告诉调用者本方法可能产生的的异常，但方法本身不处理，用throws向上层抛出。

```java
//throws关键字的简单使用
public class Test{
    public static void main(String[] args) {
        try{
            System.out.println(print(10,0));
        }catch (Exception e){//程序中出现错误的所有具体异常均是由Exception继承而来，e为Exception类的一个对象，该类对象由IVM产生，不需new就可直接使用
            e.printStackTrace();
        }
    }
    public static int print(int x,int y)throws Exception{//在方法声明上使用，但不做任何处理，将异常抛回上层
        return x/y;
    }
}
```

![1](C:\Users\14665\source\JAVA学习\学习总结Java--throws、throw关键字\1.png)

那么其他类若想调用throws声明的方法时该怎么办呢？

```java
//调用的正确使用
public class Test{
    public static void main(String[] args) {
        try{
            System.out.println(print(10,0));
        }catch (Exception e){
            e.printStackTrace();
        }
        fun();
    }
    public static int print(int x,int y)throws Exception{
        return x/y;
    }
    public static void fun(){
        try{
            System.out.println(print(12,3));
        }catch(Exception e){
            e.printStackTrace();
        }
    }
}

```

![2](C:\Users\14665\source\JAVA学习\学习总结Java--throws、throw关键字\2.png)

##### 由以上代码结果我们可以看到，其他类若想调用throws关键字声明的方法，在调用时必须使用try...catch进行异常捕获。因该方法可能出现异常，故必须按照异常方法进行处理。

其他类若直接调用throws声明的类时

![3](C:\Users\14665\source\JAVA学习\学习总结Java--throws、throw关键字\3.png)

或出现异常提醒，提醒此时要调用的类可能会出现异常。

那么此时我们考虑另外一个问题，主方法也是方法，那它可以用throws关键字声明吗？

```java
public class Test{
    public static void main(String[] args)throws Exception {
        System.out.println(print(10,0));
    }
    public static int print(int x,int y)throws Exception{
        return x/y;
    }
}
```

若主类解决异常，则输出正常语句，若继续向上层抛出，则会抛到JVM中，只会输出错误提示。且两个均被throws关键字声明的类可直接调用。

#### throw关键字

throw是直接编写在语句中，表示人为进行异常抛出。**如果异常类对象实例化不希望由JVM产生而由用户产生，此时使用throw关键字来完成。**一般与分支语句搭配使用来抛出自定义异常（用户可继承Exception或RuntimeException来实现自定义异常）

```java
//throw关键字的简单使用
public class Test{
    public static void main(String[] args){
        try{
            throw new Exception("你今天真好看！");
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

![4](C:\Users\14665\source\JAVA学习\学习总结Java--throws、throw关键字\4.png)

一般而言，throw和break，continue，if要结合使用。

### throw关键字与throws关键字的区别（面试）

1.throw关键字用于方法内部，表示人为异常抛出。

2.throws关键字用于方法声明上，明确告诉用户本方法可能产生的异常，同时该方法可能不处理该异常。

#### 异常处理标准格式

```java
public class Test{
    public static void main(String[] args){
        try{
            System.out.println(print(10,0));
        }catch (Exception e){
            e.printStackTrace();
        }
    }
    public static int print(int x,int y)throws Exception{
        int result=0;
        System.out.println("计算开始前");
        try{
            return result=x/y;
        }finally {//finally为程序出口，无论是否有异常均会执行finally中代码块内容，且若try代码块中有return语句，在之前执行
            System.out.println("计算结束");
        }
    }
}
```

![5](C:\Users\14665\source\JAVA学习\学习总结Java--throws、throw关键字\5.png)

主方法中修改异常后：

![6](C:\Users\14665\source\JAVA学习\学习总结Java--throws、throw关键字\6.png)