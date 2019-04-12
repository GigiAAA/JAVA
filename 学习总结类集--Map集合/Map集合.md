### Map集合

>HashMap
>
>Hashtable
>
>TreeMap

**Map集合：Map接口是Java中保存二元偶对象(键值对)的最顶层接口。Map集合会一次性保存两个对象，且这两个对象的关系为：key=value结构。这种结构最大的特点是可以通过key找到对应的value值。**

```java
public interface Map<K,V> {}//其中key值唯一，value值可重复
//key值唯一，通过一个key值一定能找到唯一的一个value值。但一个value值可能对应多个key值
```

先来学习一下Map接口的重要方法：

```java
1.public V put(K key, V value);//添加元素
2.public V get(Object key);//根据key值取得对应的value值，若key值不存在返回null
3.public Set<K> keySet();//取得集合中所有key值(key不能重复)
4.public Collection<V> values();//取得所有在value值(value值可重复)
5.public Set<Map.Entry<K, V>> entrySet();//将Map集合转换为Set集合(重要)
```

Map本身是一个接口，要对其实例化还需依赖其子类，Map接口常用的几个子类如下：

HashMap、TreeMap、Hashtable、ConcurrentHashMap。

#### 1.HashMap(重要面试必问)--类比HashSet/无序存储

#### **自定义类作为key判断是否重复时需要覆写hashCode()与equals()方法。**--因HashMap底层实现为哈希表+红黑树(JDK1.8)，JDK1.8之前采用哈希表。

HashMap的基本实现：

```java
import java.util.*;

public class Test{
    public static void main(String[] args){
        Map<Integer,String> map=new HashMap<>();
        map.put(1,"小新");
        map.put(2,"小葵");
        map.put(3,"娜娜子");
        map.put(2,"野原");
        map.put(4,"小新");
        map.put(null,"小新");
        map.put(5,null);
        map.put(null,null);
        System.out.println(map);
        System.out.println("取得kay=2的value值："+map.get(2));
        System.out.println("取得所有的key值:"+map.keySet());
        System.out.println("取得所有的value值："+map.values());
    }
}
//{null=null, 1=小新, 2=野原, 3=娜娜子, 4=小新, 5=null}
//取得kay=2的value值：野原
//取得所有的key值:[null, 1, 2, 3, 4, 5]
//取得所有的value值：[null, 小新, 野原, 娜娜子, 小新, null]
```

通过运行结果我们可以发现：

(1)key可为null，且最多只能存在一个，存储于集合第一个位置；若设置时key值重复程序也不会报错，只不过当key值重复时调用put()方法变成了value值的更新操作；

(2)value可为null，且可有多个，存储依key值决定；value值可重复。

关于删除Map结合中元素，共有两种方法：

```java
1.V remove(Object key);//通过key值删除value值
//通过key值删除value值时，若value值相等则删除，不等则不删除(返回false)
//此种删除方法的应用场景为人与电话号码的删除，若相等才删除
2.default boolean remove(Object key, Object value) {
        Object curValue = get(key);
        if (!Objects.equals(curValue, value) ||
            (curValue == null && !containsKey(key))) {
            return false;
        }
        remove(key);
        return true;
    }
```

对两种删除元素方法简单使用：

```java
import java.util.*;

public class Test{
    public static void main(String[] args){
        Map<Integer,String> map=new HashMap<>();
        map.put(1,"小新");
        map.put(2,"小葵");
        map.put(3,"娜娜子");
        map.put(2,"野原");
        map.put(4,"小新");
        map.put(null,"小新");
        map.put(5,null);
        map.put(6,null);
        System.out.println(map);
        System.out.println("删除key=4的元素："+map.remove(4));
        System.out.println("删除key=2,value=小葵的元素:"+map.remove(2,"小葵"));
    }
}
//{null=小新, 1=小新, 2=野原, 3=娜娜子, 4=小新, 5=null, 6=null}
//删除key=4的元素：小新
//删除key=2,value=小葵的元素：false
```

通过以上对两种删除方法的简单使用我们可以发现，**若删除成功则会返回value值内容，若删除失败则会返回false。**

#### Map集合使用Iterator输出(重点)

Map接口与Collection接口不同，因Collection接口中存在iterator()方法，可以直接通过集合类取得迭代器输出；Map接口本身并没有迭代器相关的方法，**故若想使用Iterator输出就必须先将Map集合转换为Set集合再获取迭代器对象输出。**

首先我们先来看一下Collection集合与Map集合内部是如何保存数据的:

![1](C:\Users\14665\source\JAVA学习\学习总结类集--Map集合\1.png)

**由以上结构图可以看出Collection集合中每次保存单个元素；Map集合每次保存一个键值对，但站在宏观角度来看，将键值对抽象为一个Map.Entry对象，那么Map集合实际上也是每次保存一个对象，本质上与Collection集合一致。**

Map集合使用迭代器的简单使用：

```java
import java.util.*;

public class Test{
    public static void main(String[] args){
        Map<Integer,String> map=new HashMap<>();
        map.put(1,"小新");
        map.put(2,"小葵");
        map.put(3,"娜娜子");
        map.put(2,"野原");
        map.put(4,"小新");
        map.put(null,"小新");
        map.put(5,null);
        map.put(6,null);
        //1.首先将Map集合转换为Set集合
        Set<Map.Entry<Integer,String>> set=map.entrySet();
        //2.获得Iterator对象
        Iterator<Map.Entry<Integer,String>> iterator=set.iterator();
        //3.输出
        while (iterator.hasNext()){
            //4取得每个Map.Entry对象
            Map.Entry<Integer,String> entry=iterator.next();
            //5.取得key值和value值
            System.out.println(entry.getKey()+"="+entry.getValue());
        }
    }
}
//null=小新
//1=小新
//2=野原
//3=娜娜子
//4=小新
//5=null
//6=null
```

Map调用Iterator接口输出，走的是一个间接使用的模式，如下图：

![2](C:\Users\14665\source\JAVA学习\学习总结类集--Map集合\2.png)

#### 自定义类作为key判断是否重复时需要覆写hashCode()与equals()方法

以上代码使用的是系统类作为key(Integer,String等)，实际上我们也可以自定义类作为key，那么此时要判断key值是否重复时就需要覆写Object类的hashCode()方法与equals()方法。

自定义类作为key值的简单实现：

```java
import java.util.*;
class Person{
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return Objects.equals(name, person.name) &&
                Objects.equals(age, person.age);
    }

    @Override
    public int hashCode() {

        return Objects.hash(name, age);
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
public class Test{
    public static void main(String[] args){
       Map<Person,Integer> map=new HashMap<>();
       map.put(new Person("小新",5),1);
       map.put(new Person("娜娜子",0),2);
       map.put(new Person("小新",5),3);
       map.put(new Person(null,null),2);
       Set<Map.Entry<Person,Integer>> set=map.entrySet();
       Iterator<Map.Entry<Person,Integer>> iterator=set.iterator();
       while (iterator.hasNext()){
           Map.Entry<Person,Integer> entry=iterator.next();
           System.out.println(entry.getValue()+"="+entry.getKey());
       }
    }
}
//3=Person{name='小新', age=5}
//2=Person{name='null', age=null}
//2=Person{name='娜娜子', age=0}
```

自定义类作为key值和value值，作为value值的类不用覆写hashCode()与equals()方法，因value值可重复；但作为key值的自定义类必须同时覆写hashCode()与equals()方法。

```java
import java.util.*;
class Person{
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return Objects.equals(name, person.name) &&
                Objects.equals(age, person.age);
    }

    @Override
    public int hashCode() {

        return Objects.hash(name, age);
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
class Student{
    private String schoolName;

    public Student(String schoolName) {
        this.schoolName = schoolName;
    }

    @Override
    public String toString() {
        return "Student{" +
                "schoolName='" + schoolName + '\'' +
                '}';
    }
}
public class Test{
    public static void main(String[] args){
       Map<Person,Student> map=new HashMap<>();
       map.put(new Person("小新",5),new Student("双叶幼稚园"));
       map.put(new Person("娜娜子",0),new Student("东京女子大学"));
       map.put(new Person("小新",5),new Student(null));
       map.put(new Person(null,null),new Student(null));
       Set<Map.Entry<Person,Student>> set=map.entrySet();
       Iterator<Map.Entry<Person,Student>> iterator=set.iterator();
       while (iterator.hasNext()){
           Map.Entry<Person,Student> entry=iterator.next();
           System.out.println(entry.getKey()+"="+entry.getValue());
       }
    }
}
Person{name='小新', age=5}=Student{schoolName='null'}
Person{name='null', age=null}=Student{schoolName='null'}
Person{name='娜娜子', age=0}=Student{schoolName='东京女子大学'}
```

#### 2.TreeMap--类比TreeSet/有序存储

