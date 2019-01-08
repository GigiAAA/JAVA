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

![1](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\1.png)

直接赋值内存图分析：

![2](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\2.png)

构造方法赋值内存图分析：

![3](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\3.png)

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

![4](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\4.png)

![5](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\5.png)

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

![6](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\6.png)

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

![7](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\7.png)

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

![8](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\8.png)

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

#### **Object类**

Object类是Java默认提供的一个类。Java里除了Object类，所有的类都存在继承关系。故**Object类是所有类的父类，即所有类的对象都可以使用Object类进行接受（除成员变量不可接受）。**

Object类存在一些定义好的方法：

```java
1.public Object()  //构造方法  Object类的无参构造，为子类服务（因若Object类无无参构造则所有类均没有）
2.pulic String toString() //取得对象信息，避免Object类接受所有类后输出的信息无实际意义，现有几乎所有类均以覆写toString方法，自定义类需自行覆写toString方法来获取对象信息
3.public boolean equals(Object obj)//用于对象比较  也需要覆写 下文会讲到
```

```java
//使用Object类接受所有类对象
class Person{
    public void fun(){
        System.out.println("1.我是人类！");
    }
}
class Student{
    public void fun(){
        System.out.println("2.我是学生！");
    }
}
public class Test{
    public static void main(String[] args) {
        print(new Person());//向上转型
        print(new Student());
    }
    public static void print(Object obj){
        System.out.println(obj);//但此时输出的为类名+类地址（无实际意义）
    }
}
```

**1.Object类获取对象信息（覆写toString类）**

```java
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){//在Person类中覆写toString方法
        return this.name+"今年"+this.age+"岁";
    }
}
public class Test{
    public static void main(String[] args) {
        print(new Person("张三",18));
        print("hello");
    }
    public static void print(Object obj){
        System.out.println(obj);
        System.out.println(obj.toString());
    }
}
```

![9](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\9.png)

由以上代码结果可以看出，Object类接受类输出时默认调用toString（）方法，写不写结果均一样。**但值得注意的是，自定义的类一定要进行toString（）方法的覆写，String类的匿名对象“hello”并无覆写方法就可直接输出信息原因是String类JAVA系统已进行了toString()方法的覆写。故Object类接受对象后能否正确输出类中信息，在于是否覆写toString()方法。**

通过以往的经验我们都知道，任何数据类型与字符串进行了“+”运算后，均会以字符串形式输出。故若是其他数据类型与String类进行“+”运算，输出时要想都变成字符串就默认使用toString()方法。

```java
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){
        return this.name+"今年"+this.age+"岁";
    }
}
public class Test{
    public static void main(String[] args) {
        String msg="hello"+new Person("张三",18);
        System.out.println(msg);//hello张三今年18岁
    }
}
```

**2.对象比较（equals覆写）**

我们通过前面的学习可以知道，String类比较内容是否相等使用equals()方法，原因就是系统已经覆写了String类中的equals()方法。故能否正常比较两个类实例化对象内容是否相等，关键在于此类是否覆写了equals()方法。

**覆写equals()方法思路：**

**（1）判断要比较对象的是否为null，若是直接返回false，若不比较则可能会出现空指针异常（NullPointerException）；**

**（2）判断是否在与自身比较（通过==比较地址），若是直接返回true；**

**（3）判断要比较的两个对象是否为同类，若是再进行接下来比较，若不是直接返回false。若不判断，则可能出现强转异常（classCastException）；**

**（4）通过向下转型，比较两对象内容是否相等。**

