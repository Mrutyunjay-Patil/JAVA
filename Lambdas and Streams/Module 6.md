# Module 6: Parallel Streams and Concurrency

This module covers how to use parallel streams for concurrent processing in Java and explores the concurrency considerations when working with streams. It explains the basics of parallel streams and thread safety, along with practical exercises to ensure safe and efficient concurrent stream operations.

---

## Week 11: Introduction to Parallel Streams

### 11.1 When and How to Use Parallel Streams
Parallel streams allow processing elements concurrently using multiple threads, which can speed up operations on large data sets. However, they should be used carefully:
- Use parallel streams for CPU-intensive tasks and large collections.
- Ensure operations are stateless and non-interfering to avoid thread-safety issues.
- Test performance; not every scenario benefits from parallelization.

**Example: Converting sequential to parallel**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Sequential stream
List<Integer> sequentialResult = numbers.stream()
                                        .map(n -> n * 2)
                                        .collect(Collectors.toList());

// Parallel stream
List<Integer> parallelResult = numbers.parallelStream()
                                      .map(n -> n * 2)
                                      .collect(Collectors.toList());

System.out.println(parallelResult);
```
*Explanation:* 
- `.parallelStream()` creates a parallel stream that processes elements concurrently.

### 11.2 Understanding the Fork/Join Framework Basics
Parallel streams use the Fork/Join framework under the hood:
- It divides tasks into smaller subtasks (forks) and then combines results (joins).
- Java handles most of this automatically for parallel streams.
- For custom tasks, understanding Fork/Join can help fine-tune performance.

**Key Concepts:**
- **Fork:** Split a task into subtasks.
- **Join:** Combine results from subtasks.
- **Work Stealing:** Threads that finish their tasks can help others.

### 11.3 Differences Between Sequential and Parallel Streams
- **Sequential Streams:** Process elements one after another in a single thread.
- **Parallel Streams:** Split tasks across multiple threads to process elements concurrently.
  
**Considerations:**
- Parallel streams can improve performance but add overhead.
- Not suitable for small data sets or operations with side effects.
- Order of operations might not be preserved unless specified.

### 11.4 Hands-On: Converting Sequential Streams to Parallel Streams Safely
**Exercise:** Convert a sequential stream to a parallel one and compare performance.
```java
List<Integer> numbers = IntStream.rangeClosed(1, 1_000_000).boxed().collect(Collectors.toList());

// Sequential sum
long startTime = System.currentTimeMillis();
int sequentialSum = numbers.stream().mapToInt(Integer::intValue).sum();
long sequentialTime = System.currentTimeMillis() - startTime;

// Parallel sum
startTime = System.currentTimeMillis();
int parallelSum = numbers.parallelStream().mapToInt(Integer::intValue).sum();
long parallelTime = System.currentTimeMillis() - startTime;

System.out.println("Sequential Sum: " + sequentialSum + " Time: " + sequentialTime + "ms");
System.out.println("Parallel Sum: " + parallelSum + " Time: " + parallelTime + "ms");
```
*Explanation:*
- Measure and compare the execution time of sequential vs. parallel processing.
- Use parallel streams only after verifying thread safety and performance gain.

---

## Week 12: Advanced Concurrency with Streams

### 12.1 Thread Safety and Side-Effects in Streams
- **Thread Safety:** Ensure that operations within a parallel stream do not modify shared state or cause race conditions.
- **Side-Effects:** Avoid operations that change external variables or state, as they can lead to unpredictable results in parallel streams.

**Problematic Example:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> result = new ArrayList<>();
numbers.parallelStream().forEach(n -> result.add(n * 2)); // Unsafe! 
```
*Explanation:* 
- Modifying `result` concurrently is not thread-safe.

### 12.2 Synchronization Issues and Solutions
To manage shared state safely:
- Use thread-safe collections (e.g., `ConcurrentHashMap`, `CopyOnWriteArrayList`).
- Use proper synchronization when updating shared variables.
- Prefer immutable or local variables inside stream operations.

**Safe Example:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> safeResult = Collections.synchronizedList(new ArrayList<>());

numbers.parallelStream().forEach(n -> safeResult.add(n * 2));
```
*Explanation:* 
- Using a synchronized list helps avoid concurrency issues when adding elements.

### 12.3 Custom Thread Pool and Parallel Stream Management
By default, parallel streams use the common ForkJoinPool. You can provide a custom thread pool for more control:

**Example: Using a custom ForkJoinPool**
```java
ForkJoinPool customThreadPool = new ForkJoinPool(4); // 4 threads
try {
    List<Integer> results = customThreadPool.submit(
        () -> IntStream.range(1, 1000).parallel()
                       .map(n -> n * 2)
                       .boxed()
                       .collect(Collectors.toList())
    ).get();
    System.out.println(results);
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
} finally {
    customThreadPool.shutdown();
}
```
*Explanation:* 
- Create a custom `ForkJoinPool` with a specified number of threads.
- Submit a parallel stream task to this pool.
- Shutdown the pool after use.

### 12.4 Exercises on Writing Thread-Safe and Concurrent Stream Operations

**Exercise 1:** Process a list of items in parallel and collect results in a thread-safe manner.
```java
List<String> items = Arrays.asList("a", "b", "c", "d", "e");
List<String> processedItems = Collections.synchronizedList(new ArrayList<>());

items.parallelStream().forEach(item -> {
    // Simulate a thread-safe operation
    processedItems.add(item.toUpperCase());
});

System.out.println(processedItems);
```
*Explanation:* 
- Demonstrates safe addition of processed elements to a shared list.

**Exercise 2:** Use a custom thread pool to process a large dataset with a parallel stream.
```java
ForkJoinPool customPool = new ForkJoinPool(4);
List<Integer> largeData = IntStream.rangeClosed(1, 1_000_000).boxed().collect(Collectors.toList());

try {
    int sum = customPool.submit(
        () -> largeData.parallelStream()
                       .mapToInt(Integer::intValue)
                       .sum()
    ).get();
    System.out.println("Sum: " + sum);
} catch (Exception e) {
    e.printStackTrace();
} finally {
    customPool.shutdown();
}
```
*Explanation:*
- Processes a large list using a custom thread pool to compute the sum concurrently.

---

**Tips for Module 6:**
- Always consider thread safety when using parallel streams.
- Test both sequential and parallel code for correctness and performance.
- Use thread-safe collections or synchronization when needed.
- Understand the underlying Fork/Join mechanism for advanced control.

By the end of Module 6, you should:
- Know when and how to convert sequential streams to parallel streams.
- Understand the basics of the Fork/Join framework.
- Identify thread safety issues and apply solutions.
- Use custom thread pools to manage parallel streams effectively.
- Write concurrent stream operations that are safe and optimized.
