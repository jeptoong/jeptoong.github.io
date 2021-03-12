---
layout: post
title: Tạo server tại nhà
subtitle: Tạo server tại nhà có thể truy cập được từ mạng bên ngoài
categories: [lung-tung]
tags: [server, home]
---

Hôm nay mình tìm hiểu được một thứ rất hay, đó là publish web site để có thể truy cập từ mạng bên ngoài mà không cần phải thuê ip tĩnh của nhà mạng.
Những thứ mình cần là:
* Một máy tính
* Quyền truy cập vào router

#### Config hệ thống mạng để truy cập server từ bên ngoài

### 1. Set IP tĩnh cho server

Tại sao chúng ta lại phải set IP tĩnh cho server? 
>1. Như các bạn đã biết thì địa chỉ IP local của mỗi máy trong mạng không cố định, việc set IP tĩnh sẽ giúp các bạn ssh vào server dễ hơn, không cần phải check lại IP mỗi lần muốn ssh.
>2. Để thực hiện Port Forwarding thì server các bạn phải có địa chỉ IP cố định.

Để thực hiện set IP tĩnh cho server chúng ta có thể config cho máy tính chứa server một cách thủ công, hoặc chúng ta config ở route sao cho mỗi khi máy tính connect mạng thì route sẽ cấp IP cố định cho máy tính đó (Ở đây mình áp dụng cách này vì tiện hơn khi mang laptop đi nơi khác không phải chuyển IP tĩnh sang IP động).

Các bước thực hiện:

1. Truy cập vào trang Management của modem, ở đây modem Viettel của mình truy cập bằng link https://192.168.1.1. Nhập id và password vào (thông tin này được cấp khi ký hợp đồng mạng, nếu bạn không có thì gọi điện lên tổng đài để hỏi).
2. Click vào **Advance Setup**
3. Ở side menu bên trái, chọn **Advanced Setup**/**LAN Setup**
4. Bạn tìm tới mục **Add DHCP Reservation**, Add IP Address và MAC Address của máy tính chứa server vào

Vậy là bạn đã set được IP tĩnh cho máy tính

### 2. Config Port Forwarding

Một mạng máy tính khi kết nối internet thì đều được nhà cung cấp dịch vụ mạng cung cấp một địa chỉ IP được gọi là **Public IP**. Khi người dùng bên ngoài muốn liên hệ với các máy trong hệ thống mạng nhà bạn sẽ liên hệ thông qua địa chỉ này. Tuy nhiên trong một mạng máy tính có rất nhiều thiết bị kết nối, nhưng khi ra ngoài internet chỉ có 1 Public IP duy nhất, vậy làm sao bên ngoài biết mà liên hệ đúng thiết bị nào trong mạng? Đó chính là lúc chúng ta phải dùng đến **Port Forwarding**. **Port Forwarding** cho phép chúng ta sẽ chuyển tiếp port của Public IP sang port của một địa chỉ bất kỳ trong mạng.

Ví dụ: Khi người dùng bên ngoài muốn truy cập vào giao thức http (port 80), thì chúng ta sẽ chuyển tiếp tất cả traffic qua IP của server local, server lúc này sẽ nhận được yêu cầu truy cập và trả về trang web cho người dùng.

Để thực hiện Port Forwarding ta thực hiện các bước sau:

1. Truy cập **Firewall Setup**/**NAT**
2. Chọn **Port Forwarding**
3. Setup thông tin tương tự như sau:
    - Protocol: ALL
    - Start Port Number: 80 (bắt đầu mở từ port này)
    - End Port Number: 80 (kết thúc ở port này)
    - Local IP Address: 192.168.1.20 (địa chỉ ip tĩnh của server config ở trên)
    - Start Port Number(Local): 8080
    - End Port Number(Local): 8080
4. Nhấn **Apply**

**Lưu ý:** Có thể bạn sẽ gặp lỗi `Cannot set reserved external port!`, lỗi này là do bạn mở port mà modem đang sử dụng cho trang Management Modem, để fix lỗi này bạn chỉ cần change port của trang Management Modem.

#### Dynamic DNS và cách config để truy cập server từ bên ngoài bằng tên miền

Như ở phần trên mình đã đề cập thì gói mạng các hộ gia đình hiện nay mà hầu hết các bạn đang sử dụng sẽ không có hỗ trợ IP tĩnh, nghĩa là sau khi restart router thì Public IP của các bạn sẽ bị thay đổi, dẫn đến việc các bạn muốn truy cập server từ bên ngoài thì phải ghi nhận lại IP mới mỗi khi bị thay đổi. Đương nhiên chúng ta có thể mua thêm gói IP tĩnh từ các nhà mạng nhưng không phải ai cũng có đủ điều kiện, và ngoài ra thì địa chỉ IP cũng không phải là một con số dễ nhớ. Đó là lúc chúng ta cần một giải pháp khác là **Dynamic DNS**, **Dynamic DNS** là một dịch vụ cho phép người dùng map IP động của một modem sang một tên miền cố định. Sau khi đã setup thì để truy cập server các bạn bây giờ chỉ cần nhớ tên miền cố định mà các bạn đã chọn thôi.

Hầu hết modem và router hiện nay đều có option hỗ trợ DDNS, chúng ta chỉ cần đăng ký tài khoản ở những nhà cung cấp dịch vụ DDNS, điền thông tin username, password và domain ở chỗ config DDNS là xong. Modem sẽ tự động gửi request lên server để cập nhật lại IP mới ngay khi có thay đổi.

Một số nhà cung cấp DDNS như No-IP, DynDNS, .... Mình thì đang sử dụng No-IP vì nó free, cách đăng ký rất đơn giản. Sau khi đăng ký xong, bạn vào config username (password đã tạo khi create account) và add new hostname. Sử dụng hostname, username, password này để setup cho modem của bạn.

Các bước setup như sau:

1. Vào **Advanced Setup**/**Dynamic DNS**
2. Chọn **Activated** Dynamic DNS
3. Chọn **Service Provider** là **www.no-ip.com**
4. Điền hostname, username, password của bạn vào
5. Nhấn **Apply**

Để kiểm tra xem config đã hoạt động chưa, bạn vào trang quản lý hostname trên trang no-ip.com, **Modify** hostname của bạn thành 1.1.1.1 rồi reboot modem của bạn, sau khi reboot thành công, vào lại trang quản lý hostname no-ip.com của bạn mà thấy ip của hostname đã cập nhập lại thì xác định là thành công, nếu không bạn xem lại các bước bạn làm đã đúng chưa.

Từ đây bạn có thể truy cập trang web của bạn từ hostname mà bạn tạo ở no-ip.com

#### Tổng kết

Như vậy ta có thể truy cập trang web local từ mạng bên ngoài thông qua Dynamic DNS, thật sự là rất tiện lợi và tiết kiệm chi phí. Bạn có thể dùng máy Raspberry Pi để setup server, nó sẽ tiết kiệm điện hơn
