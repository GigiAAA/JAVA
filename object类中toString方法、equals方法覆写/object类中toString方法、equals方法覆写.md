### object类中toString方法、equals方法覆写

#### 覆写toString方法

```java
//未覆写toString方法时(无意义)
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
}
public class Test{
    public static void main(String[] args){
        fun(new Person("野原新之助",5));
    }
    public static void fun(Object obj){
        System.out.println(obj);//toString()方法未被覆写时输出的是地址编码（类名称+@+地址）
        System.out.println(obj.toString());
    }
}
```

![1](C:\Users\14665\source\JAVA学习\object类中toString方法、equals方法覆写\1.png)

```java
//覆写toString方法
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){//toString()方法覆写
        return "姓名："+this.name+",年龄:"+this.age;
    }
}
public class Test{
    public static void main(String[] args){
        fun(new Person("野原新之助",5));
    }
    public static void fun(Object obj){//Object为所有类的父类，故可接受所有对象
        System.out.println(obj);//默认调用toString()方法
        System.out.println(obj.toString());
    }
}
```

![2](C:\Users\14665\source\JAVA学习\object类中toString方法、equals方法覆写\2.png)

#### 覆写equals方法

```java
//未覆写equals方法时（无意义）
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){
        return "姓名："+this.name+",年龄:"+this.age;
    }
}
public class Test{
    public static void main(String[] args){
        Person per1=new Person("野原新之助",5);
        Person per2=new Person("野原新之助",5);
        System.out.println(per1.equals(per2));//equals方法未被覆写时比较的依旧是对象地址
    }
}
```

![3](C:\Users\14665\source\JAVA学习\object类中toString方法、equals方法覆写\3.png)

```java
//覆写equals方法
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){
        return "姓名："+this.name+",年龄:"+this.age;
    }
    public boolean equals(Object obj){//覆写equals方法(Object为所有类的父类，故可接受所有对象)
        if(this==obj){//先判断若当前对象为obj，则自身等于自身（即地址相同），肯定两者相同返回true
            return true;
        }
        if(!(obj instanceof Person)){//判断传入obj类是否为Person类，若不是直接返回false
            return false;
        }
        //向下转型（为了对比传入的类与现有类属性值是否相同）
        //此时传进的类肯定是地址不相同且为Person类的类
        Person per=(Person) obj;//向下转型
        return name.equals(this.name) && this.age==age;//判断属性值是否相等
    }
}
public class Test{
    public static void main(String[] args){
        Person per1=new Person("野原新之助",5);
        Person per2=new Person("野原新之助",5);
        System.out.println(per1.equals(per2));//equals被覆写后比较的是内容是否相同
    }
}
```

![4](C:\Users\14665\source\JAVA学习\object类中toString方法、equals方法覆写\4.png)

**结论：若想正确使用toString()、equals()方法，必须进行方法覆写。**