```java
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){
        return this.name+"今年"+this.age+"岁";
    }
    public boolean equals(Object obj){//Object类可接受任何类
        if(obj==null){//判断是否为空，若不判断则会出先空指针异常（NullPointerException）
            return false;
        }
        if(this==obj){//判断是否在与自身比较（通过比较地址），若是则直接返回true
            return true;
        }
        if(!(obj instanceof Person)){//instanceof作用为判断其左边对象是否为右边对象的实例，此处为判断主方法中equals()方法括号中的对象是否为Person类
            return false;
        }
        //到达此处时必定是同类但不同地址的对象在比较
        Person per=(Person)obj;//向下转型，比较属性值
        return this.name.equals(per.name)&&this.age==per.age;//判定属性内容是否相等(易错点)
    }
}
class Student{}
public class Test{
    public static void main(String[] args) {
       Person per1=new Person("张三",18);
       Person per2=new Person("张三",18);
       Person per3=new Person("lisi",19);
       Person per4=null;
       Student stu=new Student();
       System.out.println(per1.equals(per1));//true
       System.out.println(per1.equals(stu));//false
       System.out.println(per1.equals(per3));//false
       System.out.println(per1.equals(per4));//false
    }
}
```

**3.Object类接收引用数据类型（接口 数组）**

```java
//接收接口
interface IA{
    void fun();
}
class AImpl implements IA{
    @Override
    public void fun() {//方法覆写
        System.out.println("你今天真好看！");
    }

    @Override
    public String toString() {//覆写toString()方法
        return "hello";
    }
}
public class Test{
    public static void main(String[] args) {
       IA a=new AImpl();
       Object obj=a;//Object类接收接口
       System.out.println(obj);
       IA ia=(IA)obj;//还原为接口
       ia.fun();//结果为覆写后的内容
    }
}
//输出结果为
//hello
//你今天真好看！
```

```java
//接收数组
public class Test{
    public static void main(String[] args) {
       Object obj=new int[]{1,2,3,4,5,6};
       int[] data=(int[]) obj;//还原为数组
       for(int i:data){
           System.out.print(i+" ");//1 2 3 4 5 6 
       }
    }
}
```

**object真正达到了参数的统一，如果一个类希望收到所有的数据类型，就用Object类来完成。**

#### 包装类

通过上文学习我们可以知道，Object类可以接受所有类的对象。但Java中除了引用数据类型，还有基本数据类型，**包装类的作用就是将基本数据类型封装到一个类中，便于Object类接收，**这时Object类可以接收任意数据类型。

```java
//简单自定义一个包装类
class Person{//Person类此时其实就是int数据类型的包装类
    private int age;
    public Person(int age) {
        this.age = age;
    }
    public int intValue(){
        return this.age;
    }
}
public class Test{
    public static void main(String[] args) {
        Object obj=new Person(19);//将基本类型封装到类中（手动封箱）
        Person per=(Person)obj;
        System.out.println(per.intValue());//取出基本数据类的操作（手动拆箱）
    }
}
```

```
class Person{
    private int age;

    public Person(int age) {
        this.age = age;
    }
    public int intValue(){
        return this.age;
    }
}
public class Test{
    public static void main(String[] args) {
        Object obj=new Person(19);
        Person per=(Person)obj;
        System.out.println(per.intValue());
    }
}
```

由以上代码我们可以得到，将基本数据类型封装到类中的好处是便于Object类接受处理；但缺点是如果要对基本数据类型进行运算，就必须将包装类中的数据取出（拆箱）后才可进行。若均按照上述代码则代码冗余度较高，故**JAVA为了方便开发，专门提供了包装类的使用，共两种类型。**

**（1）对象型（Object的直接子类）：Blooean、Character(char);**

**（2）数值型（Number的直接子类）:Byte、Double、Long、Integer(int)、Short、Float.**

**注意：除了char、int变为类时，有所改变，其余均是首字母大写即可。**

关于Number类：

（1）Number类的定义：

```java
public abstract class Number implements java.io.Serializable
```

**（2）在Number类中已定义了六种重要方法用于获取包装类中的数据。**

```java
intValue() doublieValue() longValue() byteValue() shortValue() floatValue()
```

**1.装箱和拆箱**

装箱：将基本数据类型变为包装类对象，利用每个类提供的构造方法实现装箱处理。

拆箱：将包装类中包装的基本数据类型取出，利用Number类中六种方法。

```java
//装箱、拆箱的简单实现
public class Test{
    public static void main(String[] args) {
        Integer num=new Integer(99);//装箱
        System.out.println(num.intValue());//num.intValue()为拆箱
    }
}
```

