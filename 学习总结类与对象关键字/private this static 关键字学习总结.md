## 类与对象关键字学习总结

>private
>
>this
>
>static

### private实现封装处理&构造方法（匿名对象）

>private基本介绍
>
>构造方法
>
>构造方法的重载
>
>匿名对象

#### 1.private实现封装（只是封装的第一步）

当属性或方法被private关键字修饰后，该属性或方法无法在类外部调用，只能在本类中使用。

```java
class Person{
    private String name;
    private int age;
}

public class Test{
    public static void main(String[] args){
        Person per=new Person();
        per.name="张三";
        per.age=18;
        System.out.println(per.name);
        System.out.println(per.age);
    }
}
```

![1](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\1.png)

那么如果类中属性被private修饰后，怎么初始化、获得其内容呢？

```java
getter方法：主要用于进行属性内容的设置与修改
setter方法：主要用于属性内容的取得
```

```java
class Person{
    private String name;
    private int age;

    public void setName(String n){
        name=n;
    }

    public void setAge(int a){
        if(a<0||a>200){
            System.out.println("年龄设置非法!");
        }
        else{
            age=a;
        }
    }

    public String getName(){
        return name;
    }

    public int getAge(){
        return age;
    }

    public void getPersonInfo(){
        System.out.println(name+"今年"+age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person();
        per.setName("张三");
        per.setAge(18);
        per.getPersonInfo();
    }
}
```

![2](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\2.png)

但是getter、setter的缺点是有几个属性就要调用几次进行初始化，为了减少代码冗余，就出现了**构造方法，用其代替setter方法进行类属性的初始化操作。**

#### 2.构造方法

所谓**构造方法就是使用关键字new实例化新对象时调用的操作方法**。定义构造方法时需遵循以下原则：

（1）方法名称必须与类名称相同

（2）构造方法没有返回值类型声明

（3）每一个类中至少存在一个构造方法（若无明确定义时，系统将自行生成一个无参构造），若自定义了构造方法，系统将不再生成无参构造。

```java
public person(){}//无参构造
public void person(){}
```

那么构造方法为什么没有返回值类型呢？

因为**构造方法是在产生新对象时自动调用的**，普通方法（不加static关键字的方法，必须由对象调用）若不调用就不会执行。故构造方法没有返回值类型。

那么以上代码就可以修改为：

```java
class Person{
    private String name;//属性
    private int age;

    //构造方法，进行类属性的初始化（此构造方法为自定义，系统将不再生成无参构造）
    public Person(String n,int a){
        name=n;
        age=a;
    }

    public void getPersonInfo(){//普通方法
        System.out.println(name+"今年"+age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person("张三",18);
        per.getPersonInfo();//调用普通方法
    }
}
```

![2](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\2.png)

**注意：**

**（1）若自定义了构造方法，则系统默认的无参构造将不再产生。**

**（2）因构造方法的调用和对象内存分配几乎是同步进行的，所以构造方法能为类中属性进行初始化操作。**

那么到现在我们学到的定义一个类的构造为属性、构造方法、普通方法。

（1）属性是对象在开辟堆内存时开辟的空间；

（2）构造方法是在使用new之后调用的；

（3）普通方法是在空间开辟后、构造方法执行后可以进行多次调用。

#### 3.构造方法的重载（参数个数不同）

接下来考虑下一个问题，既然普通方法都有方法重载（方法名称相同，参数类型或者个数不同，与返回值无关（建议返回值相同）），当然构造方法也就有，接下来让我们认识一下构造方法的重载。

```java
class Person{
    private String name;
    private int age;

    public Person(){}//无参构造

    public Person(String n){//一个参数的有参构造
        name=n;
    }

    public Person(String n,int a){//两个参数的有参构造
        name=n;
        age=a;
    }

    public void getPersonInfo(){
        System.out.println(name+"今年"+age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person("张三");
        per.getPersonInfo();
    }
}
```

![3](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\3.png)

观察以上代码输出结果，我们可以知道调用哪个构造方法，是根据生成对象时是否传参及传参个数决定的。那么以上代码我只赋值name，并没有赋值age，为什么age输出时是数字0（int型默认值）而不是乱码呢？

**原因是因为在生成对象时会执行无参构造。**

```java
public Person(){
        name=null;
        age=0;
    }
```

