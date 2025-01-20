# Module 9 (Bonus Module): 50 Critical Questions on Lambdas and Streams

The following set of 50 questions covers theoretical concepts, practical scenarios, code analysis, and tricky pitfalls related to Lambdas and Streams in Java. Some questions are Multiple Choice Questions (MCQ) with one correct answer, while others are Multiple Select Questions (MSQ) where more than one answer may be correct. Each question is followed by its answer for self-assessment.

---

### Questions

**Q1.** What is a functional interface in Java?  
A. An interface with multiple abstract methods  
B. An interface with exactly one abstract method  
C. An interface that cannot be implemented  
D. An interface with no methods  
**Answer:** B

**Q2.** Which annotation is used to indicate a functional interface?  
A. @Functional  
B. @Interface  
C. @FunctionalInterface  
D. @Lambda  
**Answer:** C

**Q3.** Which of the following are built-in functional interfaces in Java? (MSQ)  
A. Predicate  
B. Supplier  
C. Consumer  
D. Executor  
**Answer:** A, B, C

**Q4.** Given the lambda expression `(x, y) -> x + y`, what is its type?  
A. Runnable  
B. Comparator  
C. BiFunction<Integer, Integer, Integer>  
D. Consumer<Integer>  
**Answer:** C

**Q5.** What does the following code print?
```java
List<Integer> list = Arrays.asList(1,2,3);
list.stream().map(n -> n * 2).forEach(System.out::println);
```  
A. 1 2 3  
B. 2 4 6  
C. 1 4 9  
D. Compilation error  
**Answer:** B

**Q6.** Which method(s) can convert a Stream to a List in JDK 21? (MSQ)  
A. collect(Collectors.toSet())  
B. collect(Collectors.toMap())  
C. collect(Collectors.toList())  
D. toList()  
**Answer:** C, D

**Q7.** What is the output of the following code?
```java
Stream.of("a", "b", "c")
      .filter(s -> s.length() > 1)
      .findFirst()
      .orElse("none");
```  
A. a  
B. b  
C. none  
D. Compilation error  
**Answer:** C

**Q8.** In a stream pipeline, which of the following are terminal operations? (MSQ)  
A. forEach  
B. filter  
C. collect  
D. map  
**Answer:** A, C

**Q9.** Which of these statements about streams is FALSE?  
A. Streams are lazy  
B. Streams can be reused multiple times  
C. Streams do not modify their source  
D. Intermediate operations are lazy  
**Answer:** B

**Q10.** What does `flatMap` do?  
A. Flattens nested collections  
B. Filters elements  
C. Maps each element to a stream and flattens the result  
D. Sorts the stream  
**Answer:** C

**Q11.** Identify the error in this code:
```java
List<String> list = Arrays.asList("a", "b", "c");
list.stream().forEach(n -> list.add(n.toUpperCase()));
```  
A. No error  
B. UnsupportedOperationException at runtime  
C. Compilation error  
D. NullPointerException  
**Answer:** B

**Q12.** Which of these are correct ways to create a stream? (MSQ)  
A. list.stream()  
B. Stream.of(1, 2, 3)  
C. new Stream<>()  
D. Arrays.stream(new int[]{1,2,3})  
**Answer:** A, B, D

**Q13.** What does the following code return?
```java
Optional<String> opt = Optional.of("Java");
System.out.println(opt.map(String::toUpperCase).orElse("Empty"));
```  
A. JAVA  
B. Java  
C. Empty  
D. Null  
**Answer:** A

**Q14.** Which collector groups elements based on a classification function?  
A. toList()  
B. groupingBy()  
C. partitioningBy()  
D. toMap()  
**Answer:** B

**Q15.** What does `Collectors.partitioningBy(predicate)` return?  
A. Map<T, List<T>>  
B. Map<Boolean, List<T>>  
C. Set<T>  
D. List<T>  
**Answer:** B

**Q16.** How can you safely handle a potentially null list before streaming? (MSQ)  
A. list.stream() without check  
B. Use Optional.ofNullable(list).orElse(Collections.emptyList()).stream()  
C. Check if list != null then stream  
D. Use a try-catch block  
**Answer:** B, C

