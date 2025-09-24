# Operators

This chapter covers 30 comprehensive PHP operator examples demonstrating  
the complete range of operators available in PHP 8.4. These examples  
progress from basic arithmetic operations to advanced operator patterns,  
including precedence and associativity rules. Each example includes clear  
explanations to help you understand operator behavior and apply them  
effectively in your code.  

## Arithmetic operators

Basic mathematical operations with numbers.  

```php
<?php

$a = 15;
$b = 4;

echo $a + $b . "\n";      // 19 (addition)
echo $a - $b . "\n";      // 11 (subtraction)
echo $a * $b . "\n";      // 60 (multiplication)
echo $a / $b . "\n";      // 3.75 (division)
echo $a % $b . "\n";      // 3 (modulo)
echo $a ** $b . "\n";     // 50625 (exponentiation)
echo -$a . "\n";          // -15 (unary minus)
echo +$a . "\n";          // 15 (unary plus)
```

PHP provides standard arithmetic operators for mathematical calculations.  
Division always returns float; use `intdiv()` for integer division.  
The exponentiation operator `**` was added in PHP 5.6.  

## Assignment operators

Assigning values to variables with various patterns.  

```php
<?php

$x = 10;                  // Basic assignment
$y = $x;                  // Copy assignment
$x += 5;                  // $x = $x + 5 (15)
$x -= 3;                  // $x = $x - 3 (12)
$x *= 2;                  // $x = $x * 2 (24)
$x /= 4;                  // $x = $x / 4 (6)
$x %= 4;                  // $x = $x % 4 (2)
$x **= 3;                 // $x = $x ** 3 (8)

echo "Final value: $x\n"; // Final value: 8
```

Assignment operators combine assignment with arithmetic operations.  
These compound operators provide concise ways to modify variables  
while performing calculations in a single step.  

## String concatenation operators

Joining and modifying strings efficiently.  

```php
<?php

$greeting = "Hello";
$name = "Alice";
$full = $greeting . " " . $name;      // Hello Alice
echo $full . "\n";

$message = "Welcome";
$message .= " to PHP";                // Append to existing string
$message .= "!";                      // Welcome to PHP!
echo $message . "\n";

// Multiple concatenations
$result = "PHP" . " " . "8.4" . " " . "rocks";
echo $result . "\n";                  // PHP 8.4 rocks
```

The dot operator `.` concatenates strings, while `.=` appends to  
existing strings. String concatenation has specific precedence  
rules and works with automatic type conversion.  

## Comparison operators

Comparing values for equality and ordering.  

```php
<?php

$a = 10;
$b = "10";
$c = 20;

echo ($a == $b) ? "true" : "false" . "\n";    // true (loose equality)
echo ($a === $b) ? "true" : "false" . "\n";   // false (strict equality)
echo ($a != $c) ? "true" : "false" . "\n";    // true (not equal)
echo ($a !== $b) ? "true" : "false" . "\n";   // true (strict not equal)
echo ($a < $c) ? "true" : "false" . "\n";     // true (less than)
echo ($a > $c) ? "true" : "false" . "\n";     // false (greater than)
echo ($a <= $b) ? "true" : "false" . "\n";    // true (less or equal)
echo ($a >= $b) ? "true" : "false" . "\n";    // true (greater or equal)
```

Comparison operators test relationships between values. Use `==`/`!=`  
for loose comparison and `===`/`!==` for strict type-aware comparison.  
Ordering operators work with numbers, strings, and other types.  

## Spaceship operator

Three-way comparison for sorting and advanced comparisons.  

```php
<?php

function compare($a, $b) {
    $result = $a <=> $b;
    if ($result < 0) return "$a < $b";
    if ($result > 0) return "$a > $b";
    return "$a == $b";
}

echo compare(5, 10) . "\n";           // 5 < 10
echo compare(10, 5) . "\n";           // 10 > 5
echo compare(7, 7) . "\n";            // 7 == 7

// Useful for sorting
$numbers = [3, 1, 4, 1, 5];
usort($numbers, fn($a, $b) => $a <=> $b);
echo implode(", ", $numbers) . "\n";  // 1, 1, 3, 4, 5
```

