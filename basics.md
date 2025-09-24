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
echo "Hello, World!" . PHP_EOL;
```

The `echo` statement outputs text. `PHP_EOL` provides a platform-  
independent newline. PHP scripts start with `<?php` tag when mixing  
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

Reading content from files safely.  

```raku
# Read entire file
if "/etc/hostname".IO.f {
    my $content = "/etc/hostname".IO.slurp;
    say "Hostname: $content.chomp()";
}

# Read line by line
if "/etc/passwd".IO.f {
    for "/etc/passwd".IO.lines[0..2] -> $line {
        say "Line: $line";
    }
}

# Handle missing files
try {
    my $data = "nonexistent.txt".IO.slurp;
    CATCH {
        default { say "File not found or error reading" }
    }
}
```

The `.IO` method creates file objects with methods like `.slurp`  
(read all), `.lines` (read lines), and `.f` (file exists check).  
Use `try/CATCH` for error handling.  

## Exception handling

Managing errors gracefully with try/CATCH blocks.  

```raku
# Basic exception handling
try {
    my $result = 10 / 0;  # Creates a Failure object
    say $result;          # Accessing the Failure throws the exception
    CATCH {
        when X::Numeric::DivideByZero {
            say "Cannot divide by zero!";
        }
        default {
            say "Unknown error: $_";
        }
    }
}

# Multiple exception types
try {
    die "Custom error";
    CATCH {
        when X::AdHoc { say "Custom error caught" }
        default { say "Other error: $_.message()" }
    }
}
```

The `try/CATCH` construct handles exceptions. Different exception  
types can be caught with specific when clauses. Use die to  
throw custom exceptions. Note that operations like division by zero  
return Failure objects, which only throw exceptions when accessed  
(e.g., via say or assignment to another variable). This allows  
deferred error handling.  



## Basic classes

Creating simple classes with attributes and methods.  

```raku
class Person {
    has $.name;
    has $.age;
    
    method greet {
        say "Hi, I'm $.name and I'm $.age years old";
    }
}

my $person = Person.new(name => "Alice", age => 30);
$person.greet;           # Hi, I'm Alice and I'm 30 years old
say $person.name;        # Alice
```

Classes are defined with the `class` keyword. Attributes use `has`  
with the `$.` twigil for read-only public access. Methods are  
defined with the `method` keyword.  

## Method calls

Different ways to call methods on objects.  

```raku
my $text = "hello world";

# Standard method calls
say $text.uc;            # HELLO WORLD
say $text.chars;         # 11
say $text.words;         # (hello world)
say $text.words.elems;   # 2

# Method chaining  
say $text.uc.split(' ').join('-');  # HELLO-WORLD

# Indirect method calls
say uc($text);           # HELLO WORLD (function style)
```

Methods can be called with dot notation, chained together for  
fluent interfaces, or used as functions. Chaining allows concise  
transformation pipelines.  

## Array operations

Essential operations for working with arrays.  

```raku
my @numbers = 1, 3, 2, 5, 4;

say @numbers.sort;       # (1 2 3 4 5)
say @numbers.reverse;    # (4 5 2 3 1)
say @numbers.elems;      # 5 (count)
say @numbers.sum;        # 15
say @numbers.max;        # 5
say @numbers.min;        # 1

@numbers.push(6);        # Add to end
@numbers.unshift(0);     # Add to beginning
say @numbers;            # [0 1 3 2 5 4 6]
```

Arrays have many built-in methods for sorting, counting, finding  
extrema, and modifying contents. These methods make array  
manipulation concise and readable.  

## Hash operations

Working with hash data structures efficiently.  

```raku
my %scores = Alice => 95, Bob => 87, Carol => 92;

say %scores.keys;        # (Alice Bob Carol)
say %scores.values;      # (95 87 92)
say %scores.pairs;       # (Alice => 95 Bob => 87 Carol => 92)

%scores<David> = 88;     # Add new key-value pair
%scores<Alice>:delete;   # Remove a key

