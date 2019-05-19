### 数组、链表两种方式实现Stack

#### 数组：

```java
package Stack_Array_Link;

public interface StackTest<T> {
    /**
     * 入栈
     * @param t
     * @return
     */
    boolean push(T t);

    /**
     * 出栈
     * @return
     */
    T pop();

    /**
     * 返回栈顶元素
     * @return
     */
    T peek();

    /**
     * 返回栈大小
     * @return
     */
    int size();

    /**
     * 返回栈是否为空
     * @return
     */
    boolean isEmpty();
}

package Stack_Array_Link;

import java.util.Arrays;

public class ArrayStackTest<T> implements StackTest<T> {
    private Object[] arr;
    private Integer currentSize=0;
    private Integer maxSize;

    public ArrayStackTest(Integer maxSize) {
        this.maxSize = maxSize;
        arr=new Object[maxSize];
    }

    @Override
    public boolean push(T t) {
        if(currentSize==maxSize){
            int oldSize=maxSize;
            int newSize=oldSize<<1;
            if(newSize+8>Integer.MAX_VALUE){
                System.out.println("超出范围");
                return false;
            }
            arr=Arrays.copyOf(arr,newSize);
        }
        arr[currentSize++]=t;
        return true;
    }

    @Override
    public T pop() {
        if(isEmpty()){
            System.out.println("栈为空");
            return null;
        }
        return (T) arr[--currentSize];
    }

    @Override
    public T peek() {
        if(isEmpty()){
            System.out.println("栈为空");
            return null;
        }
        return (T) arr[currentSize-1];
    }

    @Override
    public int size() {
        return currentSize;
    }

    @Override
    public boolean isEmpty() {
        return currentSize==0;
    }
}

package Stack_Array_Link;

public class Test {
    public static void main(String[] args) {
        StackTest<Integer> stackTest=new ArrayStackTest<>(5);
        stackTest.push(1);
        stackTest.push(2);
        stackTest.push(3);
        stackTest.push(4);
        stackTest.push(5);
        stackTest.push(6);
        System.out.println(stackTest.peek());
        System.out.println(stackTest.pop());
        System.out.println(stackTest.size());
        System.out.println(stackTest.peek());
        System.out.println(stackTest.isEmpty());
    }
}

```



#### 链表：

```java
package Stack_Array_Link;

public interface StackTest<T> {
    /**
     * 入栈
     * @param t
     * @return
     */
    boolean push(T t);

    /**
     * 出栈
     * @return
     */
    T pop();

    /**
     * 返回栈顶元素
     * @return
     */
    T peek();

    /**
     * 返回栈大小
     * @return
     */
    int size();

    /**
     * 返回栈是否为空
     * @return
     */
    boolean isEmpty();
}

package Stack_Array_Link;

public class LinkedStackTest<T> implements StackTest<T> {
    private Integer currentSize=0;
    private class Node{
        private T t;
        private Node next;
        public Node(T t, Node next) {
            this.t = t;
            this.next = next;
        }
    }
    private Node top=null;
    @Override
    public boolean push(T t) {
        Node newNode=new Node(t,null);
        if(top==null){
            top=newNode;
        }else {
            newNode.next=top;
            top=newNode;
        }
        currentSize++;
        return true;
    }

    @Override
    public T pop() {
        if(top==null){
            System.out.println("栈为空");
            return null;
        }
        T result=top.t;
        top=top.next;
        currentSize--;
        return result;
    }

    @Override
    public T peek() {
        if(top==null){
            System.out.println("栈为空");
            return null;
        }
        return top.t;
    }

    @Override
    public int size() {
        return currentSize;
    }

    @Override
    public boolean isEmpty() {
        return currentSize==0;
    }
}

package Stack_Array_Link;

public class Test {
    public static void main(String[] args) {
        StackTest<Integer> stackTest=new LinkedStackTest<>();
        stackTest.push(1);
        stackTest.push(2);
        stackTest.push(3);
        stackTest.push(4);
        stackTest.push(5);
        stackTest.push(6);
        System.out.println(stackTest.peek());
        System.out.println(stackTest.pop());
        System.out.println(stackTest.size());
        System.out.println(stackTest.peek());
        System.out.println(stackTest.isEmpty());
    }
}
```

