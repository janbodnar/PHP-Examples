# Arrays

This chapter covers 35 comprehensive PHP array examples demonstrating all  
aspects of working with arrays in PHP. These examples progress from basic  
array creation to advanced array manipulation techniques using modern PHP  
8.4 features. Each example includes clear explanations to help you master  
PHP's powerful array capabilities.  

## Array creation

Creating arrays using different syntaxes and methods.  

```php
<?php

$fruits = ["apple", "banana", "cherry"];
$numbers = [1, 2, 3, 4, 5];
$mixed = ["text", 42, true, 3.14];
$empty = [];

echo $fruits[0] . "\n";        // apple
echo $numbers[2] . "\n";       // 3
echo count($mixed) . "\n";     // 4
echo count($empty) . "\n";     // 0
```

Arrays use square bracket notation for creation and access. They can  
contain mixed data types and are zero-indexed. Use `count()` to get  
the number of elements in an array.  

## Array initialization with range

Generating arrays with sequential values efficiently.  

```php
<?php

$numbers = range(1, 10);
$letters = range('a', 'z');
$evens = range(2, 20, 2);
$countdown = range(10, 1, -1);

print_r($numbers);    // [1, 2, 3, ..., 10]
print_r($evens);      // [2, 4, 6, ..., 20]
print_r($countdown);  // [10, 9, 8, ..., 1]
echo $letters[7] . "\n";  // h
```

The `range()` function creates arrays with sequences of numbers or  
letters. It accepts start, end, and optional step parameters for  
flexible array generation with custom intervals.  

## Associative arrays

Working with key-value pair arrays for structured data.  

```php
<?php

$person = [
    "name" => "Alice",
    "age" => 30,
    "city" => "New York",
    "active" => true
];

$config = [
    "host" => "localhost",
    "port" => 8080,
    "ssl" => false
];

echo $person["name"] . "\n";     // Alice
echo $config["port"] . "\n";     // 8080
$person["email"] = "alice@example.com";  // Add new key
unset($person["active"]);        // Remove key
print_r(array_keys($person));    // [name, age, city, email]
```

Associative arrays store key-value pairs using string keys. They're  
ideal for structured data and configuration settings. Use square  
brackets to add/modify elements and `unset()` to remove them.  

## Array element access and modification

Different ways to access and modify array elements.  

```php
<?php

$colors = ["red", "green", "blue"];
$scores = ["alice" => 95, "bob" => 87, "carol" => 92];

// Access elements
echo $colors[1] . "\n";           // green
echo $scores["alice"] . "\n";     // 95

// Modify elements
$colors[1] = "yellow";
$scores["alice"] = 98;

// Add elements
$colors[] = "purple";             // Append to end
$colors[10] = "orange";          // Specific index
$scores["david"] = 89;           // New key

print_r($colors);
print_r($scores);
```

Array elements can be accessed using indices or keys in square brackets.  
Assignment modifies existing elements or creates new ones. Empty brackets  
`[]` append to the end of indexed arrays.  

## Array operations and information

Essential functions for working with array data.  

```php
<?php

$numbers = [3, 1, 4, 1, 5, 9, 2, 6];

echo count($numbers) . "\n";      // 8 (length)
echo array_sum($numbers) . "\n";  // 31 (sum of all elements)
echo max($numbers) . "\n";        // 9 (maximum value)
echo min($numbers) . "\n";        // 1 (minimum value)

$unique = array_unique($numbers);
print_r($unique);                 // [3, 1, 4, 5, 9, 2, 6]

$reversed = array_reverse($numbers);
print_r($reversed);               // [6, 2, 9, 5, 1, 4, 1, 3]

echo array_product([2, 3, 4]) . "\n";  // 24 (product)
```

PHP provides comprehensive functions for array analysis including  
counting, summing, finding extremes, removing duplicates, and reversing.  
These functions don't modify the original array unless specified.  

## Multidimensional arrays

Creating and working with arrays containing other arrays.  

```php
<?php

$matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

$students = [
    ["name" => "Alice", "grade" => 95, "subject" => "Math"],
    ["name" => "Bob", "grade" => 87, "subject" => "Science"],
    ["name" => "Carol", "grade" => 92, "subject" => "English"]
];

echo $matrix[1][2] . "\n";                // 6
echo $students[0]["name"] . "\n";         // Alice
echo $students[2]["grade"] . "\n";        // 92

// Add new student
$students[] = ["name" => "David", "grade" => 88, "subject" => "History"];
print_r($students[3]);
```

Multidimensional arrays contain arrays as elements, creating matrix-like  
structures. Access elements using multiple bracket notations for each  
dimension level. Useful for tabular data and complex structures.  

## Array concatenation and merging

Combining multiple arrays into single arrays.  

```php
<?php

$fruits = ["apple", "banana"];
$vegetables = ["carrot", "broccoli"];
$colors = ["red" => "#FF0000", "green" => "#00FF00"];
$more_colors = ["blue" => "#0000FF", "yellow" => "#FFFF00"];

// Concatenate indexed arrays
$combined = array_merge($fruits, $vegetables);
print_r($combined);  // [apple, banana, carrot, broccoli]

// Merge associative arrays
$all_colors = array_merge($colors, $more_colors);
print_r($all_colors);

// Using spread operator (PHP 7.4+)
$spread = [...$fruits, ...$vegetables];
print_r($spread);

// Plus operator preserves keys
$preserved = $colors + $more_colors;
print_r($preserved);
```

Array merging combines elements from multiple arrays. `array_merge()`  
reindexes numeric keys while preserving string keys. The spread operator  
provides modern syntax, while `+` preserves original keys.  

## Array slicing and chunking

Extracting portions of arrays efficiently.  

