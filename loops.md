# Loops

This chapter covers 20 comprehensive PHP loop examples designed to help you  
master various looping patterns and techniques. These examples progress from  
basic loop constructs to advanced patterns including nested loops, iterators,  
and generator functions. Each example demonstrates practical use cases with  
clear explanations to help you understand when and how to apply different  
looping strategies in your PHP applications.  

## Basic for loop

Counting with a traditional C-style for loop.  

```php
<?php

for ($i = 1; $i <= 5; $i++) {
    echo "Iteration: $i\n";
}
```

The basic for loop uses three expressions: initialization (`$i = 1`),  
condition (`$i <= 5`), and increment (`$i++`). This pattern is ideal  
for counting operations or when you need precise control over the loop  
variable.  

## For loop with custom increment

Using different increment patterns for specialized counting.  

```php
<?php

// Count by twos
echo "Even numbers:\n";
for ($i = 2; $i <= 10; $i += 2) {
    echo "$i ";
}
echo "\n\n";

// Countdown
echo "Countdown:\n";
for ($i = 5; $i > 0; $i--) {
    echo "$i ";
}
echo "Launch!\n";
```

For loops support any increment pattern using operators like `+=`, `-=`,  
`*=`, or `/=`. This flexibility allows counting by any value, backwards  
counting, or even exponential growth patterns.  

## Foreach with indexed arrays

Iterating over array elements without managing indices manually.  

```php
<?php

$fruits = ["apple", "banana", "cherry", "date"];

foreach ($fruits as $fruit) {
    echo "Fruit: $fruit\n";
}

echo "\nWith indices:\n";
foreach ($fruits as $index => $fruit) {
    echo "$index: $fruit\n";
}
```

Foreach loops provide the most convenient way to iterate over arrays.  
The first form gives you just the values, while the second form  
provides both the key and value for each element.  

## Foreach with associative arrays

Working with key-value pairs in associative arrays.  

```php
<?php

$person = [
    "name" => "Alice",
    "age" => 30,
    "city" => "New York",
    "occupation" => "Engineer"
];

foreach ($person as $key => $value) {
    echo ucfirst($key) . ": $value\n";
}

// Process only specific keys
$display_keys = ["name", "city"];
foreach ($display_keys as $key) {
    if (isset($person[$key])) {
        echo "$key: " . $person[$key] . "\n";
    }
}
```

Associative arrays use string keys instead of numeric indices.  
Foreach loops naturally handle both the key and value, making  
them perfect for processing configuration data, user profiles,  
or any structured information.  

## While loop

Repeating code while a condition remains true.  

```php
<?php

$count = 1;
$total = 0;

while ($count <= 5) {
    $total += $count;
    echo "Adding $count, total is now: $total\n";
    $count++;
}

echo "Final total: $total\n";
```

While loops continue executing as long as the condition is true.  
They're ideal when you don't know exactly how many iterations  
you'll need, or when the loop should depend on changing conditions  
rather than a simple counter.  

## Do-while loop

Ensuring at least one execution of the loop body.  

```php
<?php

$guess = 0;
$target = 7;

do {
    $guess = rand(1, 10);
    echo "Guessed: $guess\n";
} while ($guess !== $target);

echo "Found the target number: $target\n";
```

Do-while loops execute the body at least once before checking  
the condition. This is useful for input validation, game loops,  
or any scenario where you need to perform an action before  
determining whether to continue.  

## Nested loops

Combining loops for multi-dimensional operations.  

```php
<?php

echo "Multiplication Table:\n";
for ($i = 1; $i <= 5; $i++) {
    for ($j = 1; $j <= 5; $j++) {
        printf("%3d", $i * $j);
    }
    echo "\n";
}

echo "\nPattern with stars:\n";
for ($row = 1; $row <= 4; $row++) {
    for ($col = 1; $col <= $row; $col++) {
        echo "* ";
    }
    echo "\n";
}
```

