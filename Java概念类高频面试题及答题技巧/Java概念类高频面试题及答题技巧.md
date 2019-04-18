### Java概念类高频面试题及答题技巧

#### 1.Java与C的区别？

简单分析该题，属于概念类+抽象性(除非你可一句话将其解释清楚，否则可以考虑举例说明)

提到Java与C我们第一反应应该是该题实际上是在问我们**面向对象与面向过程的区别。**

明确了题目的隐含意思后我们可直接通过举例说明：

**面向对象可举例为人吃饭，面向过程可举例为人具体吃的什么饭。**

既然我们在回答此问题时提到了面向对象，那么面试官就很有可能顺着再问一个经典面试题：

##### 面向对象的三大特征是什么？

**那谈到这个相信所有人都能异口同声的说出封装、继承、多态。**可是在面试过程中仅仅这几个字是远远不够的，我们应分三步走回答这个问题：

**(1)首先解释概念**

**(2)其次解释作用**

**(3)Java中如何实现(加分项)**

那么接下来就示范一下对封装、继承、多态的答题技巧：

#### 封装：

概念为内部操作对外部不可见；作用为体现其保护性，可举例汽车与发动机；

在Java中的实现：private关键字、内部类、final关键字。

被private关键字修饰的内部类、方法、属性均对外部不可见；

内部类分为成员内部类、静态内部类、方法内部类和匿名内部类，非私有化内部类可在外部类外部进行初始化但必须依附于外部类(即必须外部类初始化后才可初始化内部类)，私有化内部类必须在外部类中初始化(对外部不可见，类比于私有方法)，方法内部类只对该方法可见(类比于局部变量)；

final关键字可修饰类、方法、属性，被修饰的类不可被继承，方法不可被覆写，属性内容不可更改。

#### 继承：

概念为子类无须重复编写代码的前提下，就可拥有父类所有属性和方法；作用为强调代码的可重用性；

Java中如何实现:子类 extends 父类

加分项：Java中继承存在单继承缺陷和要想实现继承必须满足“is-a”原则，那么解决单继承缺陷可通过多层继承、内部类和接口三种方式实现，至于继承必须满足"is-a"原则我们可通过内部类或者选择接口优先避免继承。

解决单继承缺陷三种方法：

多层继承：

```java
//若C想继承A，B，通过多层循环可达到C间接继承A
class A{}
class B extends A{}
class C extends B{}
```

内部类：

```java
class A{
    public void testA(){
        System.out.println("这是A类");
    }
}
class B{
    public void testB(){
        System.out.println("这是B类");
    }
}
class C{
    class PrintA extends A{ }//继承A类
    class PrintB extends B{ }//继承B类
}
public class Test{
    public static void main(String[] args) {
        C.PrintA a=new C().new PrintA();
        C.PrintB b=new C().new PrintB();
        a.testA();
        b.testB();
    }
}
```

接口：

```java
interface A{
    void testA();
}
interface B{
    void testB();
}
class C implements A,B{

    @Override
    public void testA() {
        System.out.println("这是A类");
    }

    @Override
    public void testB() {
        System.out.println("这是B类");
    }
}
public class Test{
    public static void main(String[] args) {
        C c=new C();
        c.testA();
        c.testB();
    }
}
```

#### 多态：

概念为一个类实例的相同方法在不同情况下有不同的表现形式；

作用为强调灵活性和拓展性，使程序更加灵活开放；

在Java中如何实现：

(1)方法多态：

-方法重载

-方法覆写

(2)对象多态：

-向上转型

-向下转型

#### 方法重载：方法名称相同，参数个数/类型不同，与返回值无关(但为了更好的程序设计一般返回值保持一致)。

```java
class A{
    public void test(){
        System.out.println("你今天真好看！");
    }
    public void test(String str){
        System.out.println("你今天真好看！"+str);
    }
}
public class Test{
    public static void main(String[] args) {
        A a=new A();
        a.test();
        a.test("哈哈");
    }
}
```

#### 方法覆写：方法名称、参数个数、参数类型、返回值均相同，但子类不可有比父类更严格的访问权限(即大于等于)

