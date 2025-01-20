# Module 2: Deep Dive into Lambda Expressions

This module takes a closer look at lambda expressions and functional interfaces in Java. We will explore advanced uses of lambdas, variable scopes, method references, and how to effectively capture variables. The second week of this module dives deeper into creating custom functional interfaces and composing them. Examples and hands-on exercises will help solidify these concepts through practice.

---

## Week 3: Advanced Lambda Usage

### 3.1 Variable Scope and Closures in Lambdas
When using lambdas, it's important to understand how variables from outside the lambda are used. A lambda can "capture" variables from its surrounding scope. These captured variables must be _effectively final_, meaning they are not changed after being defined.

**Example: Capturing a variable**
```java
public class LambdaScopeExample {
    public static void main(String[] args) {
        int baseValue = 10; // Effectively final variable
        // Lambda expression capturing baseValue
        Function<Integer, Integer> addBase = x -> x + baseValue;
        System.out.println("5 + baseValue = " + addBase.apply(5));
    }
}
```
*Explanation:*
- `baseValue` is defined outside the lambda.
- The lambda `x -> x + baseValue` uses `baseValue`.
- Since `baseValue` is not changed after being assigned, it is effectively final and can be captured safely.

**Key Point:** You cannot change the value of a variable inside a lambda that was defined outside of it.

### 3.2 Method References: Static, Instance, and Constructor References
Method references are shortcuts for writing lambda expressions. They refer directly to methods or constructors by name.

**Static Method Reference:**
```java
public class StaticMethodRef {
    public static void print(String message) {
        System.out.println(message);
    }

    public static void main(String[] args) {
        Consumer<String> printer = StaticMethodRef::print;
        printer.accept("Hello, world!");
    }
}
```
*Explanation:* 
- `StaticMethodRef::print` refers to the static method `print`. 
- It is equivalent to `s -> StaticMethodRef.print(s)`.

**Instance Method Reference:**
```java
public class InstanceMethodRef {
    public void sayHello(String name) {
        System.out.println("Hello, " + name);
    }

    public static void main(String[] args) {
        InstanceMethodRef instance = new InstanceMethodRef();
        Consumer<String> greeter = instance::sayHello;
        greeter.accept("Alice");
    }
}
```
*Explanation:* 
- `instance::sayHello` refers to the instance method `sayHello` on `instance`.
- It is the same as writing `s -> instance.sayHello(s)`.

**Constructor Reference:**
```java
import java.util.function.Supplier;

public class ConstructorRef {
    static class Person {
        String name;
        Person() { this.name = "Unknown"; }
        Person(String name) { this.name = name; }
        @Override
        public String toString() { return "Person: " + name; }
    }

    public static void main(String[] args) {
        // Constructor reference for no-arg constructor
        Supplier<Person> personSupplier = Person::new;
        Person person = personSupplier.get();
        System.out.println(person);
    }
}
```
*Explanation:* 
- `Person::new` refers to the constructor of `Person`.
- `Supplier<Person> personSupplier` uses this reference to create new `Person` objects when needed.

### 3.3 Default and Static Methods in Interfaces
Interfaces can have default and static methods. This helps in providing implementations without breaking existing code.

**Example: Default Method in Interface**
```java
interface Greeting {
    void sayHello(String name);

    default void sayGoodbye(String name) {
        System.out.println("Goodbye, " + name);
    }
}

public class GreetingExample implements Greeting {
    @Override
    public void sayHello(String name) {
        System.out.println("Hello, " + name);
    }

    public static void main(String[] args) {
        GreetingExample greet = new GreetingExample();
        greet.sayHello("Alice");
        greet.sayGoodbye("Alice");
    }
}
```
*Explanation:* 
- `Greeting` interface has a default method `sayGoodbye`.
- `GreetingExample` class implements `sayHello` but inherits `sayGoodbye`.
- Default methods help avoid having to implement every method when not needed.

**Static Methods in Interface:**
```java
interface MathUtils {
    static int square(int x) {
        return x * x;
    }
}

public class StaticMethodInterface {
    public static void main(String[] args) {
        int result = MathUtils.square(5);
        System.out.println("Square of 5 is " + result);
    }
}
```
*Explanation:*
- `MathUtils` interface declares a static method `square`.
- We call `MathUtils.square(5)` directly without an instance.

### 3.4 Capturing Variables Effectively
When capturing variables inside lambdas:
- Only use variables that do not change after assignment.
- Prefer using local copies inside the lambda if necessary.
- Understand that capturing large objects can affect performance, so capture only what is needed.

**Example: Local copying inside lambda**
```java
public class CaptureExample {
    public static void main(String[] args) {
        String greeting = "Hello";
        Runnable greet = () -> {
            // Create a local copy for use inside lambda
            String localGreeting = greeting;
            System.out.println(localGreeting + ", world!");
        };
        greet.run();
    }
}
```
*Explanation:*
- `greeting` is captured by the lambda.
- We make a local copy `localGreeting` inside the lambda to demonstrate capturing.

---

## Week 4: Functional Interfaces in Depth

### 4.1 Custom Functional Interfaces
You can create your own functional interfaces for custom behaviors.