Nested loops allow you to work with multi-dimensional data or  
create patterns. The inner loop completes all its iterations  
for each iteration of the outer loop, creating a grid-like  
processing pattern.  

## Loop with break statement

Exiting loops early based on conditions.  

```php
<?php

$numbers = [1, 3, 7, 2, 8, 4, 9];
$target = 8;

foreach ($numbers as $index => $number) {
    echo "Checking position $index: $number\n";
    
    if ($number === $target) {
        echo "Found $target at position $index!\n";
        break;  // Exit the loop immediately
    }
}

// Break with nested loops
echo "\nSearching 2D array:\n";
$matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
$found = false;

for ($i = 0; $i < count($matrix) && !$found; $i++) {
    for ($j = 0; $j < count($matrix[$i]); $j++) {
        if ($matrix[$i][$j] === 5) {
            echo "Found 5 at position [$i][$j]\n";
            $found = true;
            break;
        }
    }
}
```

The break statement immediately exits the current loop. In nested  
loops, break only exits the innermost loop unless you use labeled  
breaks or additional flags. This is essential for search operations  
where you want to stop once you find what you're looking for.  

## Loop with continue statement

Skipping iterations based on conditions.  

```php
<?php

echo "Odd numbers only:\n";
for ($i = 1; $i <= 10; $i++) {
    if ($i % 2 === 0) {
        continue;  // Skip even numbers
    }
    echo "$i ";
}
echo "\n\n";

echo "Processing valid emails:\n";
$emails = ["user@example.com", "invalid-email", "admin@site.org", ""];

foreach ($emails as $email) {
    if (empty($email) || !filter_var($email, FILTER_VALIDATE_EMAIL)) {
        echo "Skipping invalid: '$email'\n";
        continue;
    }
    
    echo "Processing valid email: $email\n";
}
```

The continue statement skips the rest of the current iteration  
and moves to the next one. This is perfect for filtering data  
or skipping invalid entries while processing collections.  

## Loop with array functions

Combining loops with PHP's built-in array functions.  

```php
<?php

$scores = [85, 92, 78, 96, 88, 71];

// Find scores above average
$average = array_sum($scores) / count($scores);
echo "Average score: " . round($average, 1) . "\n";

echo "Scores above average:\n";
foreach ($scores as $index => $score) {
    if ($score > $average) {
        echo "Student " . ($index + 1) . ": $score\n";
    }
}

// Group scores by grade
$grades = [];
foreach ($scores as $score) {
    if ($score >= 90) {
        $grades['A'][] = $score;
    } elseif ($score >= 80) {
        $grades['B'][] = $score;
    } else {
        $grades['C'][] = $score;
    }
}

foreach ($grades as $grade => $score_list) {
    echo "Grade $grade: " . implode(", ", $score_list) . "\n";
}
```

Loops work excellently with array functions like `array_sum()`,  
`count()`, and `implode()`. This combination allows for powerful  
data processing and analysis operations on collections.  

## Infinite loop with controlled exit

Creating loops that run until specific conditions are met.  

```php
<?php

$attempts = 0;
$max_attempts = 5;

while (true) {
    $attempts++;
    $number = rand(1, 100);
    
    echo "Attempt $attempts: Generated $number\n";
    
    if ($number > 90) {
        echo "Success! Found a number greater than 90.\n";
        break;
    }
    
    if ($attempts >= $max_attempts) {
        echo "Max attempts reached. Giving up.\n";
        break;
    }
}

echo "Loop completed after $attempts attempts.\n";
```

Infinite loops using `while (true)` are useful for game loops,  
server processes, or retry mechanisms. Always include exit  
conditions with break statements to prevent actual infinite  
loops that could hang your program.  

## Loop with multidimensional arrays

Processing complex nested data structures.  

