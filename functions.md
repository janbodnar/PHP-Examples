# Functions

This chapter covers 25 comprehensive PHP function examples designed to  
demonstrate the full range of function capabilities in modern PHP 8.4.  
These examples progress from basic function definitions to advanced  
concepts like generators, closures, and functional programming patterns.  
Each example includes detailed explanations to help you master PHP  
function development.  

## Basic function definition

Creating simple functions with parameters and return values.  

```php
<?php
function greet($name) {
    return "Hello, $name!";
}

function add($a, $b) {
    return $a + $b;
}

function sayWelcome() {
    echo "Welcome to our application!" . PHP_EOL;
}

echo greet("Alice") . PHP_EOL;        // Hello, Alice!
echo add(10, 15) . PHP_EOL;           // 25
sayWelcome();                         // Welcome to our application!
```

Functions are defined using the `function` keyword followed by a name,  
parameter list in parentheses, and a code block. Use `return` to send  
values back to the caller, or `echo`/`print` for direct output.  

## Default parameter values

Providing default values for function parameters.  

```php
<?php
function createUser($name, $role = 'user', $active = true) {
    return [
        'name' => $name,
        'role' => $role,
        'active' => $active,
        'created_at' => date('Y-m-d H:i:s')
    ];
}

$user1 = createUser("John");
$user2 = createUser("Jane", "admin");
$user3 = createUser("Bob", "moderator", false);

print_r($user1);
print_r($user2);
print_r($user3);
```

Default parameters allow functions to work with fewer arguments. They  
must appear after required parameters and provide fallback values when  
arguments are omitted during function calls.  

## Type declarations

Enforcing parameter and return types for better code safety.  

```php
<?php
function calculateArea(float $width, float $height): float {
    return $width * $height;
}

function formatPrice(int $cents): string {
    return "$" . number_format($cents / 100, 2);
}

function getUsers(): array {
    return ['Alice', 'Bob', 'Charlie'];
}

echo calculateArea(5.5, 3.2) . PHP_EOL;     // 17.6
echo formatPrice(1299) . PHP_EOL;           // $12.99
print_r(getUsers());                        // Array of users
```

Type declarations specify expected parameter types and return types.  
PHP 8.4 supports scalar types (int, float, string, bool), array,  
callable, and class types for improved type safety.  

## Variable-length argument lists

Using variadic functions to accept unlimited parameters.  

```php
<?php
function sum(...$numbers): int|float {
    return array_sum($numbers);
}

function logMessage(string $level, string $message, ...$context): void {
    $timestamp = date('Y-m-d H:i:s');
    echo "[$timestamp] $level: $message";
    
    if (!empty($context)) {
        echo " | Context: " . implode(', ', $context);
    }
    echo PHP_EOL;
}

echo sum(1, 2, 3, 4, 5) . PHP_EOL;          // 15
echo sum(10.5, 20.3, 15.2) . PHP_EOL;       // 46

logMessage('INFO', 'User logged in', 'user_id=123', 'ip=192.168.1.1');
logMessage('ERROR', 'Database connection failed');
```

The `...` operator creates variadic functions that accept unlimited  
arguments. Inside the function, variadic parameters become an array  
containing all passed values.  

## Named parameters

Using named arguments for clearer function calls.  

```php
<?php
function connectDatabase(
    string $host,
    int $port = 3306,
    string $username = 'root',
    string $password = '',
    string $database = 'test',
    bool $ssl = false
): array {
    return [
        'host' => $host,
        'port' => $port,
        'username' => $username,
        'database' => $database,
        'ssl' => $ssl ? 'enabled' : 'disabled'
    ];
}

$connection1 = connectDatabase(
    host: 'localhost',
    database: 'myapp',
    ssl: true
);

$connection2 = connectDatabase(
    'remote-server',
    username: 'admin',
    password: 'secret123',
    port: 5432
);

print_r($connection1);
print_r($connection2);
```

Named parameters allow passing arguments by name rather than position,  
making function calls more readable and allowing parameters to be  
passed in any order when using names.  

## Union types

Functions accepting multiple types using union type declarations.  

```php
<?php
function processValue(int|string|array $value): string {
    return match(gettype($value)) {
        'integer' => "Processing integer: $value",
        'string' => "Processing string: $value",
        'array' => "Processing array with " . count($value) . " elements",
        default => "Unknown type"
    };
}

function formatOutput(mixed $data): string {
    if (is_array($data)) {
        return json_encode($data, JSON_PRETTY_PRINT);
    }
    if (is_object($data)) {
        return get_class($data) . ' object';
    }
    return (string)$data;
}

echo processValue(42) . PHP_EOL;
echo processValue("Hello World") . PHP_EOL;
echo processValue([1, 2, 3]) . PHP_EOL;

echo formatOutput(['name' => 'John', 'age' => 30]) . PHP_EOL;
echo formatOutput("Simple string") . PHP_EOL;
```

Union types with the pipe `|` operator allow functions to accept  
multiple specific types. The `mixed` type accepts any type, providing  
maximum flexibility while maintaining type information.  

## Anonymous functions and closures

Creating functions without names and capturing variables from scope.  

```php
<?php
$multiplier = 3;

$calculate = function($x, $y) use ($multiplier) {
    return ($x + $y) * $multiplier;
};

$greetings = array_map(function($name) {
    return "Hello, $name!";
}, ['Alice', 'Bob', 'Charlie']);

$numbers = [1, 2, 3, 4, 5];
$evens = array_filter($numbers, function($n) {
    return $n % 2 === 0;
});

echo $calculate(5, 10) . PHP_EOL;           // 45
print_r($greetings);
print_r($evens);                            // [2, 4]
```