以上方法为手动装箱拆箱。**在JDK1.5之后就提供了自动拆装箱的机制，自动拆装箱机制的最主要作用是可直接利用包装类中对象进行数学计算。**

```java
public class Test{
    public static void main(String[] args) {
        Integer num=99;
        System.out.println(num+2);//101
    }
}
```

包装类中依旧存在“==”和equals问题。

```java
public class Test{
    public static void main(String[] args) {
        Integer num1=99;
        Integer num2=99;
        Integer num3=130;
        Integer num4=130;
        Integer num5=new Integer(99);
        System.out.println(num1==num2);
        System.out.println(num3==num4);
        System.out.println(num1==num5);
        System.out.println(num1.equals(num2));
        System.out.println(num3.equals(num4));
        System.out.println(num1.equals(num5));
    }
}
```

![10](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\10.png)

我们知道String类中只要是直接赋值均会自动入池以供下次使用，那么包装类中直接赋值比较怎么一个false,一个true呢？原因是==比较的是地址数值是否相同，**包装类中Integer类-128到127范围内直接赋值会自动入池，其他数字则不会入池均在堆上存储，故直接赋值130时==比较地址不同错误。**而使用equals()方法比较的是内容，返回true。

**故所有相同类型的包装类对象之间值的比较，全部使用equals()方法。**

阿里编码规范：（1）所有POJO类(自定义类)属性必须使用包装类；（2）RPC方法（自定义方法）的返回值和参数使用包装类或基本数据类型均可；（3）所有局部变量使用基本数据类型。

**2.字符串与基本数据类型转换**

**(1)字符串转换为基本数据类型**

以后要进行各数据输入，一定都是字符串类型接收。故将字符串转换为其他数据类型时就需要包装类的支持。

```java
//字符串与整形转换
public class Test{
    public static void main(String[] args) {
        String str="768";
        int num=Integer.parseInt(str);
        System.out.println(num+2);//770
    }
}
```

**但需要注意的是当字符串转为数值型（Number直接子类）时，若字符串组成中含有非数字那么转换就会出现运行错误--数值转换异常（NumberFormatException），故我们在进行字符串转换为数值时，首先应判断字符串是否由纯数字构成。**

```java
public class Test{
    public static void main(String[] args) {
        String str="12345jk";
        if(isNumber(str)){
            int num=Integer.parseInt(str);
            System.out.println(num);
        }
    }
    public static boolean isNumber(String str){//判断字符串是否为纯数字构成
        char[] data=str.toCharArray();
        for(int i=0;i<data.length;i++){
            if(data[i]<'0'&&data[i]>'9'){
                return false;
            }
        }
        return true;
    }
}
```

若字符串中存在非数字，则会报错

![11](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\11.png)

若全为数字则会正常输出

![12](C:\Users\14665\source\JAVA学习\学习总结Java三大特殊类--String类Object类包装类\12.png)

```java
//字符串与Boolean转换
public class Test{
    public static void main(String[] args) {
        String str1="768vf";
        String str2="true";
        String str3="false";
        Boolean num1=Boolean.parseBoolean(str1);
        Boolean num2=Boolean.parseBoolean(str2);
        Boolean num3=Boolean.parseBoolean(str3);
        System.out.println(num1);//false
        System.out.println(num2);//true
        System.out.println(num3);//false
    }
}
```

**而将字符串转换为Boolean型时，不会报错，而是已定义的Boolean值返回原有值，如true返回true，不存在的均返回false，但不报错。**

**（2）基本类型转换为字符串**

任何数据类型使用“+”连接字符串均会变成字符串，但此方法会产生垃圾内存。

String类中定义的valueOf()方法，此方法不产生垃圾。

```java
public class Test{
    public static void main(String[] args) {
        String str=String.valueOf(19);
        System.out.println(str);//19
        System.out.println(str+4);//194 原因是已将19变为字符串，数字4+字符串变为字符串194
    }
}
```

