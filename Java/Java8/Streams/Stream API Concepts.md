### **Java Stream API

**Introduction

The Java Stream API, introduced in Java 8, provides a powerful way to process sequences of elements in a functional style. Streams allow you to perform operations such as filtering, mapping, and reducing on collections in a declarative manner. The Stream API is part of the `java.util.stream` package and is designed to work seamlessly with the Java Collections Framework.

Key Points:
• Functional Programming: Streams enable a functional approach to processing collections.
• Laziness: Operations on streams are lazy and are executed only when necessary.
• Parallelism: Streams can be processed in parallel to improve performance.
• Pipeline: Streams support a pipeline of operations, including intermediate and terminal operations.

Table of Contents
1. Creating Streams
2. Intermediate Operations
    ◦ filter
    ◦ map
    ◦ flatMap
    ◦ distinct
    ◦ sorted
    ◦ peek
    ◦ limit
    ◦ skip
3. Terminal Operations
    ◦ forEach
    ◦ collect
    ◦ reduce
    ◦ toArray
    ◦ count
    ◦ anyMatch, allMatch, noneMatch
    ◦ findFirst, findAny
4. Short-Circuiting Operations
5. Parallel Streams
6. Conclusion

1. Creating Streams
Streams can be created from various data sources, such as collections, arrays, and I/O channels.
Example:

`import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class StreamCreationExample {
    public static void main(String[] args) {
        // From a collection
        List<String> list = Arrays.asList("a", "b", "c");
        Stream<String> streamFromList = list.stream();

        // From an array
        String[] array = {"x", "y", "z"};
        Stream<String> streamFromArray = Arrays.stream(array);

        // Using Stream.of
        Stream<String> streamOf = Stream.of("one", "two", "three");
    }
}`

2. Intermediate Operations
Intermediate operations return a new stream and are lazy; they are not executed until a terminal operation is invoked.
filter
Filters elements based on a predicate.
Example:

`import java.util.Arrays;
import java.util.List;

public class FilterExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        list.stream()
            .filter(s -> s.startsWith("a"))
            .forEach(System.out::println); // apple
    }
}`

map
Transforms each element by applying a function.
Example:

`import java.util.Arrays;
import java.util.List;

public class MapExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        list.stream()
            .map(String::toUpperCase)
            .forEach(System.out::println); // APPLE BANANA CHERRY
    }
}`

flatMap
Flattens nested structures into a single stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class FlatMapExample {
    public static void main(String[] args) {
        List<List<String>> listOfLists = Arrays.asList(
            Arrays.asList("a", "b"),
            Arrays.asList("c", "d")
        );

        listOfLists.stream()
                   .flatMap(List::stream)
                   .forEach(System.out::println); // a b c d
    }
}`

distinct
Removes duplicate elements from the stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class DistinctExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "apple");
        list.stream()
            .distinct()
            .forEach(System.out::println); // apple banana
    }
}`

sorted
Sorts the elements in natural order or by a custom comparator.
Example:

`import java.util.Arrays;
import java.util.List;

public class SortedExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        list.stream()
            .sorted()
            .forEach(System.out::println); // apple banana cherry
    }
}`

peek
Performs an action for each element of the stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class PeekExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        list.stream()
            .peek(System.out::println)
            .map(String::toUpperCase)
            .forEach(System.out::println);
    }
}`

limit
Truncates the stream to a specified number of elements.
Example:

`import java.util.Arrays;
import java.util.List;

public class LimitExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        list.stream()
            .limit(2)
            .forEach(System.out::println); // apple banana
    }
}`

skip
Skips the first `n` elements of the stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class SkipExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        list.stream()
            .skip(1)
            .forEach(System.out::println); // banana cherry
    }
}`

3. Terminal Operations
Terminal operations produce a result or side-effect and mark the end of the stream processing.
forEach
Performs an action for each element of the stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class ForEachExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        list.stream()
            .forEach(System.out::println); // apple banana cherry
    }
}`

collect
Collects the elements of the stream into a collection.
Example:

`import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class CollectExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        List<String> upperCaseList = list.stream()
                                         .map(String::toUpperCase)
                                         .collect(Collectors.toList());
        System.out.println(upperCaseList); // [APPLE, BANANA, CHERRY]
    }
}`

reduce
Reduces the elements of the stream to a single value.
Example:

`import java.util.Arrays;
import java.util.List;