Anonymous functions (closures) are functions without names, often used  
for callbacks. The `use` keyword captures variables from the parent  
scope, making them available inside the closure.  

## Arrow functions

Concise syntax for simple anonymous functions.  

```php
<?php
$numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

$doubled = array_map(fn($x) => $x * 2, $numbers);
$filtered = array_filter($numbers, fn($n) => $n > 5);
$sum = array_reduce($numbers, fn($carry, $item) => $carry + $item, 0);

$users = [
    ['name' => 'Alice', 'age' => 25],
    ['name' => 'Bob', 'age' => 30],
    ['name' => 'Charlie', 'age' => 35]
];

$names = array_map(fn($user) => $user['name'], $users);
$adults = array_filter($users, fn($user) => $user['age'] >= 18);

print_r($doubled);                          // [2, 4, 6, ..., 20]
print_r($filtered);                         // [6, 7, 8, 9, 10]
echo "Sum: $sum" . PHP_EOL;                 // Sum: 55
print_r($names);                            // ['Alice', 'Bob', 'Charlie']
print_r($adults);
```

Arrow functions provide a shorter syntax for simple functions using  
`fn()`. They automatically capture variables from parent scope and  
consist of a single expression that becomes the return value.  

## Higher-order functions

Functions that accept other functions as parameters or return functions.  

```php
<?php
function applyOperation(array $numbers, callable $operation): array {
    return array_map($operation, $numbers);
}

function createMultiplier(int $factor): callable {
    return fn($x) => $x * $factor;
}

function compose(callable $f, callable $g): callable {
    return fn($x) => $f($g($x));
}

$numbers = [1, 2, 3, 4, 5];

$squared = applyOperation($numbers, fn($x) => $x * $x);
$doubled = applyOperation($numbers, createMultiplier(2));

$addTen = fn($x) => $x + 10;
$multiplyByTwo = fn($x) => $x * 2;
$composed = compose($multiplyByTwo, $addTen);

echo "Original: " . implode(', ', $numbers) . PHP_EOL;
echo "Squared: " . implode(', ', $squared) . PHP_EOL;
echo "Doubled: " . implode(', ', $doubled) . PHP_EOL;
echo "Composed (5): " . $composed(5) . PHP_EOL;    // (5 + 10) * 2 = 30
```

Higher-order functions enable functional programming patterns by  
treating functions as first-class values. They can accept functions  
as parameters or return new functions, enabling powerful abstractions.  

## Recursive functions

Functions that call themselves to solve problems iteratively.  

```php
<?php
function factorial(int $n): int {
    if ($n <= 1) {
        return 1;
    }
    return $n * factorial($n - 1);
}

function fibonacci(int $n): int {
    if ($n <= 1) {
        return $n;
    }
    return fibonacci($n - 1) + fibonacci($n - 2);
}

function flattenArray(array $array): array {
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

echo "Factorial of 5: " . factorial(5) . PHP_EOL;        // 120
echo "Fibonacci of 7: " . fibonacci(7) . PHP_EOL;        // 13

$nested = [1, [2, 3], [[4, 5], 6], 7];
$flat = flattenArray($nested);
echo "Flattened: " . implode(', ', $flat) . PHP_EOL;     // 1, 2, 3, 4, 5, 6, 7
```

Recursive functions solve complex problems by breaking them into  
smaller, similar subproblems. They require a base case to prevent  
infinite recursion and make progress toward that base case.  

## Function scope and global variables

Understanding variable scope and accessing global variables in functions.  

```php
<?php
$globalVar = "I'm global";
$counter = 0;

function demonstrateScope() {
    $localVar = "I'm local";
    global $globalVar, $counter;
    
    echo $localVar . PHP_EOL;               // I'm local
    echo $globalVar . PHP_EOL;              // I'm global
    $counter++;
}

function useGlobalsArray() {
    echo $GLOBALS['globalVar'] . PHP_EOL;   // I'm global
    $GLOBALS['counter'] += 10;
}

function createClosure() {
    $capturedVar = "Captured by closure";
    
    return function() use ($capturedVar) {
        echo $capturedVar . PHP_EOL;
    };
}

demonstrateScope();
echo "Counter after function: $counter" . PHP_EOL;      // 1

useGlobalsArray();
echo "Counter after globals: $counter" . PHP_EOL;       // 11

$closure = createClosure();
$closure();                                             // Captured by closure
```

Function scope determines variable visibility. Local variables exist  
only within functions, while global variables need `global` keyword  
or `$GLOBALS` array access. Closures capture variables with `use`.  

## Static variables in functions

Preserving variable values between function calls.  

