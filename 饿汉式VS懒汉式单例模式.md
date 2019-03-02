### 饿汉式单例模式VS懒汉式单例模式

单例模式：有且只有一个对象

**设计模板：**

**1.首先从源头上控制对象的产生数量--将类的构造方法私有化，即其他任何类均不能产生此类对象。**

**2.类内部生成唯一静态私有对象。**

**3.类内部提供静态属性返回唯一对象。**

#### 饿汉式单例模式

```java
class Person{
    private String name;
    private Integer age;
    private final static Person person=new Person("小黑",89);//第二步，在类内部生成唯一静态私有对象
    private Person(String name,Integer age){//第一步，将构造方法私有化（起到从源头上阻止生成多个对象）
        this.name=name;
        this.age=age;
    }
    public static Person getPerson() {//第三步，生成静态方法返回唯一对象
        return person;
    }
    public void display(){
        System.out.println(Person.person);
    }
    public String toString(){
        return "姓名为："+this.name+" 年龄为："+this.age;
    }
}
    public class Test{
        public static void main(String[] args){
            Person person=null;
            person=Person.getPerson();
            person.display();
        }
    }
```

#### 懒汉式单例模式

特点：需要时才生成唯一对象。

缺点：懒汉式单例模式存在线程安全问题，在多线程并发下可能会产生不止一个对象。故无强制要求时优先饿汉式单例模式。

```java
class Person{
    private String name;
    private Integer age;
    private static Person person;
    private Person(String name,Integer age){
        this.name=name;
        this.age=age;
    }
    public static Person getPerson() {
        if(person==null){ 
            person=new Person("小黑",89);
        }
        return person;
    }
    public void display(){
        System.out.println(Person.person);
    }
    public String toString(){
        return "姓名为："+this.name+" 年龄为："+this.age;
    }
}
    public class Test{
        public static void main(String[] args){
            Person person=null;
            person=Person.getPerson();
            person.display();
        }
    }
```

