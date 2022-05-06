---
description: 理解多线程程序设计的概念；掌握多线程的创建、生命周期、调度和控制；了解线程的同步
---

# 第四章\~高级编程基础

## 4.1 多线程的概念

{% hint style="info" %}
线程是进程的基本执行单元，多线程是指在一个进程中多个线程并发执行，并发<mark style="color:purple;">不是同时执行</mark>而是<mark style="color:red;">轮流交换执行</mark>
{% endhint %}

## 4.2 线程的创建

### 4.2.1 继承 Thread 类实现 run 方法

```java
class MyThread extends Thread {

    private final int num;
    
    public MyThread(int num) {
        this.num = num;
    }
    
    @Override
    public void run() {
        System.out.println(num);
    }
    
    public static void main(String[] args) {
        int size = 10;
        MyThread[] threads = new MyThread[size];
        for (int i = 0; i < size; ++i) 
            threads[i] = new MyThread(i);
        for (int i = 0; i < size; ++i) 
            threads[i].run();
        for (int i = 0; i < size; ++i) 
            threads[i].start();
    }
}
```

{% hint style="info" %}
使用 run 直接运行的并不会启动线程，所以上面的代码还是会先有序的输出0-9，然后再无序输出0-9
{% endhint %}

### 4.2.2 使用匿名内部类 or lambda

```java
class Test {
    public static void main(String[] args) {
        // 使用匿名内部类
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 100; ++i)
                    System.out.println(1);                
            }
        }).start();
        // 使用 lambda 表达式
        new Thread(() -> {
            for (int i = 0; i < 100; ++i)
                System.out.println(2);
        }).start();
            
        for (int i = 0; i < 100; ++i)
            System.out.println(3);
    }
}
```

{% hint style="info" %}
上面的代码执行都一般情况下不会按顺序输出100个1、100个2、100个3
{% endhint %}

### 4.2.3 实现 JUC(java.util.concurrent) 包的 Callable 接口

{% hint style="info" %}
通过实现 Callable 可获取到线程执行的返回值，但是get方法获取返回值会阻塞当前线程，可以设置等待超时时间
{% endhint %}

```java
class Test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        FutureTask<Integer> ft1 = new FutureTask<>(new Callable<>() {
            @Override
            public Integer call() {
                int i = 0;
                while (i++ < 5) {
                    System.out.println(Thread.currentThread().getName() + "正在运行");
                }
                return i;
            }
        });

        FutureTask<Integer> ft2 = new FutureTask<>(() -> {
            int i = 0;
            while (i++ < 5) {
                System.out.println(Thread.currentThread().getName() + "正在运行");
            }
            return i;
        });

        new Thread(ft1, "Thread-1").start();
        new Thread(ft2, "Thread-2").start();
        System.out.println(ft1.get());
        System.out.println(ft2.get());
        int i = 0;
        while (i++ < 5) {
            System.out.println(Thread.currentThread().getName() + "正在运行");
        }
    }
}
```

## 4.3 线程的生命周期、调度和基本控制

### 4.3.1 生命周期

1. 新建状态：创建一个线程对象后，该线程就处于新建状态。和其它 Java 对象一样，仅仅由 JVM 为其分配了内存
2. 可运行状态：又称就绪状态，创建完对象后，调用 start() 后进入就绪状态。处于就绪状态的线程位于线程队列中，还需要等待系统的调度，获取 CPU 的使用权
3. 锁阻塞状态：当获取一个对象锁时，该对象锁如果被其它对象先获取了，那么该线程就会进入锁等待状态，当该线程持有锁时进入可运行状态
4. 无限等待状态：当一个线程需要等待另一个线程调用 notify() 或 notifyAll() 方法唤醒时，该线程进入无线等待状态
5. 计时等待状态：调用计时等待的方法（如Thread.sleep(1000)）时，进入计时等待状态
6. 被终止状态：正常结束或没有处理异常 run 被终止而结束

### 4.3.2 调度和基本控制

1. 线程的优先级：setPriority(1-10)默认是5，1(MIN\_PRIORITY)最低10(MAX\_PRIORITY)最高
2. 线程休眠：Thread.sleep(毫秒数)
3. 线程插队：当前线程调用其它线程的join()方法将一直等待其它线程执行完毕再继续执行当前线程，或者调用join(毫秒数)，等待其它线程执行多少毫秒后不管他是否结束都开始争夺 CPU 资源
4. 线程让步：当前线程 调用 yield() 方法后会让出 CPU 使用权，但是不代表不继续争夺 CPU 的使用权
5. 线程中断：interrupt()中断线程，isInterrupt()，当前线程是否中断，不代表线程不会继续执行了，只是起到标志作用

## 4.4 线程的同步

### 4.4.1 同步代码块

```java
class Test {
    public static void main(String[] args) {
        Runnable task = new Runnable() {
            private int ticket = 100;
            private final Object lock = new Object();
            
            @Override
            public void run() {
                while (true) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                    synchronized (lock) {
                        if (ticket <= 0) return;
                        ticket--;
                        System.out.println(Thread.currentThread().getName() + ", 票剩余:" + ticket);
                    }
                }
            }
        };

        new Thread(task, "线程一").start();
        new Thread(task, "线程二").start();
        new Thread(task, "线程三").start();
        new Thread(task, "线程四").start();
        new Thread(task, "线程五").start();
    }
}
```

### 4.4.2 同步方法

```java
public class Test {

    public static void main(String[] args) {
        Runnable task = new Runnable() {
            private int ticket = 100;

            @Override
            public void run() {
                while (true) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                    ticketSales();
                }
            }

            public synchronized void ticketSales() {
                if (ticket <= 0) return;
                ticket--;
                System.out.println(Thread.currentThread().getName() + ", 票剩余:" + ticket);
            }
        };

        new Thread(task, "线程一").start();
        new Thread(task, "线程二").start();
        new Thread(task, "线程三").start();
        new Thread(task, "线程四").start();
        new Thread(task, "线程五").start();
    }
}
```

### 4.4.3 重入锁 ReentrantLock

```java
public class Test {

    public static void main(String[] args) {
        Runnable task = new Runnable() {
            private int ticket = 100;

            private final ReentrantLock lock = new ReentrantLock();

            @Override
            public void run() {
                while (true) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                    lock.lock();
                    if (ticket <= 0) break;
                    ticket--;
                    System.out.println(Thread.currentThread().getName() + ", 票剩余:" + ticket);
                    lock.unlock();
                }
                lock.unlock();
            }
        };

        new Thread(task, "线程一").start();
        new Thread(task, "线程二").start();
        new Thread(task, "线程三").start();
        new Thread(task, "线程四").start();
        new Thread(task, "线程五").start();
    }
}
```
