### 三大特殊类

>String类
>
>Object类
>
>包装类

#### String类

String类在所有项目开发中均会使用。例如我们最常用的主方法。

**1.实例化方法**

**（1）直接赋值**

```java
String str="张三";//str是一个对象，故内容数值保存在堆内存中
System.out.println(str);
```

**（2）构造方法赋值**

```
String str=new String("张三");
System.out.println(str);
```

因所有底层计算机语言均不提供字符串直接提供字符串类型。故JAVA中“ ”中的均表示字符串。那么若两个字符串要比较是否相等，用直接赋值的方法可以吗？

```java
public class Test{
    public static void main(String[] args) {
        String str1="张三";
        String str2="张三";
        String str3=new String("张三");
        String str4=new String("张三");
        System.out.println(str1==str2);
        System.out.println(str1==str3);
        System.out.println(str3==str4);
    }
}
```

![1](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类\1.png)

直接赋值内存图分析：

![2](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类\2.png)

构造方法赋值内存图分析：

![3](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类\3.png)

**字符串为String类的匿名对象，在new之前就会存储于堆内存中，调用构造方法则又会新开辟空间，故构造方法赋值会有两块空间，但一块会成为垃圾空间，且不可入池。**

由以上代码我们可以直接看到，直接赋值的两对象比较返回true，其余返回false。原因是什么呢？

因String是一个类，所有字符串均为String类的匿名对象，且存储于堆内存中。故==比较的是栈内存中存放的地址是否相同（即是否指向同一块内存空间），与==直接比较普通数据类型数值不同。

JVM底层实际上维持着一个**字符串常量池**，**直接赋值的对象会自动入池**，下一个声明的String对象首先会在字符串常量池中查找，若有就直接使用，若无就生成再自动入池以供下次使用；直接赋值采用共享模式设计，目的是减少开销。而利用**构造方法赋值不会自动入池（使用intern()方法可实现手动入池操作）**，存在关键字new,故new几次便是在堆内存新开辟几次空间，故使用==比较地址肯定不同，均返回false。

**故字符串若想比较字符串内容是否相等，使用equals()方法。**

```java
public class Test{
    public static void main(String[] args) {
        String str1="张三";
        String str2="张三";
        String str3=new String("张三");
        String str4=new String("张三");
        System.out.println(str1==str2);//true
        System.out.println(str1.equals(str3));//true
        System.out.println(str3.equals(str4));//true
    }
}
```

那么思考一个问题，假如要将用户输入的字符串与已存在字符串进行是否相等比较，顺序有没有影响呢？

```java
import java.util.Scanner;

public class Test{
    public static void main(String[] args) {
        String str1=new String("野原新之助");
        Scanner scanner=new Scanner(System.in);
        System.out.println("请输入要比较的字符串：");
        String str=scanner.nextLine();
        System.out.println(str1.equals(str));
        System.out.println(str.equals(str1));
        System.out.println("野原新之助".equals(str));
    }
}
```

![4](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类\4.png)

![5](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类\5.png)

如上，若是由键盘直接输入则顺序无影响。

```java
import java.util.Scanner;
public class Test{
    public static void main(String[] args) {
        String str1=new String("野原新之助");
        String str=null;//空指针
        System.out.println(str1.equals(str));
        System.out.println(str.equals(str1));
        System.out.println("野原新之助".equals(str));
    }
}
```

![6](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类\6.png)

**此时比较时就应将已确定的字符串放在前面调用equals()方法比较，若将可修改的字符串放在前面则容易引起空指针异常（NullPointerException 运行错误）**

总结：

**（1）String类==与equals的区别**

==：进行数值比较，比较的是两个字符串对象的内存地址数值。

equals：比较两个字符串对象的内容。

**（2）String类直接赋值与构造方法赋值（传统方法）的区别**

直接赋值：开辟一块空间内存，并且该字符串对象会自动保存在字符串常量池中以供下次使用。

构造方法赋值：开辟两块空间，一块为垃圾空间，且生成的对象不能自动入池，需使用intern()方法手工入池。

```java
public class Test{
    public static void main(String[] args) {
        String str1=new String("野原新之助").intern();//手动入池
        String str2="野原新之助";
        System.out.println(str1==str2);//true
    }
}

```

**2.字符串不可变更**

**字符串一旦定义就不可变更。**

所有语言底层实现字符串，均使用字符数组，数组的最大缺陷就是长度固定。在定义字符串常量时，它的内容不可变。

