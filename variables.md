# Variables

This chapter covers 20 comprehensive PHP variable examples demonstrating  
the complete scope of variable handling in PHP 8.4. These examples  
progress from basic variable concepts to advanced variable patterns,  
including scope, references, and dynamic features. Each example includes  
clear explanations to help you understand variable behavior and apply  
them effectively in your code.  

## Basic variable declaration

Creating and assigning values to variables with different data types.  

```php
<?php

$message = "Hello there!";
$count = 42;
$price = 19.99;
$isActive = true;
$data = null;

echo $message . "\n";
echo "Count: $count\n";
echo "Price: $price\n";
echo "Active: " . ($isActive ? "yes" : "no") . "\n";
echo "Data: " . ($data ?? "none") . "\n";
```

Variables in PHP start with `$` followed by a valid identifier.  
PHP is dynamically typed, so variables can hold different data  
types without explicit declaration. Assignment is done with `=`.  

## Variable naming conventions

Valid and invalid variable names following PHP naming rules.  

```php
<?php

// Valid variable names
$userName = "Alice";
$_private = "secret";
$counter1 = 10;
$MAX_SIZE = 1000;
$camelCase = "preferred";
$snake_case = "also valid";

// Variables are case-sensitive
$name = "first";
$Name = "second";
$NAME = "third";

echo "$name, $Name, $NAME\n";

// Invalid names (commented out)
// $2invalid = "starts with number";
// $invalid-name = "contains hyphen";
// $invalid space = "contains space";
```

Variable names must start with letter or underscore, followed by  
letters, numbers, or underscores. They are case-sensitive, so  
`$name` and `$Name` are different variables.  

## Dynamic typing and type juggling

How PHP handles automatic type conversion between different data types.  

```php
<?php

$value = "123";
echo "String: $value\n";

$value = $value + 0;
echo "Number: $value\n";

$value = $value . " items";
echo "String again: $value\n";

// Type juggling in operations
$num1 = "10";
$num2 = 5;
$result = $num1 + $num2;
echo "10 + 5 = $result\n";

// Boolean context
$empty = "";
$filled = "data";
echo "Empty is " . ($empty ? "true" : "false") . "\n";
echo "Filled is " . ($filled ? "true" : "false") . "\n";
```

PHP automatically converts between types based on context. Strings  
containing numbers convert to numeric values in arithmetic operations.  
Empty strings, zero, and null evaluate to false in boolean context.  

## Variable scope basics

Understanding local, global, and function parameter scope.  

```php
<?php

$globalVar = "I'm global";

function demonstrateScope($param) {
    $localVar = "I'm local";
    global $globalVar;
    
    echo "Parameter: $param\n";
    echo "Local: $localVar\n";
    echo "Global: $globalVar\n";
    
    $globalVar = "Modified globally";
}

demonstrateScope("passed value");
echo "Global after function: $globalVar\n";

// Local variable doesn't exist outside function
// echo $localVar; // Would cause error
```

Variables have scope that determines where they can be accessed.  
Local variables exist only within functions, while global variables  
need the `global` keyword to access inside functions.  

## Static variables

Preserving variable values between function calls.  

```php
<?php

function counter() {
    static $count = 0;
    $count++;
    echo "Count: $count\n";
    return $count;
}

function fibonacci($n) {
    static $cache = [];
    
    if (isset($cache[$n])) {
        return $cache[$n];
    }
    
    if ($n <= 1) {
        return $cache[$n] = $n;
    }
    
    return $cache[$n] = fibonacci($n - 1) + fibonacci($n - 2);
}

counter(); // Count: 1
counter(); // Count: 2
counter(); // Count: 3

echo "Fibonacci 10: " . fibonacci(10) . "\n";
echo "Fibonacci 15: " . fibonacci(15) . "\n";
```

Static variables retain their values between function calls, making  
them useful for counters, caches, and maintaining state without  
using global variables. They're initialized only once.  

## Variable references

Creating aliases to variables using reference operator.  

```php
<?php

$original = "Initial value";
$reference = &$original;

echo "Original: $original\n";
echo "Reference: $reference\n";

$reference = "Modified through reference";
echo "Original after change: $original\n";

// References in arrays
$array = [1, 2, 3];
$ref = &$array[1];
$ref = 99;
print_r($array); // [1, 99, 3]

// Unsetting reference doesn't affect original
unset($reference);
echo "Original still exists: $original\n";
```

The reference operator `&` creates variable aliases pointing to the  
same memory location. Changes through any reference affect all  
aliases. References are useful for function parameters and arrays.  

## Variable variables

Dynamically accessing variables using variable names stored in other variables.  

