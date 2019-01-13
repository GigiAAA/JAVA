### 三种设计模式（重要）

>工厂设计模式
>
>代理模式
>
>单例模式

#### 工厂设计模式

##### 1.简单工厂模式

思路：专门定义一个类用来创建其他类的实例，被创建的实例通常都具有共同父类（也就是不论同类由多少产品均有一个工厂产出）。

让我们来思考一个现实生活中的实例，假设你想从京东商城买一台笔记本，京东商城已上架的电脑你只需要搜索下单即可购买。假设此时你看上了一款Thinkpad和一款mac，那接下来就一起模拟一下这个过程的实现。

```java
interface ICompute{//接口（类似于京东商城页面你可看到的物品信息）
    void print();
}
//具体类（类似于顾客下单后具体物品是由具体的工厂制作而成）
class ThinkpadImpl implements ICompute{
    public void print(){
        System.out.println("这是一台联想笔记本！");
    }
}
class MacImpl implements ICompute{
    public void print(){
        System.out.println("这是一台苹果笔记本！");
    }
}
public class Test{
    public static void main(String[] args) {
        getCompute(new ThinkpadImpl());//初始化对象（类似于你下单买了此款电脑）
        getCompute(new MacImpl());
    }
    public static void getCompute(ICompute compute){//向上转型
        compute.print();
    }
}
```

![SE1](C:\Users\14665\source\JAVA学习\学习总结Java SE必会三种设计模式--工厂 代理 单例\SE1.png)

以上程序明显是满足我们的需求，但此代码最大的问题是假如此时你又新看上了一款HUAWEI MateBook或者是京东商城要新上架物品，此时我们均必须打开源代码修改才可实现，这是非常不符合逻辑的（相当于京东每上新一款物品用户必须重新安装一次程序）。**此问题的关键在于初始化对象在主类中。**

接下来我们对以上代码进行优化。

**此时我们可以假设如果存在一个第三方工厂，用户下单后在工厂类中对比是否存在此商品，若存在直接调用对应的产品生成，**此时这种逻辑更符合现实生活，且很大程度上可对上文程序进行优化。

```java
import java.util.Scanner;

interface ICompute{
    void print();
}
class ThinkpadImpl implements ICompute{
    public void print(){
        System.out.println("这是一台联想笔记本！");
    }
}
class MacImpl implements ICompute{
    public void print(){
        System.out.println("这是一台苹果笔记本！");
    }
}
class HUAWEIMateBookImpl implements ICompute{
    public void print(){
        System.out.println("这是一台华为笔记本！");
    }
}
class EmptyComputeImpl implements ICompute{//用户所需物品不存在时调用
    public void print(){
        System.out.println("此款电脑未上架！");
    }
}
//工厂类（作用在于判断用户所需物品是否可生产，若可生产则调用对应商品输出，如不可生产返回NULL）
class ComputeFactory{
    public static ICompute getComputeInfo(String str){//返回值类型使用ICompute是因为具体产品实现了此接口
        ICompute compute=null;
        //注意：当字符串进行比较时，通常将以确定存在的放于.运算符前方调用equals()方法，目的在于避免空指针异常错误（用户可能不输入）
        if("HUAWEIMateBook".equalsIgnoreCase(str)){//判断用户所需商品是否可生产（不区分大小写）
            compute=new HUAWEIMateBookImpl();
        }else if("mac".equalsIgnoreCase(str)){
            compute=new MacImpl();
        }else if("thinkpad".equalsIgnoreCase(str)){
            compute=new ThinkpadImpl();
        }
        return new EmptyComputeImpl();//用户所需物品不存在时返回null
    }
}
public class Test{
    public static void main(String[] args) {
        Test test=new Test();
        Scanner scanner=new Scanner(System.in);
        System.out.println("请输入您想购买的笔记本：");
        String temp=scanner.nextLine();
        ICompute icompute=ComputeFactory.getComputeInfo(temp);
        test.buyCompute(icompute);
    }
    public void buyCompute(ICompute compute){//向上转型（所有子类均可自动向上转型为父类）
        compute.print();
    }
}
```

用户所需物品存在时：

![SE2](C:\Users\14665\source\JAVA学习\学习总结Java SE必会三种设计模式--工厂 代理 单例\SE2.png)

用户所需物品不存在时：

![SE3](C:\Users\14665\source\JAVA学习\学习总结Java SE必会三种设计模式--工厂 代理 单例\SE3.png)

**优化后的代码主要解决了新上架商品时需要修改客户端源代码的问题，二是将操作解耦到工厂类中（将客户端new提出来放在第三方类中）。但缺点是违反了OCP开放封闭原则（一个软件实体如类、模块和函数应该对扩展开放、对修改关闭）**

**故简单工厂模式的设计方法是：**

**（1）一个抽象的产品接口（类）**

**（2）多个具体产品类**

**（3）一个工厂（生产所有商品）**

#### 2.工厂方法模式

针对简单工厂的缺点（违反OCP开放封闭原则及一个工厂生产所有商品所呈现出的无隐秘性）做出改进。

**概念：定义一个用来创建对象的接口，让子类决定实例化哪个类，让子类决定实例化延迟到子类（针对每个产品家族提供一个工厂类，在客户端中判断使用哪个家族产品，使用哪个工厂类生产对象）。**

应用场景：多个产品呈现出家族式特点时。