```php
<?php

$students = [
    "class_a" => [
        ["name" => "Alice", "grade" => 92],
        ["name" => "Bob", "grade" => 87],
        ["name" => "Charlie", "grade" => 95]
    ],
    "class_b" => [
        ["name" => "Diana", "grade" => 88],
        ["name" => "Eve", "grade" => 91],
        ["name" => "Frank", "grade" => 83]
    ]
];

foreach ($students as $class_name => $class_students) {
    echo "=== " . strtoupper($class_name) . " ===\n";
    
    $class_total = 0;
    foreach ($class_students as $student) {
        echo $student["name"] . ": " . $student["grade"] . "\n";
        $class_total += $student["grade"];
    }
    
    $class_average = $class_total / count($class_students);
    echo "Class average: " . round($class_average, 1) . "\n\n";
}
```

Nested foreach loops handle multidimensional arrays naturally.  
This pattern is common when processing grouped data like  
database results, CSV imports, or JSON structures with  
hierarchical organization.  

## Loop with object iteration

Iterating over object properties and methods.  

```php
<?php

class Product {
    public $name;
    public $price;
    public $category;
    private $id;
    
    public function __construct($name, $price, $category) {
        $this->name = $name;
        $this->price = $price;
        $this->category = $category;
        $this->id = uniqid();
    }
    
    public function getInfo() {
        return [
            'name' => $this->name,
            'price' => $this->price,
            'category' => $this->category
        ];
    }
}

$products = [
    new Product("Laptop", 999.99, "Electronics"),
    new Product("Coffee Mug", 12.50, "Kitchen"),
    new Product("Novel", 15.99, "Books")
];

foreach ($products as $index => $product) {
    echo "Product " . ($index + 1) . ":\n";
    
    // Iterate over public properties
    foreach ($product as $property => $value) {
        echo "  $property: $value\n";
    }
    echo "\n";
}
```

Objects can be iterated with foreach to access their public  
properties. This is useful for debugging, serialization, or  
displaying object data. Private and protected properties are  
not accessible through standard iteration.  

## Loop with generators

Creating memory-efficient iterations using yield.  

```php
<?php

function numberRange($start, $end, $step = 1) {
    for ($i = $start; $i <= $end; $i += $step) {
        yield $i;
    }
}

function fibonacci($max) {
    $a = 0;
    $b = 1;
    
    while ($a <= $max) {
        yield $a;
        [$a, $b] = [$b, $a + $b];
    }
}

echo "Numbers 1 to 10 by 2s:\n";
foreach (numberRange(1, 10, 2) as $number) {
    echo "$number ";
}
echo "\n\n";

echo "Fibonacci sequence up to 50:\n";
foreach (fibonacci(50) as $fib) {
    echo "$fib ";
}
echo "\n";
```

Generators use `yield` to produce values on-demand, making them  
memory-efficient for large datasets. They work seamlessly with  
foreach loops and are perfect for mathematical sequences,  
file processing, or any situation where generating all values  
at once would be impractical.  

## Range-based loops

Using PHP's range function for numeric sequences.  

```php
<?php

// Simple range
echo "Numbers 1 to 5:\n";
foreach (range(1, 5) as $number) {
    echo "$number ";
}
echo "\n\n";

// Range with step
echo "Even numbers 2 to 20:\n";
foreach (range(2, 20, 2) as $even) {
    echo "$even ";
}
echo "\n\n";

// Character range
echo "Letters A to G:\n";
foreach (range('A', 'G') as $letter) {
    echo "$letter ";
}
echo "\n\n";

// Reverse range
echo "Countdown from 10:\n";
foreach (range(10, 1) as $countdown) {
    echo "$countdown ";
}
echo "\n";
```

The `range()` function creates arrays of sequential values,  
perfect for generating number sequences, alphabets, or  
countdown timers. It supports custom steps and works with  
both numbers and characters.  

## Loop performance optimization

Techniques for writing efficient loops.  

