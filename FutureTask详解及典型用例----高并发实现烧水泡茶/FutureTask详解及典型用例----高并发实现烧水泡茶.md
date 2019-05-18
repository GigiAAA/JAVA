### FutureTask详解及典型用例----高并发实现烧水泡茶

Future接口：作用为获取Callable接口的返回值

FutureTask类为Future接口子类，该类**独有的特点为在高并发情况下不论有多少个线程，均只执行一次任务。**

**get()方法的作用为阻塞当前线程直到有返回值为止。**

使用get()方法的两种情况：

(1)提交线程的同时调用get()方法

```java
class CallableTest implements Callable<String>{
    private static Integer ticket=10;
    @Override
    public String call() throws Exception {
        while (ticket>0){
            System.out.println(Thread.currentThread().getName()+"还剩"+ticket--+"张票");
        }
        return "票已经卖完啦";
    }
}
public class ThreadTest{
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService service=Executors.newCachedThreadPool();
        Callable<String> callable=new CallableTest();
        for(int i=0;i<5;i++){
           String str=service.submit(callable).get();
           System.out.println(str);
        }
        service.shutdown();
    }
}
/**
pool-1-thread-1还剩10张票
pool-1-thread-1还剩9张票
pool-1-thread-1还剩8张票
pool-1-thread-1还剩7张票
pool-1-thread-1还剩6张票
pool-1-thread-1还剩5张票
pool-1-thread-1还剩4张票
pool-1-thread-1还剩3张票
pool-1-thread-1还剩2张票
pool-1-thread-1还剩1张票
票已经卖完啦
票已经卖完啦
票已经卖完啦
票已经卖完啦
票已经卖完啦
**/
```

由以上代码我们可以看到，get()方法阻塞了主线程，线程1获得锁后执行完任务并返回返回值，其他线程不执行任务只返回返回值。

(2)提交线程之后调用get()方法

```java
class CallableTest implements Callable<String>{
    private static Integer ticket=10;
    @Override
    public String call() throws Exception {
        while (ticket>0){
            System.out.println(Thread.currentThread().getName()+"还剩"+ticket--+"张票");
        }
        return "票已经卖完啦";
    }
}
public class ThreadTest{
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService service=Executors.newCachedThreadPool();
        Callable<String> callable=new CallableTest();
        Future<String> future=null;
        for(int i=0;i<5;i++){
           future=service.submit(callable);
        }
        String str=future.get();//阻塞主线程，高并发执行任务，不安全
        System.out.println(str);
        service.shutdown();
    }
}
/**
pool-1-thread-1还剩10张票
pool-1-thread-2还剩9张票
pool-1-thread-1还剩8张票
pool-1-thread-2还剩7张票
pool-1-thread-3还剩5张票
pool-1-thread-1还剩6张票
pool-1-thread-3还剩3张票
pool-1-thread-2还剩4张票
pool-1-thread-3还剩1张票
pool-1-thread-1还剩2张票
票已经卖完啦
**/
```

#### FutureTask典型用例----高并发实现烧水泡茶实现流程图：

![1](C:\Users\14665\Desktop\FutureTask详解及典型用例----高并发实现烧水泡茶\1.png)

解题思路：

通过流程图的分析我们不难看出，烧水与洗茶壶、洗茶杯、拿茶叶是可以并行的，两线程在泡茶处有交集。我们可以将横行看作线程1，竖行看作线程2，因此线程1必须等待线程2执行完毕后才可执行泡茶操作(泡茶必须在拿到茶叶之后)，故需在线程1中调用线程2的get方法对线程1进行阻塞，直到线程2完成任务后方可泡茶。

实现源代码：

```java
//执行线程1任务
class Task1 implements Callable<String>{
    private FutureTask<String> futureTask;
    public Task1(FutureTask<String> futureTask) {//通过构造方法将线程2传入
        this.futureTask = futureTask;
    }
    @Override
    public String call() throws Exception {
        System.out.println("T1:洗水壶...");
        TimeUnit.SECONDS.sleep(1);
        System.out.println("T1:烧开水...");
        TimeUnit.SECONDS.sleep(15);
        String str=futureTask.get();//调用线程2的get()方法用于阻塞当前线程直到线程2任务结束
        System.out.println("T1：泡茶");
        return "上茶";
    }
}
//执行线程2任务
class Task2 implements Callable<String>{

    @Override
    public String call() throws Exception {
        System.out.println("T2:洗茶壶");
        TimeUnit.SECONDS.sleep(1);
        System.out.println("T2:洗茶杯");
        TimeUnit.SECONDS.sleep(2);
        System.out.println("T2:拿茶叶");
        TimeUnit.SECONDS.sleep(1);
        return "上好龙井";
    }
}
public class ThreadTest{
    public static void main(String[] args) throws ExecutionException, InterruptedException {
       FutureTask<String> ft2=new FutureTask<>(new Task2());
       FutureTask<String> ft1=new FutureTask<>(new Task1(ft2));
       new Thread(ft1).start();
       new Thread(ft2).start();
       System.out.println(ft1.get());//用于阻塞主线程
    }
}

/**
T1:洗水壶...
T2:洗茶壶
T1:烧开水...
T2:洗茶杯
T2:拿茶叶
T1：泡茶
上茶
**/
```

