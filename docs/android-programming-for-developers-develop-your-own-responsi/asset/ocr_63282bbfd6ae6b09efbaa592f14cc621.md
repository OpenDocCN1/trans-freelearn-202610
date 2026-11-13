To achieve this correctness, we can make use of the synchronized construct to solve the correctness issue on the following piece of code:

```
Object lock = new Object();
public class MyThread extends Thread {
    public void run() {
        super.run();
        synchronized(lock) {
            myInt++;
        }
    }
}
```

In the preceding code, we used the intrinsic lock available in each Java object to create a mutually exclusive scope of code that will enforce that the increment sentence will work properly and will not suffer from correctness issues as explained previously. When one of the threads gets access to the protected scope, it is said that the thread acquired the lock, and after the thread gets out of the protected scope, it releases the lock that could be acquired by another thread.

Another way to create mutually exclusive scopes is to create a method with a synchronized method:

```
int myInt = 2;
synchronized void increment(){
    myInt++;
}
...
public class IncrementThread extends Thread {
    public void run() {
        super.run();
        incremnt();
    }
}
```

The synchronized method will use the object-intrinsic lock, where myInt is defined to create a mutually exclusive zone so IncrementThread, incrementing myInt through the increment(), will prevent any thread interference and memory consistency errors.