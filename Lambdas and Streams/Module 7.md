# Module 7: Best Practices, Testing, and Real-World Applications

This module focuses on how to write clean, maintainable, and testable code when using lambdas and streams. It covers best practices, debugging techniques, code reviews, refactoring from imperative to stream-based code, and methods for unit testing stream operations.

---

## Week 13: Best Practices in Lambdas and Streams

### 13.1 Writing Readable and Maintainable Lambda Code
- **Keep Lambdas Simple:** Lambdas should be short and focused on a single task. If complex logic is needed, consider extracting a method.
- **Use Descriptive Variable Names:** This helps readers understand the purpose of a lambda.
- **Avoid Overuse:** Not every situation needs a lambda or stream. Use them where they make code clearer.
- **Consistent Formatting:** Format code in a consistent way for easier reading.

**Example: Readable lambda**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Instead of a long lambda, break logic into methods
Predicate<String> startsWithA = name -> name.startsWith("A");
List<String> namesStartingWithA = names.stream()
                                       .filter(startsWithA)
                                       .collect(Collectors.toList());
```
*Explanation:* 
- Define separate predicates or methods for clarity instead of nesting too many operations.

### 13.2 Debugging Streams and Lambdas Effectively
- **Use `peek`:** The `peek` method can inspect elements as they flow through the pipeline for debugging.
- **Log Intermediate Steps:** Print statements or logging inside lambdas can help trace values.
- **Break Down Pipelines:** Split complex pipelines into smaller parts to isolate issues.

**Example: Using `peek` for debugging**
```java
List<String> result = names.stream()
    .filter(name -> name.length() > 3)
    .peek(name -> System.out.println("After filter: " + name))
    .map(String::toUpperCase)
    .peek(name -> System.out.println("After map: " + name))
    .collect(Collectors.toList());
```
*Explanation:* 
- `peek` prints intermediate results, helping understand the flow of data.

### 13.3 Code Reviews and Refactoring Imperative Code to Streams
- **Code Reviews:** Share your stream-based solutions with peers. Check for readability, performance, and proper use of stream operations.
- **Refactoring:** Convert loops and if-else constructs to stream operations where it improves clarity.
  
**Example: Refactoring**
Imperative:
```java
List<String> uppercaseNames = new ArrayList<>();
for (String name : names) {
    if (name.length() > 3) {
        uppercaseNames.add(name.toUpperCase());
    }
}
```
Refactored with streams:
```java
List<String> uppercaseNames = names.stream()
                                   .filter(name -> name.length() > 3)
                                   .map(String::toUpperCase)
                                   .collect(Collectors.toList());
```
*Explanation:* 
- The stream version is shorter and often easier to read, but always consider if the change improves clarity.

### 13.4 Group Discussion: Real-World Scenarios and Solutions
- Discuss common challenges encountered when applying lambdas and streams in real projects.
- Share strategies on performance, readability, and maintainability.
- Explore case studies where refactoring to streams improved code quality.

---

## Week 14: Testing and Debugging Streams

### 14.1 Unit Testing Code That Uses Lambdas and Streams
- **Test Methods in Isolation:** Test functions and lambdas separately before integrating into streams.
- **Assert Results of Stream Operations:** Verify that stream pipelines produce the expected output.

**Example: Unit test using JUnit**
```java
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class StreamTest {
    @Test
    public void testUppercaseFilter() {
        List<String> names = Arrays.asList("Alice", "Bob", "Sam");
        List<String> expected = Arrays.asList("ALICE");
        
        List<String> result = names.stream()
                                   .filter(name -> name.startsWith("A"))
                                   .map(String::toUpperCase)
                                   .collect(Collectors.toList());
        
        assertEquals(expected, result);
    }
}
```
*Explanation:* 
- The test checks that the stream returns the correct list when filtering and mapping names.

### 14.2 Tools and Techniques for Debugging Streams
- **Debuggers:** Use IDE debuggers to step through lambda executions.
- **Logging:** Insert logging statements inside lambda expressions to monitor behavior.
- **Visualization Tools:** Some IDEs offer visualization of stream pipelines to see how data flows.

### 14.3 Common Libraries and Utilities to Ease Testing (e.g., AssertJ)
- **AssertJ:** Provides a fluent API for assertions, making tests more readable.
  
**Example with AssertJ:**
```java
import static org.assertj.core.api.Assertions.assertThat;

@Test
public void testStreamResults() {
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
    List<Integer> evenNumbers = numbers.stream()
                                       .filter(n -> n % 2 == 0)
                                       .collect(Collectors.toList());
    
    assertThat(evenNumbers).containsExactly(2, 4);
}
```
*Explanation:* 
- AssertJ offers clear, readable assertions that verify collection contents.

### 14.4 Hands-On: Writing Tests for Stream-Based Code

**Exercise 1:** Write unit tests for a method that filters a list of strings, converts them to lowercase, and sorts them.
```java
public List<String> processNames(List<String> names) {
    return names.stream()
                .filter(name -> name.length() > 2)
                .map(String::toLowerCase)
                .sorted()
                .collect(Collectors.toList());
}
```
*Test Idea:*
- Provide a list with mixed case and lengths.
- Assert that the output list contains only names longer than two letters, in lowercase and sorted order.

**Exercise 2:** Debug a stream pipeline using `peek` and unit tests.  
- Given a failing stream operation, insert `peek` statements to monitor the flow.
- Write tests to isolate which step introduces unexpected behavior.
  
**Hints:**
- Start by testing individual stream steps.
- Check assumptions: input data format, expected output, and intermediate results.

---

**Tips for Module 7:**
- Use best practices for cleaner, more maintainable code.
- Regularly write and run unit tests to catch errors early.
- Embrace code reviews to share knowledge and improve code quality.
- Practice debugging streams with tools, logging, and structured tests.

By the end of Module 7, you should:
- Write lambda and stream code that is clear, maintainable, and efficient.
- Debug and refactor stream operations effectively.
- Unit test stream-based code using modern libraries and frameworks.
- Apply best practices and testing strategies in real-world applications.
