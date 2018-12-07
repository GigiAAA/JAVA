### java斐波那契数列

问题描述：

一个斐波那契数列是由数字1、1、2、3、5、8、13、21、34等等组成的，其中每一个数字(从第三个数字起)都是前两个数字的和。创建一个方法，接受一个整数参数，并显示从第一个元素开始总共由该参数指定的个数所构成的所有斐波那契数字。例如，如果运行 java Fibonacci 5(Fibonacci为类名)，那么输出应该是1、1、2、3、5。

问题思路：

（1）构造主类、主方法；

（2）构造Fibonacci类（定义类的顺序：定义属性——>定义构造方法——>定义普通方法）

（3）普通方法中采用递归思路；

（4）在主类中生成对象，调用Fibonacci类中普通方法。

```java
class Fibonacci{
    private int number;//私有属性

    public Fibonacci(int number){//构造方法（生成对象时使用）
        this.number=number;//实例化对象，此时this调用本类属性
        //构造方法可为类中属性做初始化操作，是因构造方法的调用和对象内存分配几乎是同步完成的
    }

    public int result(int number){//普通方法
        if(number==1){
            return 1;
        }
        else if(number==2){
            return 1;
        }
        else{
            return result(number-1)+result(number-2);//递归
        }
    }
}
public class Test{//主类
    public static void main(String[] args){
        Fibonacci num=new Fibonacci(9);
        //根据Fibonacci类生成一个Fibonacci对象，对象名称叫做num
        for(int i=1;i<=9;i++){//输出
            System.out.print(num.result(i)+" ");
        }
    System.out.println("");//换行
    }
}
```

结果如下：

![1](C:\Users\14665\source\JAVA学习\java斐波那契数列\1.png)