```php
<?php

$varName = "message";
$message = "Hello there!";

// Variable variable syntax
echo $$varName . "\n"; // Same as echo $message;

// With arrays
$fruit = "apple";
$apple = "red";
$$fruit = "green"; // Changes $apple to "green"
echo "Apple color: $apple\n";

// Multiple levels
$level1 = "level2";
$level2 = "target";
$target = "Found it!";
echo $$$level1 . "\n"; // Same as $target

// Dynamic property access
class Product {
    public $name = "Laptop";
    public $price = 999;
}

$product = new Product();
$property = "name";
echo $product->$property . "\n"; // Laptop
```

Variable variables use `$$variable` syntax to access variables whose  
names are stored in other variables. This enables dynamic programming  
patterns but should be used carefully to maintain code readability.  

## Superglobal variables

Accessing built-in global arrays available in all scopes.  

```php
<?php

// Simulating some superglobals (normally populated by PHP)
$_GET['name'] = 'Alice';
$_POST['action'] = 'submit';
$_COOKIE['session'] = 'abc123';
$_SERVER['HTTP_HOST'] = 'example.com';

function displayRequest() {
    // Superglobals accessible without global keyword
    echo "GET name: " . ($_GET['name'] ?? 'none') . "\n";
    echo "POST action: " . ($_POST['action'] ?? 'none') . "\n";
    echo "Cookie session: " . ($_COOKIE['session'] ?? 'none') . "\n";
    echo "Server host: " . ($_SERVER['HTTP_HOST'] ?? 'none') . "\n";
    
    // $GLOBALS contains all global variables
    $GLOBALS['customGlobal'] = 'accessible everywhere';
}

displayRequest();
echo "Custom global: " . ($customGlobal ?? 'not set') . "\n";
```

Superglobal variables like `$_GET`, `$_POST`, `$_COOKIE`, and `$_SERVER`  
are automatically available in all scopes without needing `global`  
keyword. `$GLOBALS` provides access to all global variables.  

## Variable functions

Using variables to store and call function names dynamically.  

```php
<?php

function greet($name) {
    return "Hello, $name!";
}

function calculate($a, $b) {
    return $a + $b;
}

// Store function names in variables
$funcName = 'greet';
$mathFunc = 'calculate';

// Call functions through variables
echo $funcName('Alice') . "\n";
echo $mathFunc(10, 5) . "\n";

// Dynamic function selection
$operations = [
    'add' => fn($a, $b) => $a + $b,
    'multiply' => fn($a, $b) => $a * $b,
    'greet' => fn($name) => "Hello, $name!"
];

$operation = 'add';
if (isset($operations[$operation])) {
    echo "Result: " . $operations[$operation](7, 3) . "\n";
}

// Method names as variables
class Calculator {
    public function add($a, $b) { return $a + $b; }
    public function subtract($a, $b) { return $a - $b; }
}

$calc = new Calculator();
$method = 'add';
echo "Calculator result: " . $calc->$method(15, 8) . "\n";
```

Variable functions allow storing function or method names in variables  
and calling them dynamically. This enables flexible callback systems  
and dynamic method dispatch patterns.  

## Variable unpacking and destructuring

Extracting values from arrays and assigning them to variables.  

```php
<?php

// Array destructuring
$colors = ['red', 'green', 'blue'];
[$primary, $secondary, $tertiary] = $colors;

echo "Primary: $primary\n";
echo "Secondary: $secondary\n";
echo "Tertiary: $tertiary\n";

// Associative array destructuring
$person = ['name' => 'Bob', 'age' => 30, 'city' => 'Prague'];
['name' => $name, 'age' => $age] = $person;

echo "Name: $name, Age: $age\n";

// Skipping elements
$numbers = [1, 2, 3, 4, 5];
[$first, , $third, , $fifth] = $numbers;

echo "First: $first, Third: $third, Fifth: $fifth\n";

// Rest operator
$data = ['Alice', 'Bob', 'Charlie', 'David'];
[$leader, ...$team] = $data;

echo "Leader: $leader\n";
echo "Team: " . implode(', ', $team) . "\n";

// Nested destructuring
$nested = [['x' => 1, 'y' => 2], ['x' => 3, 'y' => 4]];
[['x' => $x1, 'y' => $y1], ['x' => $x2]] = $nested;

echo "Point 1: ($x1, $y1), Point 2 x: $x2\n";
```

Array destructuring assigns multiple variables from array elements  
in one operation. It supports associative arrays, skipping elements,  
and the rest operator for collecting remaining elements.  

## Variable validation and type checking

Checking variable types and validating values safely.  