The spaceship operator `<=>` returns -1, 0, or 1 for less than,  
equal to, or greater than comparisons. It's particularly useful  
for custom sorting functions and multi-criteria comparisons.  

## Logical operators

Combining boolean expressions with logical operations.  

```php
<?php

$a = true;
$b = false;
$x = 10;
$y = 0;

echo ($a && $b) ? "true" : "false" . "\n";    // false (AND)
echo ($a || $b) ? "true" : "false" . "\n";    // true (OR)
echo (!$a) ? "true" : "false" . "\n";         // false (NOT)
echo ($a and $b) ? "true" : "false" . "\n";   // false (lower precedence AND)
echo ($a or $b) ? "true" : "false" . "\n";    // true (lower precedence OR)
echo ($a xor $b) ? "true" : "false" . "\n";   // true (exclusive OR)

// Short-circuit evaluation
echo ($x > 5 && $y > 0) ? "both true" : "not both" . "\n";  // not both
```

Logical operators combine boolean values with AND, OR, and NOT logic.  
Symbol forms (`&&`, `||`) have higher precedence than word forms  
(`and`, `or`). XOR returns true when operands differ.  

## Bitwise operators

Manipulating individual bits in integer values.  

```php
<?php

$a = 12;    // Binary: 1100
$b = 10;    // Binary: 1010

echo ($a & $b) . "\n";        // 8 (bitwise AND: 1000)
echo ($a | $b) . "\n";        // 14 (bitwise OR: 1110)
echo ($a ^ $b) . "\n";        // 6 (bitwise XOR: 0110)
echo (~$a) . "\n";            // -13 (bitwise NOT: complement)
echo ($a << 2) . "\n";        // 48 (left shift: 110000)
echo ($a >> 1) . "\n";        // 6 (right shift: 110)

// Practical bit manipulation
$flags = 5;                   // Binary: 101
echo ($flags & 1) . "\n";     // 1 (check if bit 0 is set)
```

Bitwise operators work on individual bits of integer values.  
They're useful for flags, permissions, optimization, and  
low-level programming tasks requiring bit manipulation.  

## Increment and decrement

Modifying numeric values by one.  

```php
<?php

$i = 5;
$j = 5;

echo "Pre-increment: " . (++$i) . "\n";      // 6 (increment then return)
echo "After pre-increment: " . $i . "\n";   // 6

echo "Post-increment: " . ($j++) . "\n";     // 5 (return then increment)
echo "After post-increment: " . $j . "\n";  // 6

$k = 10;
$l = 10;

echo "Pre-decrement: " . (--$k) . "\n";      // 9 (decrement then return)
echo "Post-decrement: " . ($l--) . "\n";     // 10 (return then decrement)
echo "Final values: k=$k, l=$l\n";           // k=9, l=9
```

Increment (`++`) and decrement (`--`) operators modify variables by one.  
Pre-operators modify first then return new value, post-operators return  
current value then modify. Understanding this difference prevents bugs.  

## Null coalescing operators

Handling null values gracefully with default fallbacks.  

```php
<?php

$username = null;
$config = [];

// Null coalescing operator
$display_name = $username ?? "Guest";
echo "Welcome, $display_name!\n";            // Welcome, Guest!

// Chaining null coalescing
$setting = $config['theme'] ?? $_ENV['THEME'] ?? 'default';
echo "Theme: $setting\n";                    // Theme: default

// Null coalescing assignment (PHP 7.4+)
$count = null;
$count ??= 0;                                // Assign 0 if null
$count ??= 10;                               // Won't assign, already set
echo "Count: $count\n";                      // Count: 0
```

