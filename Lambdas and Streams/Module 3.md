# Module 3: Introduction to Streams API

This module introduces the Java Streams API, a powerful tool for processing collections of data in a functional style. Students will learn what streams are, how to create them, and how to use basic and intermediate operations to manipulate data. The focus is on hands-on exercises to build a strong practical understanding of streams.

---

## Week 5: Basics of Streams

### 5.1 What are Streams?
A **Stream** in Java is a sequence of elements that supports various operations to perform computations on the data. Streams do not store data themselves; they operate on a data source like collections, arrays, or I/O channels. They allow you to write concise and readable code for processing data in a functional style.

**Key Points:**
- Streams do not modify their source.
- They support lazy evaluation, meaning operations are performed only when needed.
- Streams can be chained with multiple operations.

### 5.2 Stream Creation from Collections, Arrays, I/O, etc.
You can create streams from various data sources:
- **From a Collection:**
  ```java
  List<String> list = Arrays.asList("apple", "banana", "cherry");
  Stream<String> streamFromList = list.stream();
  ```
- **From an Array:**
  ```java
  String[] fruits = {"apple", "banana", "cherry"};
  Stream<String> streamFromArray = Arrays.stream(fruits);
  ```
- **From I/O lines:**
  ```java
  try (Stream<String> lines = Files.lines(Paths.get("file.txt"))) {
      // Use the stream
  } catch (IOException e) {
      e.printStackTrace();
  }
  ```
*Explanation:* These examples show different ways to start a stream from a list, an array, or lines in a file.

### 5.3 Differences Between Collections and Streams
- **Collections** hold data; streams process data.
- **Collections** are about storing and accessing data; **streams** are about describing and executing operations on data.
- Streams cannot be reused once a terminal operation is executed.

### 5.4 Overview of Stream Pipeline
A stream pipeline has three main parts:
1. **Source:** Where the data comes from (e.g., a collection).
2. **Intermediate Operations:** These operations transform or filter the stream (e.g., `filter()`, `map()`).
3. **Terminal Operation:** This operation produces a result or a side-effect (e.g., `collect()`, `forEach()`).

**Example of a simple pipeline:**
```java
List<String> fruits = Arrays.asList("apple", "banana", "cherry");
List<String> result = fruits.stream()            // Source
                            .filter(s -> s.startsWith("a")) // Intermediate
                            .collect(Collectors.toList());  // Terminal
System.out.println(result); // Output: [apple]
```
*Explanation:*
- We start with a list of fruits.
- Create a stream from the list.
- Use `filter` to keep only fruits starting with "a".
- Use `collect` to gather results into a new list.

### 5.5 Hands-On Exercises Creating and Consuming Streams
**Exercise 1:** Create a stream from a list of integers and print each number.
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.stream()
       .forEach(System.out::println);
```
*Explanation:* 
- `forEach(System.out::println)` prints each element in the stream.

**Exercise 2:** Create a stream from an array of strings, convert each string to uppercase, and collect the results into a list.
```java
String[] words = {"hello", "world"};
List<String> upperWords = Arrays.stream(words)
                                .map(String::toUpperCase)
                                .collect(Collectors.toList());
System.out.println(upperWords);
```
*Explanation:* 
- `map(String::toUpperCase)` converts each word to uppercase.
- `collect(Collectors.toList())` collects the results into a list.

---

## Week 6: Intermediate Stream Operations

### 6.1 Filter, Map, and FlatMap Operations

**Filter:** Selects elements that match a condition.
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
List<String> namesWithA = names.stream()
                               .filter(name -> name.contains("a") || name.contains("A"))
                               .collect(Collectors.toList());
System.out.println(namesWithA);
```
*Explanation:* 
- `filter` keeps only names with the letter "a" or "A".

**Map:** Transforms each element of the stream.
```java
List<Integer> numbers = Arrays.asList(1, 2, 3);
List<Integer> squares = numbers.stream()
                               .map(n -> n * n)
                               .collect(Collectors.toList());
System.out.println(squares);
```
*Explanation:* 
- `map(n -> n * n)` changes each number to its square.