```php
<?php

$data = [
    'string' => 'Hello',
    'integer' => 42,
    'float' => 3.14,
    'boolean' => true,
    'null' => null,
    'array' => [1, 2, 3]
];

foreach ($data as $key => $value) {
    echo "$key: ";
    
    // Type checking functions
    if (is_string($value)) {
        echo "string(" . strlen($value) . ") '$value'";
    } elseif (is_int($value)) {
        echo "integer($value)";
    } elseif (is_float($value)) {
        echo "float($value)";
    } elseif (is_bool($value)) {
        echo "boolean(" . ($value ? 'true' : 'false') . ")";
    } elseif (is_null($value)) {
        echo "null";
    } elseif (is_array($value)) {
        echo "array(" . count($value) . ")";
    }
    
    echo " - gettype: " . gettype($value) . "\n";
}

// Type validation with filter functions
$inputs = ['123', '45.67', 'true', 'false', 'invalid'];

foreach ($inputs as $input) {
    $int = filter_var($input, FILTER_VALIDATE_INT);
    $float = filter_var($input, FILTER_VALIDATE_FLOAT);
    $bool = filter_var($input, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE);
    
    echo "'$input' -> int: " . ($int !== false ? $int : 'invalid') . 
         ", float: " . ($float !== false ? $float : 'invalid') . 
         ", bool: " . ($bool !== null ? ($bool ? 'true' : 'false') : 'invalid') . "\n";
}
```

PHP provides various functions for type checking (`is_*` family) and  
validation (`filter_var`). Use these for safe type checking and  
input validation instead of relying on type juggling.  

## Variable constants and immutability

Working with constants and creating immutable-like variable patterns.  

```php
<?php

// Constants
define('APP_NAME', 'PHP Examples');
const VERSION = '1.0.0';

echo APP_NAME . " v" . VERSION . "\n";

// Class constants
class Config {
    const MAX_CONNECTIONS = 100;
    const DEFAULT_TIMEOUT = 30;
    
    public static function getSettings() {
        return [
            'max_conn' => self::MAX_CONNECTIONS,
            'timeout' => self::DEFAULT_TIMEOUT
        ];
    }
}

echo "Max connections: " . Config::MAX_CONNECTIONS . "\n";

// Readonly properties (PHP 8.1+)
class User {
    public function __construct(
        public readonly string $name,
        public readonly int $id
    ) {}
}

$user = new User('Alice', 123);
echo "User: {$user->name} (ID: {$user->id})\n";

// Immutable-like patterns with arrays
$originalData = ['a' => 1, 'b' => 2];
$modifiedData = array_merge($originalData, ['c' => 3]);

echo "Original: " . json_encode($originalData) . "\n";
echo "Modified: " . json_encode($modifiedData) . "\n";

// Freezing objects (custom implementation)
class ImmutablePoint {
    private $x, $y;
    
    public function __construct($x, $y) {
        $this->x = $x;
        $this->y = $y;
    }
    
    public function getX() { return $this->x; }
    public function getY() { return $this->y; }
    
    public function withX($newX) {
        return new self($newX, $this->y);
    }
}

$point = new ImmutablePoint(1, 2);
$newPoint = $point->withX(5);
echo "Original point: ({$point->getX()}, {$point->getY()})\n";
echo "New point: ({$newPoint->getX()}, {$newPoint->getY()})\n";
```

Constants provide immutable values using `define()` or `const`.  
Readonly properties prevent modification after initialization.  
Use immutable patterns for safer data handling.  

## Variable serialization

Converting variables to strings for storage and transmission.  

```php
<?php

$data = [
    'name' => 'Alice',
    'scores' => [85, 92, 78],
    'active' => true,
    'joined' => new DateTime('2023-01-15')
];

// PHP serialization
$serialized = serialize($data);
echo "Serialized length: " . strlen($serialized) . "\n";

$unserialized = unserialize($serialized);
echo "Name: " . $unserialized['name'] . "\n";
echo "First score: " . $unserialized['scores'][0] . "\n";

// JSON serialization (more portable)
$jsonData = [
    'name' => 'Bob',
    'scores' => [90, 85, 88],
    'active' => true,
    'joined' => '2023-01-15'
];

$json = json_encode($jsonData);
echo "JSON: $json\n";

$decoded = json_decode($json, true);
echo "Decoded name: " . $decoded['name'] . "\n";

// Custom serialization
class SerializableUser implements JsonSerializable {
    private $name;
    private $email;
    private $created;
    
    public function __construct($name, $email) {
        $this->name = $name;
        $this->email = $email;
        $this->created = time();
    }
    
    public function jsonSerialize(): array {
        return [
            'name' => $this->name,
            'email' => $this->email,
            'created' => date('Y-m-d H:i:s', $this->created)
        ];
    }
}

$user = new SerializableUser('Charlie', 'charlie@example.com');
echo "User JSON: " . json_encode($user) . "\n";
```

