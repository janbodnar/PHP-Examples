# Expressions

This chapter covers 25 comprehensive PHP expression examples demonstrating  
various ways to create, combine, and evaluate expressions in modern PHP.  
These examples progress from basic arithmetic and string operations to  
advanced patterns like closures, generators, and pattern matching using  
PHP 8.4 features. Each example includes clear explanations to help you  
understand the underlying concepts and apply them effectively in your code.  

## Basic arithmetic expressions

Fundamental mathematical operations and calculations.  

```php
<?php

$a = 10;
$b = 3;

// Basic arithmetic operations
$sum = $a + $b;           // 13 (addition)
$difference = $a - $b;    // 7 (subtraction)  
$product = $a * $b;       // 30 (multiplication)
$quotient = $a / $b;      // 3.3333333333333 (division)
$remainder = $a % $b;     // 1 (modulo)
$power = $a ** $b;        // 1000 (exponentiation)

echo "Sum: $sum\n";
echo "Difference: $difference\n";
echo "Product: $product\n";
echo "Quotient: $quotient\n";
echo "Remainder: $remainder\n";
echo "Power: $power\n";
```

Arithmetic expressions form the foundation of mathematical calculations  
in PHP. The division operator always returns a float, while the modulo  
operator returns the remainder. Exponentiation uses the `**` operator  
introduced in PHP 5.6.  

## String expressions and concatenation

Building and manipulating text through string operations.  

```php
<?php

$first_name = "Alice";
$last_name = "Smith";
$age = 30;

// String concatenation with dot operator
$full_name = $first_name . " " . $last_name;
echo "Full name: $full_name\n";

// String interpolation in double quotes
$greeting = "Hello there, $first_name!";
echo "$greeting\n";

// Complex string expressions
$message = "Welcome " . $full_name . ", you are " . $age . " years old.";
echo "$message\n";

// String repetition and manipulation
$border = str_repeat("-", 20);
$formatted = $border . "\n" . strtoupper($full_name) . "\n" . $border;
echo "$formatted\n";
```

String expressions combine text using the concatenation operator `.` or  
variable interpolation within double quotes. PHP automatically converts  
numeric values to strings when used in string contexts, making mixed  
expressions intuitive and flexible.  

## Variable assignment expressions

Assigning values and chaining assignments in expressions.  

```php
<?php

// Basic assignment
$x = 5;
$y = $x;  // Assignment expression returns the assigned value

// Chained assignments
$a = $b = $c = 10;
echo "a: $a, b: $b, c: $c\n";

// Assignment within expressions
$result = ($temp = 25) * 2;  // temp gets 25, result gets 50
echo "Temperature: $temp°C, Doubled: $result\n";

// Reference assignment
$original = [1, 2, 3];
$reference = &$original;  // $reference points to same array
$reference[] = 4;
print_r($original);  // Shows [1, 2, 3, 4]

// Null coalescing assignment (PHP 7.4+)
$value = null;
$value ??= "default";  // Only assigns if $value is null
echo "Value: $value\n";
```

Assignment expressions not only set values but also return the assigned  
value, enabling chained assignments and assignments within larger  
expressions. Reference assignments create aliases to the same memory  
location rather than copying values.  

## Comparison expressions

Evaluating relationships between values using comparison operators.  

```php
<?php

$score1 = 85;
$score2 = 92;
$name1 = "Alice";
$name2 = "alice";

// Numeric comparisons
echo ($score1 > $score2) ? "Score1 higher" : "Score2 higher";
echo "\n";

echo ($score1 >= 80) ? "Passing grade" : "Failing grade";
echo "\n";

// String comparisons (case-sensitive)
echo ($name1 === $name2) ? "Names identical" : "Names different";
echo "\n";

// Loose vs strict comparison
$number = "42";
echo ($number == 42) ? "Loose equal" : "Not equal";    // true
echo "\n";
echo ($number === 42) ? "Strict equal" : "Not equal";  // false
echo "\n";

// Spaceship operator (PHP 7.0+)
$comparison = $score1 <=> $score2;  // Returns -1, 0, or 1
echo "Comparison result: $comparison\n";
```

Comparison expressions evaluate relationships between values, returning  
boolean results. The spaceship operator `<=>` provides three-way  
comparison, returning -1 for less than, 0 for equal, and 1 for greater  
than comparisons.  

## Logical expressions

Combining boolean values with logical operators for complex conditions.  

```php
<?php

$age = 25;
$has_license = true;
$has_insurance = true;
$is_weekend = false;

// Basic logical operations
$can_drive = $age >= 18 && $has_license && $has_insurance;
echo $can_drive ? "Can drive legally\n" : "Cannot drive\n";

// OR conditions
$can_relax = $is_weekend || $age >= 65;
echo $can_relax ? "Time to relax\n" : "Keep working\n";

// NOT operator
$needs_license = !$has_license;
echo $needs_license ? "Get a license\n" : "License OK\n";

// Short-circuit evaluation
$result = false && expensive_function();  // expensive_function() not called
$result2 = true || expensive_function();  // expensive_function() not called

// Mixed logical expressions
$eligible = ($age >= 21 && $age <= 65) && ($has_license || $has_insurance);
echo $eligible ? "Eligible for service\n" : "Not eligible\n";

function expensive_function() {
    echo "This won't be called due to short-circuit\n";
    return true;
}
```

Logical expressions combine boolean values using AND (`&&`), OR (`||`),  
and NOT (`!`) operators. PHP uses short-circuit evaluation, meaning the  
second operand is only evaluated if necessary, which can improve  
performance and prevent errors.  

## Bitwise expressions

Manipulating individual bits in integer values.  

