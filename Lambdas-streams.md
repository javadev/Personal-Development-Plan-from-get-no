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

```
