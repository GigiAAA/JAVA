### 类集---List接口与Set接口

java类集所有类均在java.util包下。

**java类集本质上是动态数组(当元素个数达到最大值时，动态增加容量)**，在JDK1.2引出。作用为解决数组定长问题。

java集合类框架是加上就是java针对于数据结构的一种实现。

在细谈List接口和Set接口之前我们先来看一下源代码：

```java
//List接口
public interface List<E> extends Collection<E> {}
//Set接口
public interface Set<E> extends Collection<E> {}
```

通过源码可以发现，List接口与Set接口均继承了Collection接口。那么我们先来研究Collection接口：

```java
public interface Collection<E> extends Iterable<E> {}
//迭代器接口
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

我们通过源码可以看到，Collection接口继承了Iterable接口，但需要注意的是Iterable接口是迭代器接口，继承该类的类才可以进行遍历(如for、foreach)。