```php
<?php
function generateId(): int {
    static $id = 0;
    return ++$id;
}

function fibonacci_optimized(int $n): int {
    static $cache = [];
    
    if (isset($cache[$n])) {
        return $cache[$n];
    }
    
    if ($n <= 1) {
        $cache[$n] = $n;
    } else {
        $cache[$n] = fibonacci_optimized($n - 1) + fibonacci_optimized($n - 2);
    }
    
    return $cache[$n];
}

function requestCounter(): string {
    static $count = 0;
    static $lastReset = null;
    
    if ($lastReset === null) {
        $lastReset = time();
    }
    
    $count++;
    $elapsed = time() - $lastReset;
    
    return "Request #$count (uptime: {$elapsed}s)";
}

echo "ID: " . generateId() . PHP_EOL;                   // ID: 1
echo "ID: " . generateId() . PHP_EOL;                   // ID: 2
echo "ID: " . generateId() . PHP_EOL;                   // ID: 3

echo "Fibonacci 10: " . fibonacci_optimized(10) . PHP_EOL;  // 55
echo "Fibonacci 15: " . fibonacci_optimized(15) . PHP_EOL;  // 610

echo requestCounter() . PHP_EOL;
sleep(1);
echo requestCounter() . PHP_EOL;
```

Static variables retain their values between function calls, making  
them useful for counters, caching, and maintaining state without  
global variables. They're initialized only once.  

## Reference parameters

Passing parameters by reference to modify original variables.  

```php
<?php
function swap(&$a, &$b): void {
    $temp = $a;
    $a = $b;
    $b = $temp;
}

function increment(&$value, int $amount = 1): void {
    $value += $amount;
}

function appendToArray(&$array, ...$values): void {
    foreach ($values as $value) {
        $array[] = $value;
    }
}

function processString(&$str): int {
    $originalLength = strlen($str);
    $str = trim(strtoupper($str));
    return $originalLength;
}

$x = 10;
$y = 20;
echo "Before swap: x=$x, y=$y" . PHP_EOL;
swap($x, $y);
echo "After swap: x=$x, y=$y" . PHP_EOL;               // x=20, y=10

$counter = 5;
increment($counter, 3);
echo "Counter: $counter" . PHP_EOL;                    // Counter: 8

$fruits = ['apple'];
appendToArray($fruits, 'banana', 'cherry', 'date');
print_r($fruits);                                      // [apple, banana, cherry, date]

$text = "  hello world  ";
$length = processString($text);
echo "Original length: $length, Processed: '$text'" . PHP_EOL;
```

Reference parameters use `&` to pass variables by reference, allowing  
functions to modify the original variables rather than working with  
copies. This is useful for performance and when functions need to  
modify multiple values.  

## Generator functions

Creating memory-efficient iterators using yield.  

```php
<?php
function generateNumbers(int $start, int $end, int $step = 1): Generator {
    for ($i = $start; $i <= $end; $i += $step) {
        yield $i;
    }
}

function readLargeFile(string $filename): Generator {
    $file = fopen($filename, 'r');
    if ($file) {
        while (($line = fgets($file)) !== false) {
            yield trim($line);
        }
        fclose($file);
    }
}

function fibonacci_generator(): Generator {
    $a = 0;
    $b = 1;
    
    yield $a;
    yield $b;
    
    while (true) {
        $next = $a + $b;
        yield $next;
        $a = $b;
        $b = $next;
    }
}

// Generate numbers without storing all in memory
foreach (generateNumbers(1, 10, 2) as $number) {
    echo "Number: $number" . PHP_EOL;
}

// Generate Fibonacci sequence
$fib = fibonacci_generator();
for ($i = 0; $i < 10; $i++) {
    echo "Fib $i: " . $fib->current() . PHP_EOL;
    $fib->next();
}

// Create a temporary file for demonstration
file_put_contents('/tmp/test.txt', "Line 1\nLine 2\nLine 3\n");
foreach (readLargeFile('/tmp/test.txt') as $lineNumber => $line) {
    echo "Line " . ($lineNumber + 1) . ": $line" . PHP_EOL;
}
```

Generators use `yield` to produce values on-demand, creating memory-  
efficient iterators. They maintain state between yields, making them  
perfect for processing large datasets or infinite sequences.  

## Callback functions

Using functions as parameters for flexible behavior.  

```php
<?php
function processData(array $data, callable $validator, callable $transformer): array {
    $result = [];
    
    foreach ($data as $item) {
        if ($validator($item)) {
            $result[] = $transformer($item);
        }
    }
    
    return $result;
}

function sortBy(array $array, callable $keyExtractor): array {
    $sorted = $array;
    usort($sorted, fn($a, $b) => $keyExtractor($a) <=> $keyExtractor($b));
    return $sorted;
}

function executeWithRetry(callable $operation, int $maxAttempts = 3): mixed {
    $attempts = 0;
    
    while ($attempts < $maxAttempts) {
        try {
            return $operation();
        } catch (Exception $e) {
            $attempts++;
            if ($attempts >= $maxAttempts) {
                throw $e;
            }
            echo "Attempt $attempts failed, retrying..." . PHP_EOL;
            sleep(1);
        }
    }
}

$numbers = [-5, -2, 0, 3, 8, -1, 12];

$positives = processData(
    $numbers,
    fn($n) => $n > 0,           // validator
    fn($n) => $n * 2            // transformer
);

$users = [
    ['name' => 'Charlie', 'age' => 35],
    ['name' => 'Alice', 'age' => 25],
    ['name' => 'Bob', 'age' => 30]
];

$sortedByAge = sortBy($users, fn($user) => $user['age']);
$sortedByName = sortBy($users, fn($user) => $user['name']);

echo "Positive doubled: " . implode(', ', $positives) . PHP_EOL;
print_r($sortedByAge);
print_r($sortedByName);

// Simulate operation that might fail
$result = executeWithRetry(fn() => random_int(1, 10) > 8 ? "Success!" : throw new Exception("Failed"));
echo $result . PHP_EOL;
```

