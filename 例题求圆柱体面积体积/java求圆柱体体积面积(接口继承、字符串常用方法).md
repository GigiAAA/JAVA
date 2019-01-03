##### Java求圆柱体体积面积（接口继承、字符串用法）

**问题描述：**

按如下要求编写Java程序： 

（1）定义接口A，里面包含值为3.14的常量PI和抽象方法double area()。 

（2）定义接口B，里面包含抽象方法void setColor(String c)。 

（3）定义接口C，该接口继承了接口A和B，里面包含抽象方法void volume()。 

（4）定义圆柱体类Cylinder实现接口C，该类中包含三个成员变量：底圆半径radius、 

圆柱体的高height、颜色color。 

（5）创建主类来测试类Cylinder。

源代码如下：

```java
import java.util.Scanner;//引入Scanner类

interface IA{
    Double PI=3.14;
    double area();
}
interface IB{
    void setColor(String C);
}
interface IC extends IA,IB{ //子接口可继承多个父接口
    void volume();
}
class CylinderImpl implements IC{//非抽象子类可实现接口
    private Double radius;
    private Double height;
    private String color;
    public CylinderImpl(Double radius,Double height){
        this.radius=radius;
        this.height=height;
    }
    public double area(){
        return 2*PI*radius*(radius+height);
    }
    public void setColor(String C){
        this.color=C;
    }
    public void volume(){
        System.out.println(PI*radius*radius*height);
        System.out.println(this.color.trim());//删除字符串左右两端空格（因输入颜色时前面有空格，故输出若不用此方法也会输出空格）
    }
}
public class Test{
    public static void main(String[] args){
        Scanner scanner=new Scanner(System.in);
        System.out.println("请输入圆柱体的半径、高、颜色：");
        double type1=scanner.nextDouble();
        double type2=scanner.nextDouble();
        String type3=scanner.nextLine();
        CylinderImpl cy=new CylinderImpl(type1,type2);
        cy.setColor(type3);
        System.out.println(cy.area());
        cy.volume();
    }
}

```

![圆柱体1](C:\Users\14665\source\JAVA学习\例题求圆柱体面积体积\圆柱体1.png)