```php
<?php

$a = 12;  // Binary: 1100
$b = 5;   // Binary: 0101

// Bitwise operations
$and_result = $a & $b;   // 4 (Binary: 0100)
$or_result = $a | $b;    // 13 (Binary: 1101)
$xor_result = $a ^ $b;   // 9 (Binary: 1001)
$not_result = ~$a;       // -13 (Binary: ...11110011)

echo "AND ($a & $b): $and_result\n";
echo "OR ($a | $b): $or_result\n";
echo "XOR ($a ^ $b): $xor_result\n";
echo "NOT (~$a): $not_result\n";

// Bit shifting
$left_shift = $a << 2;   // 48 (multiply by 4)
$right_shift = $a >> 1;  // 6 (divide by 2)

echo "Left shift ($a << 2): $left_shift\n";
echo "Right shift ($a >> 1): $right_shift\n";

// Practical example: permissions system
$read = 1;    // Binary: 001
$write = 2;   // Binary: 010  
$execute = 4; // Binary: 100

$permissions = $read | $write | $execute;  // 7 (all permissions)
$has_write = ($permissions & $write) !== 0;
echo $has_write ? "Has write permission\n" : "No write permission\n";
```

Bitwise expressions operate on individual bits of integer values. They're  
commonly used for flags, permissions systems, and low-level operations.  
Left shift multiplies by powers of 2, while right shift divides by  
powers of 2.  

## Ternary and null coalescing expressions

Concise conditional expressions for common patterns.  

```php
<?php

$score = 85;
$username = null;
$config = [];

// Ternary operator
$grade = ($score >= 90) ? "A" : (($score >= 80) ? "B" : "C");
echo "Grade: $grade\n";

// Short ternary (Elvis operator)
$display_name = $username ?: "Guest";
echo "Welcome, $display_name!\n";

// Null coalescing operator (PHP 7.0+)
$theme = $config['theme'] ?? 'default';
echo "Theme: $theme\n";

// Chained null coalescing
$value = $config['primary'] ?? $config['fallback'] ?? 'default';
echo "Selected value: $value\n";

// Null coalescing assignment (PHP 7.4+)
$settings = [];
$settings['debug'] ??= false;  // Only assign if key doesn't exist
$settings['timeout'] ??= 30;

print_r($settings);

// Complex ternary expressions
$status = ($score >= 90) ? "Excellent" : 
          (($score >= 70) ? "Good" : 
          (($score >= 50) ? "Average" : "Poor"));
echo "Status: $status\n";
```

Ternary expressions provide concise conditional assignment. The null  
coalescing operator `??` checks for null values specifically, while the  
Elvis operator `?:` checks for falsy values. These operators help write  
cleaner code for common conditional patterns.  

## Array expressions

Creating, accessing, and manipulating arrays through expressions.  

```php
<?php

// Array creation expressions
$numbers = [1, 2, 3, 4, 5];
$mixed = [10, "hello", 3.14, true];
$associative = ['name' => 'Alice', 'age' => 30, 'city' => 'New York'];

// Array access expressions
$first = $numbers[0];
$last = $numbers[count($numbers) - 1];
$name = $associative['name'];

echo "First: $first, Last: $last, Name: $name\n";

// Array modification expressions
$numbers[] = 6;  // Append element
$associative['country'] = 'USA';  // Add key-value pair

// Array spread expressions (PHP 7.4+)
$more_numbers = [6, 7, 8];
$combined = [...$numbers, ...$more_numbers];
print_r($combined);

// Array destructuring expressions
[$a, $b, $c] = [10, 20, 30];
echo "a: $a, b: $b, c: $c\n";

// Associative array destructuring
['name' => $person_name, 'age' => $person_age] = $associative;
echo "Person: $person_name, Age: $person_age\n";

// Complex array expressions
$matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];
$diagonal = $matrix[1][1];  // Access nested element
echo "Center element: $diagonal\n";
```

Array expressions encompass creation, access, modification, and  
destructuring operations. The spread operator allows combining arrays  
efficiently, while destructuring provides elegant ways to extract  
values into variables.  

## Object expressions

Working with objects, properties, and method calls.  

```php
<?php

// Object creation expressions
class Person {
    public string $name;
    public int $age;
    
    public function __construct(string $name, int $age) {
        $this->name = $name;
        $this->age = $age;
    }
    
    public function greet(): string {
        return "Hello there, I'm {$this->name}!";
    }
    
    public function getAge(): int {
        return $this->age;
    }
}

// Object instantiation
$person = new Person("Alice", 30);

// Property access expressions
$name = $person->name;
$age = $person->age;

echo "Name: $name, Age: $age\n";

// Method call expressions
$greeting = $person->greet();
echo "$greeting\n";

// Chained method calls
class Calculator {
    private float $value = 0;
    
    public function add(float $n): self {
        $this->value += $n;
        return $this;
    }
    
    public function multiply(float $n): self {
        $this->value *= $n;
        return $this;
    }
    
    public function getValue(): float {
        return $this->value;
    }
}

$result = (new Calculator())->add(10)->multiply(2)->getValue();
echo "Calculation result: $result\n";

// Object property expressions with complex access
$data = (object) ['user' => (object) ['profile' => (object) ['email' => 'alice@example.com']]];
$email = $data->user->profile->email;
echo "Email: $email\n";
```

Object expressions include instantiation, property access, method calls,  
and method chaining. The arrow operator `->` is used for both property  
access and method invocation, enabling fluent interfaces and complex  
object manipulations.  

## Function call expressions

Invoking functions and working with their return values.  

