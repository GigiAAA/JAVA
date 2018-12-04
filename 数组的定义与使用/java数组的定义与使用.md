### java数组的定义与使用学习总结

>基本概念
>
>二维数组
>
>数组与方法互操作
>
>java对数组的支持

#### 基本概念

数组时相同类型元素的集合，并且这些变量可以按照统一的方式进行操作。

**数组是引用数据类型，有内存分配问题。**（所有引用数据类型均在堆区开辟空间）

**那么什么是引用数据类型呢？除了引用数据类型还有其他类型吗？有什么样的特点呢？**

java中数据类型分为两大类，即**基本数据类型和引用数据类型。**

![1](C:\Users\14665\source\JAVA学习\数组的定义与使用\1.png)

从上图我们可以看到JAVA中基本数据类型有8种，引用数据类型有3种。

**两者特点：**

（1）从概念方面：基本数据类型是变量名指向具体的数值；引用数据类型是变量名指向存储数据对象的内存地址。

（2）从内存构建方面：基本数据类型在变量声明后系统会立即为其分配存储空间；引用数据类型变量是以特殊的方法指向对象（类似c语言中的指针），这类变量声明时系统没有分配内存，其中存储的是内存的地址。

```java
public class Day12_4{
    public static void main(String[] args){
        int[] arr=null;//数组动态初始化，下文会详细介绍
        arr=new int[3];//在java中看到new关键字，就是在堆区开辟空间
        arr[0]=7;
        arr[1]=3;
        arr[2]=0;
        for(int i:arr){//foreach输出数组，但不可修改数组内容，若要修改数组内容需要使用for循环
            System.out.print(i+" ");
        }
        System.out.print("");
    }
}
```

以下是数组初始化内存分配：

![2](C:\Users\14665\source\JAVA学习\数组的定义与使用\2.png)

运行结果：

![3](C:\Users\14665\source\JAVA学习\数组的定义与使用\3.png)

（3）从使用方面：基本数据类型使用时需要赋值，判断时需要使用“==”；引用数据类型使用时可赋值null。

那么数组时怎么进行初始化的呢？

**其实在JAVA中数组初始化有两种方式，一种是动态初始化、另一种是静态初始化。**

**动态初始化：指数组首先开辟空间内存，再进行内容设置的方式称为动态初始化；**

**静态初始化：指数组在定义时直接设置其内容。**

数组**动态初始化**格式：

```java
数据类型[] 数组名称=new 数据类型[长度]；
eg:int[] arr=new int[5];
```

数组**静态初始化**格式：

```java
数据类型[] 数组名称=new 数据类型[]{值1，值2，……}；（推荐使用完整格式）
eg:int[] arr=new int[]{1,3,5,7,9};
简化格式：
数组类型[] 数组名称={值1，值2，……}；
```

**动态初始化注意：**

（1）数组不可越界访问，若越界则会出现"java.lang.ArrayIndexOutOfBoundsException(越界异常)"错误提示。

```java
public class Day12_4{
    public static void main(String[] args){
        int[] arr=new int[]{1,2,4};
        for(int i=0;i<arr.length;i++){
            System.out.println(arr[5]);//测试越界输出
        }
    }
}
```

![4](C:\Users\14665\source\JAVA学习\数组的定义与使用\4.png)

（2）数组采用动态初始化开辟空间后，数组中的每个元素均是该数据类型的默认值。

```java
public class Day12_4{
    public static void main(String[] args){
        int[] arr=new int[3];
        char[] A=new char[2];
        for(int i=0;i<arr.length;i++){
            System.out.print(arr[i]+" ");
        }
        System.out.println("");
        for(int j=0;j<A.length;j++){
            System.out.println(A[j]+" ");//默认值为空格
        }
    }
}
```

![5](C:\Users\14665\source\JAVA学习\数组的定义与使用\5.png)

#### 引用传递：

```java
public class Day12_4{
    public static void main(String[] args){
        int[] A=new int[3];
        int[] B=null;
        A[0]=7;
        A[1]=3;
        A[2]=66;
        B=A;//引用传递
        B[1]=99;
        for(int j:A){
           System.out.print(j+" "); 
        }
        System.out.println("");
    }
}
```

引用传递内存分配：

![6](C:\Users\14665\source\JAVA学习\数组的定义与使用\6.png)

运行结果：

![7](C:\Users\14665\source\JAVA学习\数组的定义与使用\7.png)

上文已经说过使用数组静态初始化时建议使用完整格式，原因是这样就可以灵活使用**匿名数组**这一概念了。

**所为匿名数组就是无栈内存指向的数组空间，且匿名数组只能使用完整格式，只能使用一次（因使用一次后找不到其地址，无法再使用）。**

```java
//数组静态初始化使用
public class Day12_4{
    public static void main(String[] args){
        int[] A=new int[]{1,2,3,4,5};
        for(int i:A){
            System.out.print(i+" ");
        }
        System.out.println("");
        System.out.print(new int[]{66,77,88,99}.length);//匿名数组
        System.out.println("");
    }
}
```