Callback functions enable flexible, reusable code by allowing functions  
to accept behavior as parameters. They're essential for functional  
programming patterns and event-driven programming.  

## Built-in string functions

Leveraging PHP's extensive string manipulation capabilities.  

```php
<?php
function analyzeText(string $text): array {
    return [
        'length' => strlen($text),
        'word_count' => str_word_count($text),
        'upper' => strtoupper($text),
        'lower' => strtolower($text),
        'title' => ucwords($text),
        'reversed' => strrev($text),
        'first_10' => substr($text, 0, 10),
        'contains_php' => str_contains($text, 'PHP'),
        'starts_with_the' => str_starts_with($text, 'The'),
        'ends_with_period' => str_ends_with($text, '.')
    ];
}

function formatString(string $template, array $values): string {
    $formatted = $template;
    
    foreach ($values as $key => $value) {
        $formatted = str_replace("{{$key}}", $value, $formatted);
    }
    
    return $formatted;
}

function cleanInput(string $input): string {
    $cleaned = trim($input);                    // Remove whitespace
    $cleaned = strip_tags($cleaned);            // Remove HTML tags  
    $cleaned = htmlspecialchars($cleaned);      // Escape HTML entities
    return $cleaned;
}

$text = "The quick brown fox jumps over the lazy dog.";
$analysis = analyzeText($text);

foreach ($analysis as $key => $value) {
    echo ucfirst(str_replace('_', ' ', $key)) . ": ";
    echo is_bool($value) ? ($value ? 'Yes' : 'No') : $value;
    echo PHP_EOL;
}

$template = "Hello {name}, your order #{order} for {amount} is ready!";
$message = formatString($template, [
    'name' => 'John',
    'order' => '12345',
    'amount' => '$25.99'
]);

echo PHP_EOL . $message . PHP_EOL;

$userInput = "  <script>alert('xss')</script>Hello & goodbye  ";
echo "Cleaned input: '" . cleanInput($userInput) . "'" . PHP_EOL;
```

PHP provides rich string functions for text processing, validation,  
and transformation. These functions handle common tasks like case  
conversion, searching, sanitization, and formatting efficiently.  

## Built-in array functions

Utilizing PHP's powerful array processing capabilities.  

```php
<?php
function processNumbers(array $numbers): array {
    return [
        'original' => $numbers,
        'sum' => array_sum($numbers),
        'average' => array_sum($numbers) / count($numbers),
        'min' => min($numbers),
        'max' => max($numbers),
        'sorted_asc' => [...$numbers, ...[]],  // Copy before sort
        'sorted_desc' => [...$numbers, ...[]],
        'evens' => array_filter($numbers, fn($n) => $n % 2 === 0),
        'doubled' => array_map(fn($n) => $n * 2, $numbers),
        'unique' => array_unique($numbers)
    ];
}

function groupBy(array $items, callable $keyExtractor): array {
    return array_reduce($items, function($groups, $item) use ($keyExtractor) {
        $key = $keyExtractor($item);
        $groups[$key][] = $item;
        return $groups;
    }, []);
}

function arrayOperations(): void {
    $fruits = ['apple', 'banana', 'cherry'];
    $vegetables = ['carrot', 'broccoli'];
    
    // Array merging and operations
    $food = array_merge($fruits, $vegetables);
    $combined = [...$fruits, ...$vegetables];  // Spread operator
    
    // Array keys and values
    $inventory = [
        'apples' => 50,
        'bananas' => 30,
        'cherries' => 25
    ];
    
    $items = array_keys($inventory);
    $quantities = array_values($inventory);
    $flipped = array_flip($inventory);
    
    echo "Food items: " . implode(', ', $food) . PHP_EOL;
    echo "Items: " . implode(', ', $items) . PHP_EOL;
    echo "Quantities: " . implode(', ', $quantities) . PHP_EOL;
    print_r($flipped);
}

$numbers = [5, 2, 8, 2, 9, 1, 8, 3];
$results = processNumbers($numbers);

// Sort the copies
sort($results['sorted_asc']);
rsort($results['sorted_desc']);

foreach ($results as $key => $value) {
    if (is_array($value)) {
        echo ucfirst(str_replace('_', ' ', $key)) . ": [" . implode(', ', $value) . "]" . PHP_EOL;
    } else {
        echo ucfirst(str_replace('_', ' ', $key)) . ": $value" . PHP_EOL;
    }
}

$students = [
    ['name' => 'Alice', 'grade' => 'A', 'subject' => 'Math'],
    ['name' => 'Bob', 'grade' => 'B', 'subject' => 'Math'],
    ['name' => 'Charlie', 'grade' => 'A', 'subject' => 'Science'],
    ['name' => 'Diana', 'grade' => 'B', 'subject' => 'Science']
];

$byGrade = groupBy($students, fn($s) => $s['grade']);
$bySubject = groupBy($students, fn($s) => $s['subject']);

echo PHP_EOL . "Grouped by grade:" . PHP_EOL;
print_r($byGrade);

arrayOperations();
```

Array functions are fundamental to PHP programming, providing tools  
for sorting, filtering, transforming, and analyzing data structures.  
They enable functional programming patterns and efficient data processing.  

## Mathematical functions

Using PHP's mathematical functions for calculations and number processing.  