**Example: Creating and using a custom functional interface**
```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

public class CustomFunctionalInterfaceExample {
    public static void main(String[] args) {
        // Using the custom interface with lambda expressions
        MathOperation addition = (a, b) -> a + b;
        MathOperation subtraction = (a, b) -> a - b;

        System.out.println("5 + 3 = " + addition.operate(5, 3));
        System.out.println("5 - 3 = " + subtraction.operate(5, 3));
    }
}
```
*Explanation:*
- We define a custom functional interface `MathOperation` with one abstract method.
- Lambdas implement the `operate` method for addition and subtraction.

### 4.2 Composing Functions using `andThen`, `compose`
You can chain functions together using default methods provided by `Function` interface.

**Example: Using `andThen` and `compose`**
```java
import java.util.function.Function;

public class FunctionCompositionExample {
    public static void main(String[] args) {
        Function<Integer, Integer> multiplyByTwo = x -> x * 2;
        Function<Integer, Integer> addFive = x -> x + 5;

        // First multiply, then add
        Function<Integer, Integer> combinedAndThen = multiplyByTwo.andThen(addFive);
        System.out.println("Result of andThen ( (x*2) + 5 ) for 3: " + combinedAndThen.apply(3));

        // First add, then multiply
        Function<Integer, Integer> combinedCompose = multiplyByTwo.compose(addFive);
        System.out.println("Result of compose ( (x+5) * 2 ) for 3: " + combinedCompose.apply(3));
    }
}
```
*Explanation:*
- `multiplyByTwo.andThen(addFive)` means do multiplication then addition.
- `multiplyByTwo.compose(addFive)` means do addition first then multiplication.
- These methods allow chaining operations in desired order.

### 4.3 Combining Predicates, Consumers, Suppliers, and Functions
You can combine functional interfaces to create more complex behavior.

**Example: Combining Predicates**
```java
import java.util.function.Predicate;

public class PredicateCombinationExample {
    public static void main(String[] args) {
        Predicate<Integer> isPositive = x -> x > 0;
        Predicate<Integer> isEven = x -> x % 2 == 0;

        // Check if a number is positive and even
        Predicate<Integer> isPositiveAndEven = isPositive.and(isEven);
        System.out.println("Is 4 positive and even? " + isPositiveAndEven.test(4));
    }
}
```
*Explanation:*
- `isPositive.and(isEven)` creates a new predicate that checks if a number is both positive and even.

**Example: Combining Consumers**
```java
import java.util.function.Consumer;

public class ConsumerCombinationExample {
    public static void main(String[] args) {
        Consumer<String> printUpperCase = s -> System.out.println(s.toUpperCase());
        Consumer<String> printLowerCase = s -> System.out.println(s.toLowerCase());

        // Combine consumers: print the string in upper and lower case
        Consumer<String> printBoth = printUpperCase.andThen(printLowerCase);
        printBoth.accept("Hello World");
    }
}
```
*Explanation:*
- `printUpperCase.andThen(printLowerCase)` runs one consumer after another on the same input.

### 4.4 Exercises on Chaining and Composing Functional Interfaces

**Exercise 1: Create a chain of functions**
- Create a `Function<Integer, Integer>` that multiplies a number by 3.
- Create another that subtracts 2 from a number.
- Compose them to first multiply by 3, then subtract 2, and apply the result to an input.

*Sample code:*
```java
import java.util.function.Function;

public class FunctionChainingExercise {
    public static void main(String[] args) {
        Function<Integer, Integer> multiplyByThree = x -> x * 3;
        Function<Integer, Integer> subtractTwo = x -> x - 2;
        
        Function<Integer, Integer> combined = multiplyByThree.andThen(subtractTwo);
        
        int input = 4;
        int result = combined.apply(input);
        System.out.println("Result of (4 * 3) - 2 = " + result); // Should print 10
    }
}
```

**Exercise 2: Combine multiple predicates**
- Create two `Predicate<String>`: one to check if a string starts with "A" and another to check if its length is greater than 3.
- Combine them using `.and()` to filter a list of strings and print only those that satisfy both conditions.

*Sample code:*
```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;

public class PredicateExercise {
    public static void main(String[] args) {
        Predicate<String> startsWithA = s -> s.startsWith("A");
        Predicate<String> lengthGreaterThan3 = s -> s.length() > 3;

        Predicate<String> combinedPredicate = startsWithA.and(lengthGreaterThan3);

        List<String> words = Arrays.asList("Apple", "Air", "Apex", "Bat", "All");
        words.stream()
             .filter(combinedPredicate)
             .forEach(System.out::println); // Should print "Apple" and "Apex"
    }
}
```

**Exercise 3: Compose custom functional interfaces**
- Create a custom functional interface to transform a string (e.g., to uppercase).
- Compose multiple transformations using default methods if possible.
- Apply the composed function to a list of strings.

*Hints:*
- You might define a custom interface with a default method `andThen` similar to `Function`.
- Alternatively, reuse the `Function<String, String>` interface to compose transformations.

---

**Tips:**
- Write and run each code snippet to see how it behaves.
- Try modifying examples to explore different outcomes.
- Use the Java documentation as a reference for method behaviors.

By the end of Module 2, you should:
- Understand how lambdas interact with variable scope.
- Use method references for cleaner code.
- Create and use default and static interface methods.
- Build complex behaviors by composing and chaining functions, predicates, consumers, suppliers, and custom interfaces.
- Feel comfortable applying these advanced topics in your own Java programs.
