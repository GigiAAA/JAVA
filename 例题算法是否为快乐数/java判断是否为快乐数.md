### Java算法例题判断是否为快乐数

#### 问题描述：

编写一个算法来判断一个数是不是“快乐数”。

一个“快乐数”定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直

到这个数变为 1，也可能是无限循环但始终变不到 1。如果可以变为 1，那么这个数就是快乐数 。

 示例:

输入: 19 输出: true 解释:

1^2 + 9^2 = 82

8^2 + 2^2 = 68

6^2 + 8^2 = 100

1^2 + 0^2 + 0^2 = 1

#### 问题思路：

**准备工作：首先要明确的是此种涉及到数字的算法题，一般都遵循某种规律，需要自行验证3-4个数字一般就可以找到规律。**

例如此题我们验证一下：

20

2^2+0^2=4

4^2+0^2=16

1^2+6^2=37

3^2+7^2=58

**5^2+8^2=89**

8^2+9^2=145

1^2+4^2+5^2=42

4^2+2^2=20

接下来从头开始重复……

21

2^2+1^2=5

5^2+0^2=25

2^2+5^2=29

2^2+9^2=85

**8^2+5^2=89**

······

22

2^2+2^2=8

8^2+0^2=64

6^2+4^2=52

5^2+2^2=29

2^2+9^2=85

**8^2+5^2=89**

.……

**通过分析我们可以得到，若为快乐数则最终结果为1；若非快乐数，则必定在运算过程中会出现89，故我们可以认为当数字在运算过程中若出现89则直接返回此数为非快乐数。**

1.建立一个方法判断是否为快乐数；

2.建立一个方法获得下一次数字相加结果。

**源代码如下：**

```java
import java.util.Scanner;

public class Test{
    public static void main(String[] args) {
        Scanner scanner=new Scanner(System.in);//监视键盘
        System.out.println("请输入一个正整数：");
        int num=scanner.nextInt();
        isHappy(num);
        System.out.println(isHappy(num));
    }
    public static boolean isHappy(int num){
       Integer temp=num;
       while(true){//while(true)表示死循环，与C语言中while(1)相同
           temp=numberNext(temp);//获得每次数字各位平方和
           //判断是否为快乐数
           if(temp==1){
               return true;
           }else if(temp==89){
               return false;
           }
       }
    }
    public static int numberNext(int num){
        Integer result=0;//包装类属性
        while(num>0){//while语句只知道判断条件，不知道执行次数
            result+=(num%10)*(num%10);//数字各位平方和
            num/=10;//通过模10除10获得数字各位数字
        }
        return result;
    }
}
```

![2](C:\Users\14665\source\JAVA学习\例题算法是否为快乐数\2.png)

程序运行分析：

![1](C:\Users\14665\source\JAVA学习\例题算法是否为快乐数\1.png)