Serialization converts complex variables into strings for storage  
or transmission. PHP's `serialize()` preserves object types, while  
JSON is more portable but limited to basic types.  

## Variable memory management

Understanding variable memory usage and cleanup patterns.  

```php
<?php

function memoryUsage($label) {
    echo "$label: " . number_format(memory_get_usage()) . " bytes\n";
}

memoryUsage("Start");

// Large array creation
$largeArray = range(1, 100000);
memoryUsage("After large array");

// Variable copying vs referencing
$copy = $largeArray;
memoryUsage("After copy");

$reference = &$largeArray;
memoryUsage("After reference");

// Memory cleanup
unset($copy);
memoryUsage("After unset copy");

unset($largeArray);
memoryUsage("After unset original");

// $reference still holds the data
memoryUsage("Reference still active");
unset($reference);
memoryUsage("After unset reference");

// Circular reference example
class Node {
    public $data;
    public $parent;
    public $children = [];
    
    public function __construct($data) {
        $this->data = $data;
    }
    
    public function addChild(Node $child) {
        $child->parent = $this;
        $this->children[] = $child;
    }
}

$root = new Node('root');
$child1 = new Node('child1');
$child2 = new Node('child2');

$root->addChild($child1);
$root->addChild($child2);

memoryUsage("After creating nodes");

// Break circular references
$root = null;
$child1 = null;
$child2 = null;

memoryUsage("After cleanup");

// Force garbage collection
if (function_exists('gc_collect_cycles')) {
    $collected = gc_collect_cycles();
    echo "Garbage collected: $collected cycles\n";
}

memoryUsage("After garbage collection");
```

PHP automatically manages memory, but understanding variable copying,  
references, and circular references helps optimize memory usage.  
Use `unset()` to free large variables when done.  

## Variable debugging and introspection

Tools and techniques for examining variables during development.  

```php
<?php

$testData = [
    'string' => 'Hello there!',
    'number' => 42,
    'float' => 3.14159,
    'boolean' => true,
    'null' => null,
    'array' => [1, 2, ['nested' => 'value']],
    'object' => new stdClass()
];

// Basic variable inspection
echo "=== var_dump ===\n";
var_dump($testData['string']);
var_dump($testData['boolean']);

echo "\n=== print_r ===\n";
print_r($testData['array']);

echo "\n=== var_export ===\n";
echo var_export($testData['number'], true) . "\n";

// JSON for readable output
echo "\n=== JSON ===\n";
echo json_encode($testData, JSON_PRETTY_PRINT) . "\n";

// Custom debug function
function debug_var($var, $name = 'Variable') {
    echo "=== $name ===\n";
    echo "Type: " . gettype($var) . "\n";
    
    if (is_scalar($var) || is_null($var)) {
        echo "Value: " . var_export($var, true) . "\n";
    } elseif (is_array($var)) {
        echo "Array with " . count($var) . " elements\n";
        foreach ($var as $key => $value) {
            echo "  [$key] => " . gettype($value);
            if (is_scalar($value)) {
                echo " (" . var_export($value, true) . ")";
            }
            echo "\n";
        }
    } elseif (is_object($var)) {
        echo "Object of class: " . get_class($var) . "\n";
        $props = get_object_vars($var);
        foreach ($props as $prop => $value) {
            echo "  $prop => " . gettype($value) . "\n";
        }
    }
    echo "\n";
}

debug_var($testData['array'], 'Test Array');
debug_var($testData['object'], 'Test Object');

// Variable existence and emptiness
$vars = [
    'unset' => null,
    'empty_string' => '',
    'zero' => 0,
    'false' => false,
    'empty_array' => []
];

unset($vars['unset']);

foreach (['unset', 'empty_string', 'zero', 'false', 'empty_array'] as $key) {
    echo "$key: ";
    echo "isset=" . (isset($vars[$key]) ? 'true' : 'false');
    echo ", empty=" . (empty($vars[$key]) ? 'true' : 'false');
    echo ", array_key_exists=" . (array_key_exists($key, $vars) ? 'true' : 'false');
    echo "\n";
}
```

PHP provides multiple debugging functions: `var_dump()` shows detailed  
type information, `print_r()` displays readable array structure, and  
`var_export()` generates valid PHP code. Use appropriate tools for context.  

## Variable best practices

Recommended patterns for clean and maintainable variable usage.  