```php
<?php

$numbers = range(1, 20);

// Extract slice
$slice = array_slice($numbers, 5, 5);  // From index 5, take 5 elements
print_r($slice);                       // [6, 7, 8, 9, 10]

// Slice with negative offset
$end = array_slice($numbers, -3);      // Last 3 elements
print_r($end);                         // [18, 19, 20]

// Preserve keys
$preserved = array_slice($numbers, 2, 4, true);
print_r($preserved);

// Chunk array
$chunks = array_chunk($numbers, 5);
print_r($chunks);                      // [[1,2,3,4,5], [6,7,8,9,10], ...]

// Chunk with preserved keys
$keyed_chunks = array_chunk($numbers, 3, true);
print_r($keyed_chunks[0]);
```

Array slicing extracts portions using offset and length parameters.  
Negative offsets count from the end. `array_chunk()` splits arrays  
into smaller arrays of specified size for batch processing.  

## Array iteration with foreach

Traversing arrays using modern iteration syntax.  

```php
<?php

$fruits = ["apple", "banana", "cherry", "date"];
$scores = ["alice" => 95, "bob" => 87, "carol" => 92];

// Simple foreach
foreach ($fruits as $fruit) {
    echo "Fruit: $fruit\n";
}

// Foreach with index
foreach ($fruits as $index => $fruit) {
    echo "$index: $fruit\n";
}

// Foreach with associative array
foreach ($scores as $name => $score) {
    echo "$name scored $score\n";
}

// Reference modification
foreach ($fruits as &$fruit) {
    $fruit = strtoupper($fruit);
}
unset($fruit);  // Break reference
print_r($fruits);
```

The `foreach` loop provides clean array iteration. Use references (`&`)  
to modify elements during iteration. Always `unset()` references after  
loops to prevent unexpected behavior in subsequent code.  

## Array searching and filtering

Finding elements and filtering arrays based on conditions.  

```php
<?php

$numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
$words = ["apple", "banana", "cherry", "date"];

// Search for values
echo array_search(5, $numbers) . "\n";    // 4 (index)
echo in_array("cherry", $words) ? "found" : "not found" . "\n";

// Filter arrays
$evens = array_filter($numbers, fn($n) => $n % 2 === 0);
print_r(array_values($evens));  // [2, 4, 6, 8, 10]

$long_words = array_filter($words, fn($w) => strlen($w) > 5);
print_r(array_values($long_words));  // [banana, cherry]

// Custom search function
$found_index = array_search(true, array_map(fn($w) => str_contains($w, 'err'), $words));
echo $found_index !== false ? "Found at: $found_index\n" : "Not found\n";
```

PHP provides various search functions: `array_search()` returns the key  
of found values, `in_array()` checks existence, and `array_filter()`  
creates new arrays with elements matching conditions.  

## Array mapping and transformation

Transforming array elements using mapping functions.  

```php
<?php

$numbers = [1, 2, 3, 4, 5];
$words = ["hello", "world", "php", "programming"];

// Map with arrow functions
$squared = array_map(fn($n) => $n * $n, $numbers);
print_r($squared);  // [1, 4, 9, 16, 25]

$lengths = array_map(fn($w) => strlen($w), $words);
print_r($lengths);  // [5, 5, 3, 11]

$uppercase = array_map('strtoupper', $words);
print_r($uppercase);  // [HELLO, WORLD, PHP, PROGRAMMING]

// Map multiple arrays
$names = ["alice", "bob", "carol"];
$ages = [25, 30, 35];
$combined = array_map(fn($name, $age) => "$name ($age)", $names, $ages);
print_r($combined);  // [alice (25), bob (30), carol (35)]

// Complex transformations
$processed = array_map(fn($n) => ['value' => $n, 'squared' => $n * $n], $numbers);
print_r($processed);
```

`array_map()` applies functions to every element, creating new arrays.  
It accepts arrow functions, regular functions, or built-in function  
names. Multiple arrays can be processed simultaneously.  

## Array reduction and aggregation

Reducing arrays to single values through aggregation.  

```php
<?php

$numbers = [1, 2, 3, 4, 5];
$words = ["PHP", "is", "awesome"];

// Basic reduction
$sum = array_reduce($numbers, fn($carry, $item) => $carry + $item, 0);
echo "Sum: $sum\n";  // 15

$product = array_reduce($numbers, fn($carry, $item) => $carry * $item, 1);
echo "Product: $product\n";  // 120

// String concatenation
$sentence = array_reduce($words, fn($carry, $word) => $carry . " " . $word, "");
echo "Sentence:" . $sentence . "\n";  // PHP is awesome

// Complex aggregation
$stats = array_reduce($numbers, function($carry, $item) {
    $carry['sum'] += $item;
    $carry['count']++;
    $carry['average'] = $carry['sum'] / $carry['count'];
    return $carry;
}, ['sum' => 0, 'count' => 0, 'average' => 0]);

print_r($stats);
```

`array_reduce()` processes arrays element by element, accumulating  
results into a single value. The carry parameter holds the accumulated  
result, making it powerful for aggregations and statistics.  

## Array sorting - basic

Sorting arrays using various criteria and functions.  

```php
<?php

$numbers = [3, 1, 4, 1, 5, 9, 2, 6];
$words = ["banana", "apple", "cherry", "date"];

// Sort indexed arrays
$sorted_numbers = $numbers;
sort($sorted_numbers);
print_r($sorted_numbers);  // [1, 1, 2, 3, 4, 5, 6, 9]

$sorted_desc = $numbers;
rsort($sorted_desc);
print_r($sorted_desc);  // [9, 6, 5, 4, 3, 2, 1, 1]

// Sort strings
$sorted_words = $words;
sort($sorted_words);
print_r($sorted_words);  // [apple, banana, cherry, date]

// Natural sorting
$versions = ["v1.10", "v1.2", "v1.21", "v1.3"];
natsort($versions);
print_r($versions);  // [v1.2, v1.3, v1.10, v1.21]

// Case-insensitive sorting
$mixed_case = ["Banana", "apple", "Cherry"];
natcasesort($mixed_case);
print_r($mixed_case);
```

