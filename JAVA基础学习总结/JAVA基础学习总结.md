### JAVA之初体验

>主方法中字符串数组传值
>
>JAVA标识符与关键字
>
>JAVA数据类型划分
>
>JAVA运算符

我们都知道，JAVA基本**不存在平台问题**，那么**原因**是什么呢？

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

![1](C:\Users\14665\source\JAVA学习\JAVA基础学习总结\1.png)

其中Hello是args[0],World是args[1].

##### 注意：（1）主方法一定是在主类（public class）中定义，主类名称与源文件名称保持一致，一个源文件有且只有一个主类。

**（2）若输出中有中文，需要在编译时使用如下命令：**

```java
javac -encoding UTF-8 源文件名称
```

![2](C:\Users\14665\source\JAVA学习\JAVA基础学习总结\2.png)

#### 2.JAVA标识符与关键字

在认识标识符与关键字之前，我们先来看一下JAVA是怎么注释代码的。

```java
（1）单行注释://注释内容（用得最多）

（2）多行注释：/**注释内容*/
```

那么JAVA标识符是什么呢？

**在JAVA语言中，对于变量、常量、函数、语句块均有名称，故我们将此称为JAVA标识符。**

既然是名称，那就有命名规则及注意事项，接下来让我们一起认识一下吧。

命名规则：

（1）大驼峰命名——主要针对类名。（eg:FirstData）并且源文件的文件名必须与公共类的名字相同。

（2）小驼峰命名——主要针对变量。（eg:int firstData=10;）

（3）单词全部大写，多个单词之间以_分隔——常量命名。（eg:final int MY_VALUE=10;）

注意事项：

（1）标识符由字母、数字、_、所组成，其中不能以数字开头，不能使用JAVA中的关键字命名。

（2）标识符尽量采用有意义的命名

（3）“$”不要在代码块中出现

（4）虽然JAVA支持各种语言，但标准还是英语。

**JAVA关键字中需要注意的是：**

（1）goto、const也为关键字，只不过没任何意义。

（2）JAVA中有三个特殊含义的单词：null、ture、false。

（3）JDK1.4后追加了assert关键字；JDK1.5后追加了enum关键字。

#### JAVA数据类型划分

![3](C:\Users\14665\source\JAVA学习\JAVA基础学习总结\3.png)

**在JAVA程序中，所有能写出来的整型常量均默认为int型，其余整形均需自行声明；浮点型均为double型；若想定义为float型，必须在数字后面加L或l。**

那么数据类型之间能够进行转化呢？

答案是可以的。

```java
public class Day12_5{
    public static void main(String[] args){
        int i=10;
        byte a=10;//byte取值范围是-128-127，若常量在byte取值范围内时均可直接赋值
        byte b=(byte)129;//常量超出byte取值范围时需要强制类型转换
        byte e=(byte)i;//变量赋值时均需要强制类型转换，与byte取值范围无关
        int c=a;//小的数据类型转换为大的数据类型，可直接赋值，计算机可自行将小类型提升为大类型再计算
        char d='A';
        int num=d;//int型可与char型相互转化
        System.out.println(i);
        System.out.println(a);
        System.out.println(b);
        System.out.println(e);
        System.out.println(c);
        System.out.println(d);
        System.out.println(num);
    }
}
```

![4](C:\Users\14665\source\JAVA学习\JAVA基础学习总结\4.png)

**总结：大的数据类型转化为小的数据类型需要强制类型转化，byte类型需要考虑赋值方式；小的数据类型转化为大的数据类型可直接转化，系统会自行将小的数据类型转化为大的数据类型再运行。**

#### JAVA运算符

（1）基础运算符

++x,x++;--x,x--(用法与C语言一样)

（2）关系运算符（> < >= <= ==）返回类型均为布尔型（ture,false），可与逻辑判断语句一起使用。

（3）逻辑运算符（& | && || ^）用法与C语言一样

#### JAVA程序结构与逻辑控制

在JAVA中程序共有三种结构：顺序结构、分支结构、循环结构。(用法与C语言一样)

分支结构：if   switch

循环结构：while do-while for

**注意：数组和类集输出时可使用foreach语句，但此语句不可修改内容；若需要修改还需使用for语句。**

foreach格式：

```java
for(数据类型 临时变量名称：数组名称){

}
```

#### 方法的定义与使用

方法的声明：

```java
public static 方法返回值 方法名称{[参数类型 变量……]){
    方法代码块：
    [return 返回值]；
}
```

注意：

**当方法返回值为void时，表示此方法没有返回值；若有返回值，返回值可为基本数据类型和引用数据类型。**

**如果方法返回值为void，方法内使用return时，表示结束方法调用。**

#### **方法重载：**

定义：方法名称相同，参数的类型或个数不同，与返回类型无关。

```java
public class Day12_5{
    public static void main(String[] args){
        System.out.println(add(5,5));
        System.out.println(add(5,5,5));
    }
    public static int add(int x,int y){
        return x+y;
    }
    public static int add(int x,int y,int z){//参数个数不同
        return x+y+z;
    }
}
```

![5](C:\Users\14665\source\JAVA学习\JAVA基础学习总结\5.png)

**但要注意不能有两个名字相同，参数类型也想通但返回值不同的方法。**

```java
public class Day12_5{
    public static void main(String[] args){
        System.out.println(add(5,5));
        System.out.println(add(5,5));
    }
    public static int add(int x,int y){
        return x+y;
    }
    public static int add(int x,int y){//方法重定义，编译不通过
        int z=10;
        return x+y+z;
    }
}

```

![6](C:\Users\14665\source\JAVA学习\JAVA基础学习总结\6.png)

```java
System.out.print()//()中可为任意类型
那么你是否考虑为什么JAVA中输出不需要像C一样使用%d等就可以直接输出所有类型；原因就是系统重载了print函数
```