```php
<?php

// Basic function calls
function add(int $a, int $b): int {
    return $a + $b;
}

$sum = add(5, 3);
echo "Sum: $sum\n";

// Function calls in expressions
$result = add(10, 20) * 2;
echo "Result: $result\n";

// Variable functions
$function_name = 'strtoupper';
$uppercase = $function_name('hello there');
echo "Uppercase: $uppercase\n";

// Anonymous function expressions
$multiply = function(int $a, int $b): int {
    return $a * $b;
};

$product = $multiply(4, 5);
echo "Product: $product\n";

// Arrow function expressions (PHP 7.4+)
$square = fn($x) => $x * $x;
$squared = $square(6);
echo "Squared: $squared\n";

// Higher-order function expressions
$numbers = [1, 2, 3, 4, 5];
$doubled = array_map(fn($n) => $n * 2, $numbers);
$filtered = array_filter($numbers, fn($n) => $n > 3);

print_r($doubled);
print_r($filtered);

// Nested function calls
$text = "hello there world";
$processed = ucwords(str_replace("there", "beautiful", $text));
echo "Processed: $processed\n";
```

Function call expressions invoke functions and use their return values  
in larger expressions. PHP supports variable functions, anonymous  
functions, arrow functions, and higher-order functions for functional  
programming patterns.  

## Complex nested expressions

Combining multiple expression types for sophisticated operations.  

```php
<?php

$users = [
    ['name' => 'Alice', 'age' => 30, 'score' => 85],
    ['name' => 'Bob', 'age' => 25, 'score' => 92],
    ['name' => 'Charlie', 'age' => 35, 'score' => 78]
];

// Complex filtering and transformation
$qualified_users = array_filter(
    array_map(function($user) {
        return [
            'name' => strtoupper($user['name']),
            'qualified' => $user['age'] >= 25 && $user['score'] >= 80,
            'category' => $user['score'] >= 90 ? 'excellent' : 
                         ($user['score'] >= 80 ? 'good' : 'average')
        ];
    }, $users),
    fn($user) => $user['qualified']
);

print_r($qualified_users);

// Mathematical expression with conditionals
$x = 5;
$y = 3;
$complex_result = ($x > $y ? $x ** 2 : $y ** 2) + 
                  (($x + $y) % 2 === 0 ? 10 : 5);
echo "Complex result: $complex_result\n";

// String manipulation chain
$text = "  Hello There, Beautiful World!  ";
$cleaned = trim(strtolower(str_replace(['beautiful', ','], ['amazing', ''], $text)));
echo "Cleaned: '$cleaned'\n";

// Nested array operations
$data = [
    'users' => [
        ['profile' => ['email' => 'alice@example.com', 'active' => true]],
        ['profile' => ['email' => 'bob@example.com', 'active' => false]]
    ]
];

$active_emails = array_map(
    fn($user) => $user['profile']['email'],
    array_filter($data['users'], fn($user) => $user['profile']['active'])
);

print_r($active_emails);
```

Complex nested expressions combine multiple operators, function calls,  
and control structures. While powerful, they should be used judiciously  
to maintain code readability. Breaking complex expressions into smaller  
parts often improves maintainability.  

## Type casting expressions

Converting between different data types explicitly.  

```php
<?php

$number_string = "42";
$float_string = "3.14";
$boolean_string = "true";

// Basic type casting
$integer = (int) $number_string;        // 42
$float = (float) $float_string;         // 3.14
$string = (string) $integer;            // "42"
$boolean = (bool) $number_string;       // true

echo "Integer: $integer\n";
echo "Float: $float\n";
echo "String: '$string'\n";
echo "Boolean: " . ($boolean ? 'true' : 'false') . "\n";

// Array and object casting
$array_data = (array) "hello";          // ["hello"]
$object_data = (object) ['a' => 1, 'b' => 2];

print_r($array_data);
print_r($object_data);

// Type casting in expressions
$price = "19.99";
$quantity = "5";
$total = (float) $price * (int) $quantity;
echo "Total: $total\n";

// Null casting behavior
$null_value = null;
$cast_results = [
    'int' => (int) $null_value,      // 0
    'float' => (float) $null_value,  // 0.0
    'string' => (string) $null_value, // ""
    'bool' => (bool) $null_value,    // false
    'array' => (array) $null_value   // []
];

print_r($cast_results);

// Advanced casting scenarios
$mixed_array = ["10", "20.5", "30"];
$sum = array_sum(array_map('floatval', $mixed_array));
echo "Sum of casted values: $sum\n";
```

Type casting expressions explicitly convert values between different  
types. PHP provides cast operators for all major types, and also  
functions like `intval()`, `floatval()`, and `strval()` for type  
conversion with additional formatting options.  

## Assignment operators

Combining assignment with arithmetic and other operations.  

```php
<?php

$counter = 10;
$text = "Hello";
$total = 100;

// Arithmetic assignment operators
$counter += 5;    // Equivalent to: $counter = $counter + 5
echo "Counter after +=: $counter\n";

$counter -= 3;    // Equivalent to: $counter = $counter - 3
echo "Counter after -=: $counter\n";

$counter *= 2;    // Equivalent to: $counter = $counter * 2
echo "Counter after *=: $counter\n";

$counter /= 4;    // Equivalent to: $counter = $counter / 4
echo "Counter after /=: $counter\n";

$counter %= 3;    // Equivalent to: $counter = $counter % 3
echo "Counter after %=: $counter\n";

// String assignment operator
$text .= " World";  // Equivalent to: $text = $text . " World"
echo "Text after .=: $text\n";

// Bitwise assignment operators
$flags = 7;  // Binary: 111
$flags &= 5; // Binary: 101, Result: 101 (5)
echo "Flags after &=: $flags\n";

$flags |= 2; // Binary: 010, Result: 111 (7)
echo "Flags after |=: $flags\n";

// Null coalescing assignment (PHP 7.4+)
$config = ['theme' => 'dark'];
$config['debug'] ??= true;     // Only assigns if key doesn't exist
$config['theme'] ??= 'light';  // Won't assign, key exists

print_r($config);

// Complex assignment expressions
$data = [1, 2, 3];
$data[count($data)] = ($data[0] += 10);  // Add 10 to first element, append it
print_r($data);
```

Assignment operators combine assignment with other operations, providing  
concise syntax for common patterns. These operators are particularly  
useful in loops, counters, and accumulator patterns where values are  
repeatedly modified.  

