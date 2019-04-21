#### 自定义Lock锁

```java
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.AbstractQueuedSynchronizer;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;

class MyLock implements Lock{
    private Sync sync=new Sync();
    //内部类实现AQS，覆写protected修饰的方法即可
    static class Sync extends AbstractQueuedSynchronizer{
        @Override
        //规定arg=1时表示拿到锁
        protected boolean tryAcquire(int arg) {
            if(arg!=1){
                throw new RuntimeException("arg不为1！");
            }
            if(compareAndSetState(0,1)){
                setExclusiveOwnerThread(Thread.currentThread());
                return true;
            }
            return false;
        }

        @Override
        protected boolean tryRelease(int arg) {
            if(arg==0){
                throw new IllegalMonitorStateException();
            }
            setExclusiveOwnerThread(null);
            setState(0);
            return true;
        }

        @Override
        protected boolean isHeldExclusively() {//判断当前线程是否拿到锁
            return getState()==1;
        }
    }
    @Override
    public void lock() {
        sync.acquire(1);
    }

    @Override
    public void lockInterruptibly() throws InterruptedException {
        sync.acquireInterruptibly(1);
    }

    @Override
    public boolean tryLock() {
        return sync.tryAcquire(1);
    }

    @Override
    public boolean tryLock(long time, TimeUnit unit) throws InterruptedException {
        return sync.tryAcquireNanos(1,time);
    }

    @Override
    public void unlock() {
        sync.release(1);
    }

    @Override
    public Condition newCondition() {
        return null;
    }
}
class MyThread implements Runnable{
    Lock lock=new MyLock();
    @Override
    public void run() {
        try{
            lock.lock();
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }finally {
            lock.unlock();
        }
    }
}
public class Test{
    public static void main(String[] args) {
        MyThread mt=new MyThread();
        for(int i=0;i<10;i++){
            Thread thread=new Thread(mt);
            thread.start();
        }
    }
}
```

