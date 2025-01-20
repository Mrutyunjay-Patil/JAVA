# Module 5: Specialized Stream Use-Cases

This module explores advanced and specialized topics in using streams, with a focus on safe handling of nulls, combining streams, and optimizing stream performance. The content emphasizes practical coding, error handling, and efficient use of streams.

---

## Week 9: Optional and Stream Combination

### 9.1 Deep Dive into Optional Usage in Stream Pipelines
`Optional` is a container that may hold a value or be empty. It helps avoid `NullPointerException` and makes code safer.

**Example: Using Optional with Streams**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
Optional<String> firstLongName = names.stream()
                                      .filter(name -> name.length() > 5)
                                      .findFirst();

if (firstLongName.isPresent()) {
    System.out.println("Found: " + firstLongName.get());
} else {
    System.out.println("No name longer than 5 letters found.");
}
```
*Explanation:* 
- `findFirst()` returns an `Optional<String>`.
- We check if a value is present before using it.

**Tip:** Use methods like `orElse`, `orElseGet`, or `ifPresent` to work with optionals safely:
```java
String longName = firstLongName.orElse("Default Name");
System.out.println(longName);
```

### 9.2 Handling Nulls Safely with Streams and Optionals
Null values in streams can cause errors. Use `Optional` or filter out nulls to handle them safely.

**Example: Filtering out nulls**
```java
List<String> possiblyNullNames = Arrays.asList("Alice", null, "Charlie");
List<String> safeNames = possiblyNullNames.stream()
                                          .filter(Objects::nonNull)
                                          .collect(Collectors.toList());
System.out.println(safeNames);
```
*Explanation:*
- `Objects::nonNull` filters out null values.

**Example: Using Optional to handle possible null returns**
```java
public Optional<String> getNickname(String name) {
    if (name.equals("Alice")) {
        return Optional.of("Ally");
    }
    return Optional.empty();
}

public void greet(String name) {
    getNickname(name).ifPresentOrElse(
        nickname -> System.out.println("Hello " + nickname),
        () -> System.out.println("Hello " + name)
    );
}
```
*Explanation:*
- `getNickname` returns an `Optional`.
- `ifPresentOrElse` handles both cases: when a nickname exists or not.

### 9.3 Combining Streams: Concatenation and Merging
Sometimes you need to combine two or more streams.

**Concatenation Example:**
```java
Stream<String> stream1 = Stream.of("a", "b", "c");
Stream<String> stream2 = Stream.of("d", "e");
Stream<String> combinedStream = Stream.concat(stream1, stream2);
combinedStream.forEach(System.out::println);
```
*Explanation:*
- `Stream.concat(stream1, stream2)` creates one stream containing elements from both.

**Merging Streams:**
To merge streams in more complex ways, combine them with operations:
```java
List<Integer> list1 = Arrays.asList(1, 2, 3);
List<Integer> list2 = Arrays.asList(4, 5, 6);

List<Integer> merged = Stream.of(list1, list2)
                             .flatMap(List::stream)
                             .collect(Collectors.toList());
System.out.println(merged);
```
*Explanation:*
- `Stream.of(list1, list2)` creates a stream of lists.
- `flatMap(List::stream)` merges them into one stream.

### 9.4 Exercises Focusing on Error Handling and Safe Operations

**Exercise 1:** Given a list of possible null strings, filter out null values, trim whitespace, and collect non-empty strings.
```java
List<String> rawStrings = Arrays.asList(" apple ", null, " ", "banana", "  cherry  ");
List<String> cleanStrings = rawStrings.stream()
    .filter(Objects::nonNull)              // Remove nulls
    .map(String::trim)                     // Trim whitespace
    .filter(s -> !s.isEmpty())             // Remove empty strings
    .collect(Collectors.toList());