```php
<?php
function calculateStatistics(array $values): array {
    $count = count($values);
    $sum = array_sum($values);
    $mean = $sum / $count;
    
    // Calculate standard deviation
    $variance = array_sum(array_map(fn($x) => pow($x - $mean, 2), $values)) / $count;
    $stdDev = sqrt($variance);
    
    return [
        'count' => $count,
        'sum' => $sum,
        'mean' => round($mean, 2),
        'median' => calculateMedian($values),
        'std_deviation' => round($stdDev, 2),
        'min' => min($values),
        'max' => max($values),
        'range' => max($values) - min($values)
    ];
}

function calculateMedian(array $values): float {
    sort($values);
    $count = count($values);
    $middle = floor($count / 2);
    
    if ($count % 2 === 0) {
        return ($values[$middle - 1] + $values[$middle]) / 2;
    } else {
        return $values[$middle];
    }
}

function geometryCalculations(): void {
    $radius = 5;
    $width = 10;
    $height = 8;
    
    // Circle calculations
    $circumference = 2 * pi() * $radius;
    $area = pi() * pow($radius, 2);
    
    // Rectangle calculations
    $rectArea = $width * $height;
    $diagonal = sqrt(pow($width, 2) + pow($height, 2));
    
    echo "Circle (radius $radius):" . PHP_EOL;
    echo "  Circumference: " . round($circumference, 2) . PHP_EOL;
    echo "  Area: " . round($area, 2) . PHP_EOL;
    
    echo "Rectangle ({$width}x{$height}):" . PHP_EOL;
    echo "  Area: $rectArea" . PHP_EOL;
    echo "  Diagonal: " . round($diagonal, 2) . PHP_EOL;
}

function numberUtilities(): void {
    $numbers = [-15, -7.5, 0, 3.7, 12];
    
    echo "Number utilities:" . PHP_EOL;
    foreach ($numbers as $num) {
        echo "  $num: ";
        echo "abs=" . abs($num) . ", ";
        echo "ceil=" . ceil($num) . ", ";
        echo "floor=" . floor($num) . ", ";
        echo "round=" . round($num) . PHP_EOL;
    }
    
    // Random numbers
    echo "Random integer (1-100): " . random_int(1, 100) . PHP_EOL;
    echo "Random float (0-1): " . round(mt_rand() / mt_getrandmax(), 4) . PHP_EOL;
    
    // Base conversions
    $decimal = 255;
    echo "Decimal $decimal: hex=" . dechex($decimal) . ", binary=" . decbin($decimal) . PHP_EOL;
}

$dataset = [23, 45, 67, 34, 89, 12, 56, 78, 90, 43];
$stats = calculateStatistics($dataset);

echo "Dataset: [" . implode(', ', $dataset) . "]" . PHP_EOL;
foreach ($stats as $key => $value) {
    echo ucfirst(str_replace('_', ' ', $key)) . ": $value" . PHP_EOL;
}

echo PHP_EOL;
geometryCalculations();

echo PHP_EOL;
numberUtilities();
```

Mathematical functions provide essential tools for scientific computing,  
statistics, geometry, and numerical analysis. PHP includes functions  
for basic math, trigonometry, logarithms, and random number generation.  

## Date and time functions

Working with dates, times, and temporal calculations.  

```php
<?php
function formatDates(): void {
    $now = new DateTime();
    $timestamp = time();
    
    echo "Current date/time formats:" . PHP_EOL;
    echo "  Default: " . $now->format('Y-m-d H:i:s') . PHP_EOL;
    echo "  ISO 8601: " . $now->format('c') . PHP_EOL;
    echo "  RFC 2822: " . $now->format('r') . PHP_EOL;
    echo "  Unix timestamp: " . $timestamp . PHP_EOL;
    echo "  Human readable: " . date('l, F j, Y \a\t g:i A', $timestamp) . PHP_EOL;
}

function calculateAge(string $birthdate): array {
    $birth = new DateTime($birthdate);
    $now = new DateTime();
    $interval = $birth->diff($now);
    
    return [
        'years' => $interval->y,
        'months' => $interval->m,
        'days' => $interval->d,
        'total_days' => $birth->diff($now)->days,
        'next_birthday' => $birth->modify('+' . ($interval->y + 1) . ' years')->format('Y-m-d')
    ];
}

function businessDays(DateTime $start, DateTime $end): int {
    $days = 0;
    $current = clone $start;
    
    while ($current <= $end) {
        $dayOfWeek = $current->format('N');  // 1 = Monday, 7 = Sunday
        if ($dayOfWeek < 6) {  // Monday to Friday
            $days++;
        }
        $current->add(new DateInterval('P1D'));
    }
    
    return $days;
}

function timezoneDemo(): void {
    $utc = new DateTime('2024-01-01 12:00:00', new DateTimeZone('UTC'));
    $newYork = clone $utc;
    $newYork->setTimezone(new DateTimeZone('America/New_York'));
    $tokyo = clone $utc;
    $tokyo->setTimezone(new DateTimeZone('Asia/Tokyo'));
    
    echo "Timezone demonstration:" . PHP_EOL;
    echo "  UTC: " . $utc->format('Y-m-d H:i:s T') . PHP_EOL;
    echo "  New York: " . $newYork->format('Y-m-d H:i:s T') . PHP_EOL;
    echo "  Tokyo: " . $tokyo->format('Y-m-d H:i:s T') . PHP_EOL;
}

formatDates();

echo PHP_EOL;
$age = calculateAge('1990-06-15');
echo "Age calculation for birthdate 1990-06-15:" . PHP_EOL;
foreach ($age as $key => $value) {
    echo "  " . ucfirst(str_replace('_', ' ', $key)) . ": $value" . PHP_EOL;
}

echo PHP_EOL;
$startDate = new DateTime('2024-01-01');
$endDate = new DateTime('2024-01-31');
$workDays = businessDays($startDate, $endDate);
echo "Business days in January 2024: $workDays" . PHP_EOL;

echo PHP_EOL;
timezoneDemo();

// Date arithmetic
echo PHP_EOL . "Date arithmetic:" . PHP_EOL;
$date = new DateTime('2024-03-15');
echo "Original: " . $date->format('Y-m-d') . PHP_EOL;
$date->add(new DateInterval('P1M'));  // Add 1 month
echo "Plus 1 month: " . $date->format('Y-m-d') . PHP_EOL;
$date->sub(new DateInterval('P2W'));  // Subtract 2 weeks
echo "Minus 2 weeks: " . $date->format('Y-m-d') . PHP_EOL;
```

