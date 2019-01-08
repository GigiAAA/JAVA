### 自定义类覆写equals方法（对象比较）

我们通过前面的学习可以知道，String类比较内容是否相等使用equals()方法，原因就是系统已经覆写了String类中的equals()方法。故能否正常比较两个类实例化对象内容是否相等，关键在于此类是否覆写了equals()方法。

**覆写equals()方法思路：**

**（1）判断要比较对象的是否为null，若是直接返回false，若不比较则可能会出现空指针异常（NullPointerException）；**

**（2）判断是否在与自身比较（通过==比较地址），若是直接返回true；**

**（3）判断要比较的两个对象是否为同类，若是再进行接下来比较，若不是直接返回false。若不判断，则可能出现强转异常（classCastException）；**

**（4）通过向下转型，比较两对象内容是否相等。**

```java
class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){
        return this.name+"今年"+this.age+"岁";
    }
    public boolean equals(Object obj){//Object类可接受任何类
        if(obj==null){//判断是否为空，若不判断则会出先空指针异常（NullPointerException）
            return false;
        }
        if(this==obj){//判断是否在与自身比较（通过比较地址），若是则直接返回true
            return true;
        }
        if(!(obj instanceof Person)){//instanceof作用为判断其左边对象是否为右边对象的实例，此处为判断主方法中equals()方法括号中的对象是否为Person类
            return false;
        }
        //到达此处时必定是同类但不同地址的对象在比较
        Person per=(Person)obj;//向下转型，比较属性值
        return this.name.equals(per.name)&&this.age==per.age;//判定属性内容是否相等(易错点)
    }
}
class Student{}
public class Test{
    public static void main(String[] args) {
       Person per1=new Person("张三",18);
       Person per2=new Person("张三",18);
       Person per3=new Person("lisi",19);
       Person per4=null;
       Student stu=new Student();
       System.out.println(per1.equals(per1));//true
       System.out.println(per1.equals(stu));//false
       System.out.println(per1.equals(per3));//false
       System.out.println(per1.equals(per4));//false
    }
}
```

