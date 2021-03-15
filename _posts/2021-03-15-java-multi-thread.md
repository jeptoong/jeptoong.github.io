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
> 4. Ưu điểm & nhược điểm trong đa luồng

### Tại sao cần dùng thread, multi-thread?
Nói theo cách thông thường, chúng ta sử dụng java Thread bởi vì nó giúp ứng dụng chạy nhanh hơn bằng cách thực hiện nhiều công việc cùng lúc. Theo ngôn ngữ kỹ thuật, Thread giúp bạn đạt được tính song song trong các chương trình java.

Các lý do:
* **Parallel Programming**: Làm cho một task có thể chạy song song với các task khác. Ví dụ IDE code của chúng ta, vừa nhận input của dev vừa download document source hay update SVN, ... dưới background. Nếu các task này chạy tuần tự ta sẽ có cảm giác như IDE bị treo, việc này sẽ ảnh hưởng trải nghiệm của người sử dụng và không tận dụng được sức mạnh của các bộ vi xử lý nhiều cores hiện đại ngày nay.

* **Tận dụng hết sức mạnh của CPU**: Như đã nói ở trên, sử dụng multi-thread sẽ cải thiện được tốc độ của app bằng cách sử dụng toàn bộ sức mạnh của CPU. Giả sử ta có CPU 32 core, ta chỉ sử dụng 1 core để phục vụ 1000 client, ta sẽ cải thiện được tốc độ 32 lần nếu chúng ta dùng 32 thread.

* **Giảm response time**: Chúng ta có thể giảm response time khi sử dụng multi-thread bằng cách chia một vấn đề lớn thành các phần nhỏ hơn.

* **Để phục vụ nhiều client hơn tại một thời điểm**: Đối với các ứng dụng client-server, nếu sử dụng signle-thread thì có nghĩa là chỉ có một client connect tới server tại một thời điểm, còn đối với mutil-thread, nhiều client có thể cùng lúc connect tới server, điều này có nghĩa là client tiếp theo không phải đợi server hoàn thành xử lý request của client trước.


### Ứng dụng của thread, multi-thread?
Ngày nay hầu hết các ứng dụng điều sử dụng multi-thread. Ví dụ browser mở nhiều tab cùng lúc, mỗi tab lại hiển thị mỗi nội dung khác nhau, nhiều thread được sử dụng để load nội dung, display animation, play video, ... Hoặc như ứng dụng Word, multi-thread được sử dụng để hiển thị text, check spell và grammer.


### Cách tạo thread
Có 2 cách để tạo thread trong java:
* Extending Thread class
* Implementing Runnable interface

***Ex:***
{% highlight java linenos %}
// Creates thread by extending Thread class
public class ThreadOne extends Thread {

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("Thread One " + i);
        }
    }
}

// Creates thread by implementing Runnable interface
public class ThreadTwoRunnable implements Runnable {

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("Thread Two " + i);
        }
    }
}

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

Ở cách 1, khi class chúng ta extends Thread class, chúng ta không có cơ hội để extend bất kỳ class nào khác, do đó chúng ta sẽ không tận dụng được benefit của Inheritance. Ở cách 2, khi impelement Runnable interface chúng ta có thể extend thêm bất kỳ class nào khác, do đó ta tận dụng được benefit của Inheritance.