The null coalescing operator `??` returns the right operand when  
the left is null. The assignment version `??=` only assigns when  
the variable is null, providing clean default value patterns.  

## Ternary conditional operators

Compact conditional expressions for simple decisions.  

```php
<?php

$score = 85;
$age = 16;

// Basic ternary
$result = ($score >= 80) ? "Excellent" : "Good";
echo "Performance: $result\n";               // Performance: Excellent

// Nested ternary (use sparingly)
$grade = ($score >= 90) ? "A" : (($score >= 80) ? "B" : "C");
echo "Grade: $grade\n";                      // Grade: B

// Elvis operator (PHP 5.3+)
$name = "";
$display = $name ?: "Anonymous";
echo "User: $display\n";                     // User: Anonymous

// Ternary with complex conditions
$access = ($age >= 18 && $score >= 75) ? "Granted" : "Denied";
echo "Access: $access\n";                    // Access: Denied
```

Ternary operator `condition ? true_value : false_value` provides  
compact conditional expressions. Elvis operator `?:` uses left  
operand if truthy, otherwise right operand.  

## Array operators

Combining and comparing arrays with specialized operators.  

```php
<?php

$fruits = ["apple", "banana"];
$vegetables = ["carrot", "lettuce"];
$colors = ["red", "green"];

// Union operator
$combined = $fruits + $vegetables;
print_r($combined);                          // [apple, banana]

// Array comparison
$arr1 = ["a" => 1, "b" => 2];
$arr2 = ["b" => 2, "a" => 1];
$arr3 = ["a" => 1, "b" => "2"];

echo ($arr1 == $arr2) ? "equal" : "not equal" . "\n";     // equal
echo ($arr1 === $arr2) ? "identical" : "not identical" . "\n"; // not identical
echo ($arr1 === $arr3) ? "same" : "different types" . "\n";    // different types
```

Array union operator `+` combines arrays, keeping left-side values  
for duplicate keys. Array comparison operators compare elements  
and their order with varying strictness levels.  

## Type operators

Testing object types and class relationships.  

```php
<?php

class Animal {}
class Dog extends Animal {}
class Cat extends Animal {}

$dog = new Dog();
$cat = new Cat();
$animal = new Animal();

echo ($dog instanceof Dog) ? "true" : "false" . "\n";        // true
echo ($dog instanceof Animal) ? "true" : "false" . "\n";     // true
echo ($cat instanceof Dog) ? "true" : "false" . "\n";        // false

// Using with strings
$className = 'Animal';
echo ($dog instanceof $className) ? "true" : "false" . "\n"; // true

// Interface checking
interface Flyable {}
class Bird implements Flyable {}
$bird = new Bird();
echo ($bird instanceof Flyable) ? "true" : "false" . "\n";   // true
```

The `instanceof` operator tests whether an object is an instance  
of a specific class, its parent classes, or implemented interfaces.  
It supports inheritance hierarchies and interface implementations.  

## Error control operator

Suppressing error messages for specific operations.  

```php
<?php

// Without error suppression
echo "Normal operation:\n";
$result1 = 10 / 2;
echo "Result: $result1\n";                   // Result: 5

// With error suppression
echo "Suppressed errors:\n";
$file_contents = @file_get_contents('nonexistent.txt');
if ($file_contents === false) {
    echo "File not found, but no error displayed\n";
}

// Suppressing warnings
$array = [];
$value = @$array['nonexistent_key'];         // No notice about undefined index
echo "Value: " . ($value ?? 'null') . "\n"; // Value: null

// Use sparingly - proper error handling is better
```

The error suppression operator `@` suppresses error messages  
for the expression it precedes. Use sparingly as it can hide  
important debugging information and mask real problems.  

## Execution operator

Executing shell commands and capturing output.  

