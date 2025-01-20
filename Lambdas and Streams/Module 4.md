# Module 4: Advanced Stream Operations

In this module, we will explore advanced stream operations. This includes terminal operations, reductions, collectors, and working with different data structures like Lists, Sets, and Maps. The focus remains on hands-on exercises to help you understand and apply these concepts in practice.

---

## Week 7: Terminal Operations and Reduction

### 7.1 Collectors: groupingBy, partitioningBy, toList, toSet, toMap
Collectors are used with the `collect()` terminal operation to gather stream elements into collections or other results.

**Examples:**

- **toList and toSet:**
  ```java
  List<String> fruits = Arrays.asList("apple", "banana", "apple", "cherry");
  List<String> uniqueFruitsList = fruits.stream()
                                        .distinct()
                                        .collect(Collectors.toList());
  Set<String> uniqueFruitsSet = fruits.stream()
                                      .collect(Collectors.toSet());
  System.out.println(uniqueFruitsList); // [apple, banana, cherry]
  System.out.println(uniqueFruitsSet);  // [banana, cherry, apple]
  ```
  *Explanation:* 
  - `toList()` collects elements into a list.
  - `toSet()` collects elements into a set, removing duplicates.

- **groupingBy:**
  ```java
  List<String> names = Arrays.asList("Alice", "Bob", "Anna", "Mike", "Brian");
  Map<Character, List<String>> groupedByFirstLetter = names.stream()
      .collect(Collectors.groupingBy(name -> name.charAt(0)));
  System.out.println(groupedByFirstLetter);
  ```
  *Explanation:* 
  - `groupingBy(name -> name.charAt(0))` groups names by their first letter.

- **partitioningBy:**
  ```java
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
  Map<Boolean, List<Integer>> partitioned = numbers.stream()
      .collect(Collectors.partitioningBy(n -> n % 2 == 0));
  System.out.println(partitioned);
  ```
  *Explanation:* 
  - `partitioningBy(n -> n % 2 == 0)` splits numbers into even (true) and odd (false).

- **toMap:**
  ```java
  List<String> words = Arrays.asList("apple", "banana", "cherry");
  Map<String, Integer> wordLengthMap = words.stream()
      .collect(Collectors.toMap(word -> word, word -> word.length()));
  System.out.println(wordLengthMap);
  ```
  *Explanation:* 
  - `toMap(word -> word, word -> word.length())` creates a map with words as keys and their lengths as values.

### 7.2 Reduction Operations: reduce, collect, summarizing
Reduction operations combine stream elements into a single result.

- **reduce:**
  ```java
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
  int sum = numbers.stream()
                   .reduce(0, (a, b) -> a + b);
  System.out.println("Sum: " + sum);
  ```
  *Explanation:* 
  - `reduce(0, (a, b) -> a + b)` sums all numbers, starting from 0.

- **collect summarizing:**
  ```java
  List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
  IntSummaryStatistics stats = numbers.stream()
                                      .collect(Collectors.summarizingInt(Integer::intValue));
  System.out.println("Sum: " + stats.getSum());
  System.out.println("Max: " + stats.getMax());
  System.out.println("Min: " + stats.getMin());
  ```
  *Explanation:* 
  - `summarizingInt` computes summary statistics like sum, max, and min.

### 7.3 Custom Collectors: Creating and Using Them
You can create a custom collector to define your own collection logic.

**Example: Custom collector to join strings with a delimiter**
```java
import java.util.stream.Collector;
import java.util.stream.Collectors;

public class CustomCollectorExample {
    public static void main(String[] args) {
        List<String> items = Arrays.asList("apple", "banana", "cherry");

        Collector<String, ?, String> joiningWithComma = 
            Collectors.joining(", ");

        String result = items.stream().collect(joiningWithComma);
        System.out.println(result);  // Output: apple, banana, cherry
    }
}
```
*Explanation:* 
- `Collectors.joining(", ")` is a built-in collector that joins strings with a comma.
- You can build more complex custom collectors if needed by implementing the `Collector` interface.

### 7.4 Hands-On Exercises with Reduction and Collectors

**Exercise 1:** Group a list of names by their length.
```java
List<String> names = Arrays.asList("John", "Amanda", "Steve", "Amanda", "Kate");
Map<Integer, List<String>> namesByLength = names.stream()
    .collect(Collectors.groupingBy(String::length));
System.out.println(namesByLength);
```
*Explanation:* 
- Groups names with the same length into lists.

