---
layout: post
title: Join Threads in Java
categories: [java]
tags: [thread, join, join-thread]
---

**java.lang.Thread** class cung cấp method join(), nó cho phép một thread đợi cho đến khi một thread khác hoàn thành việc thực thi của mình.

Ở ví dụ sau, ta có thread Main, thread One và thread Two, 3 thread này chạy song song với nhau, khi code chạy tới dòng threadOne.join() thì thead Main sẽ "gộp" với thread One, tức là thread Main sẽ đợi thread One thực thi xong mới chạy tiếp, trong khi đó thread Two vẫn chạy song song.

**Ex:**
{% highlight java linenos %}
// TheadOne.java
public class ThreadOne extends Thread {

    @Override
    public void run() {
        for (int i = 0; i < 40; i++) {
            System.out.println("Thread One " + i);
        }
    }
}

// TheadTwo.java
public class ThreadTwo extends Thread{

    @Override
    public void run() {
        for (int i = 0; i < 40; i++) {
            System.out.println("Thread Two " + i);
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
{% hightlight plaintext %}
Main Start
Thread Two 0
Thread One 0
Thread One 1
Thread One 2
Thread One 3
Thread One 4
Thread One 5
Thread One 6
Thread One 7
Thread One 8
Thread One 9
Thread One 10
Thread One 11
Thread One 12
Thread One 13
Thread One 14
Thread One 15
Thread One 16
Thread One 17
Thread One 18
Thread One 19
Thread One 20
Thread One 21
Thread One 22
Thread Two 1
Thread Two 2
Thread Two 3
Thread Two 4
Thread Two 5
Thread Two 6
Thread Two 7
Thread Two 8
Thread Two 9
Thread Two 10
Thread Two 11
Thread One 23
Thread One 24
Thread One 25
Thread Two 12
Thread Two 13
Thread Two 14
Thread Two 15
Thread Two 16
Thread Two 17
Thread Two 18
Thread Two 19
Thread Two 20
Thread Two 21
Thread Two 22
Thread Two 23
Thread Two 24
Thread Two 25
Thread Two 26
Thread Two 27
Thread Two 28
Thread Two 29
Thread Two 30
Thread Two 31
Thread Two 32
Thread Two 33
Thread Two 34
Thread Two 35
Thread One 26
Thread One 27
Thread One 28
Thread One 29
Thread One 30
Thread One 31
Thread One 32
Thread One 33
Thread One 34
Thread One 35
Thread One 36
Thread One 37
Thread One 38
Thread One 39
Thread Two 36
Main End
Thread Two 37
Thread Two 38
Thread Two 39
{% endhighlight %}