Date and time functions handle temporal data, formatting, calculations,  
and timezone conversions. PHP's DateTime class provides object-oriented  
date manipulation with support for intervals and timezone operations.  

## File system functions

Reading, writing, and manipulating files and directories.  

```php
<?php
function fileOperations(string $filename): array {
    $info = [];
    
    if (file_exists($filename)) {
        $info['exists'] = true;
        $info['size'] = filesize($filename);
        $info['type'] = filetype($filename);
        $info['readable'] = is_readable($filename);
        $info['writable'] = is_writable($filename);
        $info['modified'] = date('Y-m-d H:i:s', filemtime($filename));
        $info['extension'] = pathinfo($filename, PATHINFO_EXTENSION);
        $info['basename'] = basename($filename);
        $info['dirname'] = dirname($filename);
    } else {
        $info['exists'] = false;
    }
    
    return $info;
}

function processLogFile(string $filename): array {
    if (!file_exists($filename)) {
        return ['error' => 'File not found'];
    }
    
    $lines = file($filename, FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES);
    $stats = [
        'total_lines' => count($lines),
        'error_lines' => 0,
        'warning_lines' => 0,
        'info_lines' => 0,
        'first_line' => $lines[0] ?? '',
        'last_line' => end($lines) ?: '',
        'size_bytes' => filesize($filename)
    ];
    
    foreach ($lines as $line) {
        if (str_contains($line, 'ERROR')) {
            $stats['error_lines']++;
        } elseif (str_contains($line, 'WARNING')) {
            $stats['warning_lines']++;
        } elseif (str_contains($line, 'INFO')) {
            $stats['info_lines']++;
        }
    }
    
    return $stats;
}

function directoryListing(string $path): array {
    $items = [];
    
    if (!is_dir($path)) {
        return ['error' => 'Directory not found'];
    }
    
    $iterator = new RecursiveDirectoryIterator($path);
    $recursiveIterator = new RecursiveIteratorIterator($iterator);
    
    foreach ($recursiveIterator as $file) {
        if ($file->isFile()) {
            $items[] = [
                'name' => $file->getFilename(),
                'path' => $file->getPathname(),
                'size' => $file->getSize(),
                'extension' => $file->getExtension(),
                'modified' => date('Y-m-d H:i:s', $file->getMTime())
            ];
        }
    }
    
    return $items;
}

// Create sample files for demonstration
$tempDir = '/tmp/php_examples';
if (!is_dir($tempDir)) {
    mkdir($tempDir, 0755, true);
}

$sampleFile = $tempDir . '/sample.txt';
$logFile = $tempDir . '/app.log';

// Create sample text file
file_put_contents($sampleFile, "Hello, World!\nThis is a sample file.\nCreated by PHP functions example.");

// Create sample log file
$logEntries = [
    "2024-01-01 10:00:00 INFO Application started",
    "2024-01-01 10:01:15 INFO User logged in: user123",
    "2024-01-01 10:05:30 WARNING Low memory detected",
    "2024-01-01 10:10:45 ERROR Database connection failed",
    "2024-01-01 10:11:00 INFO Retrying database connection",
    "2024-01-01 10:11:30 INFO Database connection restored"
];

file_put_contents($logFile, implode("\n", $logEntries));

// Demonstrate file operations
echo "File information for $sampleFile:" . PHP_EOL;
$fileInfo = fileOperations($sampleFile);
foreach ($fileInfo as $key => $value) {
    echo "  " . ucfirst($key) . ": " . (is_bool($value) ? ($value ? 'Yes' : 'No') : $value) . PHP_EOL;
}

echo PHP_EOL . "Log file analysis:" . PHP_EOL;
$logStats = processLogFile($logFile);
foreach ($logStats as $key => $value) {
    echo "  " . ucfirst(str_replace('_', ' ', $key)) . ": $value" . PHP_EOL;
}

echo PHP_EOL . "Directory contents (/tmp):" . PHP_EOL;
$dirContents = directoryListing('/tmp');
if (isset($dirContents['error'])) {
    echo "  " . $dirContents['error'] . PHP_EOL;
} else {
    $count = min(5, count($dirContents));  // Show only first 5 files
    for ($i = 0; $i < $count; $i++) {
        $file = $dirContents[$i];
        echo "  {$file['name']} ({$file['size']} bytes, {$file['modified']})" . PHP_EOL;
    }
    if (count($dirContents) > 5) {
        echo "  ... and " . (count($dirContents) - 5) . " more files" . PHP_EOL;
    }
}
```