```java
class A{
    public void test(){
        System.out.println("你今天真好看！");
    }
}
class B extends A{
    public void test(){
        System.out.println("你明天也好看！");
    }
}
public class Test{
    public static void main(String[] args) {
        A a=new B();
        a.test();
    }
}
```

#### 方法重载与方法覆写的区别：

##### 概念不同：

方法重载：方法名称相同，参数个数/类型不同，与返回值无关(但为了更好的程序设计一般返回值保持一致)。

方法覆写：方法名称、参数个数、参数类型、返回值均相同，但子类不可有比父类更严格的访问权限(即大于等于)

##### 适用范围：

方法重载：同一个类中

方法覆写：有继承关系的两个类中

##### 权限要求：

方法重载：无

方法覆写：子类不能有比父类更为严格的访问权限，且父类中该方法不可被private关键字修饰，若被修饰就不是方法覆写。

#### 向上转型：天然发生的，不存在线程安全问题

```java
class A{
    public void test(){
        System.out.println("你今天真好看！");
    }
}
class B extends A{
    public void test(){
        System.out.println("你明天也好看！");
    }
}
public class Test{
    public static void main(String[] args) {
        A a=new B();//向上转型
        a.test();
    }
}
```

#### 向下转型：强制操作，不安全

```java
class Person {
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if(this==obj){
            return true;
        }else if(obj==null||!(obj instanceof Person)){
            return false;
        }
        Person person=(Person) obj;//向下转型
        return person.name.equals(this.name)&&person.age.equals(this.age);
    }
}
public class Test{
    public static void main(String[] args) {
        Person per1=new Person("张三",19);
        Person per2=new Person("张三",19);
        System.out.println(per1.equals(per2));
    }
}
```

#### 第二个问题：假如面试官问你是否了解23种设计模式，能简单谈几个最熟悉的吗？

分析此题，跟第一个问题一样，回答技巧遵循概念+抽象性(举例)

##### 我们必须明确所有设计模式均遵循OCP(开闭原则)：对修改关闭，对扩展开发。均是用于解耦(提高效率)

那么接下来我就挑选四种我最为熟悉的设计模式简单回答：工厂、代理、模板、单例。

##### 工厂模式：将客户端的new操作解耦到第三方的工厂中，如现实生活中的各种工厂。

```java
interface ICompute{
    void buyCompute();
}
class MacImpl implements ICompute{

    @Override
    public void buyCompute() {
        System.out.println("生产一台Mac电脑");
    }
}
class ThinkPadImpl implements ICompute{

    @Override
    public void buyCompute() {
        System.out.println("生产一台联想电脑");
    }
}
class InfoCompute{
    public static ICompute wantsCompute(String compute){
        if("mac".equalsIgnoreCase(compute)){
            return new MacImpl();
        }else if("thinkPad".equalsIgnoreCase(compute)){
            return new ThinkPadImpl();
        }
        return null;
    }
}
public class Test{
    public void buyCompute(ICompute compute){
        compute.buyCompute();
    }
    public static void main(String[] args) {
        Test test=new Test();
        test.buyCompute(InfoCompute.wantsCompute("mac"));
    }
}

//开发中实际用到的简单工厂模型(利用反射实现)
interface ISubject{
    void compute();
}
class MacImpl implements ISubject{

    @Override
    public void compute() {
        System.out.println("生产一台苹果电脑");
    }
}
class ThinkPadImpl implements ISubject{

    @Override
    public void compute() {
        System.out.println("生产一台联想电脑");
    }
}
class ComputeFactory{
    private ISubject subject;
    public ISubject wantsCompute(String computeName){
        try {
            subject=(ISubject)Class.forName(computeName).newInstance();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        return subject;
    }
}
public class Test{
    public void buyCompute(ISubject iSubject){
        iSubject.compute();
    }
    public static void main(String[] args) {
        Test test=new Test();
        ComputeFactory computeFactory=new ComputeFactory();
        test.buyCompute(computeFactory.wantsCompute("ThinkPadImpl"));
    }
}
```

##### 代理模式：类比于现实生活中代购。是对真实业务与辅助操作的解耦。

