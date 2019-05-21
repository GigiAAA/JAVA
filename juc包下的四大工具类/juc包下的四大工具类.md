### juc包下的四大工具类

>CountDownLatch
>
>CyclicBarrier
>
>Exchange
>
>Semaphore

#### 1.CountDownLatch--闭锁

作用为：解决一个线程需等待其他线程完成任务后再次恢复执行的情景。

##### 注意：每个CountDownLatch对象的计数器在减为0时不可恢复原值。

CountDownLatch类常用方法：

```java
//调用await()方法的线程阻塞直到其他线程完成任务才可恢复执行
1.public void await() throws InterruptedException {}
//await()方法的重载方法，作用为等待timeout时间后无论其他线程是否完成任务均恢复执行
2.public boolean await(long timeout, TimeUnit unit) throws InterruptedException {}
//CountDownLatch类唯一的构造方法，count为需要等待完成任务线程数量
3.public CountDownLatch(int count) {}
//该方法为兵法执行任务的线程调用，调用countDown方法相当于已执行完任务，count--，直到count=0时最初调用await()方法的阻塞线程才可恢复执行
4.public void countDown() {}
```

假想一个这样的场景：运动场上，裁判一声哨响，运动员起跑，裁判开始计时，等待最后一位运动员到达终点时裁判才可按下计时器，结束比赛。那么也就是说裁判要等到所有运动员都到达终点后才可按下计时器。那这个场景我们要怎么实现呢？使用之前多线程部分的知识，我们很容易就能想到join()方法，假想裁判是主线程，运动员均是子线程，使用join()方法就需要在所有子线程在主线程中调用join()方法，但线程在调用join()方法的先后顺序上也可能影响运行结果。

针对以上场景，我们可用CountDownLatch完美解决：

```java
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.TimeUnit;

class CountDownLatchTest implements Runnable{
    private CountDownLatch countDownLatch;
    public CountDownLatchTest(CountDownLatch countDownLatch) {
        this.countDownLatch = countDownLatch;
    }
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"到达终点");
        countDownLatch.countDown();
    }
}
public class ThreadTest{
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch=new CountDownLatch(3);
        System.out.println("比赛开始...");
        new Thread(new CountDownLatchTest(countDownLatch),"运动员A").start();
        TimeUnit.SECONDS.sleep(2);
        new Thread(new CountDownLatchTest(countDownLatch),"运动员B").start();
        TimeUnit.SECONDS.sleep(2);
        new Thread(new CountDownLatchTest(countDownLatch),"运动员C").start();
        countDownLatch.await();
        System.out.println("所有运动员均到达终点，比赛结束！");
    }
}

/*
比赛开始...
运动员A到达终点
运动员B到达终点
运动员C到达终点
所有运动员均到达终点，比赛结束！
*/
```

CountDownLatch图示：

![1](C:\Users\14665\source\JAVA学习\juc包下的四大工具类\1.png)

#### 2.CyclicBarrier--循环栅栏

##### 注意：每个CyclicBarrier对象可重复使用。

CyclicBarrier常用方法：

```java
//parties为需阻塞子线程个数
1.public CyclicBarrier(int parties) {}
//parties为需阻塞子线程个数，barrierAction为等所有线程到达同步点后，随机唤醒一个线程执行barrierAction任务，之后再全部线程恢复执行
2.public CyclicBarrier(int parties, Runnable barrierAction) {}
//子线程调用await()方法阻塞当前线程，CyclicBarrier计数器减一，直到减为0后恢复执行
3.public int await() throws InterruptedException, BrokenBarrierException {}
```

假想这样场景：有一本书，三个人分模块同时书写，假设A先写完，此时书不可能直接出版，必须等到剩余两人都写完才可以出版。下面用代码实现此场景：

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.TimeUnit;

class CycliBarrierTest implements Runnable{
    private CyclicBarrier cyclicBarrier;

    public CycliBarrierTest(CyclicBarrier cyclicBarrier) {
        this.cyclicBarrier = cyclicBarrier;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"已到达栅栏");
        try {
            cyclicBarrier.await();
            System.out.println(Thread.currentThread().getName()+"已完成任务");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}
public class ThreadTest{
    public static void main(String[] args) throws InterruptedException {
        CyclicBarrier cyclicBarrier=new CyclicBarrier(3);
        for(int i=0;i<3;i++){
            new Thread(new CycliBarrierTest(cyclicBarrier),"线程"+(i+1)).start();
            TimeUnit.SECONDS.sleep(2);
        }
    }
}
/**
线程1已到达栅栏
线程2已到达栅栏
线程3已到达栅栏
线程3已完成任务
线程1已完成任务
线程2已完成任务
**/

import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.TimeUnit;

class CycliBarrierTest implements Runnable{
    private CyclicBarrier cyclicBarrier;

