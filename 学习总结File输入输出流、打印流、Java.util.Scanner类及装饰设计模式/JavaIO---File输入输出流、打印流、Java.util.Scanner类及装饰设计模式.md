## JavaIO---File输入输出流、打印流、Java.util.Scanner类及装饰设计模式

#### IO基于抽象类。

>File文件操作类（字节输入输出流、字符输入输出流）
>
>字符编码
>
>转换流
>
>内存流
>
>打印流
>
>System对IO类的支持
>
>两种输入流

### File文件操作类

在Java.io包中，File类是唯一一个与文件本身操作（创建、删除、获取信息等）有关的程序类。

**File类不包括文件内容，它是文件本身，文件的读取与写入需要使用输入输出流。**

#### 1.1File类的使用--文件/文件夹

##### 文件：

```java
//File类源码
public class File implements Serializable, Comparable<File>{}
```

由源码可得File类是Java.io包中的一个普通类，我们可以直接对其实例化对象。实例化对象可用到以下构造方法：

```java
1.public File(String pathname){};//根据文件的绝对路径产生File对象
```

创建一个新文件：

```java
public boolean createNewFile() throws IOException {}//根据绝对路径创建新文件
```

删除已有文件：

```java
public boolean delete() {}//删除已存在文件
```

判断一个文件是否存在：

```java
public boolean exists() {}//判断指定路径文件是否存在
```

判断是否为文件：

```java
public boolean isFile() {}
```

路径分隔符：出现原因是在Windows下路径分隔符通常为“//”,linux操作系统下通常为"/".故使用路径分隔符可更好的达到跨平台性。

```java
public static final String separator = "" + separatorChar;
//使用时为File.separator(因separator在File类中为静态属性可通过类名直接调用)
```

若为文件即可获得文件大小：

```java
 public long length() {}//获得当前路径下的文件大小（字节）通常使用时/1024（KB）
```

获取文件最新修改时间：

```java
public long lastModified() {}//获得当前路径文件最新修改时间，但要结合Date类使用，否则直接调用得到的是从1970年1月1日至文件最新修改时间的差值（无意义）
```

对以上方法的简单实用：

```java
import java.io.*;
import java.text.SimpleDateFormat;
import java.util.Date;

public class Test{
    public static void main(String[] args) throws Exception {
        //在桌面查找名称为lala.jpg的文件
        File file=new File("C:"+File.separator+"Users"
                +File.separator+"14665"+File.separator+"Desktop"+File.separator+"lala.jpg");
        //判断当前路径下文件是否存在,不存在就创建
        if(!file.exists()){
            file.createNewFile();
        }
        //获取文件大小
        System.out.println(file.length()/1024);
        //获得文件最新修改时间(三种方法)
        //1
        System.out.println(file.lastModified());
        //2
        System.out.println(new Date(file.lastModified()));
        //3
        Date date=new Date(file.lastModified());
        SimpleDateFormat simpleDateFormat=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println(simpleDateFormat.format(date));
    }
}
```

![1](C:\Users\14665\source\JAVA学习\学习总结File输入输出流、打印流、Java.util.Scanner类及装饰设计模式\1.png)

##### 文件夹（目录）：

创建目录（一次性创建多级不存在的父路径）：

```java
public boolean mkdirs() {}
```

取得父路径/父路径File对象：

```java
public String getParent(){};//获取父路径
public File getParentFile(){};//获得父路径File对象
```

判断是否为文件夹：

```java
public boolean isDirectory() {}
```

获得当前目录下所有文件：

```java
public File[] listFiles() {}//但直接调用该方法只能得到当前路径下的文件及目录，若想进一步得到该路径下所有目录的文件就需采用递归方法
```

创建目录方法简单使用：

```java
import java.io.*;

public class Test{
    public static void main(String[] args) throws Exception {
        //在桌面创建新目录
        File file=new File("C:"+File.separator+"Users"
                +File.separator+"14665"+File.separator+"Desktop"+File.separator
                + "www"+File.separator+"ss"+File.separator+"hello.txt");
        //判断该文件夹是否存在
        if(!file.getParentFile().exists()){
            file.getParentFile().mkdirs();
            //若该处为file.mkdirs()那么hello.txt也会被创建为文件夹
        }
        if(file.exists()){
            file.delete();
        }else {
            file.createNewFile();
        }
    }
}
```