## Increment and decrement expressions

Pre and post increment/decrement operations.  

```php
<?php

$a = 5;
$b = 5;

echo "Initial values - a: $a, b: $b\n";

// Pre-increment/decrement (modify then return)
$pre_increment = ++$a;   // $a becomes 6, returns 6
$pre_decrement = --$b;   // $b becomes 4, returns 4

echo "Pre-increment result: $pre_increment, a is now: $a\n";
echo "Pre-decrement result: $pre_decrement, b is now: $b\n";

$c = 5;
$d = 5;

// Post-increment/decrement (return then modify)
$post_increment = $c++;  // returns 5, then $c becomes 6
$post_decrement = $d--;  // returns 5, then $d becomes 4

echo "Post-increment result: $post_increment, c is now: $c\n";
echo "Post-decrement result: $post_decrement, d is now: $d\n";

// Increment/decrement in expressions
$counter = 0;
$values = [];

$values[] = $counter++;  // Add 0, then increment
$values[] = ++$counter;  // Increment to 2, then add 2
$values[] = $counter--;  // Add 2, then decrement  
$values[] = --$counter;  // Decrement to 0, then add 0

print_r($values);

// String increment (PHP feature)
$string = 'A';
echo "String increment: ";
for ($i = 0; $i < 5; $i++) {
    echo $string++ . " ";  // A B C D E
}
echo "\n";

// Practical example: loop with increment
$items = ['apple', 'banana', 'cherry'];
$i = 0;
while ($i < count($items)) {
    echo "Item " . ($i + 1) . ": " . $items[$i++] . "\n";
}
```

Increment and decrement expressions modify variables by 1 and return  
either the original or modified value. Pre-increment/decrement modify  
first then return, while post-increment/decrement return first then  
modify. PHP uniquely supports string increment operations.  

## Pattern matching expressions

Using match expressions for pattern-based control flow.  

```php
<?php

$grade = 'B';
$status_code = 404;
$user_type = 'admin';

// Basic match expression
$grade_description = match($grade) {
    'A' => 'Excellent',
    'B' => 'Good',
    'C' => 'Average',
    'D' => 'Below Average',
    'F' => 'Failing',
    default => 'Invalid Grade'
};

echo "Grade $grade: $grade_description\n";

// Match with multiple values
$http_message = match($status_code) {
    200 => 'OK',
    201, 202 => 'Created/Accepted',
    400, 401, 403 => 'Client Error',
    404 => 'Not Found',
    500, 502, 503 => 'Server Error',
    default => 'Unknown Status'
};

echo "Status $status_code: $http_message\n";

// Match with expressions
$access_level = match($user_type) {
    'admin' => 10,
    'moderator' => 5,
    'user' => 1,
    default => 0
};

$can_delete = match(true) {
    $access_level >= 10 => 'Full access',
    $access_level >= 5 => 'Limited access',
    $access_level >= 1 => 'Read only',
    default => 'No access'
};

echo "User type '$user_type' has: $can_delete\n";

// Complex match with function calls
function get_file_type(string $extension): string {
    return match($extension) {
        'jpg', 'jpeg', 'png', 'gif' => 'image',
        'mp4', 'avi', 'mkv' => 'video',
        'mp3', 'wav', 'ogg' => 'audio',
        'pdf', 'doc', 'docx' => 'document',
        default => 'unknown'
    };
}

$files = ['photo.jpg', 'movie.mp4', 'song.mp3', 'report.pdf'];
foreach ($files as $file) {
    $extension = pathinfo($file, PATHINFO_EXTENSION);
    $type = get_file_type($extension);
    echo "$file is a $type file\n";
}
```

Match expressions provide pattern-based branching with strict comparison  
and exhaustive checking. They're more concise than switch statements  
and can return values directly, making them ideal for functional  
programming patterns.  

## Lambda and closure expressions

Anonymous functions and closures for functional programming.  

```php
<?php

$numbers = [1, 2, 3, 4, 5];

// Basic lambda expressions
$square = function($x) {
    return $x * $x;
};

$doubled = array_map(function($n) {
    return $n * 2;
}, $numbers);

print_r($doubled);

// Arrow functions (shorter syntax)
$cubed = array_map(fn($n) => $n ** 3, $numbers);
print_r($cubed);

// Closures with variable binding
$multiplier = 3;
$multiply_by_factor = function($n) use ($multiplier) {
    return $n * $multiplier;
};

$multiplied = array_map($multiply_by_factor, $numbers);
print_r($multiplied);

// Closure with reference binding
$counter = 0;
$increment = function() use (&$counter) {
    return ++$counter;
};

echo "Count: " . $increment() . "\n";  // 1
echo "Count: " . $increment() . "\n";  // 2
echo "Count: " . $increment() . "\n";  // 3

// Higher-order functions
function create_adder(int $amount): callable {
    return fn($n) => $n + $amount;
}

$add_ten = create_adder(10);
$result = $add_ten(5);  // 15
echo "Add ten result: $result\n";

// Complex functional composition
$pipeline = function($data) {
    return array_reduce([
        fn($arr) => array_filter($arr, fn($n) => $n > 2),
        fn($arr) => array_map(fn($n) => $n * 2, $arr),
        fn($arr) => array_sum($arr)
    ], fn($carry, $func) => $func($carry), $data);
};

$result = $pipeline([1, 2, 3, 4, 5]);  // (3+4+5)*2 = 24
echo "Pipeline result: $result\n";
```

Lambda and closure expressions enable functional programming patterns  
in PHP. Closures can capture variables from their surrounding scope,  
while arrow functions provide concise syntax for simple operations.  
They're essential for array processing and callback-based APIs.  

## Generator expressions

Creating generators for memory-efficient iteration.  