```php
<?php

// Execute shell command
$output = `echo "Hello from shell"`;
echo "Command output: $output";              // Command output: Hello from shell

// Current directory
$pwd = `pwd`;
echo "Current directory: $pwd";

// List files (be careful with security)
$files = `ls -la`;
echo "Directory listing:\n$files";

// Note: This is equivalent to shell_exec()
$alt_output = shell_exec('echo "Alternative method"');
echo "Alternative: $alt_output";
```

Backticks execute shell commands and return output as strings.  
Use with extreme caution due to security implications. Consider  
`shell_exec()`, `exec()`, or `system()` for better control.  

## Clone operator

Creating copies of objects with proper cloning.  

```php
<?php

class Person {
    public $name;
    public $address;
    
    public function __construct($name, $address) {
        $this->name = $name;
        $this->address = $address;
    }
    
    public function __clone() {
        // Deep clone the address object
        $this->address = clone $this->address;
    }
}

class Address {
    public $street;
    public function __construct($street) { $this->street = $street; }
}

$address = new Address("123 Main St");
$person1 = new Person("Alice", $address);
$person2 = clone $person1;

$person2->name = "Bob";
$person2->address->street = "456 Oak Ave";

echo "Person 1: {$person1->name}, {$person1->address->street}\n";
echo "Person 2: {$person2->name}, {$person2->address->street}\n";
```

The `clone` operator creates object copies. Implement `__clone()`  
method for custom cloning behavior, especially for deep copying  
of nested objects and proper resource handling.  

## Object operators

Accessing object properties and methods dynamically.  

```php
<?php

class Calculator {
    public $precision = 2;
    
    public function add($a, $b) {
        return round($a + $b, $this->precision);
    }
    
    public function multiply($a, $b) {
        return round($a * $b, $this->precision);
    }
}

$calc = new Calculator();

// Direct property access
echo "Precision: " . $calc->precision . "\n";   // Precision: 2

// Method calls
echo "Addition: " . $calc->add(3.456, 2.789) . "\n";  // Addition: 6.25

// Dynamic property access
$property = 'precision';
echo "Dynamic property: " . $calc->$property . "\n";  // Dynamic property: 2

// Dynamic method calls
$method = 'multiply';
echo "Dynamic method: " . $calc->$method(3.14, 2) . "\n";  // Dynamic method: 6.28
```

The arrow operator `->` accesses object properties and methods.  
Support for dynamic access using variables enables flexible  
object manipulation and generic programming patterns.  

## Scope resolution operator

Accessing class constants, static properties, and methods.  

```php
<?php

class MathConstants {
    const PI = 3.14159;
    public static $precision = 4;
    
    public static function circle_area($radius) {
        return self::PI * $radius * $radius;
    }
    
    public static function get_pi() {
        return static::PI;  // Late static binding
    }
}

// Access class constant
echo "PI: " . MathConstants::PI . "\n";                 // PI: 3.14159

// Access static property
echo "Precision: " . MathConstants::$precision . "\n"; // Precision: 4

// Call static method
echo "Area: " . MathConstants::circle_area(5) . "\n";  // Area: 78.5397

// Self vs static
class ExtendedMath extends MathConstants {
    const PI = 3.14159265;
}

echo "Extended PI: " . ExtendedMath::get_pi() . "\n";   // Uses ExtendedMath::PI
```

The scope resolution operator `::` accesses class constants,  
static properties, and methods. Use `self::` for current class,  
`static::` for late static binding, or `ClassName::` for specific class.  

## Array access operators

Accessing array elements and implementing custom array-like behavior.  

```php
<?php

// Standard array access
$fruits = ["apple", "banana", "cherry"];
$scores = ["Alice" => 95, "Bob" => 87, "Carol" => 92];

echo $fruits[0] . "\n";                      // apple
echo $scores["Alice"] . "\n";                // 95

// Multi-dimensional arrays
$matrix = [[1, 2], [3, 4]];
echo $matrix[1][0] . "\n";                   // 3

// Array assignment
$colors = [];
$colors[] = "red";                           // Append
$colors[5] = "blue";                         // Specific index
echo implode(", ", $colors) . "\n";         // red, , , , , blue

// String character access
$text = "Hello";
echo $text[1] . "\n";                        // e
```