System.out.println(cleanStrings);
```
*Explanation:* 
- We chain filters and maps to safely process strings.

**Exercise 2:** Concatenate two streams of integers, remove duplicates, and sort them.
```java
Stream<Integer> streamA = Stream.of(3, 5, 7);
Stream<Integer> streamB = Stream.of(2, 3, 5, 8);
List<Integer> sortedUnique = Stream.concat(streamA, streamB)
    .distinct()
    .sorted()
    .collect(Collectors.toList());
System.out.println(sortedUnique);
```
*Explanation:* 
- Combines two streams, removes duplicates, sorts the numbers, and collects them.

---

## Week 10: Performance and Optimization

### 10.1 Stream Performance Considerations
Streams can simplify code, but performance must be considered:
- **Overhead:** Creating streams and using many operations can add overhead compared to simple loops.
- **Boxing/Unboxing:** Using primitive streams (IntStream, LongStream) can improve performance.
- **Parallel Streams:** They can improve performance for large data sets but may not suit every situation.

**Tip:** Choose sequential or parallel streams based on data size and computation cost.

### 10.2 Lazy Evaluation and Short-Circuiting Operations
Streams use lazy evaluation, meaning operations are not executed until a terminal operation is called. Some intermediate operations are short-circuiting (stop processing when a condition is met).

**Example: Lazy evaluation with findFirst**
```java
List<String> words = Arrays.asList("apple", "banana", "cherry", "date");
Optional<String> result = words.stream()
                               .filter(word -> {
                                   System.out.println("Checking: " + word);
                                   return word.startsWith("c");
                               })
                               .findFirst();
System.out.println("Found: " + result.orElse("None"));
```
*Explanation:* 
- The filter stops as soon as it finds a word starting with "c".
- You can see how many words are checked before stopping.

### 10.3 Avoiding Common Pitfalls and Anti-Patterns
- **Avoid Shared Mutable State:** Do not modify external variables inside stream operations.
- **Be Careful with Parallel Streams:** Not all operations parallelize well, and some data structures are not thread-safe.
- **Mind Resource Usage:** Streams that read files or network resources should be closed properly.

**Anti-Pattern Example:** Modifying a list inside a stream.
```java
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3));
numbers.stream().forEach(n -> numbers.add(n * 2)); // Dangerous!
```
*Explanation:* 
- Modifying the source inside a stream can lead to unpredictable behavior.

### 10.4 Profiling and Optimizing Stream Code
To optimize, measure performance and identify bottlenecks:
- Use Java profiling tools (VisualVM, YourKit) to analyze stream performance.
- Replace expensive operations with more efficient ones if needed.
- Use parallel streams when appropriate, and ensure thread-safety.

**Example: Optimizing with primitive streams**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream()
                 .mapToInt(Integer::intValue) // Use primitive stream
                 .sum();
System.out.println("Sum: " + sum);
```
*Explanation:* 
- `mapToInt` converts stream to an `IntStream`, which is more efficient for numeric operations.

### 10.5 Exercises for Performance and Optimization

**Exercise 1:** Refactor a stream operation to use a primitive stream for calculating the sum of squares.
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sumOfSquares = numbers.stream()
                          .mapToInt(x -> x * x)
                          .sum();
System.out.println("Sum of squares: " + sumOfSquares);
```
*Explanation:* 
- `mapToInt` transforms each element to its square.
- `sum()` efficiently adds them up.

**Exercise 2:** Identify a potential performance issue in a given stream pipeline and suggest improvements.  
*(This exercise is theoretical: students review a provided code snippet, identify inefficiencies, and propose a better approach.)*

---

**Tips for Module 5:**
- Practice error handling by working with optionals and filtering out nulls.
- Combine streams thoughtfully, understanding how concatenation and merging work.
- Keep performance in mind, and use profiling tools to guide optimizations.
- Be aware of lazy evaluation and short-circuit behavior to write efficient stream pipelines.

By the end of Module 5, you should:
- Confidently use `Optional` to handle missing data.
- Safely combine multiple streams and manage nulls.
- Understand key performance aspects of streams.
- Optimize stream pipelines for better performance while avoiding common mistakes.