#### 4.匿名对象

```java
class Person{
    private String name;
    private int age;

    public Person(){}

    public Person(String n){
        name=n;
    }

    public Person(String n,int a){
        name=n;
        age=a;
    }

    public void getPersonInfo(){
        System.out.println(name+"今年"+age);
    }
}

public class Test{
    public static void main(String[] args){
        new Person("张三",18).getPersonInfo();//匿名对象
    }
}
```

**由于匿名对象没有任何栈内存所指向，所以使用一次后就会变成垃圾空间。**

### this关键字

**this关键字主要有三个用途：**

**（1）调用本类属性（此时this在构造方法、setter方法中使用）**

**（2）调用本类方法**

**（3）表示当前对象**

##### 1.调用本类属性

```java
class Person{
    private String name;
    private int age;
    public Person(String name,int age){//属性初始化
        name=name;
        age=age;
    }
    public void getPersonInfo(){
        System.out.println(name+"今年"+age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person("张三",18);
        per.getPersonInfo();
    }
}
```

![4](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\4.png)

按照之前的学习，这段代码理应正确，可是为什么属性并没有初始化呢？

原因是我们在写代码过程中变量命名尽量都要有意义，若形参命名也为name、age时，任何语言都遵循就近原则，即name会现在构造方法中寻找是否有name变量名，此种情况就是形参恒等于形参，根本不会到达属性处，此种赋值无意义，故不会赋值。所以就有了this关键字的出现。

```java
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;//this此时表示调用本类属性（打破就近原则）
        this.age=age;
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person("张三",18);
        per.getPersonInfo();
    }
}
```

![2](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\2.png)

此时this.name代表的就是本类属性,name代表的是形参，故可正常赋值。

**2.调用本类方法**

类中方法分为构造方法和普通方法。我们先来看调用构造方法的情况。

**（1）this调用构造方法**

```java
class Person{
    private String name;
    private int age;
    public Person(){
        System.out.println("********************************");
    }
    public Person(String name){
        System.out.println("********************************");
        this.name=name;
    }
    public Person(String name,int age){
        System.out.println("********************************");
        this.name=name;
        this.age=age;
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person("张三",18);
        per.getPersonInfo();
    }
}
```

![5](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\5.png)

此段代码我们可以看到使用了构造方法的重载能正常输出，但缺点是重复代码过多（若一般情况下代码出现重复，就一定有优化的可能）此时就可以使用this关键字调用构造方法进行优化。

```java
class Person{
    private String name;
    private int age;
    public Person(){
        System.out.println("********************************");
    }
    public Person(String name){
        this();//调用无参构造
        this.name=name;
    }
    public Person(String name,int age){
        this(name);//调用一个参数的有参构造
        this.age=age;
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person("张三",18);
        per.getPersonInfo();
    }
}
```

![5](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\5.png)

我们可以看到用this（）进行构造方法调用就可以将程序优化。但这里有两个需要注意的点：

（1）this调用构造方法时必须位于本方法的首行，否则编译不通过。

```java
//对以上代码修改
public Person(String name){
        this.name=name;
        this();
    }
```

![6](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\6.png)

（2）this调用构造方法时不能成“环”，必须是线性的。意思调用关系不能是封闭的，必须留有出口。

```java
class Person{
    private String name;
    private int age;
    public Person(){
        this(name,age);//此时想调用两个参数的构造方法
        System.out.println("********************************");
    }
    public Person(String name){
        this();
        this.name=name;
    }
    public Person(String name,int age){
        this(name);
        this.age=age;
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person("张三",18);
        per.getPersonInfo();
    }
}

```

![7](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\7.png)

分析图：

![8](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\8.png)

**（2）this调用普通方法**

```java
this.方法名称（）；
```

```java
class Person{
    private String name;
    private int age;
    public Person(){
        System.out.println("********************************");
    }
    public Person(String name){
        this();
        this.name=name;
    }
    public Person(String name,int age){
        this(name);//调用本类构造方法
        this.age=age;
        this.getPersonInfo();//调用本类普通方法，与主类调用getPersonInfo方法效果一样
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age);
    }
}

public class Test{
    public static void main(String[] args){
        Person per=new Person("张三",18);
        // per.getPersonInfo();
    }
}
```

![5](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\5.png)

