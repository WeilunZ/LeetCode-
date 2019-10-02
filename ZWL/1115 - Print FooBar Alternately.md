### Description

Suppose you are given the following code:
```Java
class FooBar {
  public void foo() {
    for (int i = 0; i < n; i++) {
      print("foo");
    }
  }

  public void bar() {
    for (int i = 0; i < n; i++) {
      print("bar");
    }
  }
}
```
The same instance of FooBar will be passed to two different threads. Thread A will call foo() while thread B will call bar(). Modify the given program to output "foobar" n times.


### Idea
Use `volatile` keyword to ensure visibility and `Thread.yield()` to prevent long time spinning.

### Code
```Java
class FooBar {
    private int n;
    private volatile Boolean flag = false;
    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            while (flag){
                Thread.yield();
            }
        	// printFoo.run() outputs "foo". Do not change or remove this line.
        	printFoo.run();
            flag=true;
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            while (!flag){
                Thread.yield();
            }
            // printBar.run() outputs "bar". Do not change or remove this line.
        	printBar.run();
            flag=false;
        }
    }
}
```