```php
<?php

// Basic generator function
function count_up_to(int $max): Generator {
    for ($i = 1; $i <= $max; $i++) {
        yield $i;
    }
}

echo "Counting up to 5:\n";
foreach (count_up_to(5) as $number) {
    echo "$number ";
}
echo "\n";

// Generator with keys
function get_range(int $start, int $end): Generator {
    for ($i = $start; $i <= $end; $i++) {
        yield "number_$i" => $i;
    }
}

echo "Range with keys:\n";
foreach (get_range(3, 6) as $key => $value) {
    echo "$key: $value\n";
}

// Generator expressions for large datasets
function fibonacci(): Generator {
    $a = 0;
    $b = 1;
    
    while (true) {
        yield $a;
        [$a, $b] = [$b, $a + $b];
    }
}

echo "First 10 Fibonacci numbers:\n";
$count = 0;
foreach (fibonacci() as $fib) {
    if ($count++ >= 10) break;
    echo "$fib ";
}
echo "\n";

// Generator delegation with yield from
function inner_generator(): Generator {
    yield 1;
    yield 2;
    yield 3;
}

function outer_generator(): Generator {
    yield 0;
    yield from inner_generator();
    yield 4;
}

echo "Delegated generator:\n";
foreach (outer_generator() as $value) {
    echo "$value ";
}
echo "\n";

// Practical example: file reading generator
function read_lines(string $filename): Generator {
    $file = fopen($filename, 'r');
    if (!$file) {
        throw new RuntimeException("Cannot open file: $filename");
    }
    
    try {
        while (($line = fgets($file)) !== false) {
            yield rtrim($line, "\r\n");
        }
    } finally {
        fclose($file);
    }
}

// Create a sample file
$temp_file = '/tmp/sample.txt';
file_put_contents($temp_file, "Line 1\nLine 2\nLine 3\n");

echo "Reading file lines:\n";
foreach (read_lines($temp_file) as $line_number => $line) {
    echo "Line " . ($line_number + 1) . ": $line\n";
}

unlink($temp_file);  // Clean up
```

Generator expressions create iterators that produce values on-demand,  
providing memory-efficient alternatives to arrays for large datasets.  
They use the `yield` keyword to return values and can delegate to  
other generators using `yield from`.  

## Spread operator expressions

Unpacking arrays and argument lists with the spread operator.  

```php
<?php

// Array spreading
$fruits = ['apple', 'banana'];
$vegetables = ['carrot', 'broccoli'];
$more_fruits = ['orange', 'grape'];

$all_items = [...$fruits, ...$vegetables, ...$more_fruits];
print_r($all_items);

// Function argument spreading
function sum(int ...$numbers): int {
    return array_sum($numbers);
}

$values = [10, 20, 30];
$total = sum(...$values);  // Spread array as arguments
echo "Sum: $total\n";

// Mixed spreading
$additional = ['mango', 'kiwi'];
$combined = ['pear', ...$fruits, 'peach', ...$additional];
print_r($combined);

// Spreading in array literals
$config_base = ['debug' => false, 'timeout' => 30];
$config_dev = [...$config_base, 'debug' => true, 'log_level' => 'verbose'];
$config_prod = [...$config_base, 'timeout' => 60, 'cache' => true];

print_r($config_dev);
print_r($config_prod);

// Spreading with associative arrays (PHP 8.1+)
$user_base = ['name' => 'Alice', 'email' => 'alice@example.com'];
$user_extended = [...$user_base, 'role' => 'admin', 'active' => true];
print_r($user_extended);

// Generator spreading
function generate_numbers(): Generator {
    for ($i = 1; $i <= 3; $i++) {
        yield $i;
    }
}

$generated = [...generate_numbers()];
print_r($generated);

// Complex spreading example
function merge_configs(array ...$configs): array {
    return array_merge(...$configs);
}

$result = merge_configs(
    ['a' => 1, 'b' => 2],
    ['c' => 3, 'd' => 4],
    ['e' => 5]
);
print_r($result);
```

The spread operator `...` unpacks arrays and iterables, enabling  
efficient array combination and function argument passing. It works  
with any iterable, including generators, and provides a clean  
alternative to functions like `array_merge()`.  

## Null-safe operator expressions

Safe navigation through potentially null object chains.  

```php
<?php

class Address {
    public function __construct(
        public string $street,
        public string $city,
        public string $country
    ) {}
    
    public function getFullAddress(): string {
        return "$this->street, $this->city, $this->country";
    }
}

class User {
    public function __construct(
        public string $name,
        public ?Address $address = null
    ) {}
    
    public function getAddress(): ?Address {
        return $this->address;
    }
}

$user_with_address = new User(
    'Alice',
    new Address('123 Main St', 'New York', 'USA')
);

$user_without_address = new User('Bob');

// Traditional null checking
$address1 = null;
if ($user_with_address->getAddress() !== null) {
    $address1 = $user_with_address->getAddress()->getFullAddress();
}
echo "Traditional way: " . ($address1 ?? 'No address') . "\n";

// Null-safe operator (PHP 8.0+)
$address2 = $user_with_address->getAddress()?->getFullAddress();
$address3 = $user_without_address->getAddress()?->getFullAddress();

echo "Null-safe with address: " . ($address2 ?? 'No address') . "\n";
echo "Null-safe without address: " . ($address3 ?? 'No address') . "\n";

// Chained null-safe operations
class Profile {
    public function __construct(public ?User $user = null) {}
    
    public function getUser(): ?User {
        return $this->user;
    }
}

$profile_with_user = new Profile($user_with_address);
$profile_without_user = new Profile();

$city1 = $profile_with_user->getUser()?->getAddress()?->city;
$city2 = $profile_without_user->getUser()?->getAddress()?->city;

echo "City from profile with user: " . ($city1 ?? 'Unknown') . "\n";
echo "City from profile without user: " . ($city2 ?? 'Unknown') . "\n";

// Null-safe with method calls and array access
class Data {
    public function __construct(public array $items = []) {}
    
    public function getItems(): array {
        return $this->items;
    }
    
    public function getFirst(): mixed {
        return $this->items[0] ?? null;
    }
}

$data = new Data(['item1', 'item2', 'item3']);
$empty_data = new Data();

// Combined null-safe and array operations
$first_item = $data->getFirst();
$empty_first = $empty_data->getFirst();

echo "First item: " . ($first_item ?? 'None') . "\n";
echo "Empty first: " . ($empty_first ?? 'None') . "\n";
```