```java
public class Test{
    public static void main(String[] args) {
        String str="野原广志";
        str+="野原美伢";
        str+="小新";
        System.out.println(str);//野原广志野原美伢小新
    }
}
```

**要值得注意的是，以上代码所输出的代码并不代表是字符串常量长度在改变，而是栈内存字符串对象指向在改变。**

内存变化分析图：

![7](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类\7.png)

由内存变化可以看出字符串一旦定 义就不会改变，是字符串对象引用一直在改变，而且会形成大量的垃圾空间。故尽量不要在代码中出现太多的字符串”+“操作。

字符串适用原则：

（1）字符串使用就采用直接赋值。

（2）字符串比较就是用equals()实现。

（3）字符串别改变太多。

**3.StringBuffer类、StringBuilder类**

上文我们提到基层计算机语言中因字符串实现方式的局限性，字符串内容不可修改。针对这一问题，为了更方便地修改字符串内容的修改，引入了两个SB类。

**StringBuffer类**

```java
public synchronized StringBuffer appeend(各种数据类型 b)
    //synchronized 表示StringBuffer类采用同步处理机制（原因是StringBuffer类可进行字符串内容修改，若同时有多人进行修改，在不知道前后顺序的情况下，极容易出错，故采用同步处理机制）
```

在String类中**字符串连接**使用"+",但会产生大量的垃圾空间。故此功能在**StringBuffer类中由append()方法实现。**

```java
public class Test{
    public static void main(String[] args) {
       StringBuffer sb=new StringBuffer();
       sb.append("野原广志").append("野原美伢");//字符串拼接
       fun(sb);
       System.out.println(sb);
    }

    public static void fun(StringBuffer temp) {
        temp.append("\n").append("野原向日葵");
    }
}
```

![8](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类\8.png)

**String类与StringBuffer类的最大区别是String类不可进行字符串内容修改，StringBuffer类可修改（在一块堆内存中修改），故频繁改动字符串内容时使用StringBuffer类。**

但String类与StringBuffer类之间不可直接转换，若需要转换需采用以下方式。

(1)String类转换为StringBuffer类

调用StringBuffer的构造方法。

```java
public class Test{
    public static void main(String[] args) {
        String str="小山";
        StringBuffer sb=new StringBuffer(str);
        System.out.println(sb);//小山
    }
}
```

调用append。

```java
public class Test{
    public static void main(String[] args) {
        String str="小山";
        StringBuffer sb=new StringBuffer();
        sb.append(str);
        System.out.println(sb);
    }
}

```

(2)StringBuffer类转换为String类

调用toString()方法。

```java
public class Test{
    public static void main(String[] args) {
        StringBuffer sb=new StringBuffer();
        sb.append("小山");//通过append（）方法拼接字符串
        String str=sb.toString();
        System.out.println(str);
    }
}

```

**StringBuffer类除了append（）方法外，还有一些String类没有的方法。**

(1)字符串反转

```java
public class Test{
    public static void main(String[] args) {
       StringBuffer sb=new StringBuffer("hello");
        System.out.println(sb.reverse());//olleh
    }
}
```

方法原型：

```java
public synchronized StringBuffer reverse();
```

(2)字符串删除指定范围内的数据（左闭右开）

```java
public class Test{
    public static void main(String[] args) {
       StringBuffer sb=new StringBuffer("hello world");
        System.out.println(sb.delete(2,7));//heorld
    }
}
```

方法原型：

```java
public synchronized StringBuffer delete(int start,int end);
```

(3)字符串插入操作

```java
public class Test{
    public static void main(String[] args) {
       StringBuffer sb=new StringBuffer("hello world");
        System.out.println(sb.insert(3,"野原新之助"));//hel野原新之助lo world
    }//插入位置在设定的开始位置之前
}
```

方法原型：

```java
public synchronized StringBuffer insert(int start,各种数据类型 b);
```

**上文中说到String类若使用”+“进行大量的字符串拼接时会产生大量的垃圾空间，但其实是会产生垃圾空间但是没有想象中那么多。原因是因为JAVA进行了一定的优化，在String类进行"+"拼接字符串之前，Java编译器会自动将String变量转换为StringBuilder而后进行append()处理。但在进行频繁的字符串拼接时还是直接使用StringBuffer或StringBuilder,这样会节省编译器优化的时间。javac默认采用StringBuilder.**

总结：

**String StringBuffer StringBuilder的区别是什么？**

**（1）String类内容不可修改，StringBuffer和StringBuilder的内容可修改。**

**（2）StringBuffer采用同步处理，属于线程安全操作；StringBuilder采用异步处理，属于线程不安全操作。**