**FlatMap:** Flattens streams of streams into a single stream.
```java
List<List<String>> nestedList = Arrays.asList(
    Arrays.asList("a", "b"),
    Arrays.asList("c", "d")
);
List<String> flatList = nestedList.stream()
                                  .flatMap(list -> list.stream())
                                  .collect(Collectors.toList());
System.out.println(flatList);
```
*Explanation:* 
- `flatMap(list -> list.stream())` converts a stream of lists into a stream of elements.

### 6.2 Distinct, Sorted, and Peek Operations

**Distinct:** Removes duplicate elements.
```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3);
List<Integer> distinctNumbers = numbers.stream()
                                       .distinct()
                                       .collect(Collectors.toList());
System.out.println(distinctNumbers);
```
*Explanation:* 
- `distinct()` filters out duplicate numbers.

**Sorted:** Sorts the elements.
```java
List<String> fruits = Arrays.asList("banana", "apple", "cherry");
List<String> sortedFruits = fruits.stream()
                                  .sorted()
                                  .collect(Collectors.toList());
System.out.println(sortedFruits);
```
*Explanation:* 
- `sorted()` arranges the fruits in alphabetical order.

**Peek:** Allows you to see the elements as they flow through the pipeline (useful for debugging).
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
List<String> result = names.stream()
                           .filter(name -> name.length() > 3)
                           .peek(name -> System.out.println("Filtered: " + name))
                           .map(String::toUpperCase)
                           .peek(name -> System.out.println("Mapped: " + name))
                           .collect(Collectors.toList());
System.out.println(result);
```
*Explanation:* 
- `peek` prints the elements at different stages of the pipeline.

### 6.3 Working with Optional in Stream Pipelines
`Optional` is a container that may or may not hold a non-null value. It is often used with streams to handle cases where no result is found.

**Example: Finding a value in a list**
```java
List<Integer> numbers = Arrays.asList(2, 4, 6, 8);
Optional<Integer> firstOdd = numbers.stream()
                                    .filter(n -> n % 2 != 0)
                                    .findFirst();
System.out.println(firstOdd.orElse(-1)); // Outputs -1 since no odd number found
```
*Explanation:*
- `findFirst()` returns an `Optional` containing the first odd number if it exists.
- `orElse(-1)` provides a default value if no odd number is found.

### 6.4 Lab Exercises: Manipulating Collections with Intermediate Operations

**Exercise 1:** Given a list of words, filter out words shorter than 4 letters, convert remaining words to uppercase, remove duplicates, and sort them.
```java
List<String> words = Arrays.asList("apple", "tree", "sun", "apple", "moon", "sky");
List<String> processed = words.stream()
                              .filter(word -> word.length() >= 4)
                              .map(String::toUpperCase)
                              .distinct()
                              .sorted()
                              .collect(Collectors.toList());
System.out.println(processed);
```
*Explanation:*
- `filter` removes words with fewer than 4 letters.
- `map` converts words to uppercase.
- `distinct` removes duplicates.
- `sorted` sorts them alphabetically.

**Exercise 2:** Given a list of lists of integers, flatten it, remove even numbers, and collect odd numbers into a set.
```java
List<List<Integer>> listOfLists = Arrays.asList(
    Arrays.asList(1, 2, 3),
    Arrays.asList(4, 5, 6),
    Arrays.asList(7, 8, 9)
);
Set<Integer> oddNumbers = listOfLists.stream()
                                     .flatMap(List::stream)
                                     .filter(n -> n % 2 != 0)
                                     .collect(Collectors.toSet());
System.out.println(oddNumbers);
```
*Explanation:*
- `flatMap(List::stream)` flattens the list of lists.
- `filter(n -> n % 2 != 0)` keeps only odd numbers.
- `collect(Collectors.toSet())` gathers the result into a set, removing duplicates.

---

**Tips for Working with Streams:**
- Write small snippets to test each stream operation.
- Chain operations step by step and use `peek` for debugging.
- Use the Java documentation to explore more intermediate and terminal operations.

By the end of Module 3, students should:
- Understand the basics of streams and how to create them.
- Use fundamental stream operations to process data.
- Apply intermediate operations like filter, map, flatMap, distinct, sorted, and peek.
- Handle optional values within stream pipelines.
- Gain practical experience through hands-on exercises manipulating collections with streams.
