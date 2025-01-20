
# Module 1: Introduction to Functional Programming in Java

This module introduces the basics of functional programming in Java. We will learn what functional programming means, why it is useful, and how Java supports it with lambda expressions and functional interfaces. The focus is on doing hands-on coding, so students will write code and test it as they learn. Simple language is used to make concepts clear and understandable.

---

## Week 1: Functional Programming Concepts

### 1.1 What is Functional Programming?
Functional programming is a style of programming that treats computation as the evaluation of functions and avoids changing data or state. In simple terms, instead of giving the computer a list of commands to follow step by step, you tell the computer what result you want, and it figures out how to get that result.

**Key ideas:**
- **Functions as First-Class Citizens:** You can pass functions as arguments, return them from other functions, or assign them to variables.
- **Immutability:** Data is not changed once created. Instead, new data is created.
- **Pure Functions:** Functions that produce the same output given the same input and do not cause side effects (like changing a global variable or printing on the screen).

### 1.2 Benefits of Functional Programming in Java
- **Cleaner Code:** Functional code often requires fewer lines, making it easier to read and understand.
- **Easier Maintenance:** Small, single-purpose functions are easier to test and maintain.
- **Parallel Processing:** Functional style supports parallel processing, which can make programs faster on multi-core processors.

### 1.3 Differences Between Imperative and Functional Styles
- **Imperative Style:** Tells the computer how to do things step by step. It often changes states and uses loops.
- **Functional Style:** Focuses on what to do rather than how to do it. It uses expressions and avoids changing state.

**Example: Summing a list of numbers**

*Imperative style:*
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = 0;
for (int n : numbers) {
    sum += n;
}
System.out.println(sum);
```
*Explanation:* 
- We start with a sum of 0.
- For each number in the list, we add it to `sum`.
- Finally, we print the result.

*Functional style:*
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sumFunctional = numbers.stream().reduce(0, (a, b) -> a + b);
System.out.println(sumFunctional);
```
*Explanation:*
- `numbers.stream()` creates a stream from the list.
- `reduce(0, (a, b) -> a + b)` takes each number and adds it to an accumulator, starting from 0.
- This single line replaces the loop and accumulates the sum.

### 1.4 Hands-On Exercise for Week 1
**Task:** Write a Java program to calculate the squares of numbers in a list using both imperative and functional styles.

*Imperative approach:*
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> squares = new ArrayList<>();
for (int n : numbers) {
    squares.add(n * n);
}
System.out.println(squares);
```
*Explanation:*
- Create a new list `squares`.
- Loop through each number in `numbers` and add its square to `squares`.
- Print the list of squares.

*Functional approach:*
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
List<Integer> squaresFunctional = numbers.stream()
                                         .map(n -> n * n)
                                         .collect(Collectors.toList());
System.out.println(squaresFunctional);
```
*Explanation:*
- `numbers.stream()` creates a stream from the list.
- `.map(n -> n * n)` transforms each number `n` into its square.
- `.collect(Collectors.toList())` gathers the results into a new list.
- Print the list of squares.

*Note:* Try writing and running these code examples in your IDE to see how they work. Experiment with different numbers and lists.

---

## Week 2: Lambdas and Functional Interfaces Basics

### 2.1 Introduction to Lambda Expressions
A lambda expression in Java is a short way to write an implementation for a functional interface (an interface with one abstract method). Lambdas provide a clear and concise way to represent one method interface using an expression.

**Syntax of a lambda:**
```java
(parameters) -> expression
```
or, for multiple statements:
```java
(parameters) -> {
    // multiple statements
}
```

**Simple Example: Adding two numbers with a lambda**
```java
// Import functional interface from java.util.function
import java.util.function.BiFunction;

public class LambdaExample {
    public static void main(String[] args) {
        // Define a lambda that adds two integers
        BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
        int result = add.apply(3, 4);
        System.out.println("3 + 4 = " + result);
    }
}
```
*Explanation:*
- `BiFunction<Integer, Integer, Integer>` is a built-in functional interface that takes two `Integer` arguments and returns an `Integer`.
- `(a, b) -> a + b` is the lambda expression that adds the two numbers.
- `add.apply(3, 4)` calls the lambda, which returns `7`.

