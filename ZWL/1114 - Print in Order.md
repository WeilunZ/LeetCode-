### Description

Suppose we have a class:
```
public class Foo {
  public void first() { print("first"); }
  public void second() { print("second"); }
  public void third() { print("third"); }
}
```
The same instance of Foo will be passed to three different threads. Thread A will call first(), thread B will call second(), and thread C will call third(). Design a mechanism and modify the program to ensure that second() is executed after first(), and third() is executed after second().

> From：LeetCode(https://leetcode-cn.com/problems/print-in-order)

### Idea
* Use two `Semaphores` which are initialized to zero. The second method and the third must acquire the corresponding semaphore to run.
* Or use `CountDownLatch`.

### Code (Semaphore version)
```Java
import java.util.concurrent.Semaphore;

class Foo {
    private Semaphore sem1 = new Semaphore(0);
    private Semaphore sem2 = new Semaphore(0);
    
    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
            
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        sem1.release();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        sem1.acquire();
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        sem2.release();
    }

    public void third(Runnable printThird) throws InterruptedException {
        sem2.acquire();
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}
```

### Code (CountDownLatch version)

```Java
class Foo {

    private CountDownLatch second = new CountDownLatch(1);
    private CountDownLatch third = new CountDownLatch(1);
    
    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
            
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        second.countDown();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        second.await();
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        third.countDown();
    }

    public void third(Runnable printThird) throws InterruptedException {
        third.await();
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}
```

