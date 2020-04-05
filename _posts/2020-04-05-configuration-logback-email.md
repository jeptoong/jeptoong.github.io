---
layout: post
title: Config logback để send email khi có log level ERROR
subtitle:
categories: [java]
tags: [java, jackson]
---

### Configuration

{% highlight xml linenos %}
<?xml version="1.0" encoding="UTF-8"?>

<configuration scan="true" scanPeriod="120 seconds">
    <!-- OTHER APPENDER -->
    
    <!--  Send messages to Email  -->
    <appender name="EMAIL" class="ch.qos.logback.classic.net.SMTPAppender">
        <smtpHost>smtp.gmail.com</smtpHost>
        <smtpPort>587</smtpPort>
        <STARTTLS>true</STARTTLS>
        <asynchronousSending>false</asynchronousSending>
        <username>your-email@mail.com</username>
        <password>password</password>
        <to>receiver-email@mail.com</to>
        <from>your-email@mail.com</from>
        <subject>[ERROR] TOOL-C: %m - %d{yyyy-MM-dd HH:mm:ss.SSS}</subject>
        <layout class="ch.qos.logback.classic.html.HTMLLayout"/>
    </appender>

    <!--  Root configuration  -->
    <root level="${logback.log.level}">
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="TOOL"/>
        <appender-ref ref="EMAIL"/>
    </root>
</configuration>
{% endhighlight %}

### Các vấn đề chú ý

1. Không gửi được gmail.
- Bởi vì các lý do bảo mật chặt chẽ của Google, bạn có thể sẽ không gửi dc gmail.
> Bạn phải thay đổi setting của account [Turn off "Less secure app access"](https://support.google.com/accounts/answer/6010255){:target="_blank"}
- Nếu bạn dùng bảo mật 2 lớp.
> Bạn phải dùng `Mật khẩu ứng dụng` thay thế.

2. Config như trên rồi nhưng vẫn không gửi được email.
> Lưu ý rằng khi gặp error hoặc log error thì SMTPAppender mới send email.

3. Vấn đề bảo mật.
- Thông tin password trong file logback.xml có thể thấy dễ dàng, khi đẩy lên các kho lưu trữ như github, gitlab, bitbucket, ... thì có thể lộ thông tin.
> - Có thể sử dụng git-crypt tool để encrypt file logback.xml
> - Extend class SMTPAppender và mã hóa password.

Ex: Extend class SMTPAppender

{% highlight xml linenos %}
<configuration scan="true" scanPeriod="120 seconds">
    <!--  Send messages to Email  -->
    <appender name="EMAIL" class="org.tool.services.logback.SMTPAppenderEncryptPass">
        <smtpHost>smtp.gmail.com</smtpHost>
        <smtpPort>587</smtpPort>
        <STARTTLS>true</STARTTLS>
        <asynchronousSending>false</asynchronousSending>
        <username>your-email@mail.com</username>
        <password>[PROTECTED]</password>
        <to>receiver-email@mail.com</to>
        <from>your-email@mail.com</from>
        <subject>[ERROR] TOOL-C: %m - %d{yyyy-MM-dd HH:mm:ss.SSS}</subject>
        <layout class="ch.qos.logback.classic.html.HTMLLayout"/>
    </appender>
</configuration>
{% endhighlight %}

{% highlight java linenos %}
public class SMTPAppenderEncryptPass extends SMTPAppender {

    @Override
    public void setPassword(String password) {
        // If password is equal [PROTECTED], get password from properties file and decrypt it.
        if ("[PROTECTED]".equals(password)) {
            ResourceBundle bundle = ResourceBundle.getBundle(Constants.BUNDLE_APPLICATION);
            password = CryptoUtils.decryptString(bundle.getString("crypto.algorithm"), bundle.getString("logback.password"));
        }
        super.setPassword(password);
    }
}
{% endhighlight %}