![2](C:\Users\14665\source\JAVA学习\学习总结File输入输出流、打印流、Java.util.Scanner类及装饰设计模式\2.png)

查找桌面路径下所有文件：

```java
import java.io.*;

public class Test{
    public static void main(String[] args) throws Exception {
        File file=new File("C:"+File.separator+"Users"
                +File.separator+"14665"+File.separator+"Desktop");
        //测试所需时间
        long start=System.currentTimeMillis();
        listAllFiles(file);
        long end=System.currentTimeMillis();
        System.out.println("共耗时："+(end-start)+"毫秒");
    }
    public static void listAllFiles(File file){
        if(file.isFile()){//递归出口（若为文件直接输出）
            System.out.println(file);
        }else{
            if(file.exists()&&file.isDirectory()){//判断文件是否存在且是否为文件夹
                File[] files=file.listFiles();
                for(File file1:files){//对文件夹递归
                    listAllFiles(file1);
                }
            }
        }
    }
}
//924毫秒
```

对此我们可以看到只是查找桌面就耗时较多，那么查找C盘呢？时间会更久。出现这种情况的原因是主线程堵塞，因所有程序运行都在主线程中，如果listAllFiles()方法没有完成，那么main后续的执行将无法完成。这种情况造成了主线程堵塞，**故对此我们可以新建立一个线程对其优化。**

```java
import java.io.*;

public class Test{
    public static void main(String[] args) throws Exception {
        File file=new File("C:"+File.separator+"Users"
                +File.separator+"14665"+File.separator+"Desktop");
        //将IO操作放在子线程中进行
        new Thread(()->{
            long start=System.currentTimeMillis();
            listAllFiles(file);
            long end=System.currentTimeMillis();
            System.out.println("共耗时："+(end-start)+"毫秒");
        }).start();
        System.out.println("主线程结束");
    }
    public static void listAllFiles(File file){
        if(file.isFile()){
            System.out.println(file);
        }else{
            if(file.exists()&&file.isDirectory()){
                File[] files=file.listFiles();
                for(File file1:files){
                    listAllFiles(file1);
                }
            }
        }
    }
}
```

**一般来说，将IO操作放置子线程中进行。原因是这样IO操作就不影响主线程，且安卓开发中IO操作必须放在子线程中运行否则错误，虽然java中IO操作放置主线程中也可正常运行，但为了更好的跨平台性我们将IO操作放置子线程中运行。**

#### 1.2字节流与字符流

我们知道File类只与文件本身有关，若想进行文件读取或写入必须使用输入输出流。在Java.io包中，流分为字节流与字符流：

```java
1.字节流：输入流：InputStream,输出流：OutputStream(均为抽象类)
public abstract class InputStream implements Closeable {}//实现资源关闭流接口
Closeable(资源关闭流接口):public interface Closeable extends AutoCloseable {}//资源关闭流接口继承自动关闭流接口
AutoCloseable（自动关闭流）：public interface AutoCloseable {}
//实现资源关闭流接口与强制刷新缓冲区接口
public abstract class OutputStream implements Closeable, Flushable {}
Flushable（强制刷新缓冲区）：public interface Flushable {}
2.字符流：输入流：Reader,输出流：Writer
public abstract class Reader implements Readable, Closeable {}
public interface Readable {}
//Attempts to read characters into the specified character buffer.
public abstract class Writer implements Appendable, Closeable, Flushable {}
public interface Appendable {}
//Appends the specified character sequence to this <tt>Appendable</tt>.
```

**字节输出流（从程序中向文件中写入数据）OutputStream：**

OutputStream类中其他常用方法：