PHP provides multiple sorting functions: `sort()` for ascending,  
`rsort()` for descending, `natsort()` for natural ordering, and  
`natcasesort()` for case-insensitive natural sorting.  

## Array sorting - associative

Sorting associative arrays by keys or values.  

```php
<?php

$scores = [
    "alice" => 95,
    "bob" => 87,
    "carol" => 92,
    "david" => 89
];

// Sort by values
$by_score = $scores;
asort($by_score);  // Ascending by value
print_r($by_score);  // [bob => 87, david => 89, carol => 92, alice => 95]

$by_score_desc = $scores;
arsort($by_score_desc);  // Descending by value
print_r($by_score_desc);

// Sort by keys
$by_name = $scores;
ksort($by_name);  // Ascending by key
print_r($by_name);  // [alice => 95, bob => 87, carol => 92, david => 89]

$by_name_desc = $scores;
krsort($by_name_desc);  // Descending by key
print_r($by_name_desc);
```

Associative array sorting preserves key-value relationships:  
`asort()`/`arsort()` sort by values, `ksort()`/`krsort()` sort by  
keys, maintaining the association between keys and their values.  

## Custom array sorting

Using user-defined comparison functions for complex sorting.  

```php
<?php

$students = [
    ["name" => "Alice", "grade" => 95, "age" => 20],
    ["name" => "Bob", "grade" => 87, "age" => 22],
    ["name" => "Carol", "grade" => 92, "age" => 19],
    ["name" => "David", "grade" => 89, "age" => 21]
];

// Sort by grade (descending)
$by_grade = $students;
usort($by_grade, fn($a, $b) => $b["grade"] <=> $a["grade"]);
print_r($by_grade);

// Sort by age (ascending)
$by_age = $students;
usort($by_age, fn($a, $b) => $a["age"] <=> $b["age"]);
print_r($by_age);

// Sort by name length
$words = ["elephant", "cat", "dog", "butterfly"];
usort($words, fn($a, $b) => strlen($a) <=> strlen($b));
print_r($words);  // [cat, dog, elephant, butterfly]

// Complex multi-field sorting
$by_grade_then_age = $students;
usort($by_grade_then_age, function($a, $b) {
    $grade_cmp = $b["grade"] <=> $a["grade"];
    return $grade_cmp !== 0 ? $grade_cmp : $a["age"] <=> $b["age"];
});
print_r($by_grade_then_age);
```

`usort()` enables custom sorting with comparison functions. The  
spaceship operator `<=>` simplifies comparisons, returning -1, 0, or 1  
based on the relationship between values.  

## Array key operations

Working with array keys for data manipulation.  

```php
<?php

$data = [
    "first_name" => "John",
    "last_name" => "Doe", 
    "email" => "john@example.com",
    "age" => 30
];

// Get keys and values
$keys = array_keys($data);
$values = array_values($data);
print_r($keys);    // [first_name, last_name, email, age]
print_r($values);  // [John, Doe, john@example.com, 30]

// Key existence
if (array_key_exists("email", $data)) {
    echo "Email found: " . $data["email"] . "\n";
}

// Flip keys and values
$flipped = array_flip($data);
print_r($flipped);  // [John => first_name, Doe => last_name, ...]

// Combine arrays as keys and values
$keys_array = ["name", "age", "city"];
$values_array = ["Alice", 25, "New York"];
$combined = array_combine($keys_array, $values_array);
print_r($combined);  // [name => Alice, age => 25, city => New York]
```

Key operations include extracting keys with `array_keys()`,  
checking existence with `array_key_exists()`, flipping key-value  
relationships, and combining separate key-value arrays.  

## Array intersection and difference

Finding common elements and differences between arrays.  

```php
<?php

$array1 = [1, 2, 3, 4, 5];
$array2 = [3, 4, 5, 6, 7];
$array3 = [5, 6, 7, 8, 9];

// Intersection (common values)
$intersection = array_intersect($array1, $array2);
print_r($intersection);  // [3, 4, 5]

$three_way = array_intersect($array1, $array2, $array3);
print_r($three_way);  // [5]

// Difference (values in first array not in others)
$diff = array_diff($array1, $array2);
print_r($diff);  // [1, 2]

// Associative arrays
$colors1 = ["a" => "red", "b" => "green", "c" => "blue"];
$colors2 = ["a" => "red", "d" => "yellow", "e" => "blue"];

$key_intersect = array_intersect_key($colors1, $colors2);
print_r($key_intersect);  // [a => red]

$key_diff = array_diff_key($colors1, $colors2);
print_r($key_diff);  // [b => green, c => blue]
```

Intersection functions find common elements between arrays, while  
difference functions find elements unique to the first array.  
Key-based versions compare array keys instead of values.  

## Array padding and filling

Expanding arrays to specific sizes with default values.  

```php
<?php

$numbers = [1, 2, 3];

// Pad array to specific size
$padded = array_pad($numbers, 7, 0);
print_r($padded);  // [1, 2, 3, 0, 0, 0, 0]

// Pad on the left (negative size)
$left_padded = array_pad($numbers, -6, "x");
print_r($left_padded);  // [x, x, x, 1, 2, 3]

// Fill array with values
$filled = array_fill(0, 5, "default");
print_r($filled);  // [default, default, default, default, default]

// Fill with keys
$keyed = array_fill_keys(["name", "age", "email"], "");
print_r($keyed);  // [name => "", age => "", email => ""]

// Create range and fill
$alphabet = array_fill_keys(range('a', 'z'), 0);
print_r(array_slice($alphabet, 0, 5));  // [a => 0, b => 0, c => 0, d => 0, e => 0]
```