The null-safe operator `?->` prevents fatal errors when navigating  
through object chains that might contain null values. It short-circuits  
the expression and returns null if any link in the chain is null,  
making code more robust and readable.  

## Static expressions

Working with static properties and methods in expressions.  

```php
<?php

class MathUtils {
    public static int $calculation_count = 0;
    private static array $cache = [];
    
    public static function add(int $a, int $b): int {
        self::$calculation_count++;
        return $a + $b;
    }
    
    public static function multiply(int $a, int $b): int {
        self::$calculation_count++;
        $key = "$a*$b";
        
        if (!isset(self::$cache[$key])) {
            self::$cache[$key] = $a * $b;
        }
        
        return self::$cache[$key];
    }
    
    public static function getCalculationCount(): int {
        return self::$calculation_count;
    }
    
    public static function clearCache(): void {
        self::$cache = [];
    }
}

// Static method calls in expressions
$sum = MathUtils::add(5, 3);
$product = MathUtils::multiply(4, 6);
$complex = MathUtils::add(MathUtils::multiply(2, 3), 4);

echo "Sum: $sum\n";
echo "Product: $product\n";
echo "Complex: $complex\n";
echo "Calculations performed: " . MathUtils::getCalculationCount() . "\n";

// Static property access
echo "Initial count: " . MathUtils::$calculation_count . "\n";

// Late static binding with static::
class BaseCounter {
    protected static int $count = 0;
    
    public static function increment(): int {
        return ++static::$count;
    }
    
    public static function getCount(): int {
        return static::$count;
    }
}

class CounterA extends BaseCounter {
    protected static int $count = 100;
}

class CounterB extends BaseCounter {
    protected static int $count = 200;
}

// Each class maintains its own static count
CounterA::increment();
CounterA::increment();
CounterB::increment();

echo "CounterA: " . CounterA::getCount() . "\n";  // 102
echo "CounterB: " . CounterB::getCount() . "\n";  // 201

// Static expressions with variable class names
$class_name = 'MathUtils';
$result = $class_name::add(10, 20);
echo "Variable class call: $result\n";

// Static constant expressions
class Constants {
    public const VERSION = '1.0.0';
    public const MAX_ITEMS = 100;
    
    public static function getInfo(): string {
        return 'Version: ' . self::VERSION . ', Max items: ' . self::MAX_ITEMS;
    }
}

echo Constants::getInfo() . "\n";
echo "Direct constant: " . Constants::VERSION . "\n";
```

Static expressions access class-level properties and methods without  
requiring object instantiation. They use the `::` operator and support  
late static binding with the `static::` keyword, enabling polymorphic  
behavior in inheritance hierarchies.  

## Class expressions

Anonymous classes and class-based expressions.  

```php
<?php

// Anonymous class expressions
$logger = new class {
    private array $logs = [];
    
    public function log(string $message): void {
        $this->logs[] = date('Y-m-d H:i:s') . ': ' . $message;
    }
    
    public function getLogs(): array {
        return $this->logs;
    }
    
    public function getLastLog(): ?string {
        return end($this->logs) ?: null;
    }
};

$logger->log('Application started');
$logger->log('User logged in');

echo "Last log: " . $logger->getLastLog() . "\n";
print_r($logger->getLogs());

// Anonymous class with interface implementation
interface Formatter {
    public function format(string $text): string;
}

$json_formatter = new class implements Formatter {
    public function format(string $text): string {
        return json_encode(['message' => $text]);
    }
};

$html_formatter = new class implements Formatter {
    public function format(string $text): string {
        return "<p>" . htmlspecialchars($text) . "</p>";
    }
};

$message = "Hello there!";
echo "JSON: " . $json_formatter->format($message) . "\n";
echo "HTML: " . $html_formatter->format($message) . "\n";

// Anonymous class with constructor
$calculator = new class(10) {
    public function __construct(private int $base) {}
    
    public function add(int $value): int {
        return $this->base + $value;
    }
    
    public function multiply(int $value): int {
        return $this->base * $value;
    }
};

echo "Calculator add: " . $calculator->add(5) . "\n";
echo "Calculator multiply: " . $calculator->multiply(3) . "\n";

// Factory pattern with anonymous classes
function create_handler(string $type): object {
    return match($type) {
        'email' => new class {
            public function handle(string $message): string {
                return "Email sent: $message";
            }
        },
        'sms' => new class {
            public function handle(string $message): string {
                return "SMS sent: $message";
            }
        },
        'push' => new class {
            public function handle(string $message): string {
                return "Push notification sent: $message";
            }
        },
        default => new class {
            public function handle(string $message): string {
                return "Default handler: $message";
            }
        }
    };
}

$email_handler = create_handler('email');
$sms_handler = create_handler('sms');

echo $email_handler->handle('Welcome!') . "\n";
echo $sms_handler->handle('Code: 12345') . "\n";

// Class expressions in array context
$processors = [
    'uppercase' => new class {
        public function process(string $text): string {
            return strtoupper($text);
        }
    },
    'reverse' => new class {
        public function process(string $text): string {
            return strrev($text);
        }
    }
];

$text = "hello world";
foreach ($processors as $name => $processor) {
    echo "$name: " . $processor->process($text) . "\n";
}
```

