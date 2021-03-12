---
layout: post
title: Java 8 - Lambda Comparator
categories: [java-8]
tags: [java8, lambda, comparator]
---

### Lambda Comparator Example

Classic `Comparator` example:
{% highlight java linenos %}
Comparator<Developer> byAge = new Comparator<Developer>() {
    @Override
    public int compare(Developer dev1, Developer dev2) {
        return dev1.getAge() - dev2.getAge();
    }
};
{% endhighlight %}

Lambda expression equivalent:
{% highlight java linenos %}
Comparator<Developer> byAge = (Developer dev1, Developer dev2) -> dev1.getAge() - dev2.getAge();
{% endhighlight %}