say %scores.elems;       # 3 (count of pairs)
```

Hashes provide methods to access keys, values, and pairs. Adding  
and removing elements is straightforward with assignment and the  
`:delete` adverb.  

## String interpolation

Embedding expressions within strings for dynamic content.  

```raku
my $name = "Alice";
my $age = 30;
my @hobbies = "reading", "coding", "hiking";

say "My name is $name";
say "I am $age years old";
say "Next year I'll be {$age + 1}";
say "I have {+@hobbies} hobbies";
say "My hobbies: {@hobbies.join(', ')}";
```

String interpolation allows variables and expressions within double  
quotes. Use curly braces `{}` for complex expressions or to  
disambiguate variable boundaries.  

## Functional programming basics

Using map, grep, and sort for data transformation.  

```raku
my @numbers = 1..10;

# Transform with map
my @doubled = @numbers.map: * * 2;
say @doubled;            # [2 4 6 8 10 12 14 16 18 20]

# Filter with grep
my @evens = @numbers.grep: * %% 2;
say @evens;              # [2 4 6 8 10]

# Sort with custom criteria
my @words = "apple", "Banana", "cherry";
my @sorted = @words.sort: *.lc;
say @sorted;             # [apple Banana cherry]
```

Functional methods like `map`, `grep`, and `sort` transform data  
without modifying original arrays. The whatever star `*` creates  
anonymous functions for concise operations.  

## Type constraints

Adding type safety with explicit type declarations.  

```raku
my Int $count = 10;
my Str $message = "Hello";
my Bool $active = True;
my Rat $price = 19.99;

# Function with typed parameters
sub calculate-tax(Rat $amount, Rat $rate --> Rat) {
    $amount * $rate;
}

say calculate-tax(100.0, 0.08);  # 8

# This would cause a type error:
# $count = "not a number";
```

Type constraints ensure variables only accept values of specific  
types. Function signatures can specify parameter and return types  
for additional safety and documentation.  

## Multiple dispatch

Defining functions that behave differently based on parameter types.  

```raku
multi sub process(Int $n) {
    say "Processing integer: $n";
}

multi sub process(Str $s) {
    say "Processing string: $s";
}

multi sub process(@arr) {
    say "Processing array with {+@arr} elements";
}

process(42);             # Processing integer: 42
process("hello");        # Processing string: hello
process([1, 2, 3]);      # Processing array with 3 elements
```

Multiple dispatch allows the same function name to have different  
implementations based on the types or values of arguments. Raku  
automatically selects the best matching candidate.  

## Slurpy parameters

Functions that accept variable numbers of arguments.  

```raku
sub sum(*@numbers) {
    @numbers.sum;
}

sub format-message($template, *@args) {
    sprintf($template, |@args);
}

say sum(1, 2, 3, 4, 5);           # 15
say sum();                        # 0
say format-message("Hello %s, you have %d messages", "Alice", 5);
```

Slurpy parameters use `*` to collect remaining arguments into an  
array. This allows functions to accept flexible numbers of  
arguments while maintaining type safety.  

## Named parameters

Functions with labeled arguments for clarity and flexibility.  

```raku
sub create-user(:$name!, :$age = 18, :$active = True) {
    say "User: $name, Age: $age, Active: $active";
}

sub connect(:$host = "localhost", :$port = 8080, :$ssl = False) {
    say "Connecting to $host:$port (SSL: $ssl)";
}

create-user(name => "Alice", age => 25);
connect(host => "example.com", ssl => True);
connect();  # Uses all defaults
```

Named parameters use `:$name` syntax. The `!` makes parameters  
required, and `=` provides default values. Named parameters can  
be passed in any order.  

## Command line arguments

Accessing and processing arguments passed to your program.  

```raku
sub MAIN($filename, :$verbose = False, :$count = 1) {
    say "Processing file: $filename";
    say "Verbose mode: $verbose" if $verbose;
    say "Count: $count";
}

# Usage examples:
# script.raku myfile.txt
# script.raku myfile.txt --verbose
# script.raku myfile.txt --count=5 --verbose
```

The `MAIN` subroutine automatically processes command line arguments.  
Positional parameters become required arguments, and named parameters  
become optional flags. Raku generates help text automatically.