Padding functions expand arrays to specific sizes with default values.  
`array_fill()` creates new arrays with repeated values, while  
`array_fill_keys()` creates associative arrays with default values.  

## Array column extraction

Extracting columns from multidimensional arrays efficiently.  

```php
<?php

$records = [
    ["id" => 1, "name" => "Alice", "age" => 25, "city" => "New York"],
    ["id" => 2, "name" => "Bob", "age" => 30, "city" => "London"],
    ["id" => 3, "name" => "Carol", "age" => 35, "city" => "Tokyo"],
    ["id" => 4, "name" => "David", "age" => 28, "city" => "Paris"]
];

// Extract single column
$names = array_column($records, "name");
print_r($names);  // [Alice, Bob, Carol, David]

$ages = array_column($records, "age");
print_r($ages);  // [25, 30, 35, 28]

// Extract column with custom keys
$name_by_id = array_column($records, "name", "id");
print_r($name_by_id);  // [1 => Alice, 2 => Bob, 3 => Carol, 4 => David]

$city_by_name = array_column($records, "city", "name");
print_r($city_by_name);  // [Alice => New York, Bob => London, ...]

// Extract multiple columns (PHP 8.0+)
$partial = array_map(fn($r) => ["name" => $r["name"], "age" => $r["age"]], $records);
print_r($partial);
```

`array_column()` extracts specific columns from multidimensional arrays,  
optionally using another column as keys. This is particularly useful  
for database result processing and data transformation.  

## Array validation and checking

Validating array contents and structure.  

```php
<?php

$numbers = [1, 2, 3, 4, 5];
$mixed = [1, "hello", 3.14, true];
$empty = [];

// Check if all elements match condition
$all_positive = array_reduce($numbers, fn($carry, $n) => $carry && $n > 0, true);
echo $all_positive ? "All positive\n" : "Not all positive\n";

$all_numeric = array_reduce($mixed, fn($carry, $item) => $carry && is_numeric($item), true);
echo $all_numeric ? "All numeric\n" : "Not all numeric\n";

// Check array properties
echo is_array($numbers) ? "Is array\n" : "Not array\n";
echo empty($empty) ? "Empty array\n" : "Not empty\n";

// Validate required keys
function validateUser($user, $required_keys) {
    return empty(array_diff($required_keys, array_keys($user)));
}

$user = ["name" => "Alice", "email" => "alice@example.com", "age" => 25];
$required = ["name", "email"];
echo validateUser($user, $required) ? "Valid user\n" : "Invalid user\n";

// Type checking
$types = array_map('gettype', $mixed);
print_r($types);  // [integer, string, double, boolean]
```

Array validation includes checking element types, testing conditions  
across all elements, validating structure, and ensuring required  
keys exist. Use helper functions for complex validation logic.  

## Array flattening

Converting multidimensional arrays to single dimension.  

```php
<?php

// Simple nested array
$nested = [1, [2, 3], [4, [5, 6]], 7];

// Recursive flattening function
function flattenArray($array) {
    $result = [];
    foreach ($array as $item) {
        if (is_array($item)) {
            $result = array_merge($result, flattenArray($item));
        } else {
            $result[] = $item;
        }
    }
    return $result;
}

$flattened = flattenArray($nested);
print_r($flattened);  // [1, 2, 3, 4, 5, 6, 7]

// Using array_walk_recursive for simple cases
$values = [];
array_walk_recursive($nested, function($value) use (&$values) {
    $values[] = $value;
});
print_r($values);

// Flatten associative arrays
$complex = [
    "user" => ["name" => "Alice", "details" => ["age" => 25, "city" => "NYC"]],
    "config" => ["debug" => true, "cache" => ["enabled" => false]]
];

function flattenAssoc($array, $prefix = '') {
    $result = [];
    foreach ($array as $key => $value) {
        $newKey = $prefix . $key;
        if (is_array($value)) {
            $result = array_merge($result, flattenAssoc($value, $newKey . '.'));
        } else {
            $result[$newKey] = $value;
        }
    }
    return $result;
}

$flat_assoc = flattenAssoc($complex);
print_r($flat_assoc);
```

Flattening converts nested arrays to single-level arrays. Recursive  
functions handle arbitrary nesting depths, while `array_walk_recursive()`  
provides a simpler approach for basic cases.  

## Array grouping

Organizing array elements into groups based on criteria.  

```php
<?php

$students = [
    ["name" => "Alice", "grade" => "A", "subject" => "Math"],
    ["name" => "Bob", "grade" => "B", "subject" => "Math"], 
    ["name" => "Carol", "grade" => "A", "subject" => "Science"],
    ["name" => "David", "grade" => "B", "subject" => "Science"],
    ["name" => "Eve", "grade" => "A", "subject" => "Math"]
];

// Group by grade
function groupBy($array, $key) {
    $groups = [];
    foreach ($array as $item) {
        $groups[$item[$key]][] = $item;
    }
    return $groups;
}

$by_grade = groupBy($students, "grade");
print_r($by_grade);

// Group by subject
$by_subject = groupBy($students, "subject");
print_r($by_subject);

// Group with array_reduce
$grouped = array_reduce($students, function($groups, $student) {
    $groups[$student["subject"]][] = $student["name"];
    return $groups;
}, []);
print_r($grouped);

// Count groups
$grade_counts = array_map('count', $by_grade);
print_r($grade_counts);  // [A => 3, B => 2]
```