Square brackets `[]` provide array element access and assignment.  
Empty brackets append to arrays. Strings support character access  
using array notation for convenient string manipulation.  

## Reference operators

Creating and working with variable references.  

```php
<?php

$original = "Hello";
$reference = &$original;                     // Create reference

echo "Original: $original\n";               // Original: Hello
echo "Reference: $reference\n";             // Reference: Hello

$reference = "Modified";                     // Modify through reference
echo "Original after modification: $original\n";  // Original after modification: Modified

// Function references
function modify_by_reference(&$value) {
    $value *= 2;
}

$number = 10;
modify_by_reference($number);
echo "Number after function: $number\n";    // Number after function: 20

// Unsetting references
unset($reference);
echo "Original still exists: $original\n";  // Original still exists: Modified
```

The reference operator `&` creates variable aliases that point  
to the same memory location. Changes through any reference  
affect all aliases. Use carefully to avoid unexpected behavior.  

## Yield operators

Creating generators for memory-efficient iteration.  

```php
<?php

function number_generator($max) {
    for ($i = 1; $i <= $max; $i++) {
        yield $i;                            // Yield value
    }
}

function key_value_generator() {
    yield "name" => "Alice";
    yield "age" => 30;
    yield "city" => "New York";
}

// Using generators
echo "Numbers: ";
foreach (number_generator(5) as $num) {
    echo "$num ";                            // 1 2 3 4 5
}
echo "\n";

echo "Key-value pairs:\n";
foreach (key_value_generator() as $key => $value) {
    echo "$key: $value\n";
}

// Generator expressions
$squares = (function($max) {
    for ($i = 1; $i <= $max; $i++) {
        yield $i => $i * $i;
    }
})(4);

foreach ($squares as $num => $square) {
    echo "$num squared is $square\n";
}
```

The `yield` operator creates generators that produce values on-demand.  
Generators are memory-efficient for large datasets and support  
both indexed and associative value generation patterns.  

## Variable variables

Dynamically accessing variables using variable names.  

```php
<?php

$name = "score";
$score = 95;

// Variable variable
echo $$name . "\n";                          // 95 (same as $score)

// With arrays
$fruit = "apple";
$apple = "red";
$colors = ["apple" => "red", "banana" => "yellow"];

echo ${$fruit} . "\n";                       // red
echo $colors[$fruit] . "\n";                 // red

// Dynamic property access
class Product {
    public $name = "Laptop";
    public $price = 999;
}

$product = new Product();
$property = "name";
echo $product->$property . "\n";             // Laptop

// Multiple levels
$level1 = "level2";
$level2 = "final";
$final = "Found it!";
echo $$$level1 . "\n";                       // Found it!
```

Variable variables use `$$variable` syntax to access variables  
whose names are stored in other variables. This enables dynamic  
programming patterns but can reduce code readability.  

## Compound assignment operators

Advanced assignment patterns for complex operations.  

```php
<?php

$text = "Hello";
$numbers = [1, 2, 3];
$bitfield = 7;  // Binary: 111

// String concatenation assignment
$text .= " World";
echo "$text\n";                              // Hello World

// Arithmetic assignments
$value = 100;
$value += 50;                                // 150
$value -= 25;                                // 125
$value *= 2;                                 // 250
$value /= 5;                                 // 50
$value %= 7;                                 // 1
$value **= 3;                                // 1

// Bitwise assignments
$bitfield &= 5;                              // Bitwise AND assignment
$bitfield |= 2;                              // Bitwise OR assignment
$bitfield ^= 3;                              // Bitwise XOR assignment
$bitfield <<= 1;                             // Left shift assignment
$bitfield >>= 1;                             // Right shift assignment

echo "Final bitfield: $bitfield\n";
```