**TreeMap类比于TreeSet，判断是否重复时是根据key值，故自定义类作为key值时必须实现Comparable接口或传入比较器。作为key值的自定义类属性不可为null。**

系统类作为key值value值的简单使用：

```java
import java.util.*;
public class Test{
    public static void main(String[] args){
       Map<Integer,String> map=new TreeMap<>();
       map.put(1,"kala");
       map.put(2,"pp");
       map.put(5,"pp");
       map.put(3,"oo");
       //默认按照compareTo()方法排序(升序)，系统类均实现了Comparable接口
       System.out.println(map);
    }
}
//{1=kala, 2=pp, 3=oo, 5=pp}
```

自定义类作为key值value值的简单使用：

```java
import java.util.*;
class Person{
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
class Student{
    private String schoolName;

    public Student(String schoolName) {
        this.schoolName = schoolName;
    }

    @Override
    public String toString() {
        return "Student{" +
                "schoolName='" + schoolName + '\'' +
                '}';
    }
}
//比较器
class AscCompare implements Comparator<Person>{

    @Override
    public int compare(Person o1, Person o2) {
        if(o1.getAge()>o2.getAge()){
            return 1;
        }else if(o1.getAge()<o2.getAge()){
            return -1;
        }else{
            return o1.getName().compareTo(o2.getName());
        }
    }
}
public class Test{
    public static void main(String[] args){
       Map<Person,Student> map=new TreeMap<>(new AscCompare());
       map.put(new Person("小新",5),new Student("双叶幼稚园"));
       map.put(new Person("娜娜子",0),new Student("东京女子大学"));
       map.put(new Person("小新",5),new Student(null));
       Set<Map.Entry<Person,Student>> set=map.entrySet();
       Iterator<Map.Entry<Person,Student>> iterator=set.iterator();
       while (iterator.hasNext()){
           Map.Entry<Person,Student> entry=iterator.next();
           System.out.println(entry.getKey()+"="+entry.getValue());
       }
    }
}
//Person{name='娜娜子', age=0}=Student{schoolName='东京女子大学'}
//Person{name='小新', age=5}=Student{schoolName='null'}
```

通过上文对HashMap与TreeMap的学习，有没有发现似曾相识呢？没错，HashMap、TreeMap与HashSet、HashSet很像，那他们到底有什么关系呢？

#### Set接口与Map接口有什么关系？(面试)

我们可通过源码一探究竟：

```java
//HashSet
public boolean add(E e) {
    //HashSet的add方法返回的是map.put(),实际上就是将Set集合中的元素全部放置Map集合中的key值中，value值全部为PRESENT，因此Set集合中才不允许存在重复元素，因Set集合中元素实际上存储于Map集合中的key值
        return map.put(e, PRESENT)==null;
    }
private static final Object PRESENT = new Object();//PRESENT为空的Object对象
//TreeSet
public boolean add(E e) {
        return m.put(e, PRESENT)==null;
    }
```

#### 通过查看源码我们可以得知，Set本质上就是Map，只不过Set的value值均是相同的空的object，先有Map才有的Set，所以Set中元素才不可重复。将Set所有值均放在了Map集合的key值上。所以HashSet完全就是HashMap，TreeSet完全就是TreeMap。

#### 3.Hashtable子类--key值与value值均不允许为null

JDK1.0提供了三大类：Vector、Enumeration、Hashtable。**Hashtable是最早实现这种二元偶对象数据结构，后期的设计也让其与Vector一样多实现了Map接口而已。**

Hashtable的简单使用：

```java
import java.util.*;
public class Test{
    public static void main(String[] args){
       Map<Integer,String> map=new Hashtable<>();
       map.put(1,"小新");
       map.put(2,"小葵");
       map.put(1,"野原");
       //NullPointerException
//       map.put(null,"娜娜子");
//       map.put(9,null);
       System.out.println(map);
    }
}
//{2=小葵, 1=野原}
```

通过输出结果可以看出Hashtablekey值与value值均不能为null，否则会报空指针异常。

#### 请解释HashMap与Hashtable的区别(面试)

**HashMap:**

1.允许key和value为null，且key值有且只有一个为null，value值可有多个；

2.JDK1.2产生

3.异步处理、效率高、线程不安全

4.底层哈希表+红黑树(JDK1.8)，JDK1.8之前只有哈希表

**Hashtable:**

1.key与value均不为null；

2.JDK1.0产生

3.使用方法上加锁，效率低，线程安全

4.底层哈希表

ConcurrentHashMap还在学习中，及HashMap的实现原理，学完了立马补上啦，嘻嘻