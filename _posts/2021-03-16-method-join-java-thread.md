---
layout: post
title: Join Threads in Java
categories: [java]
tags: [thread, join, join-thread]
---

### Thread.join() method
**java.lang.Thread** class cung cấp method **join()**, nó cho phép một thread đợi cho đến khi một thread khác hoàn thành việc thực thi của mình. Khi chúng ta gọi method **join()** thì thread đang gọi (calling thread) sẽ chuyển sang trạng thái chờ, nó sẽ ở trạng thái chờ cho đến khi thead được gọi (referenced thread) kết thúc thực thi.

Ở ví dụ sau, ta có thread Main, thread One và thread Two, 3 thread này chạy song song với nhau, khi code chạy tới dòng `threadOne.join()` thì thead Main sẽ "gộp" với thread One, tức là thread Main sẽ đợi thread One hoàn thành thực thi mới thực thi tiếp các lệnh phía sau, trong khi đó thread Two vẫn chạy song song.

**Ex:**
{% highlight java linenos %}
// TheadOne.java
public class ThreadOne extends Thread {

    private int num;

    public ThreadOne(int num) {
        this.num = num;
    }

    @Override
    public void run() {
        for (int i = 0; i < num; i++) {
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
public class ThreadTwo extends Thread {

    private int num;

    public ThreadTwo(int num) {
        this.num = num;
    }

    @Override
    public void run() {
        for (int i = 0; i < num; i++) {
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
        ThreadOne threadOne = new ThreadOne(10);
        ThreadTwo threadTwo = new ThreadTwo(10);

        // Start thread One and Two
        threadOne.start();
        threadTwo.start();

        // Thread main will wait for thread One to die
        threadOne.join();

        System.out.println("Main End");
    }
}
{% endhighlight %}

***Kết quả:***
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

Ở ví dụ tiếp theo, thread Two chỉ start sau khi thread One đã hoàn thành thực thi, do thread Two start sau lệnh `threadOne.join()`

**Ex:**
{% highlight java linenos %}
public class RunExercise2 {

    public static void main(String[] args) throws InterruptedException {
        ThreadOne threadOne = new ThreadOne(2);
        ThreadTwo threadTwo = new ThreadTwo(2);

        // Start thread One
        threadOne.start();

        // Thread main wait for thread One to die
        threadOne.join();

        // Thread Two start after thread One has died
        threadTwo.start();
    }
}
{% endhighlight %}

***Kết quả:***
{% highlight plaintext %}
Thread-0 Value: 0
Thread-0 Value: 1
Thread-1 Value: 0
Thread-1 Value: 1
{% endhighlight %}

### Thread.join() method với timeout

*Thread.join()* sẽ mãi ở trạng thái đợi khi reference thread bị block hay tốn nhiều thời gian để thực thi xong. Điều này có khả năng làm calling thread không phản hồi. Để giải quyết việc này, ta sử dụng overloaded version của method *Thread.join()* cho phép chỉ định thời gian timeout.

Có hai version overload method join():

* public final synchronized void join(long millis) throws InterruptedException
* public final synchronized void join(long millis, int nanos) throws InterruptedException

**Ex:**

{% highlight java linenos %}
public class RunExercise3 {

    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main Start");
        ThreadOne threadOne = new ThreadOne(5);
        ThreadTwo threadTwo = new ThreadTwo(5);

        // Start thread One and Two
        threadOne.start();
        threadTwo.start();

        // Thread main will wait for thread One to die in 4000 millis
        threadOne.join(4000);

        System.out.println("Main End");
    }
}
{% endhighlight %}

***Kết quả:***
{% highlight plaintext %}
Main Start
Thread-0 Value: 0
Thread-1 Value: 0
Thread-0 Value: 1
Thread-0 Value: 2
Main End
Thread-1 Value: 1
Thread-0 Value: 3
Thread-0 Value: 4
Thread-1 Value: 2
Thread-1 Value: 3
Thread-1 Value: 4
{% endhighlight %}

------
* [Code Example](https://github.com/jeptoong/learn-thread){:target="_blank"}