```java
interface ICompute{
    void print();
}
class ThinkPadImpl implements ICompute{
    public void print(){
        System.out.println("这是一台联想笔记本！");
    }
}
class MacImpl implements ICompute{
    public void print(){
        System.out.println("这是一台苹果笔记本！");
    }
}
class HUAWEIMateBookImpl implements ICompute{
    public void print(){
        System.out.println("这是一台华为笔记本！");
    }
}
class EmptyComputeImpl implements ICompute{
    public void print(){
        System.out.println("此款电脑未上架！");
    }
}
interface ComputeFactory{//工厂接口
    ICompute createCompute();
}
class ThinkPadFactoryImpl implements ComputeFactory{
    public ICompute createCompute(){
        return new ThinkPadImpl();
    }
}
class MacFactoryImpl implements ComputeFactory{
    public ICompute createCompute(){
        return new MacImpl();
    }
}
class HUAWEIMateBookFactoryImpl implements ComputeFactory{
    public ICompute createCompute(){
        return new HUAWEIMateBookImpl();
    }
}
class EmptyComputerImpl implements ComputeFactory{
    public ICompute createCompute(){
        return new EmptyComputeImpl();
    }
}
public class Test{
    public static void main(String[] args) {
        Test test=new Test();
        ComputeFactory computeFactory=new HUAWEIMateBookFactoryImpl();
        test.buyCompute(computeFactory.createCompute());//这是一台华为笔记本！
    }
    public void buyCompute(ICompute compute){
        compute.print();
    }
}

```

以上代码我们可以看到优点是降低了代码耦合度，对象的生成交给具体子类实现；实现了开放封闭原则，每次添加子产品时不需要修改原有代码。但缺点是增加了代码量，每个具体产品都有一个具体工厂；当增加抽象产品时（添加其他产品类时）需要修改工厂，此时违背OCP原则。

此类开发模式还不是最优，随着以后我学习的深入，会对此做最优化修改。

#### 代理模式（重点）

代理模式顾名思义第三方就是代理类。想象一下现实生活，女生线上买东西找代购，代购就类似于代理类，顾客只需要下单，具体买东西的一系列过程都由代购完成；男生打游戏的代练也就类似于代理类，用户只需要付钱升级打怪都由代练负责。

**代理模式组成：一个接口，两个子类共同实现一个接口，其中一个子类负责真实业务实现，另一个子类完成辅助真实业务的操作。**

```java
//假设你找代购在香港购买一只MAC口红
interface Lipstick{
    void buyMacLipstick();
}
class RealSubjectImpl implements Lipstick{
    public void buyMacLipstick(){
        System.out.println("买一只MAC口红！");
    }
}
class ProxySubjectImpl implements Lipstick{
    private Lipstick lipstick;
    public ProxySubjectImpl(Lipstick lipstick){//真正的操作业务
        this.lipstick=lipstick;
    }
    public void beforeSale(){
        System.out.println("代购到达香港挑选MAC口红！");
    }
    public void afterSale(){
        System.out.println("快递！");
    }
    public void buyMacLipstick(){
        beforeSale();
        this.lipstick.buyMacLipstick();//调用真正操作业务（也就是代购只是跑腿实际还得顾客付钱）
        afterSale();
    }
}
public class Test{
    public static void main(String[] args) {
        Lipstick lipstick=new ProxySubjectImpl(new RealSubjectImpl());
        lipstick.buyMacLipstick();
    }
}
```

![SE4](C:\Users\14665\source\JAVA学习\学习总结Java SE必会三种设计模式--工厂 代理 单例\SE4.png)

**代理模式的本质是：所有的真实业务均需要一个辅助类（工具类）共同完成。**

#### 单例设计模式（重要）

**概念：一个类只允许产生一个实例化对象。**

应用场景：一个情况只能由一个对象时（例如我国婚姻法规定一夫一妻制（即在婚姻期间一个男性只能有一个法律认定的女性伴侣））。

```java
//饿汉式简单实现
class A{
    private static A a=new A();//私有属性
    private A(){}//构造方法私有化（从根源解决其他类中随意实例化此类对象）
    public static A getA(){//私有属性不能直接获取内容，需要间接使用get方法获得
        return a;
    }
    public void print(){
        System.out.println("hello world!");
    }
}
public class Test{
    public static void main(String[] args) {
        A a=A.getA();
        a.print();//hello world!
    }
}
```

**设计模板三个核心组成：**

**（1）构造方法私有化（从源头空指对象数量）；**

**（2）类内部提供唯一的静态私有对象；**

**（3）类内部提供静态方法返回唯一对象（类外部无法实例化对象，无法直接获得类中私有成员内容）。**

**使用静态方法的原因是与对象无关，类不生成对象可直接调用。**

单例设计模式还有另外一种懒汉式：

```java
//懒汉式简单实现
class A{
    private static A a;
    private A(){}
    public static A getA(){
        if(a==null){//判断是否被实例化（此时未实例化）
            a=new A();
        }
        return a;
    }
    public void print(){
        System.out.println("hello world!");
    }
}
public class Test{
    public static void main(String[] args) {
        A a=A.getA();
        a.print();
    }
```

面试笔试比较容易涉及到单例设计模式，概念思维很重要需要深刻理解，但在面试中若无强制要求优先写饿汉式，因懒汉式存在安全问题。