public class ReduceExample {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
        int sum = list.stream()
                      .reduce(0, Integer::sum);
        System.out.println(sum); // 15
    }
}`

toArray
Returns an array containing the elements of the stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class ToArrayExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        String[] array = list.stream()
                             .toArray(String[]::new);
        System.out.println(Arrays.toString(array)); // [apple, banana, cherry]
    }
}`

count
Returns the number of elements in the stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class CountExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");
        long count = list.stream()
                         .count();
        System.out.println(count); // 3
    }
}`

anyMatch, allMatch, noneMatch
Checks if any, all, or none of the elements match a given predicate.
Example:

`import java.util.Arrays;
import java.util.List;

public class MatchExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");

        boolean anyMatch = list.stream()
                               .anyMatch(s -> s.startsWith("a"));
        System.out.println(anyMatch); // true

        boolean allMatch = list.stream()
                               .allMatch(s -> s.length() > 2);
        System.out.println(allMatch); // true

        boolean noneMatch = list.stream()
                                .noneMatch(s -> s.startsWith("z"));
        System.out.println(noneMatch); // true
    }
}`

findFirst, findAny
Returns the first element or any element of the stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class FindExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry");

        String first = list.stream()
                           .findFirst()
                           .orElse("none");
        System.out.println(first); // apple

        String any = list.stream()
                         .findAny()
                         .orElse("none");
        System.out.println(any); // apple (or any other element)
    }
}`

4. Short-Circuiting Operations
Short-circuiting operations allow the stream to stop processing as soon as a certain condition is met. These operations include `anyMatch`, `allMatch`, `noneMatch`, `findFirst`, and `findAny`.
Example:

`import java.util.Arrays;
import java.util.List;

public class ShortCircuitingExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry", "date", "elderberry");

        boolean found = list.stream()
                            .anyMatch(s -> {
                                System.out.println("Checking: " + s);
                                return s.startsWith("d");
                            });
        System.out.println("Found: " + found); // Found: true
    }
}`

Explanation:
In this example, the stream stops processing once it finds an element that starts with “d”, demonstrating the short-circuiting behavior of `anyMatch`.
5. Parallel Streams
Parallel streams leverage multi-core processors to process data in parallel, potentially improving performance for large datasets. To create a parallel stream, you can use the `parallelStream()` method or call `parallel()` on an existing stream.
Example:

`import java.util.Arrays;
import java.util.List;

public class ParallelStreamExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("apple", "banana", "cherry", "date", "elderberry");

        list.parallelStream()
            .forEach(s -> System.out.println(Thread.currentThread().getName() + ": " + s));
    }
}`

Explanation:
In this example, the elements are processed in parallel, as indicated by the different thread names in the output.
6. Conclusion
The Java Stream API provides a powerful and flexible way to process sequences of elements in a functional style. By leveraging streams, you can write more concise and readable code for tasks such as filtering, mapping, reducing, and more. Understanding and utilizing both intermediate and terminal operations allows for efficient data processing, while parallel streams offer the potential for performance improvements in multi-core environments.**

- • **Functional Programming**: Streams enable a functional approach to processing collections.
- • **Laziness**: Operations on streams are lazy and are executed only when necessary.
- • **Parallelism**: Streams can be processed in parallel to improve performance.
- • **Pipeline**: Streams support a pipeline of operations, including intermediate and terminal operations.
1. 1. Creating Streams
2. 2. Intermediate Operations
    - ◦ filter
    - ◦ map
    - ◦ flatMap
    - ◦ distinct
    - ◦ sorted
    - ◦ peek
    - ◦ limit
    - ◦ skip
- 
- 
- 
- 
- 
- 
- 
- 
1. 3. Terminal Operations
    - ◦ forEach
    - ◦ collect
    - ◦ reduce
    - ◦ toArray
    - ◦ count
    - ◦ anyMatch, allMatch, noneMatch
    - ◦ findFirst, findAny
- 
- 
- 
- 
- 
- 
- 
1. 4. Short-Circuiting Operations
2. 5. Parallel Streams
3. 6. Conclusion