Grouping organizes array elements by common properties. Custom functions  
can group by any field, while `array_reduce()` provides flexible  
grouping with transformation capabilities.  

## Array serialization

Converting arrays to strings and back for storage or transmission.  

```php
<?php

$data = [
    "name" => "Alice",
    "age" => 25,
    "hobbies" => ["reading", "swimming", "coding"],
    "active" => true
];

// JSON serialization
$json = json_encode($data);
echo "JSON: $json\n";

$from_json = json_decode($json, true);
print_r($from_json);

// JSON with formatting
$pretty_json = json_encode($data, JSON_PRETTY_PRINT);
echo "Pretty JSON:\n$pretty_json\n";

// PHP serialization
$serialized = serialize($data);
echo "Serialized: $serialized\n";

$unserialized = unserialize($serialized);
print_r($unserialized);

// Query string format
$query_string = http_build_query($data);
echo "Query string: $query_string\n";

// Custom delimiter format
$csv_data = [["Alice", 25, "NYC"], ["Bob", 30, "LA"]];
$csv = implode("\n", array_map(fn($row) => implode(",", $row), $csv_data));
echo "CSV:\n$csv\n";
```

Serialization converts arrays to strings for storage or transmission.  
JSON is standard for web APIs, PHP serialize() preserves exact types,  
and custom formats serve specific requirements.  

## Array performance optimization

Techniques for efficient array operations and memory usage.  

```php
<?php

// Efficient array creation
$large_array = range(1, 100000);

// Use isset() instead of array_key_exists() for performance
$test_array = array_flip($large_array);
$start = microtime(true);
for ($i = 0; $i < 10000; $i++) {
    isset($test_array[50000]);
}
$isset_time = microtime(true) - $start;

$start = microtime(true);
for ($i = 0; $i < 10000; $i++) {
    array_key_exists(50000, $test_array);
}
$key_exists_time = microtime(true) - $start;

echo "isset() time: " . number_format($isset_time, 6) . "s\n";
echo "array_key_exists() time: " . number_format($key_exists_time, 6) . "s\n";

// Memory-efficient iteration
function processLargeArray($callback) {
    for ($i = 1; $i <= 1000000; $i++) {
        yield $i * 2;
    }
}

// Using generators instead of creating full arrays
$sum = 0;
foreach (processLargeArray(fn($x) => $x * 2) as $value) {
    $sum += $value;
    if ($sum > 1000000) break;
}
echo "Sum: $sum\n";

// Pre-allocate when possible
$optimized = array_fill(0, 10000, null);
for ($i = 0; $i < 10000; $i++) {
    $optimized[$i] = $i * $i;
}
```

Performance optimization includes using `isset()` over  
`array_key_exists()` for lookups, generators for memory efficiency,  
and pre-allocation for known sizes. Profile critical paths to identify  
bottlenecks and optimize accordingly.  

## Advanced array patterns

Sophisticated array manipulation techniques and patterns.  

```php
<?php

// Array as a stack (LIFO)
$stack = [];
array_push($stack, "first", "second", "third");
print_r($stack);
$last = array_pop($stack);
echo "Popped: $last\n";

// Array as a queue (FIFO)
$queue = [];
array_push($queue, "first", "second", "third");
$first = array_shift($queue);
echo "Shifted: $first\n";
array_unshift($queue, "new_first");
print_r($queue);

// Memoization with arrays
$memo = [];
function fibonacci($n) {
    global $memo;
    if (isset($memo[$n])) return $memo[$n];
    if ($n <= 1) return $n;
    return $memo[$n] = fibonacci($n - 1) + fibonacci($n - 2);
}

echo "Fibonacci(20): " . fibonacci(20) . "\n";

// Array-based configuration system
class Config {
    private static $settings = [];
    
    public static function set($key, $value) {
        self::$settings[$key] = $value;
    }
    
    public static function get($key, $default = null) {
        return self::$settings[$key] ?? $default;
    }
}

Config::set("app.name", "MyApp");
Config::set("db.host", "localhost");
echo Config::get("app.name") . "\n";
```

Advanced patterns include using arrays as data structures (stacks,  
queues), implementing memoization for performance, and creating  
configuration systems with nested key access patterns.  

## Array utility functions

Custom helper functions for common array operations.  

```php
<?php

// Get first/last elements safely
function first($array) {
    return reset($array);
}

function last($array) {
    return end($array);
}

// Pluck specific keys from array of arrays
function pluck($array, $key) {
    return array_map(fn($item) => $item[$key] ?? null, $array);
}

// Deep array merge
function array_merge_recursive_distinct($array1, $array2) {
    $merged = $array1;
    foreach ($array2 as $key => $value) {
        if (is_array($value) && isset($merged[$key]) && is_array($merged[$key])) {
            $merged[$key] = array_merge_recursive_distinct($merged[$key], $value);
        } else {
            $merged[$key] = $value;
        }
    }
    return $merged;
}

// Remove empty values recursively
function array_filter_recursive($array) {
    return array_filter(array_map(function($item) {
        return is_array($item) ? array_filter_recursive($item) : $item;
    }, $array));
}

// Test utilities
$data = [["name" => "Alice", "age" => 25], ["name" => "Bob", "age" => 30]];
$numbers = [1, 2, 3, 4, 5];

echo "First: " . first($numbers) . "\n";
echo "Last: " . last($numbers) . "\n";
print_r(pluck($data, "name"));

$nested1 = ["a" => ["x" => 1], "b" => 2];
$nested2 = ["a" => ["y" => 2], "c" => 3];
print_r(array_merge_recursive_distinct($nested1, $nested2));
```

Utility functions provide reusable solutions for common array tasks.  
These helpers simplify complex operations and can be customized for  
specific application needs and coding patterns.  