Compound assignment operators combine operation and assignment.  
They work with arithmetic, bitwise, and string operations,  
providing concise ways to modify variables in-place.  

## Operator precedence demonstration

Understanding operator evaluation order in complex expressions.  

```php
<?php

// Arithmetic precedence
$result1 = 2 + 3 * 4;                        // 14 (not 20)
$result2 = (2 + 3) * 4;                      // 20 (parentheses override)
echo "2 + 3 * 4 = $result1\n";
echo "(2 + 3) * 4 = $result2\n";

// Comparison and logical precedence
$a = 5;
$b = 10;
$c = 15;

$result3 = $a < $b && $b < $c;               // true
$result4 = $a < $b and $b < $c;              // true, but different precedence
echo "Logical AND result: " . ($result3 ? "true" : "false") . "\n";

// Assignment precedence
$x = $y = 10;                                // Right-to-left associativity
echo "x = $x, y = $y\n";

// Mixed operators
$complex = $a + $b * $c > $a * $b + $c;     // 5 + 150 > 50 + 15 = true
echo "Complex expression: " . ($complex ? "true" : "false") . "\n";
```

Operator precedence determines evaluation order in expressions.  
Parentheses override precedence rules. Understanding precedence  
prevents bugs and makes complex expressions more predictable.  

## Short-circuit evaluation

Optimizing logical expressions with early termination.  

```php
<?php

function expensive_check($value) {
    echo "Expensive check called with $value\n";
    return $value > 50;
}

$condition1 = false;
$condition2 = true;
$value = 75;

// AND short-circuit: second condition not evaluated if first is false
echo "AND short-circuit test:\n";
$result1 = $condition1 && expensive_check($value);
echo "Result: " . ($result1 ? "true" : "false") . "\n";

// OR short-circuit: second condition not evaluated if first is true
echo "\nOR short-circuit test:\n";
$result2 = $condition2 || expensive_check($value);
echo "Result: " . ($result2 ? "true" : "false") . "\n";

// Practical example: null checking
$user = null;
$name = $user && $user->name ? $user->name : "Guest";
echo "Welcome, $name!\n";
```

Short-circuit evaluation stops evaluating logical expressions  
when the result is determined. This optimizes performance and  
prevents errors in conditional property/method access.  

## Operator associativity examples

Understanding left-to-right vs right-to-left evaluation.  

```php
<?php

// Left associativity (most operators)
$result1 = 10 - 5 - 2;                       // (10 - 5) - 2 = 3
$result2 = 20 / 4 / 2;                       // (20 / 4) / 2 = 2.5
echo "Left associative: 10 - 5 - 2 = $result1\n";
echo "Left associative: 20 / 4 / 2 = $result2\n";

// Right associativity (assignment, exponentiation)
$a = $b = $c = 5;                            // $c = 5, $b = $c, $a = $b
echo "Assignment: a=$a, b=$b, c=$c\n";

$power = 2 ** 3 ** 2;                        // 2 ** (3 ** 2) = 2 ** 9 = 512
echo "Exponentiation: 2 ** 3 ** 2 = $power\n";

// Ternary operator (left associative - unusual)
$x = 1;
$y = 2;
$z = 3;
$ternary = $x ? $y : $z ? 4 : 5;            // ($x ? $y : $z) ? 4 : 5
echo "Ternary result: $ternary\n";          // 4 (because $y is truthy)
```

Associativity determines evaluation order for operators of equal  
precedence. Most operators are left-associative, but assignment  
and exponentiation are right-associative.  

## Match expression operators

Using match expressions for pattern-based value selection.  