Class expressions create anonymous classes inline, useful for one-off  
implementations, callbacks, and factory patterns. They can implement  
interfaces, extend classes, and have constructors just like named  
classes, providing flexibility for rapid prototyping.  

## Regular expression expressions

Pattern matching and text manipulation with regex.  

```php
<?php

$email = "alice@example.com";
$phone = "123-456-7890";
$text = "Visit https://example.com for more info!";
$date_string = "Today is 2024-03-15 and tomorrow is 2024-03-16";

// Basic pattern matching
$email_valid = preg_match('/^[^\s@]+@[^\s@]+\.[^\s@]+$/', $email);
echo $email_valid ? "Valid email\n" : "Invalid email\n";

// Extracting matches
preg_match('/(\d{3})-(\d{3})-(\d{4})/', $phone, $matches);
if ($matches) {
    [$full, $area, $exchange, $number] = $matches;
    echo "Phone parts: Area=$area, Exchange=$exchange, Number=$number\n";
}

// Finding all matches
preg_match_all('/https?:\/\/[^\s]+/', $text, $urls);
if ($urls[0]) {
    echo "URLs found: " . implode(', ', $urls[0]) . "\n";
}

// Pattern replacement
$cleaned_phone = preg_replace('/[^\d]/', '', $phone);
echo "Cleaned phone: $cleaned_phone\n";

// Complex replacement with callbacks
$formatted_text = preg_replace_callback(
    '/(\d{4})-(\d{2})-(\d{2})/',
    function($matches) {
        [$full, $year, $month, $day] = $matches;
        return "$month/$day/$year";
    },
    $date_string
);
echo "Formatted dates: $formatted_text\n";

// Splitting with regex
$csv_line = "apple,banana,\"cherry, red\",date";
$parts = preg_split('/,(?=(?:[^"]*"[^"]*")*[^"]*$)/', $csv_line);
print_r($parts);

// Validation patterns
$patterns = [
    'username' => '/^[a-zA-Z0-9_]{3,20}$/',
    'password' => '/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d@$!%*?&]{8,}$/',
    'zip_code' => '/^\d{5}(-\d{4})?$/',
    'credit_card' => '/^\d{4}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}$/'
];

$test_data = [
    'username' => 'alice_123',
    'password' => 'SecurePass123',
    'zip_code' => '12345-6789',
    'credit_card' => '1234 5678 9012 3456'
];

foreach ($test_data as $field => $value) {
    $is_valid = preg_match($patterns[$field], $value);
    echo "$field '$value': " . ($is_valid ? "Valid" : "Invalid") . "\n";
}

// Advanced regex with named groups
$log_line = "2024-03-15 14:30:25 ERROR User login failed for alice@example.com";
$log_pattern = '/(?P<date>\d{4}-\d{2}-\d{2}) (?P<time>\d{2}:\d{2}:\d{2}) (?P<level>\w+) (?P<message>.*)/';

if (preg_match($log_pattern, $log_line, $matches)) {
    echo "Log entry:\n";
    echo "  Date: {$matches['date']}\n";
    echo "  Time: {$matches['time']}\n";
    echo "  Level: {$matches['level']}\n";
    echo "  Message: {$matches['message']}\n";
}
```

Regular expression expressions provide powerful pattern matching and  
text manipulation capabilities. PHP's PCRE functions support advanced  
features like named groups, callbacks, and complex patterns for  
validation, extraction, and transformation tasks.  

## Array destructuring expressions

Extracting values from arrays into individual variables.  

```php
<?php

// Basic array destructuring
$coordinates = [10, 20];
[$x, $y] = $coordinates;
echo "X: $x, Y: $y\n";

// Skipping elements
$colors = ['red', 'green', 'blue', 'yellow'];
[$primary, , $tertiary] = $colors;  // Skip green
echo "Primary: $primary, Tertiary: $tertiary\n";

// Nested array destructuring
$person = ['Alice', 30, ['123 Main St', 'New York', 'USA']];
[$name, $age, [$street, $city, $country]] = $person;
echo "Name: $name, Age: $age, Address: $street, $city, $country\n";

// Associative array destructuring
$user = ['name' => 'Bob', 'email' => 'bob@example.com', 'role' => 'admin'];
['name' => $user_name, 'email' => $user_email] = $user;
echo "User: $user_name ($user_email)\n";

// Mixed keys with default values
$config = ['host' => 'localhost', 'port' => 3306];
['host' => $host, 'port' => $port, 'database' => $database = 'default'] = $config;
echo "Connection: $host:$port/$database\n";

// Destructuring in foreach loops
$users = [
    ['name' => 'Alice', 'score' => 85],
    ['name' => 'Bob', 'score' => 92],
    ['name' => 'Charlie', 'score' => 78]
];

foreach ($users as ['name' => $name, 'score' => $score]) {
    echo "$name scored $score points\n";
}

// Function parameter destructuring
function process_point(array $point): string {
    [$x, $y, $z = 0] = $point;  // Default z to 0 if not provided
    return "Point at ($x, $y, $z)";
}

echo process_point([5, 10]) . "\n";
echo process_point([1, 2, 3]) . "\n";

// Rest elements in destructuring
$numbers = [1, 2, 3, 4, 5, 6];
[$first, $second, ...$rest] = $numbers;
echo "First: $first, Second: $second, Rest: " . implode(', ', $rest) . "\n";

// Complex destructuring with validation
function parse_csv_row(string $csv): array {
    $parts = str_getcsv($csv);
    
    // Destructure with validation
    [$name, $age, $email, $phone = null] = array_pad($parts, 4, null);
    
    return [
        'name' => trim($name) ?: 'Unknown',
        'age' => (int)$age ?: 0,
        'email' => filter_var($email, FILTER_VALIDATE_EMAIL) ?: null,
        'phone' => $phone ? preg_replace('/[^\d]/', '', $phone) : null
    ];
}

$csv_data = [
    "Alice Johnson,30,alice@example.com,123-456-7890",
    "Bob Smith,25,invalid-email",
    "Charlie Brown,35,charlie@test.com"
];

foreach ($csv_data as $row) {
    $parsed = parse_csv_row($row);
    print_r($parsed);
}
```

