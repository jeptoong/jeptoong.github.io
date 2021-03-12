---
layout: post
title: Anonymous Class
subtitle: Anonymous classes enable you to make your code more concise. They enable you to declare and instantiate a class at the same time. They are like local classes except that they do not have a name. Use them if you need to use a local class only once.
categories: [java]
tags: [java, anonymous-class]
---

[Document Anonymous Class Oracle](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html){:target="_blank"}

#### Declaring Anonymous Classes

- Local classes are class declarations, anonymous classes are expressions.

{% highlight java linenos %}
public class AnonymousLearn {

    interface HelloWorld {
        void greet();
        void greetSomeone(String name);
    }

    public void sayHello() {

        class EnglishGreeting implements HelloWorld {

            private String name = "world";

            @Override
            public void greet() {
                greetSomeone(null);
            }

            @Override
            public void greetSomeone(String name) {
                if (null != name && !"".equals(name)) {
                    this.name = name;
                }
                System.out.println("Hello " + name);
            }
        }

        // create instance of Local Class
        HelloWorld englishGreeting = new EnglishGreeting();

        // create instance of Anonymous Class
        HelloWorld frenchGreeting = new HelloWorld() {
            private String name = "tout le monde";

            @Override
            public void greet() {
                greetSomeone("tout le monde");
            }

            @Override
            public void greetSomeone(String name) {
                if (null != name && !"".equals(name)) {
                    this.name = name;
                }
                System.out.println("Salut " + name);
            }
        };

        // run
        englishGreeting.greet();
        frenchGreeting.greet();
    }

    public static void main(String[] args) {
        AnonymousLearn app = new AnonymousLearn();
        app.sayHello();
    }
}
{% endhighlight %}

#### Syntax of Anonymous Classes

The anonymous class expression consists of the following:
- The new operator
- The name of an interface to implement or a class to extend. In this example, the anonymous class is implementing the interface HelloWorld.
- Parentheses that contain the arguments to a constructor, just like a normal class instance creation expression. Note: When you implement an interface, there is no constructor, so you use an empty pair of parentheses, as in this example.
- A body, which is a class declaration body. More specifically, in the body, method declarations are allowed but statements are not.

#### Accessing Local Variables of the Enclosing Scope, and Declaring and Accessing Members of the Anonymous Class