```java
interface IbuyCompute{
    void wantsBuy();
}
class CustomImpl implements IbuyCompute{

    @Override
    public void wantsBuy() {
        System.out.println("想买一台华为电脑");
    }
}
class AgencyImpl implements IbuyCompute{
    private IbuyCompute ibuyCompute;

    public AgencyImpl(IbuyCompute ibuyCompute) {
        this.ibuyCompute = ibuyCompute;
    }

    @Override
    public void wantsBuy() {
        this.before();
        this.ibuyCompute.wantsBuy();
        this.after();
    }
    public void before(){
        System.out.println("生产电脑");
    }
    public void after(){
        System.out.println("售后服务");
    }
}
public class Test{
    public static void main(String[] args) {
        IbuyCompute ibuyCompute=new AgencyImpl(new CustomImpl());
        ibuyCompute.wantsBuy();
    }
}
```

##### 模板模式：核心算法与拓展业务之间的解耦(如连锁店，在不同店中口味均差不多，是因为核心做法在模板类中，子类直接使用即可)

```java
abstract class CaffeineBeverage{//抽象类(将流程中共有的抽象出来作为父类、具体的延迟到子类具体实现)
    public void compare(){
        boilWater();
        brew();
        pourIncup();
        addCondiments();
    }
    public void boilWater(){
        System.out.println("将水煮沸");
    }
    public void pourIncup(){
        System.out.println("将饮料倒入杯中");
    }
    abstract void brew();
    abstract void addCondiments();
}
class Tea extends CaffeineBeverage{
    public void brew(){
        System.out.println("泡茶");
    }
    public void addCondiments(){
        System.out.println("加柠檬");
    }
}
class Coffee extends CaffeineBeverage{
    public void brew(){
        System.out.println("泡咖啡");
    }
    public void addCondiments(){
        System.out.println("加牛奶");
    }
}
public class Test{
    public static void main(String[] args) {
        changeDrink(new Tea());//向上转型
        changeDrink(new Coffee());
    }
    public static void changeDrink(CaffeineBeverage caffeineBeverage){
        caffeineBeverage.compare();
    }
}
```

##### 单例模式：有些情况下只允许存在一个对象时使用。

```java
//自定义类没有设置属性时
class Person{
    private static final Person person=new Person();
    private Person() {
    }
    public static Person getPerson() {
        return person;
    }
    public void print(){
        System.out.println("hello");
    }
}
public class Test{
    public static void main(String[] args) {
        Person person=Person.getPerson();
        person.print();
    }
}
//自定义类自身设置属性时
class Person{
    private String name;
    private static final Person person=new Person("张三");
    private Person(String name) {
        this.name = name;
    }
    public static Person print(){
        return person;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}
public class Test{
    public static void main(String[] args) {
        System.out.println(Person.print());
    }
}
```

#### 第三个问题：Java实现多线程同步的方法有哪几种？

注意：java中关于多线程部分均是重点，主要在于熟练掌握，若是面试官问到该问题，那么回答完三种实现方式后面试官会根据你的回答一步步深入提问。

#### 1.内建锁(Synchronized)--JDK6之后进行优化

##### 必须在同步代码块或同步方法中使用，内建锁阻塞线程调用wait()方法唤醒线程调用notify()/notifyAll()方法(调用wait()/notify()之前必须保证该线程已经获得锁)

Object类的wait()与notify()与内建锁搭配实现线程的堵塞及唤醒。主要操作流程为：在某线程拿到锁的前提下，调用wait()方法实际上是立即阻塞自身并将其放置于等待队列中同时释放锁；调用notify()方法则分为两种情况，若等待线程只有一个就唤醒该线程将其放置于同步队列中(意味着该线程有重新竞争获取锁的权利)，若存在多个等待线程时将会随机挑选一个唤醒(随机挑选是与JDK版本有关，JDK8默认唤醒等待队列中的第一个线程)；调用notifyAll()方法是将所有等待线程全部唤醒(JDK8默认唤醒顺序为从尾到头)

##### 注意：调用wait()方法会立即释放锁，而调用notify()/notifyAll()方法并不会立即释放锁而是需要执行完唤醒方法后再释放锁。

