### ArrayList、Vector、LinkedList的区别

#### ArrayList与Vector的区别：

1.**出现版本**：

ArrayList：JDK1.2

Vector(老版动态数组实现类):JDK1.0、出现在ArrayList、Collection接口之前

2.**无参构造实现（初始化策略不同）**：

**ArrayList:采用懒加载策略，在构造方法阶段并不初始化数组，而是在第一次添加元素时才初始化对象数组大小为默认长度10。**

ArrayList无参构造源码分析：

```java
//调用无参构造后并不是直接开辟数组空间，而是在第一次添加元素时开辟空间（延迟策略）
public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
//第一次向数组中增加元素调用add()方法
public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // 设置默认数组长度为10
        elementData[size++] = e;//将数据添加至数组
        return true;
    }
//ensureCapacityInternal实现
private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);//取默认值与size+1的最大值
        }
        ensureExplicitCapacity(minCapacity);
    }
private static final int DEFAULT_CAPACITY = 10;//默认数组长度
```

**Vector：在调用无参构造后直接将数组大小初始化为10.**

Vector无参构造源码分析：

```java
public Vector() {
        this(10);
    }
```

##### 3.扩容策略

**ArrayList:扩容时新数组大小变为原数组的1.5倍。**

```java
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);//扩容为原来的1.5倍
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

**Vector：扩容时新数组大小变为原数组的2倍。**

```java
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        //capacityIncrement默认小于0，故实际上是oldCapacity+oldCapacity，扩容2倍
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

##### 4.线程安全性

**ArrayList:异步处理，线程不安全，但效率较高。**

```java
 public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```

**Vector:同步处理，线程安全，但效率较低。**

```java
//在方法上加锁，线程安全但效率较低。（即使使用线程安全的List也不用Vector）
public synchronized boolean add(E e) {
        modCount++;
        ensureCapacityHelper(elementCount + 1);
        elementData[elementCount++] = e;
        return true;
    }
```

**5.遍历**

Vector支持较老的迭代器Enumeration，ArrayList不支持，LinkedList支持。

#### ArrayList、Vector的共同点：

**底层均是使用数组实现。**

#### ArrayList与LinkedList区别：

**均是异步处理。LinkedList底层使用双向链表实现，ArrayList底层使用数组实现。**

```java
public boolean add(E e) {//LinkedList
        linkLast(e);
        return true;
    }
```