## Array destructuring

Extracting multiple values from arrays in single operations.  

```php
<?php

// Basic array destructuring
$colors = ["red", "green", "blue"];
[$first, $second, $third] = $colors;
echo "Colors: $first, $second, $third\n";

// Skip elements
[$primary, , $tertiary] = $colors;
echo "Primary: $primary, Tertiary: $tertiary\n";

// With default values (PHP 7.1+)
[$r, $g, $b, $alpha = 1.0] = ["255", "128", "64"];
echo "RGBA: $r, $g, $b, $alpha\n";

// Nested destructuring
$person = ["Alice", 25, ["reading", "swimming", "coding"]];
[$name, $age, [$hobby1, $hobby2]] = $person;
echo "$name ($age) likes $hobby1 and $hobby2\n";

// Associative destructuring
$user = ["name" => "Bob", "email" => "bob@example.com", "age" => 30];
["name" => $userName, "email" => $userEmail] = $user;
echo "User: $userName ($userEmail)\n";

// Function return destructuring
function getCoordinates() {
    return [10, 20];
}
[$x, $y] = getCoordinates();
echo "Coordinates: ($x, $y)\n";
```

Array destructuring extracts multiple values into variables efficiently.  
It supports skipping elements, default values, nested structures, and  
associative arrays for clean, readable value extraction.  

## Array splicing

Removing and replacing portions of arrays dynamically.  

```php
<?php

$fruits = ["apple", "banana", "cherry", "date", "elderberry"];

// Remove elements
$original = $fruits;
$removed = array_splice($original, 1, 2);  // Remove 2 elements starting at index 1
print_r($original);  // [apple, date, elderberry]
print_r($removed);   // [banana, cherry]

// Replace elements
$fruits2 = ["apple", "banana", "cherry", "date"];
array_splice($fruits2, 1, 2, ["kiwi", "lemon", "mango"]);
print_r($fruits2);  // [apple, kiwi, lemon, mango, date]

// Insert without removing
$fruits3 = ["apple", "cherry"];
array_splice($fruits3, 1, 0, ["banana"]);  // Insert at position 1
print_r($fruits3);  // [apple, banana, cherry]

// Remove from end
$numbers = [1, 2, 3, 4, 5];
$last_two = array_splice($numbers, -2);
print_r($numbers);  // [1, 2, 3]
print_r($last_two); // [4, 5]

// Complex replacement
$data = ["old1", "old2", "old3", "keep"];
array_splice($data, 0, 3, ["new1", "new2"]);
print_r($data);  // [new1, new2, keep]
```

Array splicing modifies arrays by removing, replacing, or inserting  
elements at specific positions. It's more flexible than simple push/pop  
operations and can handle complex array modifications in one call.  

## Array walking

Applying functions to all array elements without creating new arrays.  

```php
<?php

// Basic array_walk
$prices = [10.50, 25.00, 15.75, 30.25];
array_walk($prices, function(&$price) {
    $price *= 1.1;  // Apply 10% increase
});
print_r($prices);  // [11.55, 27.5, 17.325, 33.275]

// Walk with additional data
$products = ["laptop", "mouse", "keyboard"];
array_walk($products, function(&$item, $key, $prefix) {
    $item = "$prefix: " . ucfirst($item) . " (#$key)";
}, "Product");
print_r($products);  // [Product: Laptop (#0), Product: Mouse (#1), ...]

// Recursive walking
$nested = [
    "user" => ["name" => "alice", "email" => "alice@example.com"],
    "config" => ["debug" => "false", "cache" => "true"]
];

array_walk_recursive($nested, function(&$value) {
    if (is_string($value)) {
        $value = strtoupper($value);
    }
});
print_r($nested);

// Walk with validation
$scores = [85, 92, 78, 96, 88];
$errors = [];
array_walk($scores, function($score, $index) use (&$errors) {
    if ($score < 80) {
        $errors[] = "Score at index $index is below 80: $score";
    }
});
print_r($errors);
```

`array_walk()` applies functions to array elements in place, making it  
memory-efficient for transformations. It passes references for  
modification and supports additional parameters and recursive operation.  

## Array pivoting

Transforming array structure by changing the relationship between keys and values.  

```php
<?php

// Simple pivot - swap keys and values
$original = ["a" => 1, "b" => 2, "c" => 3];
$pivoted = array_flip($original);
print_r($pivoted);  // [1 => a, 2 => b, 3 => c]

// Pivot multidimensional data
$sales_data = [
    ["month" => "Jan", "product" => "Laptop", "sales" => 100],
    ["month" => "Jan", "product" => "Mouse", "sales" => 50],
    ["month" => "Feb", "product" => "Laptop", "sales" => 120],
    ["month" => "Feb", "product" => "Mouse", "sales" => 60]
];

// Pivot by month
function pivotByMonth($data) {
    $result = [];
    foreach ($data as $row) {
        $result[$row["month"]][$row["product"]] = $row["sales"];
    }
    return $result;
}

$monthly_pivot = pivotByMonth($sales_data);
print_r($monthly_pivot);

// Pivot by product
function pivotByProduct($data) {
    $result = [];
    foreach ($data as $row) {
        $result[$row["product"]][$row["month"]] = $row["sales"];
    }
    return $result;
}

$product_pivot = pivotByProduct($sales_data);
print_r($product_pivot);

// Dynamic pivot function
function pivot($data, $rowKey, $colKey, $valueKey) {
    $result = [];
    foreach ($data as $item) {
        $result[$item[$rowKey]][$item[$colKey]] = $item[$valueKey];
    }
    return $result;
}

$dynamic_pivot = pivot($sales_data, "month", "product", "sales");
print_r($dynamic_pivot);
```

