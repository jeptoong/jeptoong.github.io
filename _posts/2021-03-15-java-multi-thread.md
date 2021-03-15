---
layout: post
title: Java multi-thread
categories: [java]
tags: [thread, multi-thread]
---

**Q:**
1. Tại sao cần dùng thread, multi-thread?
2. Ứng dụng của thread, multi-thread?
3. Ưu điểm & nhược điểm trong đa luồng
4. Cách tạo thread

### Tại sao cần dùng thread, multi-thread?
Nói theo cách thông thường, chùng ta sử dụng java Thread bởi vì nó giúp ứng dụng chạy nhanh hơn bằng cách thực hiện nhiều công việc cùng lúc. Theo ngôn ngữ kỹ thuật, Thread giúp bạn đạt được tính song song trong các chương trình java.

Các lý dó:
* Parallel Programming: Làm cho một task có thể chạy song song với các task khác. Ví dụ IDE code của chúng ta, vừa nhận input của dev vừa download document source hay update SVN, ... dưới background. Nếu các task này chạy tuần tự ta sẽ có cảm giác như IDE bị treo, việc này sẽ ảnh hưởng trải nghiệm của người sử dụng và không tận dụng được sức mạnh của các bộ vi xử lý nhiều cores hiện đại ngày nay.

* Tận dụng hết sức mạnh của CPU: Như đã nói ở trên, sử dụng multi-thread sẽ cải thiện được tốc độ của app bằng cách sử dụng toàn bộ sức mạnh của CPU. Giả sử ta có CPU 32 core, ta chỉ sử dụng 1 core để phục vụ 1000 client, ta sẽ cải thiện được tốc độ 32 lần nếu chúng ta dùng 32 thread.

* Giảm response time: 