调用wait()与notify()方法图解：

![3](C:\Users\14665\source\JAVA学习\Java概念类高频面试题及答题技巧\3.png)

假设你在面试过程中简单讲解了上文知识点，那么面试官就很有可能顺着问一个**为什么关于多线程的均在juc包下而wait()方法与notify()方法要放置于Object类中呢？为什么要这么设计呢？这样合理吗？之类的问题**

##### 那关于这个问题我们应该这么回答：任何对象都可成为锁，也就是说任何对象都有等待唤醒机制，在Java中所有类默认继承Object类，故将wait()与notify()方法设置于Object类中是非常合理的。

内建锁实现生产者与消费者模型：

```java
import java.util.ArrayList;
import java.util.List;

//商品类
class Goods{
    private String goodsName;
    private Integer count=0;
    private Integer full;

    public Goods(Integer full) {
        this.full = full;
    }
    //生产者
    public synchronized void setGoods(String goodsName){
        while (count==full){
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        this.goodsName=goodsName;
        this.count++;
        notifyAll();
        System.out.println(Thread.currentThread().getName()+"生产"+this.toString());
    }
    //消费者
    public synchronized void getGoods(){
        while (count==0){
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        this.count--;
        notifyAll();
        System.out.println(Thread.currentThread().getName()+"消费"+this.toString());
    }

    @Override
    public String toString() {
        return "Goods{" +
                "goodsName='" + goodsName + '\'' +
                ", count=" + count +
                '}';
    }
}
//生产者类
class Productor implements Runnable{
    private Goods goods;

    public Productor(Goods goods) {
        this.goods = goods;
    }

    @Override
    public void run() {
        while (true){
            this.goods.setGoods("纪梵希口红");
        }
    }
}
//消费者类
class Customer implements Runnable{
    private Goods goods;

    public Customer(Goods goods) {
        this.goods = goods;
    }

    @Override
    public void run() {
        while (true){
            this.goods.getGoods();
        }
    }
}
public class Test{
    public static void main(String[] args) {
        Goods goods=new Goods(20);
        Productor productor=new Productor(goods);
        Customer customer=new Customer(goods);
        List<Thread> list=new ArrayList<>();
        //5个生产者
        for(int i=0;i<5;i++){
            Thread thread=new Thread(productor,"生产者"+i);
            list.add(thread);
        }
        //10个消费者
        for(int i=0;i<10;i++){
            Thread thread=new Thread(customer,"消费者"+i);
            list.add(thread);
        }
        for(Thread thread:list){
            thread.start();
        }
    }
}
```

关于内建锁有三个经典问题：

##### a.若线程1进入对象A的同步方法testA()后，能否在testA()方法中调用另外的同步方法testB()？

##### 答案是能。原因是可重入，也就是说线程1在拿到锁之后还未释放锁，此时就可再次进入。

举个例子：就好比你去火车站现场买票，假设你拿到了火车站大厅的钥匙并且你进入大厅后将门锁了，火车站大厅的各个窗口就类比于同步方法，你在打开大厅门之前，假设你正在窗口A买票但你突然发现窗口B的小哥哥长得比较好看，那么此时你当然可以换到窗口B进行购票业务。整个过程你就是线程1，锁就是火车站大厅的门锁，各个窗口就是同步方法。

```java
import java.util.concurrent.TimeUnit;

class MyThread implements Runnable{

    @Override
    public void run() {
        testA();
    }
    public synchronized void testA(){
        if("线程1".equals(Thread.currentThread().getName())){
            System.out.println(Thread.currentThread().getName()+"进入testA()方法");
            testB();
        }
    }
    public synchronized void testB(){
        System.out.println(Thread.currentThread().getName()+"进入testB()方法");
    }
}
public class Test{
    public static void main(String[] args) throws InterruptedException {
        MyThread mt=new MyThread();
        Thread thread1=new Thread(mt,"线程1");
        Thread thread2=new Thread(mt,"线程2");
        thread1.start();
        TimeUnit.SECONDS.sleep(2);
        thread2.start();
    }
}
//线程1进入testA()方法
//线程1进入testB()方法
```

