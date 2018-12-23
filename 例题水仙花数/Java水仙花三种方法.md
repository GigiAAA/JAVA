问题描述：

打印所有的“水仙花数”，所谓“水仙花数”是指一个三位数，其各位数字立方和等于该数本身。例如：153是一个“水仙花数”，因为153=1的三次方+5的三次方+3的三次方。

```java
//1.内部类求解
class Flowers{
    private int num;
    class WaterFlowers{//内部类(内部类可直接访问外部类的私有域)
        public void FlowersNum(){
            for(int num=100;num<1000;num++){
                int a=num/100;
                int b=num/10%10;
                int c=num%10;
                if(num==a*a*a+b*b*b+c*c*c){
                    System.out.println("水仙花数为："+num);
                }
            }
        }
    }
}
public class Test{
    public static void main(String[] args){
        Flowers.WaterFlowers fl=new Flowers().new WaterFlowers();//生成内部类对象
        fl.FlowersNum();
    }
}
```

```java
//2.普通方法
class Flowers{
    private int num;
    public void fun(){//Flowers类中的普通方法
        for(num=100;num<1000;num++){
            int a=num/100;
            int b=num/10%10;
            int c=num%10;
            if(num==a*a*a+b*b*b+c*c*c){
                System.out.println(num);
            }
        }
    }
}
public class Test{
    public static void main(String[] args){
        Flowers fl=new Flowers();//生成Flowers对象
        fl.fun();
    }
}
```

```java
//3.
public class Test{
    public static void main(String[] args){
        for(int num=100;num<1000;num++){
            int a=num/100;
            int b=num/10%10;
            int c=num%10;
            if(a*a*a+b*b*b+c*c*c==num){
                System.out.println(num);
            }
        }
    }
}
```

![1](C:\Users\14665\source\JAVA学习\例题水仙花数\1.png)