File system functions provide comprehensive file and directory  
manipulation capabilities. They enable reading, writing, analyzing  
file properties, and traversing directory structures efficiently.  

## Error handling in functions

Implementing robust error handling and exception management.  

```php
<?php
function divideWithValidation(float $dividend, float $divisor): float {
    if ($divisor === 0.0) {
        throw new InvalidArgumentException("Division by zero is not allowed");
    }
    
    if (!is_finite($dividend) || !is_finite($divisor)) {
        throw new InvalidArgumentException("Arguments must be finite numbers");
    }
    
    return $dividend / $divisor;
}

function validateUser(array $userData): array {
    $errors = [];
    $validated = [];
    
    // Validate email
    if (empty($userData['email'])) {
        $errors['email'] = 'Email is required';
    } elseif (!filter_var($userData['email'], FILTER_VALIDATE_EMAIL)) {
        $errors['email'] = 'Invalid email format';
    } else {
        $validated['email'] = $userData['email'];
    }
    
    // Validate age
    if (empty($userData['age'])) {
        $errors['age'] = 'Age is required';
    } elseif (!is_numeric($userData['age']) || $userData['age'] < 0 || $userData['age'] > 150) {
        $errors['age'] = 'Age must be between 0 and 150';
    } else {
        $validated['age'] = (int)$userData['age'];
    }
    
    // Validate name
    if (empty($userData['name'])) {
        $errors['name'] = 'Name is required';
    } elseif (strlen($userData['name']) < 2) {
        $errors['name'] = 'Name must be at least 2 characters';
    } else {
        $validated['name'] = trim($userData['name']);
    }
    
    return ['errors' => $errors, 'data' => $validated];
}

function executeWithErrorHandling(callable $operation, string $operationName): mixed {
    try {
        echo "Executing: $operationName" . PHP_EOL;
        $result = $operation();
        echo "Success: $operationName completed" . PHP_EOL;
        return $result;
    } catch (TypeError $e) {
        echo "Type Error in $operationName: " . $e->getMessage() . PHP_EOL;
        return null;
    } catch (ArgumentCountError $e) {
        echo "Argument Error in $operationName: " . $e->getMessage() . PHP_EOL;
        return null;
    } catch (InvalidArgumentException $e) {
        echo "Invalid Argument in $operationName: " . $e->getMessage() . PHP_EOL;
        return null;
    } catch (Exception $e) {
        echo "General Error in $operationName: " . $e->getMessage() . PHP_EOL;
        return null;
    } catch (Error $e) {
        echo "Fatal Error in $operationName: " . $e->getMessage() . PHP_EOL;
        return null;
    }
}

function customErrorHandler(int $errno, string $errstr, string $errfile, int $errline): bool {
    $errorTypes = [
        E_ERROR => 'Fatal Error',
        E_WARNING => 'Warning',
        E_NOTICE => 'Notice',
        E_USER_ERROR => 'User Error',
        E_USER_WARNING => 'User Warning',
        E_USER_NOTICE => 'User Notice'
    ];
    
    $type = $errorTypes[$errno] ?? 'Unknown Error';
    echo "[$type] $errstr in $errfile on line $errline" . PHP_EOL;
    
    return true; // Don't execute PHP's internal error handler
}

// Set custom error handler
set_error_handler('customErrorHandler');

// Test division function
echo "Testing division function:" . PHP_EOL;
executeWithErrorHandling(fn() => divideWithValidation(10, 2), "Valid division");
executeWithErrorHandling(fn() => divideWithValidation(10, 0), "Division by zero");
executeWithErrorHandling(fn() => divideWithValidation(INF, 5), "Infinite dividend");

echo PHP_EOL . "Testing user validation:" . PHP_EOL;

$testUsers = [
    ['email' => 'john@example.com', 'age' => 25, 'name' => 'John Doe'],
    ['email' => 'invalid-email', 'age' => 30, 'name' => 'Jane'],
    ['email' => '', 'age' => -5, 'name' => ''],
    ['email' => 'bob@test.com', 'age' => 'not-a-number', 'name' => 'Bob Smith']
];

foreach ($testUsers as $index => $userData) {
    echo "User " . ($index + 1) . ":" . PHP_EOL;
    $result = validateUser($userData);
    
    if (empty($result['errors'])) {
        echo "  Validation successful!" . PHP_EOL;
        echo "  Data: " . json_encode($result['data']) . PHP_EOL;
    } else {
        echo "  Validation errors:" . PHP_EOL;
        foreach ($result['errors'] as $field => $error) {
            echo "    $field: $error" . PHP_EOL;
        }
    }
    echo PHP_EOL;
}

// Demonstrate error handling with warnings
echo "Triggering a warning:" . PHP_EOL;
$result = 5 / 0;  // This will trigger a warning but not stop execution

// Restore default error handler
restore_error_handler();
```

Error handling ensures functions behave predictably when encountering  
invalid inputs or exceptional conditions. PHP provides exceptions,  
custom error handlers, and validation patterns for robust applications.  

## Function documentation with DocBlocks

Writing comprehensive documentation for functions using DocBlock format.  