##### b.若线程1进入对象A的同步方法testA()后，线程2能否进入同一对象的同步方法testB()？

##### 答案是不能。因线程1并没有释放锁还在占有锁，线程2此时应在同步队列中不断CAS尝试获得锁根本不会进入。必须等到线程1释放锁后线程2才有可能获得锁。

举个例子：继续是上文买火车票的例子，其中线程1是你，你还在大厅内进行购票业务，大厅门还在上锁，此时其他人根本不能进入大厅，只能在门外等候不断尝试打开门，必须等到你打开门(释放锁)之后他人才有可能进入大厅。

c.若线程1进入对象A的同步方法testA()后，线程2能否进入对象B的同步方法testB()？

#### 2.Lock体系(ReentLock(可重入锁))--由Java实现

##### 必须搭配try..finally代码块使用，必须在finally代码块中进行lock锁的关闭(即调用unlock()方法)。

**Condition接口（拥有头尾节点的单向队列）的await()方法与signal()方法与Lock锁搭配使用实现线程等待及唤醒功能。主要执行流程如下：当线程获得锁之后调用await()方法将其置于等待队列中，值得注意的是Lock锁中可同时存在多个等待队列(这样的好处在于同一等待队列中的等待线程属性相同便于操作)，每出现一次lock.newCondition()就说明新建立了一个等待队列；调用signal()方法将等待队列中的头节点即等待时间最长的节点移到同步队列中,移入同步队列后才有机会使得等待线程被唤醒，即从await()方法中的LockSupport.park(this)方法中返回，从而才有机会使得调用await()方法的线程成功退出；调用signalAll会将所有等待队列唤醒。**

await()与signal()方法流程图：

![4](C:\Users\14665\source\JAVA学习\Java概念类高频面试题及答题技巧\4.png)



Lock锁实现生产者与消费者模型：

```java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Goods{
    private String goodsName;
    private Integer count=0;
    private Integer full;
    private Lock lock=new ReentrantLock();
    private Condition productCondition=lock.newCondition();
    private Condition customConition=lock.newCondition();
    public Goods(Integer full) {
        this.full = full;
    }
    //生产者
    public void setGoods(String goodsName){
        try {
            lock.lock();
            while (count==full){
                System.out.println("商品还有大量库存，快来抢购哦");
                try {
                    productCondition.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            this.goodsName=goodsName;
            this.count++;
            customConition.signalAll();
            System.out.println(Thread.currentThread().getName()+"生产"+this.toString());
        }finally {
            lock.unlock();
        }
    }
    //消费者
    public void getGoods(){
       try {
           lock.lock();
           while (count==0){
               System.out.println("商品正在加急生产中，客官请稍等啦");
               try {
                   customConition.await();
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
           }
           this.count--;
           productCondition.signalAll();
           System.out.println(Thread.currentThread().getName()+"消费"+this.toString());
       }finally {
           lock.unlock();
       }
    }

    @Override
    public String toString() {
        return "Goods{" +
                "goodsName='" + goodsName + '\'' +
                ", count=" + count +
                '}';
    }
}
//生产者类
class Productor implements Runnable{
    private Goods goods;

    public Productor(Goods goods) {
        this.goods = goods;
    }

    @Override
    public void run() {
        while (true){
            this.goods.setGoods("纪梵希口红");
        }
    }
}
//消费者类
class Customer implements Runnable{
    private Goods goods;

    public Customer(Goods goods) {
        this.goods = goods;
    }

    @Override
    public void run() {
        while (true){
            this.goods.getGoods();
        }
    }
}
public class Test{
    public static void main(String[] args) {
        Goods goods=new Goods(20);
        Productor productor=new Productor(goods);
        Customer customer=new Customer(goods);
        List<Thread> list=new ArrayList<>();
        //5个生产者
        for(int i=0;i<5;i++){
            Thread thread=new Thread(productor,"生产者"+i);
            list.add(thread);
        }
        //10个消费者
        for(int i=0;i<10;i++){
            Thread thread=new Thread(customer,"消费者"+i);
            list.add(thread);
        }
        for(Thread thread:list){
            thread.start();
        }
    }
}
```

