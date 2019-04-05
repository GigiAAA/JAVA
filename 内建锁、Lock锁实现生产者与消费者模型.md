### 内建锁、Lock锁实现生产者与消费者模型

内建锁(synchronized)实现代码：

synchornized必须在同步代码块或同步方法中使用。

```java
import java.util.ArrayList;
import java.util.List;

class Goods{
    private String goodsName;
    private Integer count=0;
    //最大库存量
    private Integer full;

    public Goods(Integer full) {
        this.full = full;
    }
    public synchronized void setGoods(String goodsName){
        //当生产量等于最大库存量时，生产者等待
        while (this.count==this.full){
            System.out.println("商品库存剩余很多，快点来购买哦");
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        this.goodsName=goodsName;
        this.count++;
        System.out.println(Thread.currentThread().getName()+"生产"+this.toString());
        //唤醒消费者购买
        notifyAll();
    }
    public synchronized void getGoods(){
        //当产品数量等于0时消费者等待
        while (this.count==0){
            System.out.println("正在生产中，亲稍等哦");
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        this.count--;
        System.out.println(Thread.currentThread().getName()+"消费"+this.toString());
        //唤醒生产者生产
        notifyAll();
    }

    @Override
    public String toString() {
        return "Goods{" +
                "goodsName='" + goodsName + '\'' +
                ", count=" + count +
                '}';
    }
}
//生产者类
class Productor implements Runnable{
    private Goods goods;

    public Productor(Goods goods) {
        this.goods = goods;
    }

    @Override
    public void run() {
        while (true){
            this.goods.setGoods("mac");
        }
    }
}
//消费者类
class Customer implements Runnable{
    private Goods goods;

    public Customer(Goods goods) {
        this.goods = goods;
    }

    @Override
    public void run() {
        while (true){
            this.goods.getGoods();
        }
    }
}
public class Test{
    public static void main(String[] args) {
        Goods goods=new Goods(20);
        Productor productor=new Productor(goods);
        Customer customer=new Customer(goods);
        List<Thread> list=new ArrayList<>();
        for(int i=0;i<5;i++){
            Thread thread=new Thread(productor,"生产者"+i);
            list.add(thread);
        }
        for(int i=0;i<10;i++){
            Thread thread=new Thread(customer,"消费者"+i);
            list.add(thread);
        }
        for(Thread thread:list){
            thread.start();
        }
    }
}
```

Lock锁实现生产者与消费者模型源码：

lock锁必须搭配try...finally使用（必须在finally代码块中解锁）。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Goods{
    private String goodsName;
    private Integer count=0;
    private Integer full;
    //获得lock对象
    private Lock lock=new ReentrantLock();
    //获得生产者等待队列对象
    private Condition productorCondition=lock.newCondition();
    //获得消费者等待队列对象
    private Condition customerCondition=lock.newCondition();
    public Goods(Integer full) {
        this.full = full;
    }
    public void setGoods(String goodsName){
        try{
            lock.lock();//加锁
            while (this.count==this.full){
                System.out.println("商品库存很多，快来购买哦");
                try {
                    productorCondition.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            this.goodsName=goodsName;
            this.count++;
            System.out.println(Thread.currentThread().getName()+"生产"+this.toString());
            customerCondition.signalAll();
        }finally {
            lock.unlock();//解锁
        }
    }
    public void getGoods(){
        try {
            lock.lock();
            while (this.count==0){
                System.out.println("商品正在生产中，亲稍等哦");
                try {
                    customerCondition.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            this.count--;
            System.out.println(Thread.currentThread().getName()+"消费"+this.toString());
            productorCondition.signalAll();
        }finally {
            lock.unlock();
        }
    }

    @Override
    public String toString() {
        return "Goods{" +
                "goodsName='" + goodsName + '\'' +
                ", count=" + count +
                '}';
    }
}
class Productor implements Runnable{
    private Goods goods;

    public Productor(Goods goods) {
        this.goods = goods;
    }

    @Override
    public void run() {
        while (true){
            this.goods.setGoods("mac");
        }
    }
}
class Customer implements Runnable{
    private Goods goods;

    public Customer(Goods goods) {
        this.goods = goods;
    }

    @Override
    public void run() {
        while (true){
            this.goods.getGoods();
        }
    }
}
public class Test{
    public static void main(String[] args) {
        Goods goods=new Goods(20);
        Productor productor=new Productor(goods);
        Customer customer=new Customer(goods);
        List<Thread> list=new ArrayList<>();
        for(int i=0;i<5;i++){
            Thread thread=new Thread(productor,"生产者"+i);
            list.add(thread);
        }
        for(int i=0;i<10;i++){
            Thread thread=new Thread(customer,"消费者"+i);
            list.add(thread);
        }
        for(Thread thread:list){
            thread.start();
        }
    }
}
```

内建锁、Lock锁实现生产者与消费者模型对比：

建议使用Lock锁，因Lock锁在全部实现内建锁功能的基础上，还实现多个等待队列(一个队列中存放相同属性的线程，便于管理)、不响应中断及可设置截止时间功能，且Lock锁有手动的加锁解锁过程，更便于使用。