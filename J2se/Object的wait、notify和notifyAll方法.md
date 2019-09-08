## Object类的wait、notify和notifyAll方法
- 这三个方法都是Object类的final native修饰的方法，无法被重写
- wait()方法是当前线程阻塞，在调用wait方法前，必须先获取到锁，一般配合synchronized使用，所以一般都是在synchronized代码块里使用wait、notify和notifyAll方法
- 当调用wait方法时，当前线程会释放当前的锁，让出CPU进入等待状态
- notify是唤醒一个wait的线程、notifyAll是唤醒所有的wait线程；

```
package com.durian.helloboot;

/**
 * 类说明：Test Object wait and notify method
 * <p>
 * 详细描述：
 *
 * @author Jiang
 * @since 2019年09月08日
 */
public class ObjectTest {

    private static Object lock = new Object();

    public static void main(String[] args) {
        new Thread(() -> {
            synchronized (lock) {
                try {
                    System.out.println("Thread1 will wait...");
                    lock.wait();
                    System.out.println("Thread1 被唤醒...");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();

        new Thread(() -> {
            synchronized (lock) {
                System.out.println("Thread2 will notify thread1");
                lock.notify();
                System.out.println("Thread2 唤醒 Thread1完毕...");
            }
        }).start();


        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

