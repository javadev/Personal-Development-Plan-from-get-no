Java 8 Lambda Streams
=====================

Create new stream

```java
Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
Stream<Integer> stream2 = Arrays.asList(5, 2, 3, 1, 4).stream();
```

Get first element

```java
Integer first = stream.findFirst();
```

Get last element

```java
Integer last = stream.reduce().findFirst();
```

Get sum of elements

```java
Integer sum = stream.sum();
```

Filter elements

```java
List<Integer> filtered = stream.filter(item -> item % 2 == 0).collect(Collectors.toList());
```

Map elements

```java
List<String> strings = stream.map(item -> "" + item).collect(Collectors.toList());
```

Sort elements

```java
List<Integer> sorted = stream.sorted((i1, i2) -> Integer.compare(i1, i2)).collect(Collectors.toList());
```

Grouping elements

```java

 List<String> list = new ArrayList<>();

        list.add("Hello");
        list.add("Hello");
        list.add("World");

        Map<String, Long> counted = list.stream()
            .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        System.out.println(counted);
// Result:

{Hello=2, World=1}
```

Reverse elements
```java
Stream.of(1, 2, 3, 4, 5)
    .peek(i -> System.out.println("before : " + i))
    .sorted(Collections.reverseOrder())
    .peek(i -> System.out.println("after : " + i))
    .count();
// Output:

before : 1
before : 2
before : 3
before : 4
before : 5
after : 5
after : 4
after : 3
after : 2
after : 1    
```

Flatten elements

Sort map by value

Combine streams in one

```java
Stream.concat(s1, s2)

Stream.of(s1, s2, s3, ...).flatMap(x -> x)
```

Endless stream

Ranges



