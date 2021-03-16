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
            System.out.println("Thread One " + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// TheadTwo.java
public class ThreadTwo extends Thread{

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println("Thread Two " + i);
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// RunExercise.java
public class RunExercise {

    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main Start");
        ThreadOne threadOne = new ThreadOne();
        ThreadTwo threadTwo = new ThreadTwo();

        threadOne.start();
        threadTwo.start();

        threadOne.join();

        System.out.println("Main End");
    }
}
{% endhighlight %}

***Kết quả***
{% highlight plaintext %}
Main Start
Thread One 0
Thread Two 0
Thread One 1
Thread Two 1
Thread One 2
Thread One 3
Thread Two 2
Thread One 4
Thread One 5
Thread Two 3
Thread One 6
Thread One 7
Thread Two 4
Thread One 8
Thread One 9
Thread Two 5
Main End
Thread Two 6
Thread Two 7
Thread Two 8
Thread Two 9
{% endhighlight %}