#### 3.CAS+volatile实现的无锁处理



#### 第四个问题：对比ArrayList、Vector与LInkedList的关系？(该题型为对比若干个选项的区别与关系(面试中常见绝大部分为此类型题))

此种类型题我们一般采用总分总的方式回答：

a.先回答对比项的共同点：Arraylist、Vector与LinkedList均是List接口的常用子类。

b.分别解释各自特点：

ArrayList底层依靠数组实现，JDK1.2提供，线程不安全的集合，采用异步处理性能较高；

Vector底层依靠数组实现，JDK1.0提供，线程安全，采用同步处理性能较低；

LinkedList底层依靠双向链表实现，JDK1.2提供，线程不安全的集合，采用异步处理性能较高。

c.对比区别：

##### ArrayList与Vector对比：

1)出现版本不同。ArrayList由JDK1.2提供，Vector由JDK1.0提供。

2）初始化策略不同。ArrayList采用懒加载策略，也就是说在初始化对象时并不是直接开辟数组而是在第一次添加元素时开辟数组，数组默认初始长度为10；Vector直接在初始化对象时将数组长度设置为10。

以下为源码：

```java
//ArrayList
//无参构造
public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
//add()
public boolean add(E e) {
        ensureCapacityInternal(size + 1);//初始化情况下size=0
        elementData[size++] = e;
        return true;
    }
//该方法用于区分当前minCapacity是否为初始化情况
private void ensureCapacityInternal(int minCapacity) {
       //若是直接将数组开辟为默认长度10
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        //若不是再执行以下方法
        ensureExplicitCapacity(minCapacity);
    }
//Vector
public Vector() {
        this(10);
    }
```

3）扩容策略不同。ArrayList每次扩容为原数组的1.5倍；Vector每次扩容为原数组的2倍。

以下为源码：

```java
//ArrayList(接(2)中代码)
private void ensureExplicitCapacity(int minCapacity) {//该方法用于判断当前是否需要扩容
        modCount++;

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
```

4）线程安全性。

ArrayList线程不安全，采用异步处理性能较高；

Vector采用在方法上加锁线程安全，采用同步处理性能较低。

5）支持的迭代器不同。

ArrayList不支持Enumeration枚举输出；Vector支持(只有Vector支持)

##### ArrayList与LinkedList的区别：

ArrayList底层依靠数组实现，遍历元素时间复杂度为O(1);

LinkedList底层依靠双链表实现，遍历元素时间复杂度为O(n).



#### 第五个问题：请解释final、finalize、finally的区别与关系？

我们都知道这三者之间是完全没有共同点的，故我们就可从各自特点开始回答。

##### 主要围绕概念+作用回答。

**final**:final关键字在java中将final称为终结器。final可修饰类方法属性；被final修饰的类不能被继承、方法不可被覆写、属性内容不可更改**(若一个类被final修饰，则内部所有方法属性均会默认被final修饰)**

**finalize**:在对象GC之前由JVM调用的方法。作用是给对象一个自我拯救的机会，该方法由Object类提供，故所有类可依据自身需求覆写此方法。

**finally**:finally关键字在java用于异常处理机制中。作用是保证重要代码不论是否有异常出现均可正常执行。使用方法为try...catch...finally或try...finally两种。

**保证重要代码代码一定会正常执行的举例：**

**1.IO流的关闭写在finally代码块中，无论是否有异常产生均能保证该资源被释放。**

**2.Lock锁中释放锁写在finally代码块中，不论是否发生中断异常均可正常将Lock锁释放。**



#### 附加题(小众关于类集的知识点，加分项)：继承Iterator迭代器的List接口与Set接口存在fail-fast机制(快速失败策略)

关于这个问题我们可先来看一段代码：