Array destructuring expressions provide elegant syntax for extracting  
values from arrays into individual variables. They support nested  
structures, default values, rest elements, and work seamlessly with  
both indexed and associative arrays.  

## Complex mathematical expressions

Advanced mathematical calculations and formula expressions.  

```php
<?php

// Mathematical constants
define('GRAVITY', 9.81);
define('SPEED_OF_LIGHT', 299792458);

// Basic mathematical expressions
$radius = 5;
$area = pi() * pow($radius, 2);
$circumference = 2 * pi() * $radius;

echo "Circle (radius $radius):\n";
echo "  Area: " . round($area, 2) . "\n";
echo "  Circumference: " . round($circumference, 2) . "\n";

// Trigonometric expressions
$angle_degrees = 45;
$angle_radians = deg2rad($angle_degrees);

$sin_value = sin($angle_radians);
$cos_value = cos($angle_radians);
$tan_value = tan($angle_radians);

echo "Angle $angle_degrees degrees:\n";
echo "  Sin: " . round($sin_value, 4) . "\n";
echo "  Cos: " . round($cos_value, 4) . "\n";
echo "  Tan: " . round($tan_value, 4) . "\n";

// Logarithmic and exponential expressions
$base = 2;
$exponent = 8;
$logarithm = log($exponent, $base);
$natural_log = log($exponent);
$exp_result = exp($natural_log);

echo "Logarithms (base $base, value $exponent):\n";
echo "  Log base $base: $logarithm\n";
echo "  Natural log: " . round($natural_log, 4) . "\n";
echo "  e^ln($exponent): " . round($exp_result, 4) . "\n";

// Physics calculations
function calculate_kinetic_energy(float $mass, float $velocity): float {
    return 0.5 * $mass * pow($velocity, 2);
}

function calculate_potential_energy(float $mass, float $height): float {
    return $mass * GRAVITY * $height;
}

$mass = 10;  // kg
$velocity = 5;  // m/s
$height = 20;  // m

$kinetic = calculate_kinetic_energy($mass, $velocity);
$potential = calculate_potential_energy($mass, $height);
$total_energy = $kinetic + $potential;

echo "Physics calculations (mass: {$mass}kg):\n";
echo "  Kinetic energy (v={$velocity}m/s): {$kinetic}J\n";
echo "  Potential energy (h={$height}m): {$potential}J\n";
echo "  Total energy: {$total_energy}J\n";

// Financial calculations
function compound_interest(float $principal, float $rate, int $periods, int $time): float {
    return $principal * pow((1 + $rate / $periods), $periods * $time);
}

function present_value(float $future_value, float $rate, int $periods): float {
    return $future_value / pow((1 + $rate), $periods);
}

$principal = 1000;
$annual_rate = 0.05;  // 5%
$compounding_periods = 12;  // Monthly
$years = 10;

$compound_result = compound_interest($principal, $annual_rate, $compounding_periods, $years);
$present_val = present_value($compound_result, $annual_rate, $years);

echo "Financial calculations:\n";
echo "  Principal: $" . number_format($principal, 2) . "\n";
echo "  Compound interest (10 years): $" . number_format($compound_result, 2) . "\n";
echo "  Present value check: $" . number_format($present_val, 2) . "\n";

// Statistical calculations
$dataset = [12, 15, 18, 20, 22, 25, 28, 30, 32, 35];

$mean = array_sum($dataset) / count($dataset);
$variance = array_sum(array_map(fn($x) => pow($x - $mean, 2), $dataset)) / count($dataset);
$std_deviation = sqrt($variance);

sort($dataset);
$median = count($dataset) % 2 === 0 
    ? ($dataset[count($dataset) / 2 - 1] + $dataset[count($dataset) / 2]) / 2
    : $dataset[floor(count($dataset) / 2)];

echo "Statistical analysis:\n";
echo "  Dataset: " . implode(', ', $dataset) . "\n";
echo "  Mean: " . round($mean, 2) . "\n";
echo "  Median: $median\n";
echo "  Standard deviation: " . round($std_deviation, 2) . "\n";

// Complex number simulation (since PHP doesn't have native complex numbers)
class Complex {
    public function __construct(
        public float $real,
        public float $imaginary
    ) {}
    
    public function add(Complex $other): Complex {
        return new Complex(
            $this->real + $other->real,
            $this->imaginary + $other->imaginary
        );
    }
    
    public function multiply(Complex $other): Complex {
        return new Complex(
            $this->real * $other->real - $this->imaginary * $other->imaginary,
            $this->real * $other->imaginary + $this->imaginary * $other->real
        );
    }
    
    public function magnitude(): float {
        return sqrt($this->real ** 2 + $this->imaginary ** 2);
    }
    
    public function __toString(): string {
        $sign = $this->imaginary >= 0 ? '+' : '-';
        return $this->real . $sign . abs($this->imaginary) . 'i';
    }
}

$z1 = new Complex(3, 4);
$z2 = new Complex(1, -2);

$sum = $z1->add($z2);
$product = $z1->multiply($z2);

echo "Complex number operations:\n";
echo "  z1 = $z1, magnitude = " . round($z1->magnitude(), 2) . "\n";
echo "  z2 = $z2, magnitude = " . round($z2->magnitude(), 2) . "\n";
echo "  z1 + z2 = $sum\n";
echo "  z1 * z2 = $product\n";
```

Complex mathematical expressions encompass various domains including  
geometry, trigonometry, physics, finance, and statistics. PHP provides  
extensive mathematical functions and allows creation of custom classes  
for advanced mathematical concepts like complex numbers.