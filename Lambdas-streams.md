Java 8 Lambda Streams
=====================

1. Create new stream

 ```java
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
        Stream<Integer> stream2 = Arrays.asList(5, 2, 3, 1, 4).stream();
    }
 }
 ```

2. Get first element

 ```java
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
        Integer first = stream.findFirst().get();
    }
 }
 ```

3. Get first n elements

 ```java
 import java.util.*;
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
        List<Integer> firstList = stream.limit(3).collect(Collectors.toList());
        System.out.println(firstList);
    }
 }
 // [5, 2, 3]
 ```

4. Get last element

 ```java
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
        Integer last = stream.reduce((a, b) -> b).orElse(null);
    }
 }
 ```

5. Get sum of elements

 ```java
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
        Integer sum = stream.mapToInt(Integer::intValue).sum();
    }
 }
 ```

6. Filter elements

 ```java
 import java.util.*
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
        List<Integer> filtered = stream.filter(item -> item % 2 == 0).collect(Collectors.toList());
    }
 }
 ```

7. Map elements

 ```java
 import java.util.*;
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
        List<Integer> squares = stream.map(i -> i * i).collect(Collectors.toList());
    }
 }
 ```

8. Sort elements

 ```java
 import java.util.*;
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        Stream<Integer> stream = Stream.of(5, 2, 3, 1, 4);
        List<Integer> sorted = stream.sorted((i1, i2) -> Integer.compare(i1,
                    i2)).collect(Collectors.toList());
    }
 }
 ```

9. Grouping elements

 ```java

 import java.util.*;
 import java.util.function.*;
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        List<String> list = new ArrayList<>();
        list.add("Hello");
        list.add("Hello");
        list.add("World");

        Map<String, Long> counted = list.stream()
            .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        System.out.println(counted);
    }
 }
 // Result:

 {Hello=2, World=1}
 ```

10. Reverse elements

 ```java
 import java.util.*;
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        List<Integer> reversed = Stream.of(1, 2, 3, 4, 5)
            .sorted(Collections.reverseOrder())
            .collect(Collectors.toList());
    }
 }
 // [5, 4, 3, 2, 1]   
 ```

11. Sort map by value

 ```java
 import java.util.*;
 import java.util.stream.*;

 public class Main {
    public static void main(String ... args) {
        List<String> sortedByValue = new LinkedHashMap<String, Integer>() { {
           put("one", 1);
           put("tree", 3);
           put("two", 2);
           } }.entrySet()
           .stream()
           .sorted((e1, e2) -> e1.getValue().compareTo(e2.getValue()))
           .map(e -> e.getKey())
           .collect(Collectors.toList());
    }
 }
 ```

12. Combine streams in one

 ```java
 import java.util.*;
 import java.util.stream.*;

 public class Main {
  public static void main(String ... args) {
      Stream<Integer> stream1 = Stream.of(5, 2, 3, 1, 4);
      Stream<Integer> stream2 = Stream.of(40, 20, 30, 10, 40);
      List<Integer> concatedList = Stream.concat(stream1, stream2).collect(Collectors.toList());
      System.out.println(concatedList);
  }
 }
 ```

13. Range int stream

 ```java
 import java.util.*;
 import java.util.stream.*;

 public class Main {
  public static void main(String ... args) {
      List<Integer> rangeList = IntStream.range(1, 3).boxed().collect(Collectors.toList());
      System.out.println(rangeList);
  }
 } 
 // > [1, 2]
 ```

14. Range closed int stream

 ```java
 import java.util.*;
 import java.util.stream.*;

 public class Main {
  public static void main(String ... args) {
      List<Integer> rangeClosedList = IntStream.rangeClosed(1, 3).boxed().collect(Collectors.toList());
      System.out.println(rangeClosedList);
  }
 } 
 // > [1, 2, 3]
 ```

anyMatch

```java
IntStream.range(1, 5).anyMatch(i -> i % 2 == 0);  
// > true
```

allMatch

```java
IntStream.range(1, 5).allMatch(i -> i % 2 == 0);  
// > false
```

noneMatch

```java
IntStream.range(1, 5).noneMatch(i -> i % 2 == 0);  
// > false
```

Max/Min elements

```java
IntStream.range(1, 5).max().getAsInt();  
// > 4
IntStream.range(1, 5).min().getAsInt();  
// > 1
```

Reduce elements

```java
IntStream.range(1, 5).reduce(1, (x, y) -> x * y)  
// > 24
```

Parallel streams

```java
IntStream.range(1, 5).parallel().forEach(i -> heavyOperation());  
```

