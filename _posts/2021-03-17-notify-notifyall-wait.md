---
layout: post
title: Java notify, notifyAll and wait
categories: [java]
tags: [thread, notify, notifyAll, wait, synchronization]
---

Các thread có thể giao tiếp với nhau thông qua các method **wait()**, **notify()** và **notifyAll()**.

**wait()** method làm cho thread vào trạng thái chờ cho đến khi được đánh thức bằng method **notify()** hay **notifyAll()**. method **notify()** chỉ đánh thức duy nhất một thread đang chờ, còn **notifyAll()** đánh thức tất cả các thread đang chờ

* **Chú ý:** Các method wait, notify, notifyAll chỉ có thể được gọi trong synchronized

**Ex:**
{% highlight java linenos %}
// Processor.java
public class Processor {

    private String value;

    public void produce() throws InterruptedException {
        synchronized (this) {
            System.out.println("Producer thread is running...");
            wait();
            System.out.println("The value user enter: " + value);
        }
    }

    public void consume() throws InterruptedException {
        Scanner scanner = new Scanner(System.in);
        synchronized (this) {
            System.out.println("Consumer thread is running ...");
            System.out.print("Please enter value: ");
            value = scanner.next();
            notify();
        }
    }
}

// RunExercise.java
public class RunExercise {

    public static void main(String[] args) throws InterruptedException {
        Processor processor = new Processor();

        // Creates thread produce.
        Thread threadOne = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    processor.produce();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // Creates thread consume
        Thread threadTwo = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    processor.consume();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        // Start threads
        threadOne.start();
        threadTwo.start();
    }
}
{% endhighlight %}

***Kết quả:***
{% highlight plaintext %}
Producer thread is running...
Consumer thread is running ...
Please enter value: hello
The value user enter: hello
{% endhighlight %}

------
* [Code Example](https://github.com/jeptoong/learn-thread){:target="_blank"}