Pivoting transforms data structure by reorganizing relationships between  
keys and values. This is useful for data analysis, reporting, and  
converting between different data representations.  

## Array caching

Implementing caching mechanisms using arrays for improved performance.  

```php
<?php

// Simple in-memory cache
class ArrayCache {
    private static $cache = [];
    
    public static function get($key) {
        return self::$cache[$key] ?? null;
    }
    
    public static function set($key, $value) {
        self::$cache[$key] = $value;
    }
    
    public static function has($key) {
        return isset(self::$cache[$key]);
    }
    
    public static function forget($key) {
        unset(self::$cache[$key]);
    }
    
    public static function clear() {
        self::$cache = [];
    }
}

// Expensive operation with caching
function expensiveCalculation($n) {
    $cache_key = "calc_$n";
    
    if (ArrayCache::has($cache_key)) {
        echo "Cache hit for $n\n";
        return ArrayCache::get($cache_key);
    }
    
    echo "Computing for $n\n";
    $result = array_sum(range(1, $n)) * 2;  // Simulate expensive operation
    ArrayCache::set($cache_key, $result);
    return $result;
}

// Test caching
echo expensiveCalculation(1000) . "\n";  // Computes
echo expensiveCalculation(1000) . "\n";  // Cache hit
echo expensiveCalculation(2000) . "\n";  // Computes

// LRU Cache implementation
class LRUCache {
    private $cache = [];
    private $capacity;
    
    public function __construct($capacity) {
        $this->capacity = $capacity;
    }
    
    public function get($key) {
        if (isset($this->cache[$key])) {
            $value = $this->cache[$key];
            unset($this->cache[$key]);
            $this->cache[$key] = $value;  // Move to end
            return $value;
        }
        return null;
    }
    
    public function set($key, $value) {
        if (isset($this->cache[$key])) {
            unset($this->cache[$key]);
        } elseif (count($this->cache) >= $this->capacity) {
            reset($this->cache);
            $firstKey = key($this->cache);
            unset($this->cache[$firstKey]);
        }
        $this->cache[$key] = $value;
    }
}

$lru = new LRUCache(3);
$lru->set("a", 1);
$lru->set("b", 2);
$lru->set("c", 3);
echo $lru->get("a") . "\n";  // 1, moves 'a' to end
$lru->set("d", 4);           // Evicts 'b'
```

Array-based caching provides fast in-memory storage for computed values.  
LRU (Least Recently Used) caches automatically manage memory by evicting  
old entries, making them suitable for bounded-memory scenarios.  

## Array indexing strategies

Different approaches to organizing and accessing array data efficiently.  

```php
<?php

// Composite key indexing
$users = [
    ["country" => "US", "city" => "NYC", "name" => "Alice"],
    ["country" => "US", "city" => "LA", "name" => "Bob"],
    ["country" => "UK", "city" => "London", "name" => "Carol"],
    ["country" => "US", "city" => "NYC", "name" => "David"]
];

// Create composite index
function createCompositeIndex($data, $keys) {
    $index = [];
    foreach ($data as $item) {
        $compositeKey = implode('|', array_map(fn($k) => $item[$k], $keys));
        $index[$compositeKey][] = $item;
    }
    return $index;
}

$cityIndex = createCompositeIndex($users, ["country", "city"]);
print_r($cityIndex["US|NYC"]);  // Alice and David

// Multiple indexing
function createMultipleIndexes($data, $indexFields) {
    $indexes = [];
    foreach ($indexFields as $field) {
        $indexes[$field] = [];
        foreach ($data as $key => $item) {
            $indexes[$field][$item[$field]][] = $key;
        }
    }
    return $indexes;
}

$indexes = createMultipleIndexes($users, ["country", "city"]);
print_r($indexes["country"]["US"]);  // [0, 1, 3]

// Hash table simulation
class HashTable {
    private $buckets = [];
    private $size;
    
    public function __construct($size = 10) {
        $this->size = $size;
        $this->buckets = array_fill(0, $size, []);
    }
    
    private function hash($key) {
        return crc32($key) % $this->size;
    }
    
    public function set($key, $value) {
        $index = $this->hash($key);
        $this->buckets[$index][$key] = $value;
    }
    
    public function get($key) {
        $index = $this->hash($key);
        return $this->buckets[$index][$key] ?? null;
    }
}

$hash = new HashTable(5);
$hash->set("name", "Alice");
$hash->set("age", 25);
echo $hash->get("name") . "\n";  // Alice
```

Effective indexing strategies improve data access performance through  
composite keys, multiple indexes, and hash tables. Choose strategies  
based on access patterns and data characteristics.  

## Array batch processing

Processing large arrays in chunks for memory efficiency and performance.  

```php
<?php

// Process large dataset in batches
function processBatch($data, $batchSize, $callback) {
    $chunks = array_chunk($data, $batchSize);
    $results = [];
    
    foreach ($chunks as $chunkIndex => $chunk) {
        echo "Processing batch " . ($chunkIndex + 1) . "/" . count($chunks) . "\n";
        $batchResult = $callback($chunk);
        $results = array_merge($results, $batchResult);
    }
    
    return $results;
}

// Simulate large dataset
$largeDataset = range(1, 1000);

// Batch processing example
$processed = processBatch($largeDataset, 100, function($batch) {
    return array_map(function($item) {
        // Simulate expensive operation
        return $item * $item;
    }, $batch);
});

echo "Processed " . count($processed) . " items\n";

// Memory-efficient iteration with generators
function batchGenerator($data, $batchSize) {
    $batch = [];
    foreach ($data as $item) {
        $batch[] = $item;
        if (count($batch) >= $batchSize) {
            yield $batch;
            $batch = [];
        }
    }
    if (!empty($batch)) {
        yield $batch;
    }
}

// Process with generator
$total = 0;
foreach (batchGenerator(range(1, 10000), 500) as $batch) {
    $batchSum = array_sum($batch);
    $total += $batchSum;
    echo "Batch sum: $batchSum\n";
}
echo "Total: $total\n";

// Database-style batch processing
function batchUpdate($records, $batchSize = 100) {
    $batches = array_chunk($records, $batchSize);
    
    foreach ($batches as $batchIndex => $batch) {
        echo "Updating batch " . ($batchIndex + 1) . "\n";
        
        // Simulate database update
        foreach ($batch as $record) {
            // Process individual record
            $record['updated_at'] = date('Y-m-d H:i:s');
        }
        
        // In real scenario, you'd execute batch SQL here
        echo "Updated " . count($batch) . " records\n";
    }
}

$records = array_fill(0, 500, ["id" => 1, "name" => "Sample"]);
batchUpdate($records, 50);
```