```java
import java.util.*;

public class Test{
    public static void main(String[] args) {
        List<Integer> list=new ArrayList<>();
        Collections.addAll(list,3,4,5,2,1);
        Iterator<Integer> iterable=list.iterator();
        while (iterable.hasNext()){
            list.remove(4);//在迭代输出时修改
            System.out.print(iterable.next()+" ");
        }
    }
}
//Exception in thread "main" java.util.ConcurrentModificationException
	//at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:901)
	//at java.util.ArrayList$Itr.next(ArrayList.java:851)
	//at Test.main(Test.java:10)
//以上代码出现的原因就是存在快速失败策略，那么快速失败策略具体是什么呢？我们接着往下看
//我们先将上面代码修改正确后结合二者对比讲解
import java.util.*;

public class Test{
    public static void main(String[] args) {
        List<Integer> list=new ArrayList<>();
        Collections.addAll(list,3,4,5,2,1,4,3);
        Iterator<Integer> iterable=list.iterator();
        while (iterable.hasNext()){
            Integer data=iterable.next();
            if(data.equals(4)){
                iterable.remove();//迭代器对象的remove()方法可正常输出
                continue;
            }
            System.out.print(data+" ");
        }
    }
}
//3 5 2 1 3 
```

关于这段代码，我们可以看到在迭代输出过程中若直接调用集合对象的remove()方法，系统会报ConcurrentModificationException(并发修改异常 非受查异常)；而调用迭代器对象的remove()方法则可正确删除指定元素正常输出。那么这究竟是为什么呢？接下来我们跟着源码一起分析缘由：

##### 直接调用集合对象的remove()方法出现异常原因：

```java
//该方法用于判断modCount与expectedModCount是否相等，若不等则抛出并发修改异常
final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
//此时我们来探寻一下modCount到底是什么意思
protected transient int modCount = 0;//定义于AbstractList接口中初始数值为0
//ArrayList类中add方法存在modCount变量(增)
public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);
    }
private void ensureExplicitCapacity(int minCapacity) {
        modCount++;//每添加一次就++

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
//ArrayList类中remove方法也存在modCount变量(删)
public E remove(int index) {
        rangeCheck(index);

        modCount++;//每移除一个元素就++
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }
//get()方法(查)中不存在modCount变量
public E get(int index) {
        rangeCheck(index);

        return elementData(index);
    }
private void rangeCheck(int index) {
        if (index >= size)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
//set()方法(改)中也不存在modCount变量
public E set(int index, E element) {
        rangeCheck(index);

        E oldValue = elementData(index);
        elementData[index] = element;
        return oldValue;
    }
private void rangeCheck(int index) {
        if (index >= size)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
//此时我们来探寻一下expectedModCount是什么意思
//expectedModCount是ArrayList的Itr内部类中普通属性(Itr类实现了Runnable接口，是ArrayList类中内置的迭代器)
int expectedModCount = modCount;
```

##### 经过以上源码的分析，不难猜出modCount变量代表改变集合组织结构次数(增加删除操作是对集合在数量上进行修改，数组元素个数增多或减少属于集合组织结构的改变；而查改只是在集合元素数量不变的情况下对其内容进行修改或提取，故不属于集合组织结构的改变，所以set()方法与get()方法中不存在modCount变量；expectedModCount是ArrayList类中内置迭代器中的普通属性，我们可以将其理解为expectedModCount是modCount的副本。我们都知道一个类中属性赋值是在该类进行初始化时进行(调用该类构造方法)，那么expectedModCount = modCount操作是在list.iterator()--集合对象获得迭代器时完成；

```java
public Iterator<E> iterator() {//liest.iterator()实际上是对内置迭代器进行初始化
        return new Itr();
    }
```

##### 所以在迭代输出过程中若直接调用集合对象的remove()方法也就意味着modCount+1,每次获取当前元素时(iterator.next())均会先检查modCount与expectdModCount是否相等，若不等直接抛出异常。

```java
public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }
final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
```

##### 以上就是fail-fast机制的流程及原因。fail-fast机制保证用户每次拿到的数据均是最新数据，若有人修改则立马抛出异常以提示用户可能此时数据并不是最新的。避免"脏读"现象。

那么使用迭代器的remove()方法又为什么可以呢？下面依旧是看源码学习：

```java
public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;//更新副本的值
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }

```

#### 总结：在迭代输出时尽量不要修改元素，可在迭代前或迭代后进行修改。





之后还会慢慢整理的，，加油啦。。。