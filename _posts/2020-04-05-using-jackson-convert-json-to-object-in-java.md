---
layout: post
title: Sử dụng Jackson để convert JSON thành object trong JAVA
subtitle: Cách convert JSON thành object trong JAVA.
categories: [java]
tags: [java, jackson]
---

### 1. Tạo đối tượng JavaType

- Đối tượng này chỉ được tao bởi `com.fasterxml.jackson.databind.type.TypeFactory`
- Nó có một số method cơ bản như: constructType, constructCollectionType, constructMapType
    + **constructType:** tạo đối tượng JavaType của object
    + **constructCollectionType:** tạo đối tượng JavaType collection của object
    + **constructMapType:** tạo đối tượng JavaType Map của object

Ex:
{% highlight java linenos %}
private JavaType getJavaType(Class<?> clazz, String wrapContextType) {
    ObjectMapper objectMapper = new ObjectMapper();
    JavaType javaTypeClass = this.objectMapper.getTypeFactory().constructType(clazz);
    switch (wrapContextType) {
        case WRAP_CONTEXT_LIST:
            return this.objectMapper.getTypeFactory().constructCollectionType(List.class, javaTypeClass);
        case WRAP_CONTEXT_MAP:
            return this.objectMapper.getTypeFactory().constructMapType(Map.class,this.objectMapper.getTypeFactory().constructType(String.class), javaTypeClass);
        default:
            return javaTypeClass;
    }
}
{% endhighlight %}

### 2. Tạo object bằng method readValue từ InputStream

{% highlight java linenos %}
private Object readJavaType(JavaType javaType, InputStream body) {
    try {
        ObjectMapper objectMapper = new ObjectMapper();
        return objectMapper.readValue(body, javaType);
    } catch (IOException e) {
        throw new ReadInputStreamException(e);
    }
}
{% endhighlight %}

### 3. Convert object bằng method convertValue

{% highlight java linenos %}
private Object convertJavaType(JavaType javaType, Object objectConvert) {
    ObjectMapper objectMapper = new ObjectMapper();
    return objectMapper.convertValue(objectConvert, javaType);
}
{% endhighlight %}
