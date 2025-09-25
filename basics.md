# Basics

This chapter covers 30 fundamental PHP examples designed to help you get  
started with the language quickly. These examples demonstrate the most  
common PHP syntax and idioms, progressing from simple concepts to more  
sophisticated features. Each example includes clear explanations to help  
you understand the underlying concepts and apply them in your own code.  

## Hello World

The traditional first program in any language.  

```php
<?php

echo "Hello, there!" . PHP_EOL;
```

The `echo` statement outputs text. `PHP_EOL` provides a platform-  
independent newline. PHP scripts start with `<?php
` tag when mixing  
with HTML, though it's optional in pure PHP files.  

## Variables

PHP variables are prefixed with the dollar sign ($).  

```php
<?php

$name = "Alice";         // String variable
$colors = ["red", "green", "blue"];  // Indexed array
$person = ["name" => "Bob", "age" => 25];  // Associative array

echo $name . PHP_EOL;
echo $colors[1] . PHP_EOL;    // green
echo $person["name"] . PHP_EOL;  // Bob
```

All PHP variables use the `$` prefix regardless of type. Arrays can be  
indexed (numeric keys) or associative (string keys). PHP is  
dynamically typed, so variables can hold different types.  

## Basic data types

PHP's fundamental data types for different kinds of values.  

```php
<?php

$text = "Hello";         // string
$number = 42;            // int
$decimal = 3.14;         // float
$flag = true;            // bool
$nothing = null;         // null

var_dump($text);         // string(5) "Hello"
var_dump($number);       // int(42)
var_dump($decimal);      // float(3.14)
```

The `var_dump()` function shows detailed type and value information.  
PHP has dynamic typing but provides functions like `gettype()` and  
`is_*()` family for type checking when needed.  

## Array creation and access

Different ways to create and access array elements.  

```php
<?php

$fruits = ["apple", "banana", "cherry"];
$numbers = [1, 2, 3, 4, 5];
$mixed = ["text", 42, true];
$range = range(1, 10);

echo $fruits[0] . PHP_EOL;       // apple
echo $numbers[2] . PHP_EOL;      // 3
echo $range[9] . PHP_EOL;        // 10 (last element)
echo count($mixed) . PHP_EOL;    // 3 (number of elements)
```

Arrays can contain any type of values and use square bracket notation  
for both creation and access. Use `count()` to get array length and  
`range()` to create sequences of numbers.  

## Hash creation and access

Creating and working with key-value data structures.  

```php
<?php

$scores = ["Alice" => 95, "Bob" => 87, "Carol" => 92];
$config = ["host" => "localhost", "port" => 8080, "ssl" => true];

echo $scores["Alice"] . PHP_EOL;     // 95
echo $scores["Bob"] . PHP_EOL;       // 87
echo $config["host"] . PHP_EOL;      // localhost
print_r(array_keys($config));        // Array keys
```

