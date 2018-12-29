### Java中可实现多继承的三种方法

>多层继承
>
>内部类
>
>接口

#### 多层继承

我们知道，如果要直接继承类，子类不可直接多继承，但可通过多层继承实现多继承。但多层继承一般建议不超过三次，且代码较冗余。

```java
//多层继承实现多继承
class A{//父类A类
    private int num=10;
    public int getNum(){
        return this.num;
    }
    public void fun(){
        System.out.println("你今天真好看！");
    }
}
class B extends A{//B类继承A类
    private String name="张三";
    public String getName(){
        return this.name;
    }
    public void fun(){//方法覆写
        System.out.println(this.getNum());
        //父类私有域被继承但不可直接使用，需通过getter方法间接获得私有域的内容
    }
}
class C extends B{//C类继承B类，相当于间接继承A类
    private String name="刘能";
    public String getName(){
        return this.name;
    }
    public void fun(){//方法覆写（结果为覆写后的内容）
        System.out.println(this.getName());
        System.out.println(this.name);
    }
}
public class Test{
    public static void main(String[] args){
        A a=new A();
        a.fun();
        print(new B());//向上转型（优点在于子类可自动进行向上转型，可实现参数的统一）
        print(new C());
    }
    public static void print(A a){
        a.fun();
    }
}
```

![1](C:\Users\14665\source\JAVA学习\学习总结JAVA中可实现多继承的三种方法\1.png)

#### 内部类

第二种方法就是成员内部类可直接实现多继承。

```java
//内部类实现多继承
class A{//A类
    private int num=10;
    public int getNum(){
        return this.num;
    }
    public void fun(){
        System.out.println("你今天真好看！");
    }
}
class B {//B类（与A类无关）
    private String name="张三";
    public String getName(){
        return this.name;
    }
    public void fun(){
        System.out.println("昨天的你也很好看!");
    }
}
class C {//C类
    // private String name="刘能";
    class OneA extends A{//C中内部类继承A类
        public void printA(){
            System.out.println(this.getNum());
            fun();
        }
    }
    class OneB extends B{//C类内部类继承B类
        public void printB(){
            System.out.println(this.getName());
            fun();
        }
    }
    public void print(){//在C类中生成普通方法print()
        new OneA().printA();//匿名实例化OneA类对象并调用printA方法
        new OneB().printB();
    }
}
public class Test{
    public static void main(String[] args){
        C c=new C();//实例化C类对象
        c.print();//调用C中print方法
    }
}
```

![2](C:\Users\14665\source\JAVA学习\学习总结JAVA中可实现多继承的三种方法\2.png)

#### 接口

第三种实现多继承的方法是接口，在同时可用内部类和接口时，优先使用接口。因内部类需要应用于继承关系，接口可用于继承也可用于其他，比较灵活。

```java
//接口实现多继承
interface IA{//父接口A（接口为更纯粹的抽象类，结构组成只含全局常量和抽象方法）
    void funA();
}
interface IB {//父接口B（接口前添加I用以区分接口）
    void funB();
}
interface CImpl extends A,B{//接口可继承多个父接口，用,分隔开即可，子接口的命名可选择较为重要的父接口进行命名或自行命名，一般子接口后添加Impl用以区分
    void funC();
}
class Impl implements CImpl{//定义类实现接口（也可直接实现父接口（多个））
    public void funC(){//抽象方法的实现
        System.out.println("你昨天真好看！");
    }
    public void funA(){
        System.out.println("你今天真好看！");
    }
    public void funB(){
        System.out.println("你明天真好看!");
    }
}
public class Test{
    public static void main(String[] args){
        Impl im=new Impl();//实例化对象
        im.funA();
        im.funB();
        im.funC();
    }
}
```

![3](C:\Users\14665\source\JAVA学习\学习总结JAVA中可实现多继承的三种方法\3.png)