```php
<?php

// Descriptive naming
$userAccountBalance = 150.75;  // Good
$b = 150.75;                   // Poor

$isEmailValid = true;          // Good  
$flag = true;                  // Poor

$customerOrderItems = [];      // Good
$arr = [];                     // Poor

// Consistent naming convention
class UserManager {
    private $connectionPool;
    private $cacheManager;
    private $loggerInstance;
    
    public function getUserById($userId) {
        $userRecord = $this->findUser($userId);
        $userData = $this->processUserData($userRecord);
        return $userData;
    }
    
    private function findUser($userId) {
        // Implementation
        return ['id' => $userId, 'name' => 'Alice'];
    }
    
    private function processUserData($record) {
        // Implementation
        return $record;
    }
}

// Initialize variables appropriately
$totalAmount = 0.0;
$itemList = [];
$configOptions = [
    'timeout' => 30,
    'retries' => 3,
    'debug' => false
];

// Avoid magic numbers and strings
const MAX_LOGIN_ATTEMPTS = 3;
const DEFAULT_TIMEOUT_SECONDS = 30;
const ERROR_INVALID_USER = 'Invalid user credentials';

function validateLogin($username, $password, $attempts = 0) {
    if ($attempts >= MAX_LOGIN_ATTEMPTS) {
        throw new Exception(ERROR_INVALID_USER);
    }
    
    // Login logic here
    return true;
}

// Use null coalescing for defaults
$config = [
    'host' => $_ENV['DB_HOST'] ?? 'localhost',
    'port' => $_ENV['DB_PORT'] ?? 5432,
    'timeout' => $_ENV['TIMEOUT'] ?? DEFAULT_TIMEOUT_SECONDS
];

// Type declarations when appropriate
function calculateTotal(array $items): float {
    $total = 0.0;
    
    foreach ($items as $item) {
        if (isset($item['price']) && is_numeric($item['price'])) {
            $total += (float) $item['price'];
        }
    }
    
    return $total;
}

// Early returns to reduce nesting
function processUser(?array $userData): array {
    if ($userData === null) {
        return ['error' => 'No user data provided'];
    }
    
    if (!isset($userData['id'])) {
        return ['error' => 'User ID missing'];
    }
    
    if (!isset($userData['name'])) {
        return ['error' => 'User name missing'];
    }
    
    return [
        'success' => true,
        'processed_user' => $userData
    ];
}

echo "Best practices demonstrated successfully!\n";
```

Use descriptive variable names, consistent naming conventions, and  
appropriate initialization. Avoid magic numbers, use type declarations  
when beneficial, and structure code to minimize nesting.  

## Variable security considerations

Protecting against common security vulnerabilities related to variables.  

```php
<?php

// Input sanitization
function sanitizeInput($input): string {
    if (!is_string($input)) {
        return '';
    }
    
    // Remove null bytes, trim whitespace
    $input = str_replace("\0", '', $input);
    $input = trim($input);
    
    return $input;
}

// Simulated user input
$userInputs = [
    'name' => 'Alice<script>alert("xss")</script>',
    'email' => 'alice@example.com',
    'age' => '25',
    'comments' => "Some comment\0with null byte"
];

// Validate and sanitize
$cleanData = [];
foreach ($userInputs as $key => $value) {
    $cleanData[$key] = sanitizeInput($value);
    
    // Additional validation based on field
    switch ($key) {
        case 'email':
            $cleanData[$key] = filter_var($cleanData[$key], FILTER_VALIDATE_EMAIL) ?: '';
            break;
        case 'age':
            $cleanData[$key] = filter_var($cleanData[$key], FILTER_VALIDATE_INT, [
                'options' => ['min_range' => 1, 'max_range' => 120]
            ]) ?: 0;
            break;
        case 'name':
            // Remove HTML tags
            $cleanData[$key] = strip_tags($cleanData[$key]);
            break;
    }
}

echo "Cleaned data:\n";
print_r($cleanData);

// Safe variable interpolation
function safeTemplate(string $template, array $vars): string {
    $safe = [];
    
    foreach ($vars as $key => $value) {
        // Escape HTML entities
        $safe[$key] = htmlspecialchars((string) $value, ENT_QUOTES, 'UTF-8');
    }
    
    // Simple template replacement
    foreach ($safe as $key => $value) {
        $template = str_replace("{{$key}}", $value, $template);
    }
    
    return $template;
}

$template = "Hello {name}, your email {email} is registered!";
$safeOutput = safeTemplate($template, $cleanData);
echo "Safe output: $safeOutput\n";

// Prevent variable pollution
class SecureConfig {
    private $config = [];
    private $allowedKeys = ['host', 'port', 'timeout', 'debug'];
    
    public function set(string $key, $value): void {
        if (!in_array($key, $this->allowedKeys, true)) {
            throw new InvalidArgumentException("Invalid config key: $key");
        }
        
        $this->config[$key] = $value;
    }
    
    public function get(string $key, $default = null) {
        return $this->config[$key] ?? $default;
    }
}

// Environment variable security
function getSecureEnv(string $key, string $default = ''): string {
    $value = $_ENV[$key] ?? $default;
    
    // Validate environment variables
    if (strpos($key, 'PASSWORD') !== false || strpos($key, 'SECRET') !== false) {
        // Don't log or echo sensitive variables
        return $value;
    }
    
    return $value;
}

// Simulated environment
$_ENV['DB_HOST'] = 'localhost';
$_ENV['DB_PASSWORD'] = 'secret123';

$dbHost = getSecureEnv('DB_HOST', 'localhost');
$dbPassword = getSecureEnv('DB_PASSWORD');

echo "DB Host: $dbHost\n";
echo "Password length: " . strlen($dbPassword) . "\n"; // Don't output actual password
```