**Q17.** What is the purpose of `peek` in a stream pipeline?  
A. To transform elements  
B. To debug and inspect elements  
C. To sort elements  
D. To terminate the stream  
**Answer:** B

**Q18.** Given a stream operation with multiple intermediate steps, when are these executed?  
A. Immediately  
B. Lazily, upon a terminal operation  
C. After the first intermediate operation  
D. After each map operation  
**Answer:** B

**Q19.** Which statement about parallel streams is TRUE?  
A. They always perform faster than sequential streams  
B. They can introduce concurrency issues  
C. They maintain order without extra effort  
D. They use a separate JVM  
**Answer:** B

**Q20.** In a parallel stream, which collection type is safe to use for collecting results without extra synchronization?  
A. ArrayList  
B. HashSet  
C. ConcurrentHashMap  
D. SynchronizedList  
**Answer:** C

**Q21.** Which method is used to combine two streams sequentially?  
A. merge()  
B. concat()  
C. join()  
D. combine()  
**Answer:** B

**Q22.** What does the following code output?
```java
List<String> list = Arrays.asList("one", "two", "three", "four");
long count = list.stream().filter(s -> s.length() > 3).count();
System.out.println(count);
```  
A. 2  
B. 3  
C. 4  
D. 0  
**Answer:** A

**Q23.** Which scenario can cause a race condition in stream processing?  
A. Using distinct()  
B. Modifying a shared mutable object inside forEach() in a parallel stream  
C. Filtering elements  
D. Collecting to a new list  
**Answer:** B

**Q24.** How can you create a custom functional interface?  
A. Annotate an interface with @FunctionalInterface and declare one abstract method  
B. Write an abstract class with one method  
C. Use the keyword function in interface  
D. Implement Runnable  
**Answer:** A