```php
<?php

$large_array = range(1, 100000);

// Bad: Calculating count in every iteration
$start_time = microtime(true);
for ($i = 0; $i < count($large_array); $i++) {
    // Process element (simulated)
    $dummy = $large_array[$i] * 2;
}
$slow_time = microtime(true) - $start_time;

// Good: Calculate count once
$start_time = microtime(true);
$count = count($large_array);
for ($i = 0; $i < $count; $i++) {
    // Process element (simulated)
    $dummy = $large_array[$i] * 2;
}
$fast_time = microtime(true) - $start_time;

// Best: Use foreach for arrays
$start_time = microtime(true);
foreach ($large_array as $value) {
    // Process element (simulated)
    $dummy = $value * 2;
}
$fastest_time = microtime(true) - $start_time;

printf("Count in condition: %.4f seconds\n", $slow_time);
printf("Count cached: %.4f seconds\n", $fast_time);
printf("Foreach loop: %.4f seconds\n", $fastest_time);
```

Loop performance matters for large datasets. Cache function calls  
outside the loop condition, use foreach for arrays when possible,  
and avoid unnecessary operations inside loops. These optimizations  
can significantly improve execution time.  

## Loop with error handling

Managing exceptions and errors within loop iterations.  

```php
<?php

function processNumber($number) {
    if ($number === 0) {
        throw new DivisionByZeroError("Cannot divide by zero");
    }
    return 100 / $number;
}

$numbers = [5, 2, 0, 4, -1];
$results = [];
$errors = [];

foreach ($numbers as $index => $number) {
    try {
        $result = processNumber($number);
        $results[] = "Position $index: $number -> $result";
    } catch (DivisionByZeroError $e) {
        $errors[] = "Position $index: Error - " . $e->getMessage();
        continue; // Skip to next iteration
    } catch (Exception $e) {
        $errors[] = "Position $index: Unexpected error - " . $e->getMessage();
    }
}

echo "Results:\n";
foreach ($results as $result) {
    echo "$result\n";
}

echo "\nErrors:\n";
foreach ($errors as $error) {
    echo "$error\n";
}
```

Error handling in loops prevents one bad iteration from stopping  
the entire process. Use try-catch blocks inside loops to handle  
exceptions gracefully and continue processing valid data.  

## Loop with file processing

Reading and processing files line by line.  

```php
<?php

// Create a sample file for demonstration
$sample_data = "Alice,25,Engineer\nBob,30,Designer\nCharlie,28,Developer\n";
file_put_contents('/tmp/users.csv', $sample_data);

echo "Processing CSV file:\n";

$file_handle = fopen('/tmp/users.csv', 'r');
$line_number = 0;

while (($line = fgets($file_handle)) !== false) {
    $line_number++;
    $line = trim($line);
    
    if (empty($line)) {
        continue; // Skip empty lines
    }
    
    $fields = explode(',', $line);
    if (count($fields) >= 3) {
        [$name, $age, $job] = $fields;
        echo "Line $line_number: $name is $age years old and works as a $job\n";
    } else {
        echo "Line $line_number: Invalid format\n";
    }
}

fclose($file_handle);

// Alternative with foreach and file()
echo "\nUsing file() function:\n";
$lines = file('/tmp/users.csv', FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES);

foreach ($lines as $line_number => $line) {
    $fields = explode(',', $line);
    if (count($fields) >= 3) {
        [$name, $age, $job] = $fields;
        echo "Record " . ($line_number + 1) . ": $name ($age) - $job\n";
    }
}

// Clean up
unlink('/tmp/users.csv');
```

File processing with loops is common for log analysis, data  
import, and batch processing. Use `fgets()` with while loops  
for memory-efficient processing of large files, or `file()`  
with foreach for smaller files that fit in memory.  

## Advanced loop patterns

Complex looping strategies for specialized use cases.  

