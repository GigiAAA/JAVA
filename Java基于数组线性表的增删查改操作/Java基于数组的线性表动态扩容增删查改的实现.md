### [数据结构]Java基于数组的线性表动态扩容增删查改的实现

#### 问题思路：

1.利用接口制定线性表标准；

2.用类实现接口，进行各个方法的实现（对接口中的方法进行覆写）

3.（1）先在类中生成存放元素的数组，规定其默认长度、最大数组长度不可超过int的最大范围及存放元素个数；利用类的无参构造设定数组默认长度，利用类的有参构造可由程序员自行设定初始数组长度。

（2）因类与数组相比，数组更方便增删查改（因数组可直接通过下标进行访问）故我们可先覆写将线性表转为数组这个方法（toArray()方法）；

（3）再覆写全部清空方法（clear()方法），思路为遍历数组，将所有内容全部置为null，最后将元素个数size置为0即可；

（4）再覆写当前线性表元素个数这个方法（size()方法），思路为因size本来就代表数组元素个数，即直接返回即可；

（5）再覆写增加元素这个方法（add()方法），思路为先判断增添该元素会导致数组越界，若在数组允许长度范围内可进行扩容操作（以下代码进行每次两倍扩容机制），若扩容导致数组长度大于int型最大范围，则抛出数组越界异常，之后再存储元素；

（6）再覆写查找指定位置处内容的方法（get()方法），思路为先判断查找位置是否越界或符合规定（数组下标不可小于0），若下标异常直接抛出数组越界异常，若正常则返回数组该位置处内容；

（7）再覆写修改指定位置处内容的方法（set()方法），思路为先判断查找位置是否越界或符合规定（数组下标不可小于0），若下标异常直接抛出数组越界异常，若正常则返回该位置修改前内容，并将该位置内容修改；

（8）再覆写查找有指定内容的方法（contains()方法），思路为要明确指定内容分为两种情况，一种为判断指定内容是否为null，将数组遍历若有元素内容为null，则返回true，否则返回false;另一种是判断指定内容是否与数组某元素内容是否相同，则使用equals()方法进行判断（**此处一定要注意，要将指定内容放置equals()方法前，避免空指针异常错误，因null不能正常使用equals()方法**）

（9）最后覆写删除指定位置处内容方法（remove()方法），思路为首先判断需删除位置是否越界或符合规定（数组下标不可小于0），另外要明确删除位置有两种情况，一种为删除数组中最后一个元素，即只需要将此位置置空即可，另一种情况为删除某一位置处元素后其后面元素要向前移动；但**不论删除哪个位置元素，我们要明确的是最后一个元素位置处一定会被置空。**

#### 源代码如下：

