---
layout: post
title: Java multi-thread
categories: [java]
tags: [thread, multi-thread]
---

> **Q:**
> 1. Tại sao cần dùng thread, multi-thread?
> 2. Ứng dụng của thread, multi-thread?
> 3. Cách tạo thread
> 4. Ưu điểm & nhược điểm của đa luồng

### Tại sao cần dùng thread, multi-thread?
Nói theo cách thông thường, chúng ta sử dụng java Thread bởi vì nó giúp ứng dụng chạy nhanh hơn bằng cách thực hiện nhiều công việc cùng lúc. Theo ngôn ngữ kỹ thuật, Thread giúp bạn đạt được tính song song trong các chương trình java.

Các lý do:
* **Parallel Programming**: Làm cho một task có thể chạy song song với các task khác. Ví dụ IDE code của chúng ta, vừa nhận input của dev vừa download document source hay update SVN, ... dưới background. Nếu các task này chạy tuần tự ta sẽ có cảm giác như IDE bị treo, việc này sẽ ảnh hưởng trải nghiệm của người sử dụng và không tận dụng được sức mạnh của các bộ vi xử lý nhiều cores hiện đại ngày nay.

* **Tận dụng hết sức mạnh của CPU**: Như đã nói ở trên, sử dụng multi-thread sẽ cải thiện được tốc độ của app bằng cách sử dụng toàn bộ sức mạnh của CPU. Giả sử ta có CPU 32 core, ta chỉ sử dụng 1 core để phục vụ 1000 client, ta sẽ cải thiện được tốc độ 32 lần nếu chúng ta dùng 32 thread.

* **Giảm response time**: Chúng ta có thể giảm response time khi sử dụng multi-thread bằng cách chia một vấn đề lớn thành các phần nhỏ hơn.

* **Để phục vụ nhiều client hơn tại một thời điểm**: Đối với các ứng dụng client-server, nếu sử dụng single-thread thì chỉ có thể một client connect tới server tại một thời điểm, còn đối với mutil-thread, nhiều client có thể cùng lúc connect tới server, điều này có nghĩa là client tiếp theo không phải đợi server hoàn thành xử lý request của client trước.


### Ứng dụng của thread, multi-thread?
Ngày nay hầu hết các ứng dụng điều sử dụng multi-thread. Ví dụ browser mở nhiều tab cùng lúc, mỗi tab lại hiển thị mỗi nội dung khác nhau, nhiều thread được sử dụng để load nội dung, display animation, play video, ... Hoặc như ứng dụng Word, multi-thread được sử dụng để hiển thị text, đồng thời xử lý check spell, grammer.


### Cách tạo thread
Có 2 cách để tạo thread trong java:
* Extending Thread class
* Implementing Runnable interface

***Ex:***
{% highlight java linenos %}
// ThreadOne.java
// Creates thread by extending Thread class
public class ThreadOne extends Thread {

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("Thread One " + i);
        }
    }
}

// ThreadTwoRunnable.java
// Creates thread by implementing Runnable interface
public class ThreadTwoRunnable implements Runnable {

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("Thread Two " + i);
        }
    }
}

// RunExercise.java
// New instance and run thread
public class RunExercise {
    public static void main(String[] args) {
        System.out.println("Start Main");

        // Thread One
        ThreadOne threadOne = new ThreadOne();
        threadOne.start();

        // Thread Two
        ThreadTwoRunnable threadTwoRunnable = new ThreadTwoRunnable();
        Thread threadTwo = new Thread(threadTwoRunnable);
        threadTwo.start();

        System.out.println("End Main");
    }
}
{% endhighlight %}

***Khi nào thì dùng cách extend Thread, khi nào thì dùng cách Runnable?***

* Ở cách 1, khi extends Thread class, chúng ta không có cơ hội để extend bất kỳ class nào khác, do đó chúng ta sẽ không tận dụng được benefit của Inheritance. Ở cách 2, khi impelement Runnable interface chúng ta có thể extend thêm bất kỳ class khác, do đó ta tận dụng được benefit của Inheritance.

* Khi chúng ta extend Thread class, mỗi luồng sẽ tạo ra một đối tượng duy nhất và liên kết với nó. Khi implement Runnable, các luồng cùng chia sẻ một đối tượng.

Vì vậy tuỳ vào design có yêu cầu sử dụng đa thừa kế hay không mà chọn cách phù hợp.

### Ưu điểm & nhược điểm của đa luồng

* **Ưu điểm**:
    - Cải thiện performance của app phức tạp.
    - Cải thiện khả năng phản hồi của giao diện người dùng.
    - Được sử dụng ở server để cải thiện throughput và sử dụng resource.
    - Parallelize các task.
    - Tận dụng sức mạnh của CPU.

* **Nhược điểm**:
    - Tăng độ phức tạp coding, debuging, testing.
    - Tăng khả năng xảy ra deadlock.

------
* [Code Example](https://github.com/jeptoong/learn-thread){:target="_blank"}
* [Why we use Threads in Java?](https://javarevisited.blogspot.com/2017/03/why-we-use-threads-in-java.html#axzz6p8wehajw){:target="_blank"}
* [Implement Runnable vs Extend Thread in Java](https://www.geeksforgeeks.org/implement-runnable-vs-extend-thread-in-java/){:target="_blank"}
* [Extends Thread vs Implements Runnable](https://manikandanmv.wordpress.com/tag/extends-thread-vs-implements-runnable/){:target="_blank"}