Batch processing handles large datasets efficiently by dividing them  
into manageable chunks. This prevents memory exhaustion and enables  
progress tracking for long-running operations.  

## Array state management

Managing application state using arrays for configuration and data flow.  

```php
<?php

// Application state manager
class StateManager {
    private $state = [];
    private $listeners = [];
    
    public function setState($key, $value) {
        $oldValue = $this->state[$key] ?? null;
        $this->state[$key] = $value;
        $this->notify($key, $value, $oldValue);
    }
    
    public function getState($key = null) {
        return $key ? ($this->state[$key] ?? null) : $this->state;
    }
    
    public function subscribe($key, $callback) {
        $this->listeners[$key][] = $callback;
    }
    
    private function notify($key, $newValue, $oldValue) {
        if (isset($this->listeners[$key])) {
            foreach ($this->listeners[$key] as $callback) {
                $callback($newValue, $oldValue, $key);
            }
        }
    }
    
    public function updateState($updates) {
        foreach ($updates as $key => $value) {
            $this->setState($key, $value);
        }
    }
}

// Usage example
$state = new StateManager();

// Subscribe to state changes
$state->subscribe('user', function($new, $old, $key) {
    echo "User changed from " . json_encode($old) . " to " . json_encode($new) . "\n";
});

$state->setState('user', ['name' => 'Alice', 'age' => 25]);
$state->setState('user', ['name' => 'Alice', 'age' => 26]);

// Immutable state updates
class ImmutableState {
    private $data;
    
    public function __construct($data = []) {
        $this->data = $data;
    }
    
    public function set($key, $value) {
        $newData = $this->data;
        $newData[$key] = $value;
        return new self($newData);
    }
    
    public function get($key = null) {
        return $key ? ($this->data[$key] ?? null) : $this->data;
    }
    
    public function merge($updates) {
        return new self(array_merge($this->data, $updates));
    }
}

// Immutable usage
$initialState = new ImmutableState(['count' => 0]);
$newState = $initialState->set('count', 1);
$finalState = $newState->merge(['user' => 'Alice', 'count' => 2]);

echo "Initial: " . json_encode($initialState->get()) . "\n";
echo "Final: " . json_encode($finalState->get()) . "\n";
```

State management patterns use arrays to maintain application data flow.  
Immutable approaches prevent accidental modifications while observer  
patterns enable reactive programming with automatic updates.  

## Array-based data structures

Implementing common data structures using PHP arrays as the foundation.  

```php
<?php

// Stack implementation
class Stack {
    private $items = [];
    
    public function push($item) {
        array_push($this->items, $item);
    }
    
    public function pop() {
        return array_pop($this->items);
    }
    
    public function peek() {
        return end($this->items);
    }
    
    public function isEmpty() {
        return empty($this->items);
    }
    
    public function size() {
        return count($this->items);
    }
}

// Queue implementation
class Queue {
    private $items = [];
    
    public function enqueue($item) {
        array_push($this->items, $item);
    }
    
    public function dequeue() {
        return array_shift($this->items);
    }
    
    public function front() {
        return reset($this->items);
    }
    
    public function isEmpty() {
        return empty($this->items);
    }
}

// Priority Queue
class PriorityQueue {
    private $items = [];
    
    public function enqueue($item, $priority) {
        $this->items[] = ['item' => $item, 'priority' => $priority];
        usort($this->items, fn($a, $b) => $b['priority'] <=> $a['priority']);
    }
    
    public function dequeue() {
        return array_shift($this->items)['item'] ?? null;
    }
}

// Test the data structures
$stack = new Stack();
$stack->push("first");
$stack->push("second");
echo $stack->pop() . "\n";  // second

$queue = new Queue();
$queue->enqueue("task1");
$queue->enqueue("task2");
echo $queue->dequeue() . "\n";  // task1

$pq = new PriorityQueue();
$pq->enqueue("low priority", 1);
$pq->enqueue("high priority", 10);
$pq->enqueue("medium priority", 5);
echo $pq->dequeue() . "\n";  // high priority

// Binary search on sorted array
function binarySearch($array, $target) {
    $left = 0;
    $right = count($array) - 1;
    
    while ($left <= $right) {
        $mid = intval(($left + $right) / 2);
        if ($array[$mid] === $target) {
            return $mid;
        } elseif ($array[$mid] < $target) {
            $left = $mid + 1;
        } else {
            $right = $mid - 1;
        }
    }
    return -1;
}

$sorted = [1, 3, 5, 7, 9, 11, 13, 15];
echo "Found 7 at index: " . binarySearch($sorted, 7) . "\n";  // 3
```

Array-based data structures provide efficient implementations of stacks,  
queues, and priority queues. These fundamental structures support  
algorithms and can be optimized for specific use cases and performance  
requirements.  