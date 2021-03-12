---
layout: post
title: Inner Class
categories: [java]
tags: [java, inner-class]
---

[Document Inner Class Oracle](https://docs.oracle.com/javase/tutorial/java/javaOO/innerclasses.html){:target="_blank"}

## Inner Class Example

{% highlight java linenos %}
public class DataStructure {

    // Create an array
    private final static int SIZE = 15;
    private int[] arrayOfInts = new int[SIZE];

    public DataStructure() {
        // fill the array with ascending integer values
        for (int i = 0; i < SIZE; i++) {
            arrayOfInts[i] = i;
        }
    }

    public void printEven() {

        // Print out values of even indices of the array
        DataStructureIterator iterator = this.new EvenIterator();
        while (iterator.hasNext()) {
            System.out.print(iterator.next() + " ");
        }
        System.out.println();
    }

    interface DataStructureIterator extends java.util.Iterator<Integer> { }

    // Inner class implements the DataStructureIterator interface,
    // which extends the Iterator<Integer> interface

    private class EvenIterator implements DataStructureIterator {

        // Start stepping through the array from the beginning
        private int nextIndex = 0;

        public boolean hasNext() {

            // Check if the current element is the last in the array
            return (nextIndex <= SIZE - 1);
        }

        public Integer next() {

            // Record a value of an even index of the array
            Integer retValue = Integer.valueOf(arrayOfInts[nextIndex]);

            // Get the next even element
            nextIndex += 2;
            return retValue;
        }
    }

    public static void main(String s[]) {

        // Fill the array with integer values and print out only
        // values of even indices
        DataStructure ds = new DataStructure();
        ds.printEven();
    }
}
{% endhighlight %}

## Local and Anonymous Classes
There are two additional types of inner classes:
- You can declare an inner class **within the body of a method**. These classes are known as **local classes**.
- You can also declare an inner class **within the body of a method without naming the class**. These classes are known as **anonymous classes**.

## Modifiers
You can use the access specifiers **_private_**, **_public_**, and **_protected_** to restrict access to inner classes, just as you use them to restrict access do to other class members.