```php
<?php

$status_code = 404;
$priority = 'high';

// Basic match expression
$message = match($status_code) {
    200 => "OK",
    404 => "Not Found", 
    500 => "Internal Server Error",
    default => "Unknown Status"
};
echo "Status: $message\n";                   // Status: Not Found

// Match with conditions
$action = match($priority) {
    'low' => 'queue',
    'medium' => 'process',
    'high', 'urgent' => 'immediate',         // Multiple values
    default => 'unknown'
};
echo "Action: $action\n";                    // Action: immediate

// Match with expressions
$grade = 85;
$letter = match(true) {
    $grade >= 90 => 'A',
    $grade >= 80 => 'B', 
    $grade >= 70 => 'C',
    default => 'F'
};
echo "Grade: $letter\n";                     // Grade: B
```

Match expressions provide strict comparison-based pattern matching.  
Unlike switch statements, match expressions return values directly  
and don't require break statements or fall-through handling.  

## Nullsafe operator

Safely accessing properties and methods on potentially null objects.  

```php
<?php

class User {
    public $profile;
    public function getName() { return "John Doe"; }
}

class Profile {
    public $address;
    public function getEmail() { return "john@example.com"; }
}

$user = new User();
$user->profile = new Profile();

// Traditional null checking
$email1 = $user && $user->profile && $user->profile->getEmail() 
    ? $user->profile->getEmail() 
    : null;
echo "Email (traditional): " . ($email1 ?? 'none') . "\n";

// Nullsafe operator (PHP 8.0+)
$email2 = $user?->profile?->getEmail();
echo "Email (nullsafe): " . ($email2 ?? 'none') . "\n";

// Works with null objects
$null_user = null;
$null_email = $null_user?->profile?->getEmail();
echo "Null email: " . ($null_email ?? 'none') . "\n";  // none

// Chaining with regular operators
$name = $user?->getName() ?? 'Anonymous';
echo "Name: $name\n";                        // Name: John Doe
```

The nullsafe operator `?->` safely accesses properties and methods,  
returning null if any link in the chain is null. This eliminates  
verbose null checking and prevents null pointer exceptions.  

## Spread operator

Unpacking arrays and passing multiple arguments efficiently.  

```php
<?php

// Array unpacking
$fruits = ['apple', 'banana'];
$vegetables = ['carrot', 'lettuce'];
$combined = [...$fruits, 'orange', ...$vegetables];
print_r($combined);                          // [apple, banana, orange, carrot, lettuce]

// Function arguments
function sum($a, $b, $c) {
    return $a + $b + $c;
}

$numbers = [10, 20, 30];
$result = sum(...$numbers);                  // Unpack array as arguments
echo "Sum: $result\n";                       // Sum: 60

// Associative array unpacking (PHP 8.1+)
$defaults = ['color' => 'blue', 'size' => 'medium'];
$custom = ['size' => 'large', 'weight' => 'heavy'];
$merged = [...$defaults, ...$custom];
print_r($merged);                            // color: blue, size: large, weight: heavy

// In function definitions (variadic)
function multiply($factor, ...$numbers) {
    return array_map(fn($n) => $n * $factor, $numbers);
}

$doubled = multiply(2, 1, 2, 3, 4);
print_r($doubled);                           // [2, 4, 6, 8]
```

The spread operator `...` unpacks arrays into individual elements.  
Use for array merging, function arguments, and creating variadic  
functions that accept unlimited parameters.  

## Attribute operators

Working with PHP 8+ attributes for metadata annotation.  

```php
<?php

#[Attribute]
class Route {
    public function __construct(
        public string $path,
        public string $method = 'GET'
    ) {}
}

#[Attribute]
class Deprecated {
    public function __construct(public string $reason = '') {}
}

class ApiController {
    #[Route('/users', 'GET')]
    public function getUsers() {
        return ['user1', 'user2'];
    }
    
    #[Route('/users', 'POST')]
    #[Deprecated('Use createUser instead')]
    public function addUser() {
        return 'User added';
    }
}

// Reading attributes
$reflection = new ReflectionClass(ApiController::class);
foreach ($reflection->getMethods() as $method) {
    $attributes = $method->getAttributes();
    echo "Method: {$method->getName()}\n";
    
    foreach ($attributes as $attribute) {
        $instance = $attribute->newInstance();
        if ($instance instanceof Route) {
            echo "  Route: {$instance->method} {$instance->path}\n";
        }
        if ($instance instanceof Deprecated) {
            echo "  Deprecated: {$instance->reason}\n";
        }
    }
}
```