Always sanitize and validate input variables. Use prepared statements  
for database queries, escape output for display, and be careful with  
environment variables containing sensitive data.  

## Variable performance optimization

Techniques for efficient variable usage and memory optimization.  

```php
<?php

// Memory-efficient array processing
function processLargeDataset($filename) {
    // Instead of loading all data into memory
    // $allData = file($filename); // Memory intensive
    
    // Use generator for memory efficiency
    $handle = fopen($filename, 'r');
    if ($handle) {
        while (($line = fgets($handle)) !== false) {
            yield trim($line);
        }
        fclose($handle);
    }
}

// String concatenation optimization
function slowConcatenation(array $items): string {
    $result = '';
    foreach ($items as $item) {
        $result .= $item . ','; // Creates new string each time
    }
    return rtrim($result, ',');
}

function fastConcatenation(array $items): string {
    return implode(',', $items); // Much more efficient
}

// Variable reuse vs recreation
function efficientLoop($count) {
    $result = [];
    $temp = null; // Reuse variable
    
    for ($i = 0; $i < $count; $i++) {
        $temp = "Item $i"; // Reuse instead of creating new variable each iteration
        $result[] = $temp;
    }
    
    return $result;
}

// Reference usage for large arrays
function processByReference(array &$data): void {
    foreach ($data as &$item) { // Reference avoids copying
        $item = strtoupper($item);
    }
    unset($item); // Important: break reference
}

function processByCopy(array $data): array { // Less efficient for large arrays
    foreach ($data as $key => $item) {
        $data[$key] = strtoupper($item);
    }
    return $data;
}

// Lazy loading pattern
class LazyLoader {
    private $expensiveData = null;
    
    public function getExpensiveData(): array {
        if ($this->expensiveData === null) {
            echo "Loading expensive data...\n";
            $this->expensiveData = range(1, 10000); // Simulate expensive operation
        }
        
        return $this->expensiveData;
    }
}

// Variable pooling for frequent allocations
class ObjectPool {
    private $pool = [];
    
    public function get(): stdClass {
        if (empty($this->pool)) {
            return new stdClass();
        }
        
        return array_pop($this->pool);
    }
    
    public function release(stdClass $obj): void {
        // Reset object state
        foreach (get_object_vars($obj) as $prop => $value) {
            unset($obj->$prop);
        }
        
        $this->pool[] = $obj;
    }
}

// Performance measurement
function measurePerformance(callable $func, string $name): void {
    $start = microtime(true);
    $memoryStart = memory_get_usage();
    
    $result = $func();
    
    $end = microtime(true);
    $memoryEnd = memory_get_usage();
    
    $time = ($end - $start) * 1000; // Convert to milliseconds
    $memory = $memoryEnd - $memoryStart;
    
    echo "$name: {$time}ms, Memory: " . number_format($memory) . " bytes\n";
}

// Example usage
$testData = range(1, 1000);

measurePerformance(
    fn() => slowConcatenation(array_map('strval', array_slice($testData, 0, 100))),
    'Slow concatenation'
);

measurePerformance(
    fn() => fastConcatenation(array_map('strval', array_slice($testData, 0, 100))),
    'Fast concatenation'
);

echo "Performance optimization examples completed!\n";
```

Use generators for large datasets, prefer `implode()` over string  
concatenation loops, use references for large array modifications,  
and implement lazy loading for expensive operations.  

## Variable advanced patterns

Sophisticated variable usage patterns for complex applications.  