**Exercise 2:** Calculate the average age from a list of people using summarizing.
```java
class Person {
    String name;
    int age;
    Person(String name, int age) { this.name = name; this.age = age; }
    public int getAge() { return age; }
}

// ...

List<Person> people = Arrays.asList(
    new Person("Alice", 30),
    new Person("Bob", 25),
    new Person("Charlie", 35)
);

Double averageAge = people.stream()
    .collect(Collectors.averagingInt(Person::getAge));
System.out.println("Average age: " + averageAge);
```
*Explanation:* 
- Uses `averagingInt` to compute the average age of people.

---

## Week 8: Working with Different Data Structures

### 8.1 Streams on Lists, Sets, and Maps
Streams can work with different data structures.

- **Lists and Sets:**
  ```java
  List<String> list = Arrays.asList("a", "b", "c");
  Set<String> set = new HashSet<>(list);
  list.stream().forEach(System.out::println);
  set.stream().forEach(System.out::println);
  ```
  *Explanation:* 
  - Both lists and sets can be turned into streams and processed similarly.

- **Maps:**
  You can stream over maps by their entry set, key set, or values.
  ```java
  Map<Integer, String> map = new HashMap<>();
  map.put(1, "one");
  map.put(2, "two");

  // Stream of entries
  map.entrySet().stream().forEach(entry -> 
      System.out.println(entry.getKey() + " = " + entry.getValue())
  );

  // Stream of keys
  map.keySet().stream().forEach(System.out::println);

  // Stream of values
  map.values().stream().forEach(System.out::println);
  ```
  *Explanation:* 
  - `map.entrySet().stream()` creates a stream of map entries.
  - You can process keys and values separately.

### 8.2 Converting Streams Back to Collections
After processing data with streams, you often collect results back into collections.

**Example: Collecting to a List or Set**
```java
List<String> fruits = Arrays.asList("apple", "banana", "apple");
List<String> fruitList = fruits.stream()
                               .distinct()
                               .collect(Collectors.toList());

Set<String> fruitSet = fruits.stream()
                             .collect(Collectors.toSet());
System.out.println(fruitList);
System.out.println(fruitSet);
```
*Explanation:* 
- `collect(Collectors.toList())` and `collect(Collectors.toSet())` convert streams back to list and set respectively.

### 8.3 Stream Operations on Maps: entries, keys, values
We've covered how to create streams from map entries, keys, and values. With these streams, you can perform any stream operations such as filtering, mapping, or reducing.

**Example: Filtering map entries**
```java
Map<Integer, String> map = new HashMap<>();
map.put(1, "one");
map.put(2, "two");
map.put(3, "three");

Map<Integer, String> filteredMap = map.entrySet().stream()
    .filter(entry -> entry.getKey() % 2 == 1)
    .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));

System.out.println(filteredMap);
```
*Explanation:* 
- Filters map entries to keep only those with odd keys.
- Collects the result back into a map.

### 8.4 Practical Exercises: Manipulating Various Collection Types

**Exercise 1:** Given a map of student names to their scores, create a new map with only the students who scored above 80.
```java
Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 90);
scores.put("Bob", 75);
scores.put("Charlie", 85);

Map<String, Integer> highScores = scores.entrySet().stream()
    .filter(entry -> entry.getValue() > 80)
    .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));

System.out.println(highScores);
```
*Explanation:* 
- Filters entries where the score is greater than 80.
- Collects these entries into a new map.

**Exercise 2:** From a list of words, create a set of their lengths.
```java
List<String> words = Arrays.asList("apple", "banana", "cherry");
Set<Integer> lengths = words.stream()
                            .map(String::length)
                            .collect(Collectors.toSet());
System.out.println(lengths);
```
*Explanation:* 
- `map(String::length)` converts each word to its length.
- Collects the lengths into a set, removing duplicates.

---

**Tips for Module 4:**
- Practice each collector and reduction operation with small examples.
- Experiment with different data structures to see how streams work with them.
- Break down complex operations into small steps to understand their behavior.

By the end of Module 4, you should:
- Understand and use advanced collectors for grouping, partitioning, and reducing data.
- Create custom collectors for specialized tasks.
- Work with streams on Lists, Sets, and Maps.
- Convert streams back to collections after processing.
- Feel confident manipulating various collection types using streams.