Attributes use `#[AttributeName]` syntax to add metadata to classes,  
methods, and properties. They provide a structured way to add  
configuration and documentation directly in code.  

## Type casting operators

Converting values between different data types explicitly.  

```php
<?php

$number = 42;
$float = 3.14;
$string = "123";
$bool = true;
$array = [1, 2, 3];

// Casting to different types
echo (string)$number . " (string)\n";       // "42" (string)
echo (int)$float . " (int)\n";              // 3 (int)
echo (float)$string . " (float)\n";         // 123.0 (float)
echo (bool)$number . " (bool)\n";           // 1 (bool)
echo (int)$bool . " (int)\n";               // 1 (int)

// Array and object casting
$object = (object)$array;                   // Convert array to object
echo $object->{0} . " (object property)\n"; // 1 (object property)

$back_to_array = (array)$object;            // Convert back to array
print_r($back_to_array);                    // [0 => 1, 1 => 2, 2 => 3]

// String to array
$text = "hello";
$char_array = (array)$text;                 // Creates array with single element
print_r($char_array);                       // ["hello"]

// Null casting
$null_value = null;
echo (string)$null_value . "empty\n";       // "empty" (empty string)
echo (int)$null_value . " (zero)\n";        // 0 (zero)
```

Type casting operators `(type)` explicitly convert values between  
data types. PHP performs automatic type juggling, but explicit  
casting ensures predictable type conversion behavior.  

## Operator Precedence and Associativity Table

| Precedence | Operator | Description | Associativity |
|------------|----------|-------------|---------------|
| 1 (highest) | `clone` `new` | Clone and new | none |
| 2 | `**` | Exponentiation | right |
| 3 | `++` `--` `~` `(int)` `(float)` `(string)` `(array)` `(object)` `(bool)` `@` | Increment, decrement, bitwise NOT, casts, error control | none |
| 4 | `instanceof` | Type operator | none |
| 5 | `!` | Logical NOT | right |
| 6 | `*` `/` `%` | Multiplication, division, modulo | left |
| 7 | `+` `-` `.` | Addition, subtraction, concatenation | left |
| 8 | `<<` `>>` | Bitwise shift | left |
| 9 | `<` `<=` `>` `>=` | Comparison | none |
| 10 | `==` `!=` `===` `!==` `<>` `<=>` | Equality and spaceship | none |
| 11 | `&` | Bitwise AND | left |
| 12 | `^` | Bitwise XOR | left |
| 13 | `\|` | Bitwise OR | left |
| 14 | `&&` | Logical AND | left |
| 15 | `\|\|` | Logical OR | left |
| 16 | `??` | Null coalescing | right |
| 17 | `? :` | Ternary conditional | left |
| 18 | `??=` `=` `+=` `-=` `*=` `/=` `%=` `**=` `.=` `&=` `\|=` `^=` `<<=` `>>=` | Assignment operators | right |
| 19 | `yield from` | Generator delegation | none |
| 20 | `yield` | Generator | none |
| 21 | `and` | Logical AND (low precedence) | left |
| 22 | `xor` | Logical XOR | left |
| 23 (lowest) | `or` | Logical OR (low precedence) | left |

This table shows operator precedence from highest (1) to lowest (23).  
Higher precedence operators evaluate first. When operators have equal  
precedence, associativity determines evaluation order. Use parentheses  
to override precedence and make intentions explicit in complex expressions.