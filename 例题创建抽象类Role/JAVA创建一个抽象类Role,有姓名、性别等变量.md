**问题描述：**

定义一个抽象的"Role"类，有姓名，年龄，性别等成员变量 

1）要求尽可能隐藏所有变量(能够私有就私有,能够保护就不要公有)， 

再通过Getter()和Setter()方法对各变量进行读写。具有一个抽象的play()方法， 

该方法不返回任何值，同时至少定义两个构造方法。Role类中要体现出this的几种用法。 

2）从Role类派生出一个"Employee"类，该类具有Role类的所有成员（构造方法除外）， 

并扩展salary成员变量，同时增加一个静态成员变量“职工编号（ID）”。 

同样要有至少两个构造方法，要体现出this和super的几种用法，还要求覆盖play()方法， 

并提供一个final sing()方法。 

3）"Manager"类继承"Employee"类，有一个final成员变量"vehicle" 

在main()方法中制造Manager和Employee对象,并测试这些对象的方法。  

**问题思路:**

我们可根据题目要求创建相关类与方法、变量等。

1）首先创造一个抽象类，定义私有变量姓名、年龄、性别；定义getter、setter方法对相应变量进行读取写入功能（Employee子类中输出父类私有属性时会用到）；定义两个以上构造方法（对成员变量进行初始化操作可用构造方法也可用setter方法，此处有setter方法使用setter）；体现this用法（1.this调用类中属性，2.this调用类中方法（构造方法、普通方法），注意：构造方法调用不能成“环”，3.this代表当前对象）

2）Employee类继承父类Role，定义salary、ID成员变量（相对于父类来说为扩充属性）；定义至少两个构造方法，体现this和super几种用法（子类调用父类无参构造时super()语句可省略，此时可使用this语句调用子类中方法或属性，但调用父类有参构造时子类构造方法首行必须明确使用super(参数),指明调用父类哪个有参构造，此时子类构造方法中不可存在this语句调用本类构造方法的使用，因在构造方法中this和super调用构造方法时均需要位于构造方法首行）；覆写play()方法，即在子类中具体实现父类中的抽象方法；定义final sing()方法，即只能继承（使用）不能修改。

3）生成Manager类继承Employee类，相当于间接也继承了Role类，在Manager中定义final vehicle成员变量。

4）在主类中分别生成Employee类与Manager类对象，进行测试程序。

**源代码如下：**

```java
abstract class Role{//抽象类
    private static String name="张三";//static家族与对象无关，即在加载类时生成
    private static int age=18;
    private static String sex="男";
    public Role(){
        System.out.println("1.这是父类Role的无参构造");
    }
    public Role(String name){
        this();//调用无参构造
        System.out.println("2.这是父类Role一个有参构造");
    }
    public Role(String name,int age){
        this(name);
        System.out.println("3.这是父类Role两个有参构造");
    }
    public Role(String name,int age,String sex){
        this(name,age);
        System.out.println("4.这是父类Role三个有参构造");
        this.play();//调用普通方法play()
    }
    public void setName(String name){
        this.name=name;//this调用本类属性
    }
    public String getName(){
        return this.name;
    }
    public void setAge(int age){
        this.age=age;
    }
    public int getAge(){
        return this.age;
    }
    public void setSex(String sex){
        this.sex=sex;
    }
    public String getSex(){
        return this.sex;
    }
    public void fun(){
        System.out.println("4.#########################");
        }
    public abstract void play();//抽象方法，即只声明没有具体实现
}

class Employee extends Role{//子类Employee（相对于Role类来说）
    private static int salary;
    private static int ID;
    public void fun(){//方法覆写
        System.out.println("5........................");
    }
    public Employee(){
        this(salary);//调用一个参数的有参构造
        this.play();//调用普通方法play()
        this.sing();
    }
    public Employee(int salary){
        this.salary=salary;
        System.out.println(salary);
    }
    public Employee(int salary,int ID){
        super("张三",18,"男");
        this.salary=salary;
        this.ID=ID;
        this.sing();
    }
    public void play(){
        System.out.println(getName()+"今年"+getAge()+"性别"+getSex());
        //调用getter方法得到各成员变量数值
    }
    public final void sing(){
        System.out.println("年薪为"+this.salary+"工号为"+this.ID);
    }
}

//子类Manager（相对于Employee类而言，对于Role类Manager为孙子类，也间接继承了Role类）
class Manager extends Employee{
    private final int vehicle=23;
    public void fun(){//方法覆写（覆写Employee类中fun()方法）
        System.out.println("6.+++++++++++++++++++++++");
    }
}
public class Test{
    public static void main(String[] args){
        Role role1=new Employee(30000,12345);
        //因Role类为抽象类无法直接实例化对象，但可通过Employee类对象向上转型访问
        //生成Employee类实例化对象role1，调用两个参数的有参构造
        role1.fun();//调用fun()方法，结果为覆写后内容
        Role role2=new Manager(); //生成Manager类实例化对象role2，调用无参构造
        role2.fun();
    }
}
```

输出结果如下：

![1](C:\Users\14665\source\JAVA学习\例题创建抽象类Role\1.png)

序号1到序号5为实例化Employee类后结果，序号1到序号6为实例化Manager类后结果。

程序运行分析图：

![2](C:\Users\14665\source\JAVA学习\例题创建抽象类Role\2.png)