```php
<?php

// Fluent interface with variable chaining
class QueryBuilder {
    private $query = [];
    
    public function select(string $fields): self {
        $this->query['select'] = $fields;
        return $this;
    }
    
    public function from(string $table): self {
        $this->query['from'] = $table;
        return $this;
    }
    
    public function where(string $condition): self {
        $this->query['where'] = $condition;
        return $this;
    }
    
    public function build(): string {
        $sql = "SELECT {$this->query['select']} FROM {$this->query['from']}";
        if (isset($this->query['where'])) {
            $sql .= " WHERE {$this->query['where']}";
        }
        return $sql;
    }
}

$query = (new QueryBuilder())
    ->select('name, email')
    ->from('users')
    ->where('active = 1')
    ->build();

echo "SQL: $query\n";

// Variable containers and dependency injection
class Container {
    private $bindings = [];
    private $instances = [];
    
    public function bind(string $key, callable $factory): void {
        $this->bindings[$key] = $factory;
    }
    
    public function singleton(string $key, callable $factory): void {
        $this->bind($key, function() use ($key, $factory) {
            if (!isset($this->instances[$key])) {
                $this->instances[$key] = $factory();
            }
            return $this->instances[$key];
        });
    }
    
    public function get(string $key) {
        if (!isset($this->bindings[$key])) {
            throw new Exception("No binding found for $key");
        }
        
        return $this->bindings[$key]();
    }
}

$container = new Container();
$container->singleton('database', fn() => new stdClass()); // Simulate database
$container->bind('user_service', fn() => new stdClass());  // Simulate service

// Variable event system
class EventManager {
    private $listeners = [];
    
    public function on(string $event, callable $listener): void {
        $this->listeners[$event][] = $listener;
    }
    
    public function emit(string $event, array $data = []): void {
        if (!isset($this->listeners[$event])) {
            return;
        }
        
        foreach ($this->listeners[$event] as $listener) {
            $listener($data);
        }
    }
}

$events = new EventManager();
$events->on('user.created', function($data) {
    echo "User created: {$data['name']}\n";
});

$events->emit('user.created', ['name' => 'Alice']);

// Variable state machine
class StateMachine {
    private $state;
    private $transitions = [];
    
    public function __construct(string $initialState) {
        $this->state = $initialState;
    }
    
    public function addTransition(string $from, string $to, callable $condition = null): void {
        $this->transitions[$from][$to] = $condition ?: fn() => true;
    }
    
    public function transition(string $to, array $context = []): bool {
        if (!isset($this->transitions[$this->state][$to])) {
            return false;
        }
        
        $condition = $this->transitions[$this->state][$to];
        if ($condition($context)) {
            echo "Transitioning from {$this->state} to $to\n";
            $this->state = $to;
            return true;
        }
        
        return false;
    }
    
    public function getState(): string {
        return $this->state;
    }
}

$machine = new StateMachine('draft');
$machine->addTransition('draft', 'review');
$machine->addTransition('review', 'published', fn($ctx) => $ctx['approved'] ?? false);
$machine->addTransition('published', 'archived');

$machine->transition('review');
$machine->transition('published', ['approved' => true]);
echo "Final state: {$machine->getState()}\n";

// Variable proxy pattern
class LazyProxy {
    private $target = null;
    private $factory;
    
    public function __construct(callable $factory) {
        $this->factory = $factory;
    }
    
    public function __call(string $method, array $args) {
        if ($this->target === null) {
            $this->target = ($this->factory)();
        }
        
        return $this->target->$method(...$args);
    }
    
    public function __get(string $name) {
        if ($this->target === null) {
            $this->target = ($this->factory)();
        }
        
        return $this->target->$name;
    }
}

echo "Advanced patterns demonstrated successfully!\n";

## Variable testing and assertions

Testing variable states and implementing assertion patterns for validation.  

```php
<?php

// Custom assertion function
function assert_variable($condition, string $message = 'Assertion failed'): void {
    if (!$condition) {
        throw new AssertionError($message);
    }
}

// Variable state testing
function test_variable_states() {
    $testCases = [
        'string' => 'Hello',
        'empty_string' => '',
        'integer' => 42,
        'zero' => 0,
        'float' => 3.14,
        'boolean_true' => true,
        'boolean_false' => false,
        'null' => null,
        'array' => [1, 2, 3],
        'empty_array' => []
    ];
    
    foreach ($testCases as $name => $value) {
        echo "Testing $name:\n";
        echo "  isset: " . (isset($value) ? 'true' : 'false') . "\n";
        echo "  empty: " . (empty($value) ? 'true' : 'false') . "\n";
        echo "  is_null: " . (is_null($value) ? 'true' : 'false') . "\n";
        echo "  boolean context: " . ($value ? 'truthy' : 'falsy') . "\n\n";
    }
}