**（3）this表示当前对象**

```java
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public void getPersonInfo(){
        System.out.println("lala"+this);//this表示当前调用getPersonInfo方法的对象
    }
}

public class Test{
    public static void main(String[] args){
        Person per1=new Person("张三",18);
        System.out.println(per1);
        per1.getPersonInfo();
        Person per2=new Person("小花",22);
        System.out.println(per2);
        per2.getPersonInfo();
    }
}
```

![9](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\9.png)

结果显示当前是哪个对象调用普通方法、普通属性，this就代表它。

#### static关键字

**1.static类属性（静态属性/类属性）**

**static属性描述共享属性，只需要在属性前加static关键字即可；如果要调用即可以类名称.属性名。**

```java
class Person{
    private String name;
    private int age;
    private String country;
    public Person(String name,int age,String country){
        this.name=name;
        this.age=age;
        this.country=country;
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age+"在"+this.country);
    }
}

public class Test{
    public static void main(String[] args){
        Person per1=new Person("张三",18,"中国");
        per1.getPersonInfo();
        Person per2=new Person("小花",22,"中国");
        per2.getPersonInfo();
    }
}
```

![10](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\10.png)

内存分析：

![11](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\11.png)

以上代码中两个Person对象country属性值是一样的，但如果按照之前的方法书写，那么生成多少个对象country属性就得初始化多少次，为了节省空间提升程序性能，提出了static关键字进行优化。

```java
//在本类中定义类属性时直接赋值
class Person{
    private String name;
    private int age;
    private static String country="中国";
    public Person(String name,int age){
        this.name=name;
        this.age=age;
        //this.country=country;
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age+"在"+this.country);
    }
}

public class Test{
    public static void main(String[] args){
        Person per1=new Person("张三",18);//共享属性
        per1.getPersonInfo();
        Person per2=new Person("小花",22);
        per2.getPersonInfo();
    }
}

//从其他类给共享属性赋值
class Person{
    private String name;
    private int age;
    public static String country;//此时共享属性定义为public
    public Person(String name,int age){
        this.name=name;
        this.age=age;
        //this.country=country;
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age+"在"+this.country);
    }
}

public class Test{
    public static void main(String[] args){
        Person.country="中国";//类名.属性名
        Person per1=new Person("张三",18);
        per1.getPersonInfo();
        Person per2=new Person("小花",22);
        per2.getPersonInfo();
    }
}
```

![12](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\12.png)

static类属性就不再存储于对象中了，存在全局数据区，只需一次赋值就可为所有对象country属性赋值。

结论：

（1）访问static属性（类属性）应使用类名称.属性名。

（2）所有的非static属性（实例变量）必须在对象实例化后使用，而static属性（类属性）不受对象实例化控制

（3）static不可在方法中定义（修饰）变量，只能修饰类中属性。

（4）在定义类时99%的情况都不会考虑static属性，以非static属性为主。

**2.static类方法（类方法/静态方法） **

使用static定义的方法为类方法，与对象无关，直接通过类名称访问。

```java
class Person{
    private String name;
    private int age;
    private static String country;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public static void Country(String c){
        country=c;//此时不能使用this关键字调用本类属性
    }
    public void getPersonInfo(){
        System.out.println(this.name+"今年"+this.age+"在"+this.country);
    }
}

public class Test{
    public static void main(String[] args){
        Person.Country("中国");
        Person per1=new Person("张三",18);
        per1.getPersonInfo();
        Person per2=new Person("小花",22);
        per2.getPersonInfo();
    }
}
```

![10](C:\Users\14665\source\JAVA学习\学习总结类与对象关键字\10.png)

**注意：**

**（1）static描述的类或属性在主方法产生时便会产生。**

**（2）所有的static方法不允许调用非static定义的属性与方法。**

**（因static方法与对象无关，非static定义的属性与方法必须通过对象调用，相互矛盾）**

**（3）所有的非static方法允许访问static方法或属性。**

**（没有对象都能调用static方法，必须使用对象调用的非static方法肯定也能调用）**

帮助理解：假设static方法是0元，非static方法是超市价值10元的东西，那么（2）肯定不可行，（3）就一定可行。

使用static定义方法的目的只有一个：某些方法不希望受到对象的限制，即可以在没有实例化对象的时候执行（广泛存在于工具类中）









