---
layout: post
title: Java notify, notifyAll and wait
categories: [java]
tags: [thread, notify, notifyAll, wait, synchronization]
---

Các thread có thể giao tiếp với nhau thông qua các method **wait()**, **notify()** và **notifyAll()**.

**wait()** method làm cho thread vào trạng thái chờ cho đến khi được đánh thức bằng method **notify()** hay **notifyAll()**. method **notify()** chỉ đánh thức duy nhất một thread đang chờ, còn **notifyAll()** đánh thức tất cả các thread đang chờ

**Chú ý:** Các method wait, notify, notifyAll chỉ có thể được gọi trong synchronized

**Ex:** Dùng notify()
{% highlight java linenos %}
// ThreadSafeQueue.java
public class ThreadSafeQueue {

    private final Queue<String> queue = new LinkedList<>();
    private static final Object MUTEX = new Object();

    public void push(String value) throws InterruptedException {
        synchronized (MUTEX) {
            queue.add(value);
            MUTEX.notify();
            System.out.println("[" + Thread.currentThread().getName() + "] Release MUTEX ...");
        }
    }

    public String poll() throws InterruptedException {
        synchronized (MUTEX) {
            while (queue.isEmpty()) {
                System.out.println("[" + Thread.currentThread().getName() + "] Release MUTEX ...");
                MUTEX.wait();
            }
        }
        return queue.poll();
    }
}

// RunTest.java
public class RunTest {

    public static void main(String[] args) {
        ThreadSafeQueue threadSafeQueue = new ThreadSafeQueue();

        (new Thread("Receiver") {
            @Override
            public void run() {
                try {
                    System.out.println("Value get from queue: " + threadSafeQueue.poll());
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();

        (new Thread("Sender") {
            @Override
            public void run() {
                try {
                    Thread.sleep(1000);
                    threadSafeQueue.push("Hello from Hue");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
{% endhighlight %}

***Kết quả:***
{% highlight plaintext %}
[Receiver] Release MUTEX ...
[Sender] Release MUTEX ...
Value get from queue: Hello from Hue
{% endhighlight %}

**Ex:** Dùng notifyAll()
{% highlight java linenos %}
// ThreadSafeQueue2.java
public class ThreadSafeQueue2 {

    private final Queue<String> queue = new LinkedList<>();
    private static final Object MUTEX = new Object();

    public void push(List<String> values) throws InterruptedException {
        synchronized (MUTEX) {
            queue.addAll(values);
            MUTEX.notifyAll();
            System.out.println("[" + Thread.currentThread().getName() + "] Release MUTEX ...");
        }
    }

    public String poll() throws InterruptedException {
        synchronized (MUTEX) {
            while (queue.isEmpty()) {
                System.out.println("[" + Thread.currentThread().getName() + "] Release MUTEX ...");
                MUTEX.wait();
            }
        }
        return queue.poll();
    }
}

// RunTest2.java
public class RunTest2 {

    public static void main(String[] args) {
        ThreadSafeQueue2 threadSafeQueue = new ThreadSafeQueue2();

        (new Thread("Receiver 1") {
            @Override
            public void run() {
                try {
                    System.out.println("[" + Thread.currentThread().getName() + "] Value get from queue: " + threadSafeQueue.poll());
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();

        (new Thread("Receiver 2") {
            @Override
            public void run() {
                try {
                    System.out.println("[" + Thread.currentThread().getName() + "] Value get from queue: " + threadSafeQueue.poll());
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();

        (new Thread("Sender") {
            @Override
            public void run() {
                try {
                    Thread.sleep(1000);
                    threadSafeQueue.push(Arrays.asList("Hello from Hue", "Welcome to Hue"));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
{% endhighlight %}

***Kết quả:***
{% highlight plaintext %}
[Receiver 1] Release MUTEX ...
[Receiver 2] Release MUTEX ...
[Sender] Release MUTEX ...
[Receiver 2] Value get from queue: Hello from Hue
[Receiver 1] Value get from queue: Welcome to Hue
{% endhighlight %}

------
* [Code Example](https://github.com/jeptoong/learn-thread){:target="_blank"}