**Q25.** What will this code print?
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Optional<Integer> sum = numbers.stream().reduce(Integer::sum);
System.out.println(sum.orElse(0));
```  
A. 0  
B. 15  
C. Optional[15]  
D. Compilation error  
**Answer:** B

**Q26.** Which of these stream operations are intermediate? (MSQ)  
A. filter()  
B. collect()  
C. map()  
D. forEach()  
**Answer:** A, C

**Q27.** What does the following code do?
```java
List<String> letters = Arrays.asList("A", "B", "C");
String result = letters.stream().collect(Collectors.joining("-"));
System.out.println(result);
```  
A. A-B-C  
B. ABC  
C. [A, B, C]  
D. Compilation error  
**Answer:** A

**Q28.** Which operation can cause a stream pipeline to short-circuit?  
A. filter()  
B. map()  
C. findFirst()  
D. collect()  
**Answer:** C

**Q29.** Identify the potential problem:
```java
Stream<String> stream = Stream.of("one", "two");
stream.forEach(System.out::println);
stream.forEach(System.out::println);
```  
A. No problem  
B. IllegalStateException on second forEach  
C. Data loss  
D. Compilation error  
**Answer:** B

**Q30.** How do you create a stream from an array of integers?  
A. Arrays.stream(intArray)  
B. Stream.of(intArray)  
C. intArray.stream()  
D. new Stream<>(intArray)  
**Answer:** A

**Q31.** What does `mapToInt(String::length)` return?  
A. Stream<Integer>  
B. IntStream  
C. List<Integer>  
D. OptionalInt  
**Answer:** B

**Q32.** Which of the following can be used to sort a stream of integers in natural order?  
A. sorted()  
B. sort()  
C. orderBy()  
D. stream.sort()  
**Answer:** A

**Q33.** What is a key benefit of using Lambdas and Streams?  
A. Slower code  
B. More verbose code  
C. Functional-style programming that improves readability and maintainability  
D. Incompatible with Java 8  
**Answer:** C

**Q34.** Which of these are valid ways to handle Optional values? (MSQ)  
A. get()  
B. orElse()  
C. isPresent()  
D. map()  
**Answer:** A, B, C, D

**Q35.** What does the `reduce` method do in streams?  
A. Splits the stream  
B. Combines stream elements to produce a single result  
C. Reduces memory usage  
D. Increases performance  
**Answer:** B

**Q36.** Which code will likely cause a NullPointerException?
```java
List<String> list = null;
list.stream().filter(s -> s.isEmpty()).collect(Collectors.toList());
```  
A. Safe code  
B. Runtime error (NullPointerException)  
C. Compilation error  
D. Always returns empty list  
**Answer:** B

**Q37.** Which collector would you use to summarize statistics of integers?  
A. groupingBy  
B. summarizingInt  
C. reducing  
D. partitioningBy  
**Answer:** B

**Q38.** In a parallel stream, what ensures tasks are executed concurrently?  
A. Single-threaded execution  
B. Fork/Join framework  
C. Sequential processing  
D. Java Virtual Machine  
**Answer:** B

**Q39.** Which of these statements about `Optional` is FALSE?  
A. It can contain a null value  
B. It helps avoid null checks  
C. It provides methods like orElse()  
D. It can be empty  
**Answer:** A

**Q40.** What happens if an exception is thrown inside a lambda used in a stream?  
A. The stream continues silently  
B. The exception is propagated and may terminate the stream  
C. The lambda ignores exceptions  
D. The program crashes without error message  
**Answer:** B

**Q41.** Identify all correct statements about method references. (MSQ)  
A. They are a shorthand for lambda expressions  
B. They can reference static methods  
C. They can reference instance methods  
D. They require explicit type casting  
**Answer:** A, B, C

**Q42.** What is the result of this code?
```java
List<Integer> numbers = Arrays.asList(1,2,3);
int result = numbers.stream().reduce(10, (a,b) -> a - b);
System.out.println(result);
```  
A. 4  
B. 6  
C. 0  
D. -5  
**Answer:** A  
*Explanation:* With initial value 10: ((10 - 1) - 2) - 3 = 4.

**Q43.** Which of these operations are safe in parallel streams? (MSQ)  
A. Stateless transformations  
B. Modifying external state  
C. Using thread-safe collections  
D. Using side-effects inside map()  
**Answer:** A, C

**Q44.** What does `Collectors.toMap()` require if keys might collide?  
A. Nothing extra  
B. A merge function  
C. A List collector  
D. A Set collector  
**Answer:** B

**Q45.** How to check if a stream contains any element matching a predicate?  
A. allMatch()  
B. anyMatch()  
C. noneMatch()  
D. count()  
**Answer:** B

**Q46.** What is wrong with this code?
```java
Stream<Integer> stream = Stream.of(1, 2, 3);
stream = stream.filter(n -> n > 1);
stream.forEach(System.out::println);
stream.forEach(System.out::println);
```  
A. Using filter() twice  
B. Reusing a stream after a terminal operation  
C. Nothing wrong  
D. Streams can't filter integers  
**Answer:** B

**Q47.** Which method can be used to combine two Optional values?  
A. orElse()  
B. flatMap()  
C. combine()  
D. merge()  
**Answer:** B

**Q48.** Identify the output:
```java
Optional<Integer> opt = Optional.of(5);
System.out.println(opt.filter(n -> n > 10).orElse(-1));
```  
A. 5  
B. -1  
C. Optional[5]  
D. null  
**Answer:** B

**Q49.** Which of the following code snippets correctly uses a custom thread pool with parallel streams?  
A. Using ForkJoinPool.commonPool()  
B. Submitting tasks to a ForkJoinPool as shown in examples  
C. Using Executors.newFixedThreadPool() directly with streams  
D. There is no way to set a custom thread pool for parallel streams  
**Answer:** B

**Q50.** What is the primary advantage of using streams over loops?  
A. More lines of code  
B. Imperative style  
C. Declarative, concise, and easier to reason about operations on collections  
D. Streams replace all loops  
**Answer:** C

---

**Note:** These questions cover a wide range of topics, from theory to code analysis, and include critical thinking and error detection. Use them to test your understanding and readiness to work confidently with Lambdas and Streams.