Associative arrays (PHP's equivalent to hashes) store key-value pairs  
using the `=>` operator. Access values with square brackets and string  
keys. Use `array_keys()` and `array_values()` to get keys or values.  

## Arithmetic operations

Basic mathematical operations with numbers.  

```php
<?php

$a = 10;
$b = 3;

echo $a + $b . PHP_EOL;      // 13 (addition)
echo $a - $b . PHP_EOL;      // 7 (subtraction)
echo $a * $b . PHP_EOL;      // 30 (multiplication)
echo $a / $b . PHP_EOL;      // 3.3333333333333 (division)
echo intval($a / $b) . PHP_EOL;  // 3 (integer division)
echo $a % $b . PHP_EOL;      // 1 (modulo)
echo $a ** $b . PHP_EOL;     // 1000 (exponentiation)
```

PHP provides all standard arithmetic operators. Division `/` returns  
float by default; use `intval()` or `intdiv()` for integer division.  
The `**` operator (PHP 5.6+) performs exponentiation.  

## String operations

Common operations for working with text.  

```php
<?php

$str1 = "Hello";
$str2 = "World";

echo $str1 . " " . $str2 . PHP_EOL;      // Hello World (concatenation)
echo "$str1 $str2" . PHP_EOL;            // Hello World (interpolation)
echo str_repeat($str1, 3) . PHP_EOL;     // HelloHelloHello (repetition)
echo strlen($str1) . PHP_EOL;            // 5 (character count)
echo strtoupper($str1) . PHP_EOL;        // HELLO (uppercase)
echo strtolower($str2) . PHP_EOL;        // world (lowercase)
```

The `.` operator concatenates strings, `str_repeat()` repeats strings,  
and functions like `strtoupper()` transform case. String interpolation  
with `"$variable"` is often more readable than concatenation.  

## Comparison operators

Comparing values for equality and ordering.  

```php
<?php

$x = 10;
$y = 20;
$str = "hello";

echo ($x == $y) ? "true" : "false" . PHP_EOL;    // false (equality)
echo ($x != $y) ? "true" : "false" . PHP_EOL;    // true (inequality)
echo ($x < $y) ? "true" : "false" . PHP_EOL;     // true (less than)
echo ($x > $y) ? "true" : "false" . PHP_EOL;     // false (greater than)
echo ($str == "hello") ? "true" : "false" . PHP_EOL;  // true (equality)
echo ($str != "world") ? "true" : "false" . PHP_EOL;  // true (inequality)
```

Use `==` and `!=` for loose comparisons, `===` and `!==` for strict  
type and value comparisons. The `<`, `>`, `<=`, `>=` operators work  
for both numbers and strings (lexicographic order).  

## Logical operators

Combining boolean expressions with logical operations.  

```php
<?php

$a = true;
$b = false;
$x = 10;

echo ($a && $b) ? "true" : "false" . PHP_EOL;    // false (AND)
echo ($a || $b) ? "true" : "false" . PHP_EOL;    // true (OR)
echo (!$a) ? "true" : "false" . PHP_EOL;         // false (NOT)
echo ($a and $b) ? "true" : "false" . PHP_EOL;   // false (lower precedence)
echo ($a or $b) ? "true" : "false" . PHP_EOL;    // true (lower precedence)
echo ($x > 5 && $x < 15) ? "true" : "false" . PHP_EOL;  // true
```

Logical operators `&&`/`and`, `||`/`or`, and `!`/`not` combine boolean  
values. The word forms have lower precedence than symbolic forms,  
useful for assignment and control flow operations.  

## Conditional statements

Making decisions in your code with if/elseif/else.  

```php
<?php

$score = 85;

if ($score >= 90) {
    echo "Excellent!" . PHP_EOL;
} elseif ($score >= 70) {
    echo "Good job!" . PHP_EOL;
} else {
    echo "Keep trying!" . PHP_EOL;
}

// Ternary conditional
echo ($score > 80) ? "High score!" : "Try harder!" . PHP_EOL;
```

Conditional statements control program flow based on boolean  
expressions. The ternary operator `? :` provides a concise way for  
simple conditional assignments and output.  

## For loops

Iterating over sequences and collections.  

```php
<?php

// Loop over range
for ($i = 1; $i <= 5; $i++) {
    echo "Number: $i" . PHP_EOL;
}

// Loop over array
$colors = ["red", "green", "blue"];
foreach ($colors as $color) {
    echo "Color: $color" . PHP_EOL;
}

// Loop with index
foreach ($colors as $index => $value) {
    echo "$index: $value" . PHP_EOL;
}
```

For loops use C-style syntax with initialization, condition, and  
increment. The `foreach` loop iterates over arrays, optionally  
providing both keys and values for simultaneous access.  

## While loops

Repeating code while a condition is true.  

```php
<?php

$count = 0;
while ($count < 5) {
    echo "Count: $count" . PHP_EOL;
    $count++;
}

// Do-while loop (executes at least once)
$num = 10;
do {
    echo $num . PHP_EOL;
    $num -= 2;
} while ($num > 0);
```

While loops continue executing while the condition is true. Do-while  
loops execute the body at least once before checking the condition.  
Both increment/decrement variables to avoid infinite loops.  

## Loop control

Controlling loop execution with break and continue statements.  

```php
<?php

for ($i = 0; $i < 10; $i++) {
    if ($i === 5) {
        break;  // Exit loop completely
    }
    if ($i % 2 === 0) {
        continue;  // Skip to next iteration
    }
    echo "Odd number: $i" . PHP_EOL;
}

// Infinite loop with explicit exit
while (true) {
    $input = readline("Enter 'quit' to exit: ");
    if ($input === 'quit') {
        break;
    }
    echo "You entered: $input" . PHP_EOL;
}
```

The `break` statement exits loops early, while `continue` skips to  
the next iteration. These provide fine control over loop execution  
and are essential for interactive or conditional processing.  

## Ranges

Generating sequences of consecutive values.  

```php
<?php

$range1 = range(1, 10);         // 1 to 10 inclusive
$range2 = range(1, 10, 2);      // 1, 3, 5, 7, 9 (step of 2)
$range3 = range('a', 'z');      // letters a to z

print_r(array_slice($range1, 0, 5));  // First 5 elements
print_r($range2);                     // Odd numbers 1-9
echo $range3[0] . $range3[1] . $range3[2] . PHP_EOL;  // abc

// Use directly in loops
foreach (range(1, 3) as $n) {
    echo $n . PHP_EOL;
}
```

The `range()` function generates arrays of consecutive values between  
two endpoints. It supports numeric and alphabetic ranges, with  
optional step parameter for custom increments.  

## Basic functions

Creating reusable functions to organize code.  

```php
<?php

function greet($name) {
    return "Hello, $name!";
}

function add($a, $b) {
    return $a + $b;
}

function sayHello() {
    echo "Hello from function!" . PHP_EOL;
}

echo greet("Alice") . PHP_EOL;     // Hello, Alice!
echo add(5, 3) . PHP_EOL;          // 8
sayHello();                        // Hello from function!
```

Functions encapsulate reusable code and are defined with the  
`function` keyword. Parameters are listed in parentheses, and  
`return` statements provide output values to the caller.  

## Pattern matching with match

Elegant alternative to long if/elseif chains using PHP 8.0+ match.  

```php
<?php

$day = "Monday";

$message = match ($day) {
    "Monday" => "Start of work week",
    "Friday" => "TGIF!",
    "Saturday", "Sunday" => "Weekend!",
    default => "Regular day"
};
echo $message . PHP_EOL;

// Pattern matching with numbers
$num = 42;
$category = match (true) {
    $num >= 0 && $num <= 10 => "Small number",
    $num >= 11 && $num <= 100 => "Medium number",
    default => "Large number"
};
echo $category . PHP_EOL;
```

The `match` expression (PHP 8.0+) provides powerful pattern matching.  
It performs strict comparison and supports multiple values per arm.  
Unlike switch, match is an expression that returns values.  

## Regular expressions

Pattern matching and text processing with regex.  

```php
<?php

$text = "The year 2023 was great";

// Basic matching
if (preg_match('/\d+/', $text, $matches)) {
    echo "Found numbers: " . $matches[0] . PHP_EOL;
}

// Capture groups
if (preg_match('/year\s+(\d+)/', $text, $matches)) {
    echo "Year: " . $matches[1] . PHP_EOL;  // 2023
}

// Substitution
$newText = preg_replace('/\d+/', "2024", $text);
echo $newText . PHP_EOL;  // The year 2024 was great
```

Regular expressions use `preg_match()` for matching and store  
captures in the `$matches` array. The `preg_replace()` function  
substitutes matched patterns with replacement text.  

## File reading

Reading content from files safely with error handling.  

```php
<?php

// Read entire file
if (file_exists("/etc/hostname")) {
    $content = file_get_contents("/etc/hostname");
    echo "Hostname: " . trim($content) . PHP_EOL;
}

// Read line by line
if (file_exists("/etc/passwd")) {
    $lines = file("/etc/passwd", FILE_IGNORE_NEW_LINES);
    for ($i = 0; $i < min(3, count($lines)); $i++) {
        echo "Line: " . $lines[$i] . PHP_EOL;
    }
}

// Handle missing files
try {
    $data = file_get_contents("nonexistent.txt");
} catch (Exception $e) {
    echo "File not found or error reading" . PHP_EOL;
}
```

Use `file_exists()` to check file existence before reading.  
`file_get_contents()` reads entire files, while `file()` reads  
into array of lines. Combine with try/catch for error handling.  

## Exception handling

Managing errors gracefully with try/catch blocks.  

```php
<?php

// Basic exception handling
try {
    $result = 10 / 0;  // Generates warning but returns INF
    if (!is_finite($result)) {
        throw new DivisionByZeroError("Cannot divide by zero!");
    }
} catch (DivisionByZeroError $e) {
    echo "Division error: " . $e->getMessage() . PHP_EOL;
} catch (Exception $e) {
    echo "Unknown error: " . $e->getMessage() . PHP_EOL;
}

// Multiple exception types
try {
    throw new InvalidArgumentException("Custom error");
} catch (InvalidArgumentException $e) {
    echo "Custom error caught: " . $e->getMessage() . PHP_EOL;
} catch (Exception $e) {
    echo "Other error: " . $e->getMessage() . PHP_EOL;
}
```

The `try/catch` construct handles exceptions with specific exception  
types. Use `throw` to raise custom exceptions. PHP has built-in  
exception classes like `InvalidArgumentException` and custom ones  
can be created by extending `Exception`.  
## Basic classes

Creating simple classes with properties and methods.  

```php
<?php

class Person {
    public function __construct(
        public string $name,
        public int $age
    ) {}
    
    public function greet() {
        echo "Hi, I'm {$this->name} and I'm {$this->age} years old" . PHP_EOL;
    }
}

$person = new Person(name: "Alice", age: 30);
$person->greet();              // Hi, I'm Alice and I'm 30 years old
echo $person->name . PHP_EOL;  // Alice
```

Classes are defined with the `class` keyword. PHP 8.0+ constructor  
property promotion allows defining properties directly in the  
constructor. Use `public`, `private`, or `protected` for visibility.  

## Method calls

Different ways to call methods on objects and strings.  

```php
<?php

$text = "hello world";

// Standard method calls on strings
echo strtoupper($text) . PHP_EOL;     // HELLO WORLD
echo strlen($text) . PHP_EOL;         // 11
$words = explode(' ', $text);
echo count($words) . PHP_EOL;         // 2

// Method chaining with objects
class StringBuilder {
    private string $str = "";
    
    public function append(string $text): self {
        $this->str .= $text;
        return $this;
    }
    
    public function upper(): self {
        $this->str = strtoupper($this->str);
        return $this;
    }
    
    public function toString(): string {
        return $this->str;
    }
}

$result = (new StringBuilder())
    ->append("hello")
    ->append(" ")
    ->append("world")
    ->upper()
    ->toString();
echo $result . PHP_EOL;  // HELLO WORLD
```

PHP uses function calls for string operations rather than methods.  
Object method chaining requires returning `$this` from methods.  
The `->` operator accesses object properties and methods.  

## Array operations

Essential operations for working with arrays.  

```php
<?php

$numbers = [1, 3, 2, 5, 4];

$sorted = $numbers;
sort($sorted);                                // Sorts in place
print_r($sorted);                             // [1, 2, 3, 4, 5]
print_r(array_reverse($numbers));             // [4, 5, 2, 3, 1]
echo count($numbers) . PHP_EOL;               // 5 (count)
echo array_sum($numbers) . PHP_EOL;           // 15
echo max($numbers) . PHP_EOL;                 // 5
echo min($numbers) . PHP_EOL;                 // 1

$numbers[] = 6;                               // Add to end
array_unshift($numbers, 0);                  // Add to beginning
print_r($numbers);                            // [0, 1, 3, 2, 5, 4, 6]
```

PHP provides extensive array functions for sorting, counting, finding  
extrema, and modifying contents. Functions like `sort()`, `count()`,  
`array_sum()`, and `max()`/`min()` handle common array operations  
efficiently.  

## Associative array operations

Working with associative array data structures efficiently.  

```php
<?php

$scores = ["Alice" => 95, "Bob" => 87, "Carol" => 92];

print_r(array_keys($scores));     // [Alice, Bob, Carol]
print_r(array_values($scores));   // [95, 87, 92]
foreach ($scores as $name => $score) {
    echo "$name: $score" . PHP_EOL;
}

$scores["David"] = 88;            // Add new key-value pair
unset($scores["Alice"]);          // Remove a key
echo count($scores) . PHP_EOL;    // 3 (count of pairs)
```

Associative arrays provide functions to access keys, values, and  
pairs. Adding elements uses assignment, while `unset()` removes  
elements. Use `foreach` to iterate over key-value pairs.  

## String interpolation

Embedding expressions within strings for dynamic content.  

```php
<?php

$name = "Alice";
$age = 30;
$hobbies = ["reading", "coding", "hiking"];

echo "My name is $name" . PHP_EOL;
echo "I am $age years old" . PHP_EOL;
echo "Next year I'll be " . ($age + 1) . PHP_EOL;
echo "I have " . count($hobbies) . " hobbies" . PHP_EOL;
echo "My hobbies: " . implode(", ", $hobbies) . PHP_EOL;

// Alternative with sprintf
printf("My name is %s and I'm %d years old\n", $name, $age);
```

String interpolation allows variables within double quotes. Complex  
expressions require concatenation or curly braces `{$variable}`.  
The `sprintf()` function provides formatted string output similar  
to C-style formatting.  

## Functional programming

Using array_map, array_filter, and array transformations.  

```php
<?php

$numbers = range(1, 10);

// Transform with array_map
$doubled = array_map(fn($x) => $x * 2, $numbers);
print_r($doubled);                    // [2, 4, 6, 8, ..., 20]

// Filter with array_filter
$evens = array_filter($numbers, fn($x) => $x % 2 === 0);
print_r(array_values($evens));        // [2, 4, 6, 8, 10]

// Sort with custom criteria
$words = ["apple", "Banana", "cherry"];
usort($words, fn($a, $b) => strcasecmp($a, $b));
print_r($words);                      // [apple, Banana, cherry]
```

Functional methods like `array_map()`, `array_filter()`, and `usort()`  
transform data without modifying original arrays. PHP 7.4+ arrow  
functions `fn()` provide concise anonymous function syntax for  
simple operations.  

## Type constraints

Adding type safety with explicit type declarations.  

```php
<?php

// PHP 8.0+ typed properties
class Calculator {
    public function __construct(
        private int $count = 10,
        private string $message = "Hello",
        private bool $active = true,
        private float $price = 19.99
    ) {}
    
    // Function with typed parameters and return type
    public function calculateTax(float $amount, float $rate): float {
        return $amount * $rate;
    }
    
    public function getCount(): int {
        return $this->count;
    }
}

$calc = new Calculator();
echo $calc->calculateTax(100.0, 0.08) . PHP_EOL;  // 8
echo $calc->getCount() . PHP_EOL;                 // 10

// This would cause a TypeError:
// $calc->calculateTax("not a number", 0.08);
```

Type declarations ensure parameters and return values match specific  
types. PHP 8.0+ supports union types, nullable types, and mixed  
types for additional flexibility while maintaining type safety.  

## Method overloading alternatives

Achieving flexible function behavior through different approaches.  

```php
<?php

// Using union types (PHP 8.0+)
function process(int|string|array $input): string {
    return match (gettype($input)) {
        'integer' => "Processing integer: $input",
        'string' => "Processing string: $input",
        'array' => "Processing array with " . count($input) . " elements",
        default => "Unknown type"
    };
}

echo process(42) . PHP_EOL;              // Processing integer: 42
echo process("hello") . PHP_EOL;         // Processing string: hello
echo process([1, 2, 3]) . PHP_EOL;       // Processing array with 3 elements

// Alternative: function with optional parameters
function connect(string $host = "localhost", int $port = 8080, bool $ssl = false): string {
    return "Connecting to $host:$port (SSL: " . ($ssl ? "true" : "false") . ")";
}

echo connect() . PHP_EOL;
echo connect("example.com", 443, true) . PHP_EOL;
```

PHP doesn't have true method overloading, but union types and  
optional parameters provide flexibility. The `match` expression  
can dispatch based on argument types for polymorphic behavior.  

## Variadic parameters

Functions that accept variable numbers of arguments.  

```php
<?php

function sum(...$numbers): int|float {
    return array_sum($numbers);
}

function formatMessage(string $template, ...$args): string {
    return sprintf($template, ...$args);
}

echo sum(1, 2, 3, 4, 5) . PHP_EOL;           // 15
echo sum() . PHP_EOL;                        // 0
echo formatMessage("Hello %s, you have %d messages", "Alice", 5) . PHP_EOL;

// Unpacking arrays
$numbers = [10, 20, 30];
echo sum(...$numbers) . PHP_EOL;             // 60
```

Variadic parameters use `...` to collect remaining arguments into an  
array. The spread operator `...` can also unpack arrays into  
function arguments, providing flexible parameter handling.  

## Named parameters

Functions with labeled arguments for clarity and flexibility.  

```php
<?php

// PHP 8.0+ named arguments
function createUser(string $name, int $age = 18, bool $active = true): string {
    return "User: $name, Age: $age, Active: " . ($active ? "true" : "false");
}

function connect(string $host = "localhost", int $port = 8080, bool $ssl = false): string {
    return "Connecting to $host:$port (SSL: " . ($ssl ? "true" : "false") . ")";
}

echo createUser(name: "Alice", age: 25) . PHP_EOL;
echo connect(host: "example.com", ssl: true) . PHP_EOL;
echo connect() . PHP_EOL;  // Uses all defaults

// Named arguments can be passed in any order
echo createUser(age: 30, name: "Bob") . PHP_EOL;
```

Named arguments (PHP 8.0+) use parameter names with colons for  
clarity. They can be passed in any order and make function calls  
more readable, especially with many optional parameters.  

## Command line arguments

Accessing and processing arguments passed to your script.  

```php
<?php

// Basic command line argument access
if ($argc < 2) {
    echo "Usage: php script.php <filename> [--verbose] [--count=N]" . PHP_EOL;
    exit(1);
}

$filename = $argv[1];
$verbose = in_array("--verbose", $argv);
$count = 1;

// Parse named arguments
foreach ($argv as $arg) {
    if (str_starts_with($arg, "--count=")) {
        $count = (int)substr($arg, 8);
    }
}

echo "Processing file: $filename" . PHP_EOL;
if ($verbose) {
    echo "Verbose mode: enabled" . PHP_EOL;
}
echo "Count: $count" . PHP_EOL;

// Usage examples:
// php script.php myfile.txt
// php script.php myfile.txt --verbose
// php script.php myfile.txt --count=5 --verbose
```

The `$argc` variable contains argument count and `$argv` contains  
the argument array. Use `getopt()` for more sophisticated option  
parsing or third-party libraries for complex CLI applications.  