```java
1.public void write(byte b[]) throws IOException {}//将给定字节数组内容全部输出（直接输出到终端（文件、网络、内存中，不存在缓冲区））
2.public void write(byte b[], int off, int len) throws IOException {}//将部分字节数组内容输出(将给定字节数组从off位置开始输出len长度后停止)
3.public abstract void write(int b) throws IOException;//输出单个字节
```

我们知道OutputStream类为抽象类不可直接实例化对象，故要想实例化必须使用子类。如果要进行文件操作，可以使用FileOutputStream类来处理：

```java
1.public FileOutputStream(String name) throws FileNotFoundException {}//覆写
2.public FileOutputStream(String name, boolean append)throws FileNotFoundException{}//叠加
```

方法简单操作：

```java
import java.io.*;

public class Test{
    public static void main(String[] args) throws Exception {
        //1.获取终端对象
        File file = new File("C:" + File.separator + "Users"
                + File.separator + "14665" + File.separator + "Desktop"+File.separator+"TestIO.txt");
        //必须保证父目录存在
        if(!file.getParentFile().exists()){
            file.getParentFile().mkdirs();
        }
        //2.获得输出流
       OutputStream out=new FileOutputStream(file);
        //3.数据写入
        String str="hello";
        out.write(str.getBytes());
        //4.关闭流
        out.close();
    }
}
```

![3](C:\Users\14665\source\JAVA学习\学习总结File输入输出流、打印流、Java.util.Scanner类及装饰设计模式\3.png)

若将输出流实例化处改为叠加，

```java
import java.io.*;

public class Test{
    public static void main(String[] args) throws Exception {
        //1.获取终端对象
        File file = new File("C:" + File.separator + "Users"
                + File.separator + "14665" + File.separator + "Desktop"+File.separator+"TestIO.txt");
        //必须保证父目录存在
        if(!file.getParentFile().exists()){
            file.getParentFile().mkdirs();
        }
        //2.获得输出流(叠加)
       OutputStream out=new FileOutputStream(file,true);
        //3.数据写入
        String str="hello";
        out.write(str.getBytes(),0,3);
        //4.关闭流
        out.close();
    }
}
```

![4](C:\Users\14665\source\JAVA学习\学习总结File输入输出流、打印流、Java.util.Scanner类及装饰设计模式\4.png)

**如上代码简单使用后，我们可以发现在使用字节输出流时指定路径文件不存在时无需手动调用createNewFile()方法创建文件，而是系统自动生成文件(但必须保证该文件父目录存在)，若父目录不存在生成即可。**

**字节输入流（在程序中读取文件中数据）InputStream：**

InputStream类中其他常用方法：

```java
1.public int read(byte b[]) throws IOException {};//读取数据到字节数组中并返回数据的读取个数
该返回值有三种情况：
a.返回b.length。此种情况为未读取数据>存放的缓冲区大小，返回字节数组的大小
b.返回一个大于0小于b.length的数。此种情况为未读取数据<存放的缓冲区大小，返回实际数据大小
c.返回-1。终止标记，表示此时数据已读取完毕。
2.public int read(byte b[], int off, int len) throws IOException {}；//读取部分数据到字节数组中，该返回值有三种情况：
a.返回len。若读取满了则返回长度len
b.返回实际数据个数。若没有读取满则返回读取的数据个数
c.返回-1。若读取到最后没有数据了返回-1
3.public abstract int read() throws IOException;//每次读取一个字节，知道没有数据返回-1
```

我们也知道InputStream为抽象类，进行文件操作时实例化该类需要借助FileInputStream类。

```java
import java.io.*;

public class Test{
    public static void main(String[] args) throws Exception {
        //1.获取终端对象
        File file = new File("C:" + File.separator + "Users"
                + File.separator + "14665" + File.separator + "Desktop"+File.separator+"TestIO.txt");
        //必须保证该文件存在才可读取数据
        if(file.exists()&&file.isFile()){
            //2.获得输入流
            InputStream in=new FileInputStream(file);
            //3.数据写入
            byte[] data=new byte[1024];
            int len=in.read(data);
            System.out.println(new String(data,0,len));
            //4.关闭流
            in.close();
        }
    }
}
//hellohel
```

