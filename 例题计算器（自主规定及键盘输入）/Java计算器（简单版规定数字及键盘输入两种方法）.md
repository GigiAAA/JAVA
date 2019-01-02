### Java计算器

**问题描述：**

利用接口做参数，写个计算器，能完成加减乘除运算。 

（1）定义一个接口Compute含有一个方法int computer(int n, int m)。 

（2）设计四个类分别实现此接口，完成加减乘除运算。 

（3）设计一个类UseCompute，类中含有方法：public void useCom(Compute com, int one, int two)，此方法能够用传递过来的对象调用computer方法完成运算，并输出运算的结果。 

（4）设计一个主类Test，调用UseCompute中的方法useCom来完成加减乘除运算。 

**问题思路：**

（1）首先按照问题描述创建接口、类等；

（2）必须明确此方法中public void useCom(Compute com, int one, int two)的Compute com是父接口，可通过子类向上转型达到获得运算结果。

**方法一**：程序员直接自己给定两个数字进行运算，但此方法不符合实际，真正运行起来每次修改两个运算数字均需要改源代码。

```java
interface Compute{
    int computer(int n,int m);
}
class AddImpl implements Compute{
    public int computer(int n,int m){//方法覆写
        return m+n;
    }
}
class MinImpl implements Compute{
    public int computer(int n,int m){
        return n-m;
    }
}
class MulImpl implements Compute{
    public int computer(int n,int m){
        return n*m;
    }
}
class DivImpl implements Compute{
    public int computer(int n,int m){
        return n/m;
    }
}
class UseCompute{
    //因加减乘除四个类均实现了Compute接口，故Compute com可接受加减乘除这四个类，可通过这四个类向上转型实例化父接口
    public void useCom(Compute com, int one, int two){//调用加减乘除四个类方法，获得运算结果
        System.out.println(com.computer(one,two));
    }
}
public class Test{
    public static void main(String[] args){
        //先生成UseCompute类的匿名对象，再调用useCom方法，将加减乘除四个类通过向上转型实例化父接口对象及两个要运算的数字作为参数传入
        new UseCompute().useCom(new AddImpl(),5,6);
        new UseCompute().useCom(new MinImpl(),5,6);
        new UseCompute().useCom(new MulImpl(),5,6);
        new UseCompute().useCom(new DivImpl(),5,6);
    }
}
```

![计算器1 (1)](C:\Users\14665\source\JAVA学习\object类中toString方法、equals方法覆写\计算器1 (1).png)

程序运行分析：

![计算器3](C:\Users\14665\source\JAVA学习\object类中toString方法、equals方法覆写\计算器3.png)

**第二种方法：**由用户自行键盘输入要运算的两个数字，此方法比较贴近实际生活中使用计算器。

```java
import java.util.Scanner;//导入Scanner类

interface Compute{
    int computer(int n,int m);
}
class AddImpl implements Compute{
    public int computer(int n,int m){
        return m+n;
    }
}
class MinImpl implements Compute{
    public int computer(int n,int m){
        return n-m;
    }
}
class MulImpl implements Compute{
    public int computer(int n,int m){
        return n*m;
    }
}
class DivImpl implements Compute{
    public int computer(int n,int m){
        return n/m;
    }
}
class UseCompute{
    public void useCom(Compute com, int one, int two){
        System.out.println(com.computer(one,two));
    }
}
public class Test{
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);//监听键盘输入操作  System.in为输入操作
        System.out.println("请输入两个整数：");
        //注意：输入整数为nextInt(),输入字符串为nextLine();
        int type1=scanner.nextInt();//输入两个整数，命名分别为type1和type2
        int type2=scanner.nextInt();
        new UseCompute().useCom(new AddImpl(),type1,type2);
        new UseCompute().useCom(new MinImpl(),type1,type2);
        new UseCompute().useCom(new MulImpl(),type1,type2);
        new UseCompute().useCom(new DivImpl(),type1,type2);
    }
}
```

![计算器1 (2)](C:\Users\14665\source\JAVA学习\object类中toString方法、equals方法覆写\计算器1 (2).png)