// Type assertion helpers
class TypeAssertions {
    public static function assertString($value, string $varName = 'variable'): void {
        if (!is_string($value)) {
            throw new TypeError("$varName must be string, " . gettype($value) . " given");
        }
    }
    
    public static function assertArray($value, string $varName = 'variable'): void {
        if (!is_array($value)) {
            throw new TypeError("$varName must be array, " . gettype($value) . " given");
        }
    }
    
    public static function assertNotEmpty($value, string $varName = 'variable'): void {
        if (empty($value)) {
            throw new ValueError("$varName cannot be empty");
        }
    }
    
    public static function assertInRange($value, $min, $max, string $varName = 'variable'): void {
        if ($value < $min || $value > $max) {
            throw new ValueError("$varName must be between $min and $max, $value given");
        }
    }
}

// Variable contract validation
class Validator {
    private $rules = [];
    
    public function rule(string $field, callable $validator): self {
        $this->rules[$field] = $validator;
        return $this;
    }
    
    public function validate(array $data): array {
        $errors = [];
        
        foreach ($this->rules as $field => $validator) {
            try {
                $value = $data[$field] ?? null;
                $validator($value);
            } catch (Exception $e) {
                $errors[$field] = $e->getMessage();
            }
        }
        
        return $errors;
    }
}

// Unit test style assertions
class VariableTest {
    private $failures = 0;
    private $tests = 0;
    
    public function assertEquals($expected, $actual, string $message = ''): void {
        $this->tests++;
        if ($expected !== $actual) {
            $this->failures++;
            $msg = $message ?: "Expected " . var_export($expected, true) . 
                               ", got " . var_export($actual, true);
            echo "FAIL: $msg\n";
        } else {
            echo "PASS: " . ($message ?: "Values are equal") . "\n";
        }
    }
    
    public function assertTrue($condition, string $message = 'Expected true'): void {
        $this->assertEquals(true, (bool)$condition, $message);
    }
    
    public function assertFalse($condition, string $message = 'Expected false'): void {
        $this->assertEquals(false, (bool)$condition, $message);
    }
    
    public function assertNull($value, string $message = 'Expected null'): void {
        $this->assertEquals(null, $value, $message);
    }
    
    public function assertType(string $expectedType, $value, string $message = ''): void {
        $actualType = gettype($value);
        $msg = $message ?: "Expected type $expectedType, got $actualType";
        $this->assertEquals($expectedType, $actualType, $msg);
    }
    
    public function getSummary(): string {
        $passed = $this->tests - $this->failures;
        return "Tests: {$this->tests}, Passed: $passed, Failed: {$this->failures}";
    }
}

// Example usage
echo "=== Variable State Testing ===\n";
test_variable_states();

echo "=== Type Assertions ===\n";
try {
    TypeAssertions::assertString("Hello");
    echo "String assertion passed\n";
    
    TypeAssertions::assertArray([1, 2, 3]);
    echo "Array assertion passed\n";
    
    TypeAssertions::assertNotEmpty("Not empty");
    echo "Not empty assertion passed\n";
    
    TypeAssertions::assertInRange(50, 1, 100);
    echo "Range assertion passed\n";
    
} catch (Exception $e) {
    echo "Assertion failed: " . $e->getMessage() . "\n";
}

echo "\n=== Validation Example ===\n";
$validator = new Validator();
$validator
    ->rule('name', fn($v) => TypeAssertions::assertString($v, 'name'))
    ->rule('age', fn($v) => TypeAssertions::assertInRange($v, 0, 120, 'age'))
    ->rule('email', function($v) {
        if (!filter_var($v, FILTER_VALIDATE_EMAIL)) {
            throw new ValueError('Invalid email format');
        }
    });

$testData = [
    'name' => 'Alice',
    'age' => 25,
    'email' => 'alice@example.com'
];

$errors = $validator->validate($testData);
if (empty($errors)) {
    echo "Validation passed!\n";
} else {
    echo "Validation errors: " . json_encode($errors) . "\n";
}

echo "\n=== Unit Test Style ===\n";
$test = new VariableTest();

$test->assertEquals(10, 5 + 5, "Addition test");
$test->assertTrue(true, "Boolean true test");
$test->assertFalse(false, "Boolean false test");
$test->assertNull(null, "Null test");
$test->assertType('string', 'Hello', "String type test");
$test->assertType('integer', 42, "Integer type test");

echo "\n" . $test->getSummary() . "\n";
```

Variable testing involves creating assertions to validate variable  
states, types, and values. Use custom assertion functions, type  
validation helpers, and unit test patterns to ensure code reliability.
```

Advanced patterns include fluent interfaces for method chaining,  
dependency containers for variable management, event systems for  
decoupled communication, and proxy patterns for lazy initialization.  