```php
<?php
/**
 * Calculates the compound interest for an investment
 * 
 * This function computes compound interest using the standard formula:
 * A = P(1 + r/n)^(nt) where A is final amount, P is principal,
 * r is annual interest rate, n is compounding frequency, t is time
 * 
 * @param float $principal The initial investment amount
 * @param float $rate Annual interest rate as a decimal (0.05 for 5%)
 * @param int $compoundingFrequency Times per year interest compounds
 * @param float $years Number of years to calculate for
 * 
 * @return array Detailed breakdown of compound interest calculation
 * 
 * @throws InvalidArgumentException When any parameter is invalid
 * 
 * @example
 * // Calculate $1000 at 5% compounded quarterly for 10 years
 * $result = calculateCompoundInterest(1000, 0.05, 4, 10);
 * echo $result['final_amount']; // 1643.62
 * 
 * @since 1.0.0
 * @author John Doe <john@example.com>
 */
function calculateCompoundInterest(
    float $principal, 
    float $rate, 
    int $compoundingFrequency, 
    float $years
): array {
    // Validate inputs
    if ($principal <= 0) {
        throw new InvalidArgumentException("Principal must be positive");
    }
    if ($rate < 0) {
        throw new InvalidArgumentException("Interest rate cannot be negative");
    }
    if ($compoundingFrequency <= 0) {
        throw new InvalidArgumentException("Compounding frequency must be positive");
    }
    if ($years < 0) {
        throw new InvalidArgumentException("Years cannot be negative");
    }
    
    // Calculate compound interest
    $finalAmount = $principal * pow(
        (1 + $rate / $compoundingFrequency), 
        $compoundingFrequency * $years
    );
    
    $interestEarned = $finalAmount - $principal;
    $effectiveRate = ($finalAmount / $principal) - 1;
    
    return [
        'principal' => $principal,
        'annual_rate' => $rate * 100 . '%',
        'compounding_frequency' => $compoundingFrequency,
        'years' => $years,
        'final_amount' => round($finalAmount, 2),
        'interest_earned' => round($interestEarned, 2),
        'effective_annual_rate' => round($effectiveRate * 100, 3) . '%',
        'total_return_percentage' => round(($interestEarned / $principal) * 100, 2) . '%'
    ];
}

/**
 * Processes a collection of items using provided callbacks
 * 
 * This utility function applies validation, transformation, and filtering
 * operations to an array of items in a single pass for efficiency.
 * 
 * @param array $items The collection of items to process
 * @param callable|null $validator Optional function to validate each item
 * @param callable|null $transformer Optional function to transform items  
 * @param callable|null $filter Optional function to filter final results
 * 
 * @return array{processed: array, skipped: array, errors: array} Processing results
 * 
 * @throws TypeError When callbacks are not callable
 * 
 * @example
 * $numbers = [1, 2, 3, 4, 5];
 * $result = processCollection(
 *     $numbers,
 *     fn($n) => is_numeric($n),      // validator
 *     fn($n) => $n * 2,              // transformer
 *     fn($n) => $n > 5               // filter
 * );
 * 
 * @psalm-template T
 * @psalm-param array<T> $items
 * @psalm-param callable(T): bool $validator
 * @psalm-param callable(T): mixed $transformer
 * @psalm-param callable(mixed): bool $filter
 * 
 * @version 2.1.0
 */
function processCollection(
    array $items,
    ?callable $validator = null,
    ?callable $transformer = null,
    ?callable $filter = null
): array {
    $result = [
        'processed' => [],
        'skipped' => [],
        'errors' => []
    ];
    
    foreach ($items as $index => $item) {
        try {
            // Validation step
            if ($validator && !$validator($item)) {
                $result['skipped'][] = ['index' => $index, 'item' => $item];
                continue;
            }
            
            // Transformation step
            $processedItem = $transformer ? $transformer($item) : $item;
            
            // Filtering step
            if ($filter && !$filter($processedItem)) {
                $result['skipped'][] = ['index' => $index, 'item' => $item];
                continue;
            }
            
            $result['processed'][] = $processedItem;
            
        } catch (Exception $e) {
            $result['errors'][] = [
                'index' => $index,
                'item' => $item,
                'error' => $e->getMessage()
            ];
        }
    }
    
    return $result;
}

// Demonstration of documented functions
echo "Compound Interest Calculation:" . PHP_EOL;
try {
    $investment = calculateCompoundInterest(1000, 0.05, 4, 10);
    
    foreach ($investment as $key => $value) {
        echo "  " . ucfirst(str_replace('_', ' ', $key)) . ": $value" . PHP_EOL;
    }
} catch (InvalidArgumentException $e) {
    echo "Error: " . $e->getMessage() . PHP_EOL;
}

echo PHP_EOL . "Collection Processing:" . PHP_EOL;
$mixedData = [1, '2', 3.5, 'invalid', 5, -1, 10];

$result = processCollection(
    $mixedData,
    fn($item) => is_numeric($item),           // Only numeric values
    fn($item) => (float)$item * 2,            // Double the values
    fn($item) => $item > 0                    // Only positive results
);

echo "Processed: [" . implode(', ', $result['processed']) . "]" . PHP_EOL;
echo "Skipped: " . count($result['skipped']) . " items" . PHP_EOL;
echo "Errors: " . count($result['errors']) . " items" . PHP_EOL;

if (!empty($result['errors'])) {
    foreach ($result['errors'] as $error) {
        echo "  Error at index {$error['index']}: {$error['error']}" . PHP_EOL;
    }
}
```

DocBlock documentation provides comprehensive function descriptions,  
parameter specifications, return value details, and usage examples.  
Well-documented code is easier to maintain, test, and use by other  
developers in your team.  
