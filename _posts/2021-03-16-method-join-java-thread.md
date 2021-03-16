---
layout: post
title: Join Threads in Java
categories: [java]
tags: [thread, join, join-thread]
---

**java.lang.Thread** class cung cấp method **join()**, nó cho phép một thread đợi cho đến khi một thread khác hoàn thành việc thực thi của mình.

Ở ví dụ sau, ta có thread Main, thread One và thread Two, 3 thread này chạy song song với nhau, khi code chạy tới dòng `threadOne.join()` thì thead Main sẽ "gộp" với thread One, tức là thread Main sẽ đợi thread One thực thi xong mới chạy tiếp, trong khi đó thread Two vẫn chạy song song.

**Ex:**
{% highlight java linenos %}
// TheadOne.java
public class ThreadOne extends Thread {

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " Value: " + i);
        }
    }
}

// TheadTwo.java
public class ThreadTwo extends Thread{

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " Value: " + i);
        }
    }
}

// RunExercise.java
public class RunExercise {

    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main Start");
        ThreadOne threadOne = new ThreadOne();
        ThreadTwo threadTwo = new ThreadTwo();

        // Start thread One and Two
        threadOne.start();
        threadTwo.start();

        // Thread main will wait for thread One to die
        threadOne.join();

        System.out.println("Main End");
    }
}
{% endhighlight %}

***Kết quả***
{% highlight plaintext %}
Main Start
Thread-0 Value: 0
Thread-1 Value: 0
Thread-0 Value: 1
Thread-0 Value: 2
Thread-1 Value: 1
Thread-0 Value: 3
Thread-0 Value: 4
Thread-1 Value: 2
Thread-0 Value: 5
Thread-0 Value: 6
Thread-1 Value: 3
Thread-0 Value: 7
Thread-0 Value: 8
Thread-1 Value: 4
Thread-0 Value: 9
Main End
Thread-1 Value: 5
Thread-1 Value: 6
Thread-1 Value: 7
Thread-1 Value: 8
Thread-1 Value: 9
{% endhighlight %}