    public CycliBarrierTest(CyclicBarrier cyclicBarrier) {
        this.cyclicBarrier = cyclicBarrier;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()+"已到达栅栏");
        try {
            cyclicBarrier.await();
            System.out.println(Thread.currentThread().getName()+"已完成任务");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}
public class ThreadTest{
    public static void main(String[] args) throws InterruptedException {
        CyclicBarrier cyclicBarrier=new CyclicBarrier(3, new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName()+"执行barrierAction任务");
            }
        });
        for(int i=0;i<3;i++){
            new Thread(new CycliBarrierTest(cyclicBarrier),"线程"+(i+1)).start();
            TimeUnit.SECONDS.sleep(2);
        }
    }
}
/**
线程1已到达栅栏
线程2已到达栅栏
线程3已到达栅栏
线程3执行barrierAction任务
线程1已完成任务
线程2已完成任务
线程3已完成任务
**/
```

CyclicBarrier图示：

![2](C:\Users\14665\source\JAVA学习\juc包下的四大工具类\2.png)

#### 3.Exchanger---线程数据交换器

##### 应用场景：用于两个线程交换数据(不能用于多个)

常用方法：

```java
public V exchange(V x) throws InterruptedException {}//调用exchange()方法会阻塞当前线程，必须等到另外一个线程时才可进行数据交换
```

Exchanger简单实现：

```java
import java.util.concurrent.Exchanger;

public class ThreadTest{
    public static void main(String[] args) {
        Exchanger<String> exchanger=new Exchanger<>();
        Thread girl=new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    String str=exchanger.exchange("我喜欢你！");
                    System.out.println("女孩说："+str);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        girl.start();
        Thread boy=new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    String str=exchanger.exchange("我也是！");
                    System.out.println("男孩说："+str);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        boy.start();
    }
}
/**
男孩说：我喜欢你！
女孩说：我也是！
**/
```

Exchange图示：

![3](C:\Users\14665\source\JAVA学习\juc包下的四大工具类\3.png)

#### 4.Semaphore---信号量

常用方法：

```java
//调用acquire()方法默认为申请一个信号量
1.public void acquire() throws InterruptedException {}
//permits为申请信号量数
2.public void acquire(int permits) throws InterruptedException {}
//默认释放一个信号量
3.public void release() {}
//释放permits个信号量
4.public void release(int permits) {}
```

假想一个这样的场景：一个工厂有8个工人5台机器，每台机器最多只能一个人进行操作，因此最多5人工作。

以下为Semaphore实现以上情景：

```java
import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

class SemaphoreTest implements Runnable{
    private Semaphore semaphore;
    private Integer num;

    public SemaphoreTest(Semaphore semaphore, Integer num) {
        this.semaphore = semaphore;
        this.num = num;
    }

    @Override
    public void run() {
        try {
            semaphore.acquire();
            System.out.println("工人"+this.num+"占用一台设备");
            TimeUnit.SECONDS.sleep(2);
            System.out.println("工人"+this.num+"释放本设备");
            semaphore.release();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
public class ThreadTest{
    public static void main(String[] args) {
        Semaphore semaphore=new Semaphore(5);
        //8个工人，5台机器
        for(int i=0;i<8;i++){
            new Thread(new SemaphoreTest(semaphore,i+1)).start();
        }
    }
}

/**
工人1占用一台设备
工人2占用一台设备
工人3占用一台设备
工人4占用一台设备
工人5占用一台设备
工人2释放本设备
工人4释放本设备
工人5释放本设备
工人3释放本设备
工人8占用一台设备
工人1释放本设备
工人7占用一台设备
工人6占用一台设备
工人8释放本设备
工人7释放本设备
工人6释放本设备
**/


import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

class SemaphoreTest implements Runnable{
    private Semaphore semaphore;
    private Integer num;

    public SemaphoreTest(Semaphore semaphore, Integer num) {
        this.semaphore = semaphore;
        this.num = num;
    }

    @Override
    public void run() {
        try {
            semaphore.acquire(2);
            System.out.println("工人"+this.num+"占用一台设备");
            TimeUnit.SECONDS.sleep(2);
            System.out.println("工人"+this.num+"释放本设备");
            semaphore.release(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
public class ThreadTest{
    public static void main(String[] args) {
        Semaphore semaphore=new Semaphore(5);
        //8个工人，5台机器
        for(int i=0;i<8;i++){
            new Thread(new SemaphoreTest(semaphore,i+1)).start();
        }
    }
}
/**
工人2占用一台设备
工人3占用一台设备
工人2释放本设备
工人4占用一台设备
工人3释放本设备
工人1占用一台设备
工人1释放本设备
工人4释放本设备
工人5占用一台设备
工人6占用一台设备
工人6释放本设备
工人5释放本设备
工人7占用一台设备
工人8占用一台设备
工人8释放本设备
工人7释放本设备
**/
```

Semaphore图示：

![4](C:\Users\14665\source\JAVA学习\juc包下的四大工具类\4.png)