```php
<?php

// Sliding window pattern
function findMaxSum($array, $window_size) {
    $max_sum = 0;
    $current_sum = 0;
    
    // Calculate sum of first window
    for ($i = 0; $i < $window_size; $i++) {
        $current_sum += $array[$i];
    }
    $max_sum = $current_sum;
    
    // Slide the window
    for ($i = $window_size; $i < count($array); $i++) {
        $current_sum = $current_sum - $array[$i - $window_size] + $array[$i];
        $max_sum = max($max_sum, $current_sum);
    }
    
    return $max_sum;
}

// Two-pointer pattern
function findPairSum($array, $target) {
    sort($array);
    $left = 0;
    $right = count($array) - 1;
    
    while ($left < $right) {
        $sum = $array[$left] + $array[$right];
        
        if ($sum === $target) {
            return [$array[$left], $array[$right]];
        } elseif ($sum < $target) {
            $left++;
        } else {
            $right--;
        }
    }
    
    return null;
}

// Demonstrate patterns
$numbers = [2, 1, 5, 1, 3, 2];
echo "Array: " . implode(", ", $numbers) . "\n";
echo "Maximum sum of 3 consecutive elements: " . findMaxSum($numbers, 3) . "\n";

$sorted_numbers = [1, 2, 3, 4, 5, 6];
$target = 7;
$pair = findPairSum($sorted_numbers, $target);
if ($pair) {
    echo "Pair that sums to $target: " . implode(" + ", $pair) . "\n";
} else {
    echo "No pair found that sums to $target\n";
}
```

Advanced loop patterns like sliding window and two-pointer  
techniques are powerful tools for algorithm implementation.  
These patterns can dramatically improve performance for  
problems involving arrays, strings, and searching operations.  

## Functional loop patterns

Combining traditional loops with functional programming concepts.  

```php
<?php

// Custom implementation of map, filter, reduce using loops
function customMap(array $array, callable $callback): array {
    $result = [];
    foreach ($array as $key => $value) {
        $result[$key] = $callback($value, $key);
    }
    return $result;
}

function customFilter(array $array, callable $callback): array {
    $result = [];
    foreach ($array as $key => $value) {
        if ($callback($value, $key)) {
            $result[$key] = $value;
        }
    }
    return $result;
}

function customReduce(array $array, callable $callback, $initial = null) {
    $accumulator = $initial;
    foreach ($array as $key => $value) {
        $accumulator = $callback($accumulator, $value, $key);
    }
    return $accumulator;
}

// Pipeline processing with chained operations
function pipeline(array $data, ...$operations) {
    $result = $data;
    foreach ($operations as $operation) {
        $result = $operation($result);
    }
    return $result;
}

// Demonstrate functional patterns
$numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Traditional approach with loops
$squared_evens = [];
foreach ($numbers as $num) {
    if ($num % 2 === 0) {
        $squared_evens[] = $num * $num;
    }
}
echo "Traditional approach: " . implode(", ", $squared_evens) . "\n";

// Functional approach
$result = pipeline(
    $numbers,
    fn($arr) => customFilter($arr, fn($n) => $n % 2 === 0),
    fn($arr) => customMap($arr, fn($n) => $n * $n),
    fn($arr) => array_values($arr)  // Reset keys
);
echo "Functional approach: " . implode(", ", $result) . "\n";

// Complex reduction example
$products = [
    ['name' => 'Laptop', 'price' => 999.99, 'category' => 'Electronics'],
    ['name' => 'Book', 'price' => 12.99, 'category' => 'Education'],
    ['name' => 'Mouse', 'price' => 25.50, 'category' => 'Electronics'],
    ['name' => 'Notebook', 'price' => 3.99, 'category' => 'Education']
];

// Group by category and calculate totals
$categoryTotals = customReduce($products, function($acc, $product) {
    $category = $product['category'];
    if (!isset($acc[$category])) {
        $acc[$category] = ['count' => 0, 'total' => 0];
    }
    $acc[$category]['count']++;
    $acc[$category]['total'] += $product['price'];
    return $acc;
}, []);

foreach ($categoryTotals as $category => $data) {
    $avg = round($data['total'] / $data['count'], 2);
    echo "$category: {$data['count']} items, \${$data['total']}, avg \${$avg}\n";
}
```

Functional programming patterns can be implemented using traditional  
loops for better performance or custom behavior. This approach  
combines the expressiveness of functional programming with the  
control and efficiency of explicit iteration, giving you the  
best of both worlds.  