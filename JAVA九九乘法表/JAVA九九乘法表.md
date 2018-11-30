### JAVA九九乘法表

```java
public class Day2{//构造主类，主类名为Day2
    public static void main(String[] args){//主方法
        int i=1;
        for(;i<=9;i++)//java中能够直接写出来的整形变量均是int型
        {
            int j=1;
            for(;j<=i;j++)
            {
                System.out.print(i+"*"+j+"="+(i*j)+"\t");
                //输出时+代表的是字符串的拼接，不是运算符，任何元素类型只要与+连接均会被当做字符串处理，若想进行运算则需要添加（）改变优先级。
            }
        System.out.println("");//\t表示Tab(水平制表符)，此时输出的是换行
            //JAVA中输出时print代表不换行，println代表换行
        }
    }
}
```

![1](C:\Users\14665\source\JAVA学习\JAVA九九乘法表\1.png)![2](C:\Users\14665\source\JAVA学习\JAVA九九乘法表\2.png)