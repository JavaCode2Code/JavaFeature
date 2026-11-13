Java Features

reduce() in Java 8 Streams

reduce() is a terminal operation on streams that combines all elements into a single result by repeatedly applying an operation (sum, max, concatenation, etc.).

Three forms

1. With identity and accumulator (returns the value directly)

java
T reduce(T identity, BinaryOperator<T> accumulator)
identity: the starting value (and the result if the stream is empty), e.g. 0 for sum, 1 for product.
accumulator: combines the running result with the next element.
java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);

int sum = nums.stream().reduce(0, (a, b) -> a + b);   // 15
int sum2 = nums.stream().reduce(0, Integer::sum);     // 15 (method reference)

2. Without identity (returns an Optional, because the stream might be empty)

java
Optional<T> reduce(BinaryOperator<T> accumulator)
java
Optional<Integer> max = nums.stream().reduce(Integer::max);
System.out.println(max.get());   // 5

3. With identity, accumulator and combiner (used when the result type differs from the element type, and for parallel streams)

java
<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner)
java
List<String> words = Arrays.asList("java", "is", "fun");

int totalLength = words.stream()
    .reduce(0, (len, word) -> len + word.length(), Integer::sum);   // 10

The combiner merges partial results from different threads in a parallel stream.

How it works (step by step)

For reduce(0, (a, b) -> a + b) on [1, 2, 3]:

0 + 1 = 1
1 + 2 = 3
3 + 3 = 6   → result
More examples
java
// Concatenate strings
String joined = Stream.of("a", "b", "c").reduce("", (a, b) -> a + b);   // "abc"

// Product
int product = nums.stream().reduce(1, (a, b) -> a * b);                 // 120

// Longest string
Optional<String> longest = words.stream()
    .reduce((a, b) -> a.length() >= b.length() ? a : b);
Key points
It is a terminal operation: it consumes the stream and produces a result.
The accumulator should be associative ((a op b) op c == a op (b op c)) and stateless, so it is safe in parallel streams.
The identity must be a true identity for the operation: 0 for addition, 1 for multiplication, "" for string concatenation.
For common cases, built-ins like sum(), max(), count() and Collectors.joining() are clearer; use reduce() for custom combining logic.
Short definition (for notes or exams)

reduce() is a terminal stream operation in Java 8 that repeatedly applies a binary operator to the elements of a stream to combine them into a single result.