### 2.2 Functional Interfaces
A functional interface has exactly one abstract method. They allow lambda expressions to be used in place of an instance of the interface.

**Common built-in functional interfaces:**
- `Predicate<T>`: Tests a condition on type `T` (returns a boolean).
- `Function<T, R>`: Takes a value of type `T` and returns a value of type `R`.
- `Consumer<T>`: Performs an action on a value of type `T` but returns nothing.
- `Supplier<T>`: Provides a value of type `T` without taking any arguments.

**Example: Using Predicate and Consumer**
```java
import java.util.function.Predicate;
import java.util.function.Consumer;

public class FunctionalInterfacesExample {
    public static void main(String[] args) {
        Predicate<Integer> isEven = n -> n % 2 == 0;
        Consumer<String> printer = s -> System.out.println(s);

        int number = 5;
        if (isEven.test(number)) {
            printer.accept(number + " is even");
        } else {
            printer.accept(number + " is odd");
        }
    }
}
```
*Explanation:*
- `Predicate<Integer> isEven` creates a lambda that returns `true` if a number is even.
- `Consumer<String> printer` creates a lambda that prints a string.
- `isEven.test(number)` checks if `number` is even.
- Based on the check, `printer.accept(...)` prints the result.

### 2.3 The @FunctionalInterface Annotation
Using `@FunctionalInterface` on an interface tells the compiler that the interface is intended to be a functional interface. This annotation is optional but helps catch errors if the interface accidentally gets more than one abstract method.

**Example: Custom functional interface**
```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

public class CustomInterfaceExample {
    public static void main(String[] args) {
        // Implement the interface using a lambda
        MathOperation addition = (a, b) -> a + b;
        MathOperation multiplication = (a, b) -> a * b;
        
        System.out.println("5 + 3 = " + addition.operate(5, 3));
        System.out.println("5 * 3 = " + multiplication.operate(5, 3));
    }
}
```
*Explanation:*
- `@FunctionalInterface` ensures `MathOperation` has only one abstract method.
- We create two lambdas, one for addition and one for multiplication.
- Using `operate`, we perform operations on numbers.

### 2.4 Hands-On Exercises for Week 2
**Exercise 1:** Create and use lambdas for various functional interfaces.
- Create a `Predicate<String>` to check if a string is empty.
- Create a `Function<String, Integer>` that returns the length of a string.
- Create a `Consumer<List<String>>` that prints each string in a list.
- Create a `Supplier<Double>` that returns a random double.

**Sample Code:**
```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.function.Function;
import java.util.function.Consumer;
import java.util.function.Supplier;
import java.util.Random;

public class LambdaExercises {
    public static void main(String[] args) {
        // Predicate to check empty string
        Predicate<String> isEmpty = s -> s.isEmpty();
        // Function to get length of string
        Function<String, Integer> length = s -> s.length();
        // Consumer to print all items in a list
        Consumer<List<String>> printList = list -> {
            for (String s : list) {
                System.out.println(s);
            }
        };
        // Supplier to get a random number
        Supplier<Double> randomSupplier = () -> new Random().nextDouble();

        // Test the lambdas
        List<String> names = Arrays.asList("Alice", "", "Bob");
        for (String name : names) {
            if (isEmpty.test(name)) {
                System.out.println("Found an empty string!");
            } else {
                System.out.println("Length of '" + name + "': " + length.apply(name));
            }
        }
        System.out.println("Printing list:");
        printList.accept(names);
        System.out.println("Random number: " + randomSupplier.get());
    }
}
```
*Explanation:*
- We define lambdas for checking emptiness, getting length, printing a list, and supplying a random number.
- We apply these lambdas to a sample list of names and print the results.
- This practice demonstrates how functional interfaces and lambdas work together.

---

**Tips for Successful Learning:**
- **Write Code:** After learning a concept, write code immediately to practice.
- **Experiment:** Change parts of the code to see how it affects the output.
- **Ask Questions:** If something is unclear, write down your questions and seek answers.
- **Collaborate:** Discuss with classmates or online communities to deepen your understanding.

By the end of Module 1, students should:
- Understand the basics of functional programming.
- Write simple lambda expressions.
- Use built-in functional interfaces effectively.
- Gain confidence through hands-on coding and experimentation.
