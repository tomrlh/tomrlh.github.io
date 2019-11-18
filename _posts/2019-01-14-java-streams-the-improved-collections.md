---
layout: post
title: "Java Streams: The Improved Collections"
date: 2019-01-14 08:50:28
categories: java coding
author_name : "Thiago Medeiros"
author_url : /author/tomrlh
author_avatar: tomrlh
show_avatar : true
read_time : 3
feature_image: feature-sea
show_related_posts: true
square_related: recommend-sea
i18n-link: java-streams-the-improved-collections
---




A Java Stream is a data sequence, that are produced as in a factory: the product goes through a number of finite steps that depends each other. When the product goes through one step, it doesn't come back anymore, the operation is realized then the stream goes on.

Following this example, the product is the data, and in which step is called _Stream Operation_.
The Streams have 3 parts:

![Stream parts](/img/post-assets/java-streams-the-improved-collections/stream-parts.png)

<!-- ![The Stream Parts](/img/post-assets/java-streams-the-improved-collections/stream-parts-en.png) -->

1. **Source**: the data source;
2. **Intermediate Operations**: methods to transform/manipulate the data;
3. **Terminal Operation**: deliver the produced result.





### Source

There are some ways to create a Stream:

~~~ java
Stream<String> empty = Stream.empty();

Stream<Integer> oneElement = Stream.of(1);
Stream<Integer> array = Stream.of(1, 2, 3);

// From a list
Stream<String> list = Arrays.asList("a", "b", "c");
Stream<String> fromAList = list.stream();
~~~





### Common Intermetiate Operations

Table with methods





### Common Terminal Operations

Table with methods

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

## Why to use Streams?

Most part lists manipulations can be done with streams. However streams have some advantages like:

* readability:
* clean code:
- powerful:
- efficient:




## Hands on code

Let's say that we want the 3 first names from a string list, that begins with _"b"_ and have size _4_. Using lists, we could use:

~~~ java
List<String> list = Arrays.asList(
	"baby", "car", "book", "letter", "world", "word", "table"
);
List<String> filteredList = new ArrayList<>();

for(String word: list) {
	if(word.length() == 4 && word.startsWith("b"))
	    filteredList.add(word);
}
Collections.sort(filteredList);
Iterator<String> iter = filteredList.iterator();

for(String word: filteredList) {
	System.out.println(word);
}
~~~





Now with Streams:

~~~ java
Stream<String> stream = Stream.of(
	"baby", "car", "book", "letter", "world", "word", "table"
);
stream.filter(p -> p.length() == 4)
	.filter(p.startsWith("b"))
	.sorted()
	.forEach(System.out::println);
~~~

Notice that methods can be chained and the most part of Stream interface methods receives a
_Functional Interface_. A [filter()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#filter-java.util.function.Predicate-) 
receives a [Predicate](https://docs.oracle.com/javase/8/docs/api/java/util/function/Predicate.html). [sorted()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#sorted--) 
has two version, one receives no parameters, the other receives a [Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html). The
[forEach()](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#forEach-java.util.function.Consumer-) is very useful, cause can replace the traditional/enhanced for, bringing more readability. The second receives a [Consumer](https://docs.oracle.com/javase/8/docs/api/java/util/function/Consumer.html).

References:

* [Stream Docs](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#filter-java.util.function.Predicate-)
* [Whats the difference between Streams and Collections in Java 8](https://stackoverflow.com/questions/39432699/what-is-the-difference-between-streams-and-collections-in-java-8)