![8](C:\Users\14665\source\JAVA学习\数组的定义与使用\8.png)

#### 总结：数组不论是哪种初始化方式，最大的弊端是长度固定（存在越界问题）。

#### 二维数组（了解）

动态初始化格式：

```java
数据类型[][] 对象数组=new 数据类型[行个数][列个数];
eg:int[][] A=new int[3][4];//三行四列
```

静态初始化格式：

```java
数据类型[][] 对象数组=new 数据类型[][]{{值1，值2，……}，{值1，值2，……}}；
eg:int[][] A=new int[][]{{1,2,3},{2,3,4}};//两行三列
```

#### 数组与方法互操作

（1）**方法接受数组**

```java
public class Day12_4{
    public static void main(String[] args){
        int[] A=new int[]{1,2,3,4,5};
        printArray(A);//在主方法外新建方法，调用实现接受数组
    }
    public static void printArray(int[] data){
        for(int i:data){
            System.out.print(i+" ");
        }
        System.out.println("");
    }
}
```

![9](C:\Users\14665\source\JAVA学习\数组的定义与使用\9.png)

##### （2）方法返回数组

```java
public class Day12_4{
    public static void main(String[] args){
        int[] A=Init();
        printArray(A);
    }
    public static int[] Init(){
        return new int[]{1,2,3,4,5,6};//匿名数组
    }
    public static void printArray(int[] data){
        for(int i:data){
            System.out.print(i+" ");
        }
        System.out.println("");
    }
}
```

![10](C:\Users\14665\source\JAVA学习\数组的定义与使用\10.png)

**（3）方法修改数组**

```java
public class Day12_4{
    public static void main(String[] args){
        int[] A=Init();
        changeArray(A);
        printArray(A);
    }
    public static int[] Init(){
        return new int[]{1,2,3,4,5,6};
    }
    public static void changeArray(int[] A){
        for(int i=0;i<A.length;i++){
            A[i]*=2;
        }
    }
    public static void printArray(int[] data){
        for(int i:data){
            System.out.print(i+" ");
        }
        System.out.println("");
    }
}
```

![11](C:\Users\14665\source\JAVA学习\数组的定义与使用\11.png)

#### JAVA对数组的支持：

**在java中可使用java.util.Arrays.sort(arrayName)命令进行升序排序。**

```java
public class Day12_2{
    public static void main(String[] args){
        int[] A=new int[]{1,6,7,9,201,45,6,99};
        char[] B=new char[]{'t','a','p','o'};
        java.util.Arrays.sort(A);
        java.util.Arrays.sort(B);
        Arrayprint(A);
        Arrayprint(B);
    }
    public static void  Arrayprint(int[] temp){//数组重载
        for(int i:temp){
            System.out.print(i+" ");
        }
        System.out.println("");
    }
    public static void  Arrayprint(char[] temp){
        for(char i:temp){
            System.out.print(i+" ");
        }
        System.out.println("");
    }
}
```

![12](C:\Users\14665\source\JAVA学习\数组的定义与使用\12.png)

在java中可用**java.util.Arrays.copyOf（源数组名称，新数组长度）命令直接实现数组扩容。**

```java
public class Day12_2{
    public static void main(String[] args){
        int[] A=new int[]{1,3,5,7,9};
        int[] B=new int[]{2,4,6,8,10};
        int[] result=java.util.Arrays.copyOf(A,10);
        //使result数组长度为10，并将A数组拷贝至新数组，此时result数组与原数组不是同一块内存，原来的内存还是5个元素，result的是扩容后的空间
        for(int x=0;x<B.length;x++){
            result[A.length+x]=B[x];
        }
        for(int i:result){
            System.out.print(i+" ");
        }
        System.out.print("");//换行
    }
}
```

![13](C:\Users\14665\source\JAVA学习\数组的定义与使用\13.png)

**数组部分拷贝的语句是**

```java
System.arraycopy(源数组名称，源数组开始点，目标数组名称，目标数组开始点，拷贝长度)
```

**但一定要注意的是目标数组拷贝位置确定后是连续拷贝的。**

```java
public class Day12_2{
    public static void main(String[] args){
        int[] A=new int[]{1,2,3,4,5,6,7,8};
        int[] B=new int[]{22,102,45,34,67,89,122};
        System.arraycopy(B,2,A,4,3);//源数组为B（将数组B部分元素拷贝至A数组），拷贝起始点下标为2，即从45开始拷贝；A数组拷贝开始点为下标为4，即从5开始更改数组内容，拷贝长度为3
        for(int i:A){//用foreach语句输出数组A
            System.out.print(i+" ");
        }
        System.out.print("");
    }
}
```

![14](C:\Users\14665\source\JAVA学习\数组的定义与使用\14.png)