```java
//Sequence.java

/**
 * 线性表规范
 * @author: yuisama
 * @date: 2019-02-28 19:46
 * @description:
 */
public interface Sequence {
    /**
     * 向线性表中添加元素
     * @param data 要存储的元素
     */
    void add(Object data);

    /**
     * 线性表中删除元素
     * @param index 要删除的元素下标
     * @return 是否删除成功
     */
    boolean remove(int index);

    /**
     * 在线性表中查找指定索引的元素
     * @param index 要查找的索引
     * @return
     */
    Object get(int index);

    /**
     * 判断线性表中是否有指定元素
     * @param data 要查找的元素内容
     * @return
     */
    boolean contains(Object data);

    /**
     * 修改线性表中指定索引的内容
     * @param index 要修改的元素下标
     * @param newData 修改后的内容
     * @return
     */
    Object set(int index,Object newData);

    /**
     * 返回当前线性表元素个数
     * @return
     */
    int size();

    /**
     * 直接清空线性表内容
     */
    void clear();

    /**
     * 将线性表转为数组
     * @return
     */
    Object[] toArray();
}



//SequenceArrayImpl.java

import java.util.Arrays;

public class SequenceArrayImpl implements Sequence {
    //存放元素的对象数组
    private Object[] elemenData;
    //默认容量
    private static final int DEFAULT_CAPACITY=10;
    //存放的元素个数
    private int size;
    //线性表的最大容量
    private static final int MAX_CAPACITY=Integer.MAX_VALUE-8;
    public SequenceArrayImpl(){
        //初始化存储元素数组，初始化为10
        this.elemenData=new Object[DEFAULT_CAPACITY];
    }
    public SequenceArrayImpl(int capacity){
        if(capacity>0){
            this.elemenData=new Object[capacity];
        }
    }
    @Override
    public void add(Object data) {
        //首先判断添加元素是否越界
        //若越界先进行扩容，而后存储元素
        ensureCapacityInternal(size+1);
        elemenData[size++]=data;
    }

    @Override
    public boolean remove(int index) {
        rangeCheck(index);
        int moveSteps=size-index-1;
        if(moveSteps>0){
            System.arraycopy(elemenData,index+1,elemenData,index,moveSteps);
        }
        elemenData[--size]=null;
        return true;
    }

    @Override
    public Object get(int index) {
        rangeCheck(index);
        return elemenData[index];
    }

    @Override
    public boolean contains(Object data) {
        //判断是否有指定内容 null
        if(data==null){
            for(int i=0;i<size;i++){
                if(elemenData[i]==null){
                    return true;
                }
            }
        }else{
            for(int i=0;i<size;i++){
                if(data.equals(elemenData[i])){//采用equals()方法判断时一定要将指定内容放置前面，防止空指针异常
                    return true;
                }
            }
        }
        return false;
    }

    @Override
    public Object set(int index, Object newData) {
        rangeCheck(index);
        //取得修改前的内容
        Object oldData=elemenData[index];
        elemenData[index]=newData;
        return oldData;
    }

    @Override
    public int size() {
        return this.size;
    }

    @Override
    public void clear() {
        for(int i=0;i<size;i++){
            elemenData[i]=null;
        }
        this.size=0;
    }

    @Override
    public Object[] toArray() {
        return this.elemenData;
    }
    private void ensureCapacityInternal(int cap){
        //overflow
        if(cap-elemenData.length>0){
            //扩容策略
            grow(cap);
        }
    }
    private void grow(int cap){
        int oldCap=elemenData.length;
        int newCap=oldCap<<1;
        if(cap-newCap>0){
            newCap=cap;
        }
        if(newCap-MAX_CAPACITY>0){
            throw new IndexOutOfBoundsException("线性表超出最大值");
        }
        //数组扩容
        elemenData= Arrays.copyOf(elemenData,newCap);
    }
    private void rangeCheck(int index){
        if(index<0||index>=size){
            throw new ArrayIndexOutOfBoundsException("索引非法");
        }
    }
}



//Test.java

    package JavaIDEAStudy;

    public class Test{
        public static void main(String[] args) {
            Sequence sequence=new SequenceArrayImpl(2) ;//规定数组初始长度为2
            sequence.add(10);//添加数组第一个元素为10
            sequence.add(56);//第二个元素为56
            sequence.add(78);//第三个元素为78（此时扩容）
            sequence.add(null);//第四个元素为null
            System.out.println(sequence.get(2));//查找下标为2时元素内容
            System.out.println(sequence.set(1,89));//修改下标为1时元素内容为89
            System.out.println(sequence.get(1));//查找下标为1时元素内容，确保是否修改成功
            System.out.println(sequence.contains(null));//查找数组中是否有null
            System.out.println(sequence.contains(20));//查找数组中是否有元素为20，遍历数组若数组中存在null，equals()方法前面必须为指定内容，否则会产生空指针异常
            sequence.remove(2);//删除下标为2时的内容
            System.out.println(sequence.get(2));//获取下标为2时的元素内容，确保删除成功
        }
    }

```

![1](C:\Users\14665\Desktop\Java基于数组线性表的增删查改操作\1.png)