# Data Types

This chapter covers 25 comprehensive PHP data type examples designed to  
help you master PHP's type system in version 8.4. These examples  
progress from basic scalar types to advanced type features, including  
union types, nullable types, and type validation. Each example includes  
clear explanations to help you understand type behavior and apply type  
safety principles effectively in your code.  

## String types

Working with text data using PHP's string type.  

```php
<?php

$greeting = "Hello there!";
$multiline = "Line one\nLine two\nLine three";
$interpolated = "The answer is $answer";
$heredoc = <<<TEXT
This is a heredoc string
that spans multiple lines
and preserves formatting.
TEXT;

$nowdoc = <<<'NOWDOC'
This is a nowdoc string
where $variables are not interpolated.
NOWDOC;

echo $greeting . "\n";
echo "Length: " . strlen($greeting) . "\n";
echo "Uppercase: " . strtoupper($greeting) . "\n";
echo "Substring: " . substr($greeting, 0, 5) . "\n";
```

Strings in PHP can be created with single quotes, double quotes,  
heredoc, or nowdoc syntax. Double-quoted strings and heredoc support  
variable interpolation, while single-quoted strings and nowdoc do not.  

## Integer types

Working with whole numbers and integer operations.  

```php
<?php

$decimal = 42;
$octal = 0755;              // Octal notation
$hexadecimal = 0x2A;        // Hexadecimal notation
$binary = 0b101010;         // Binary notation

$large_number = 1_000_000;  // Numeric separator for readability

echo "Decimal: $decimal\n";
echo "Octal: $octal (decimal: " . octdec('755') . ")\n";
echo "Hex: $hexadecimal (decimal: " . hexdec('2A') . ")\n";
echo "Binary: $binary (decimal: " . bindec('101010') . ")\n";
echo "Large number: $large_number\n";

// Integer limits
echo "Max int: " . PHP_INT_MAX . "\n";
echo "Min int: " . PHP_INT_MIN . "\n";
echo "Size: " . PHP_INT_SIZE . " bytes\n";
```

PHP supports integers in various bases: decimal, octal (0-prefixed),  
hexadecimal (0x-prefixed), and binary (0b-prefixed). Numeric  
separators improve readability for large numbers.  

## Float types

Working with decimal numbers and floating-point precision.  

```php
<?php

$simple_float = 3.14;
$scientific = 1.5e3;        // 1500.0
$negative_exp = 2.5e-2;     // 0.025

$very_large = 1.7976931348623157E+308;
$very_small = 2.2250738585072014E-308;

echo "Pi: $simple_float\n";
echo "Scientific: $scientific\n";
echo "Negative exp: $negative_exp\n";

// Precision considerations
$result = 0.1 + 0.2;
echo "0.1 + 0.2 = " . $result . "\n";
echo "Is exactly 0.3? " . ($result === 0.3 ? "yes" : "no") . "\n";

// Better float comparison
$epsilon = 0.00001;
$is_equal = abs($result - 0.3) < $epsilon;
echo "Is approximately 0.3? " . ($is_equal ? "yes" : "no") . "\n";
```

Float types support scientific notation and handle very large or small  
numbers. Due to floating-point precision limitations, use epsilon  
comparison for equality testing rather than exact comparison.  

## Boolean types

Working with true/false values and boolean context.  

```php
<?php

$is_valid = true;
$is_complete = false;

echo "Valid: " . ($is_valid ? "yes" : "no") . "\n";
echo "Complete: " . ($is_complete ? "yes" : "no") . "\n";

// Boolean conversion rules
$truthy_values = [1, "1", "true", "yes", [1], new stdClass()];
$falsy_values = [0, "0", "", false, null, []];

echo "\nTruthy values:\n";
foreach ($truthy_values as $value) {
    $type = gettype($value);
    $bool_val = (bool) $value;
    echo "  $type: " . ($bool_val ? "true" : "false") . "\n";
}

echo "\nFalsy values:\n";
foreach ($falsy_values as $value) {
    $type = gettype($value);
    $bool_val = (bool) $value;
    echo "  $type: " . ($bool_val ? "true" : "false") . "\n";
}
```

Boolean type has two values: true and false. PHP automatically  
converts other types to boolean in conditional contexts. Empty  
values (0, "", [], null) convert to false; non-empty values to true.  

## Null type

Working with null values and null coalescing.  

```php
<?php

$undefined_variable = null;
$empty_result = null;

echo "Null value: ";
var_dump($undefined_variable);

// Null coalescing operator
$name = $user_name ?? "Anonymous";
$config = $settings ?? ["default" => true];

echo "Name: $name\n";
echo "Config: " . json_encode($config) . "\n";

// Null coalescing assignment (PHP 7.4+)
$counter ??= 0;
$counter++;
echo "Counter: $counter\n";

// Null-safe operator (PHP 8.0+)
$user = null;
$email = $user?->getEmail() ?? "No email";
echo "Email: $email\n";

// Checking for null
if ($undefined_variable === null) {
    echo "Variable is null\n";
}

if (is_null($empty_result)) {
    echo "Result is null\n";
}
```

Null represents the absence of a value. Use null coalescing operators  
(?? and ??=) for default value assignment, and null-safe operator  
(?->) for safe method calls on potentially null objects.  

## Indexed arrays

Working with numeric-indexed array collections.  

```php
<?php

$colors = ["red", "green", "blue"];
$numbers = [10, 20, 30, 40, 50];
$mixed = [1, "hello", 3.14, true, null];

echo "Colors:\n";
foreach ($colors as $index => $color) {
    echo "  [$index] = $color\n";
}

// Array manipulation
array_push($colors, "yellow", "purple");
$first_color = array_shift($colors);
$last_color = array_pop($colors);

echo "\nFirst color removed: $first_color\n";
echo "Last color removed: $last_color\n";
echo "Remaining colors: " . implode(", ", $colors) . "\n";

// Array access and bounds checking
$index = 2;
if (isset($numbers[$index])) {
    echo "Number at index $index: " . $numbers[$index] . "\n";
}

echo "Array length: " . count($numbers) . "\n";
```

Indexed arrays use numeric keys starting from 0. Use array functions  
like array_push(), array_pop(), array_shift(), and array_unshift()  
for stack and queue operations. Always check bounds with isset().  

## Associative arrays

Working with key-value pair collections.  

```php
<?php

$person = [
    "name" => "Alice Johnson",
    "age" => 28,
    "email" => "alice@example.com",
    "active" => true
];

$config = [
    "database" => [
        "host" => "localhost",
        "port" => 3306,
        "name" => "myapp"
    ],
    "cache" => [
        "enabled" => true,
        "ttl" => 3600
    ]
];

echo "Person info:\n";
foreach ($person as $key => $value) {
    echo "  $key: " . (is_bool($value) ? ($value ? "yes" : "no") : $value) . "\n";
}

// Accessing nested data
echo "\nDatabase config:\n";
echo "Host: " . $config["database"]["host"] . "\n";
echo "Port: " . $config["database"]["port"] . "\n";

// Key existence checking
if (array_key_exists("email", $person)) {
    echo "Email found: " . $person["email"] . "\n";
}

// Getting all keys and values
echo "Keys: " . implode(", ", array_keys($person)) . "\n";
echo "Values: " . implode(", ", array_values($person)) . "\n";
```

Associative arrays use string keys to map to values. They're perfect  
for representing structured data like configuration objects. Use  
array_key_exists() to check key existence safely.  

## Multidimensional arrays

Working with nested array structures.  

```php
<?php

$matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

$users = [
    [
        "id" => 1,
        "name" => "John",
        "permissions" => ["read", "write"]
    ],
    [
        "id" => 2,
        "name" => "Jane",
        "permissions" => ["read", "write", "delete"]
    ]
];

echo "Matrix:\n";
for ($i = 0; $i < count($matrix); $i++) {
    for ($j = 0; $j < count($matrix[$i]); $j++) {
        echo $matrix[$i][$j] . " ";
    }
    echo "\n";
}

echo "\nUsers:\n";
foreach ($users as $user) {
    echo "ID: " . $user["id"] . ", Name: " . $user["name"] . "\n";
    echo "Permissions: " . implode(", ", $user["permissions"]) . "\n\n";
}

// Searching in multidimensional arrays
$user_names = array_column($users, "name");
echo "All user names: " . implode(", ", $user_names) . "\n";

// Finding specific user
$john_key = array_search("John", $user_names);
if ($john_key !== false) {
    echo "John found at index: $john_key\n";
}
```

Multidimensional arrays contain arrays as elements. Use nested loops  
for iteration and array_column() to extract specific columns.  
array_search() helps find elements by value.  

## Type checking functions

Using PHP's built-in type detection functions.  

```php
<?php

$variables = [
    "string" => "Hello there!",
    "integer" => 42,
    "float" => 3.14,
    "boolean" => true,
    "null" => null,
    "array" => [1, 2, 3],
    "object" => new stdClass(),
    "resource" => fopen("php://memory", "r")
];

echo "Type checking results:\n";
foreach ($variables as $name => $value) {
    echo "\n$name:\n";
    echo "  gettype(): " . gettype($value) . "\n";
    
    // Specific type checks
    $checks = [
        "is_string" => is_string($value),
        "is_int" => is_int($value),
        "is_float" => is_float($value),
        "is_bool" => is_bool($value),
        "is_null" => is_null($value),
        "is_array" => is_array($value),
        "is_object" => is_object($value),
        "is_resource" => is_resource($value),
        "is_scalar" => is_scalar($value),
        "is_numeric" => is_numeric($value)
    ];
    
    foreach ($checks as $function => $result) {
        if ($result) {
            echo "  $function: true\n";
        }
    }
}

// Close resource
if (is_resource($variables["resource"])) {
    fclose($variables["resource"]);
}
```

PHP provides many is_*() functions for type checking. gettype()  
returns the type name as a string. is_scalar() checks for basic  
types (string, int, float, bool). is_numeric() checks if a value  
can be used as a number.  

## Type validation with filters

Using filter functions for safe type validation and conversion.  

```php
<?php

$inputs = [
    "123",
    "45.67", 
    "not_a_number",
    "true",
    "false",
    "user@example.com",
    "not_an_email",
    "https://example.com",
    "invalid_url"
];

echo "Filter validation results:\n";
foreach ($inputs as $input) {
    echo "\nInput: '$input'\n";
    
    // Integer validation
    $int_result = filter_var($input, FILTER_VALIDATE_INT);
    echo "  As int: " . ($int_result !== false ? $int_result : "invalid") . "\n";
    
    // Float validation
    $float_result = filter_var($input, FILTER_VALIDATE_FLOAT);
    echo "  As float: " . ($float_result !== false ? $float_result : "invalid") . "\n";
    
    // Boolean validation
    $bool_result = filter_var($input, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE);
    if ($bool_result !== null) {
        echo "  As bool: " . ($bool_result ? "true" : "false") . "\n";
    }
    
    // Email validation
    $email_result = filter_var($input, FILTER_VALIDATE_EMAIL);
    if ($email_result !== false) {
        echo "  As email: $email_result\n";
    }
    
    // URL validation
    $url_result = filter_var($input, FILTER_VALIDATE_URL);
    if ($url_result !== false) {
        echo "  As URL: $url_result\n";
    }
}
```

Filter functions provide robust validation and sanitization. Use  
FILTER_VALIDATE_* constants for validation. FILTER_NULL_ON_FAILURE  
returns null instead of false for failed boolean validation.  

## Basic type casting

Converting between different data types explicitly.  

```php
<?php

$string_number = "123";
$string_float = "45.67";
$string_bool = "true";

// Basic casting operations
$to_int = (int) $string_number;        // 123
$to_float = (float) $string_float;     // 45.67
$to_string = (string) $to_int;         // "123"
$to_bool = (bool) $string_number;      // true
$to_array = (array) $string_number;    // ["123"]
$to_object = (object) ["key" => "value"]; // stdClass object

echo "Original string: '$string_number'\n";
echo "Cast to int: $to_int\n";
echo "Cast to float: $to_float\n";
echo "Cast to string: '$to_string'\n";
echo "Cast to bool: " . ($to_bool ? "true" : "false") . "\n";

// Array casting demonstration
print_r($to_array);
print_r($to_object);

// Casting edge cases
$null_value = null;
$empty_string = "";
$zero = 0;

echo "\nEdge cases:\n";
echo "null to int: " . (int) $null_value . "\n";        // 0
echo "null to bool: " . ((bool) $null_value ? "true" : "false") . "\n"; // false
echo "empty string to bool: " . ((bool) $empty_string ? "true" : "false") . "\n"; // false
echo "zero to bool: " . ((bool) $zero ? "true" : "false") . "\n"; // false
```

Type casting forces conversion between types using cast operators.  
Each type has specific conversion rules: null becomes 0/false/"",  
non-empty values generally become true, numbers convert predictably.  

## String to number conversion

Automatic and explicit conversion between strings and numbers.  

```php
<?php

$numeric_strings = ["123", "45.67", "100abc", "abc123", "  89  ", ""];
$operations = [];

echo "String to number conversion:\n";
foreach ($numeric_strings as $str) {
    echo "\nString: '$str'\n";
    
    // Automatic conversion in arithmetic context
    $auto_int = $str + 0;           // Forces numeric context
    $auto_float = $str * 1.0;       // Forces float context
    
    // Explicit conversion functions
    $explicit_int = intval($str);
    $explicit_float = floatval($str);
    
    // Safe conversion with validation
    $safe_int = is_numeric(trim($str)) ? (int) trim($str) : 0;
    
    echo "  Auto int: $auto_int\n";
    echo "  Auto float: $auto_float\n";
    echo "  intval(): $explicit_int\n";
    echo "  floatval(): $explicit_float\n";
    echo "  Safe int: $safe_int\n";
}

// Arithmetic operations with mixed types
$str_num = "10";
$int_num = 5;
$float_num = 2.5;

echo "\nMixed arithmetic:\n";
echo "'10' + 5 = " . ($str_num + $int_num) . "\n";          // 15
echo "'10' * 2.5 = " . ($str_num * $float_num) . "\n";      // 25.0
echo "'10abc' + 5 = " . ("10abc" + $int_num) . "\n";        // 15 (warning)
```

PHP automatically converts strings to numbers in arithmetic contexts.  
Leading numeric parts are extracted; trailing non-numeric parts are  
ignored with a warning. Use is_numeric() for safe validation.  

## Array type conversion

Converting between arrays, objects, and scalar values.  

```php
<?php

$simple_array = [1, 2, 3, 4, 5];
$assoc_array = ["name" => "Alice", "age" => 30];
$object = new stdClass();
$object->name = "Bob";
$object->age = 25;

echo "Array conversions:\n";

// Array to string (implode for indexed arrays)
$array_to_string = implode(", ", $simple_array);
echo "Array to string: $array_to_string\n";

// Array to object
$array_as_object = (object) $assoc_array;
echo "Array as object name: " . $array_as_object->name . "\n";

// Object to array
$object_as_array = (array) $object;
echo "Object as array: " . json_encode($object_as_array) . "\n";

// Scalar to array
$scalar = "hello";
$scalar_as_array = (array) $scalar;
print_r($scalar_as_array);  // ["hello"]

// Array serialization for storage/transmission
$serialized = serialize($assoc_array);
echo "\nSerialized: $serialized\n";

$unserialized = unserialize($serialized);
echo "Unserialized: " . json_encode($unserialized) . "\n";

// JSON conversion for web APIs
$json = json_encode($assoc_array);
echo "JSON: $json\n";

$from_json = json_decode($json, true);  // true for array, false for object
echo "From JSON: " . json_encode($from_json) . "\n";
```

Arrays convert to objects with properties matching keys. Objects  
convert to arrays with property names as keys. Use serialize() for  
PHP-specific storage and json_encode() for interoperability.  

## Boolean context conversion

Understanding how different values behave in boolean contexts.  

```php
<?php

$test_values = [
    // Falsy values
    false,
    0,
    0.0,
    "",
    "0",
    [],
    null,
    
    // Truthy values
    true,
    1,
    -1,
    0.1,
    "false",    // String "false" is truthy!
    "0.0",      // String "0.0" is truthy!
    [0],        // Array with one element (even 0) is truthy
    new stdClass()
];

echo "Boolean context conversion:\n";
foreach ($test_values as $index => $value) {
    $type = gettype($value);
    $bool_result = $value ? "true" : "false";
    $display_value = match($type) {
        "string" => "'$value'",
        "array" => "[" . implode(",", $value) . "]",
        "object" => "object",
        "NULL" => "null",
        default => (string) $value
    };
    
    echo "  $display_value ($type) -> $bool_result\n";
}

// Practical boolean usage
function is_valid_user($user) {
    // All conditions must be truthy
    return $user && 
           isset($user['name']) && 
           $user['name'] && 
           isset($user['email']) && 
           $user['email'];
}

$users = [
    ["name" => "Alice", "email" => "alice@example.com"],
    ["name" => "", "email" => "invalid@example.com"],
    ["name" => "Bob", "email" => ""],
    null
];

echo "\nUser validation:\n";
foreach ($users as $i => $user) {
    $valid = is_valid_user($user) ? "valid" : "invalid";
    $name = $user['name'] ?? 'null';
    echo "User $i ($name): $valid\n";
}
```

Understanding boolean conversion is crucial for conditionals. Empty  
values (false, 0, "", [], null) are falsy. Non-empty values are  
truthy, including the string "0" and "false".  

## Nullable types

Working with nullable type declarations in functions.  

```php
<?php

function process_user(?string $name, ?int $age): ?array {
    if ($name === null && $age === null) {
        return null;  // No data to process
    }
    
    $result = [];
    
    if ($name !== null) {
        $result['name'] = strtoupper($name);
        $result['name_length'] = strlen($name);
    }
    
    if ($age !== null) {
        $result['age'] = $age;
        $result['is_adult'] = $age >= 18;
        $result['generation'] = match(true) {
            $age >= 77 => 'Silent',
            $age >= 58 => 'Boomer', 
            $age >= 42 => 'Gen X',
            $age >= 27 => 'Millennial',
            default => 'Gen Z'
        };
    }
    
    return $result;
}

// Test with various combinations
$test_cases = [
    ["Alice", 28],
    [null, 35], 
    ["Bob", null],
    [null, null],
    ["", 0]  // Empty string and zero are not null
];

echo "Nullable type testing:\n";
foreach ($test_cases as $i => [$name, $age]) {
    echo "\nTest case $i: name=" . ($name ?? 'null') . ", age=" . ($age ?? 'null') . "\n";
    
    $result = process_user($name, $age);
    if ($result === null) {
        echo "Result: null\n";
    } else {
        echo "Result: " . json_encode($result, JSON_PRETTY_PRINT) . "\n";
    }
}

// Nullable properties (PHP 7.4+)
class User {
    public ?string $name = null;
    public ?int $age = null;
    
    public function __construct(?string $name = null, ?int $age = null) {
        $this->name = $name;
        $this->age = $age;
    }
    
    public function getDisplayName(): string {
        return $this->name ?? 'Anonymous';
    }
}

$user1 = new User("Charlie", 30);
$user2 = new User();

echo "\nNullable properties:\n";
echo "User1 display name: " . $user1->getDisplayName() . "\n";
echo "User2 display name: " . $user2->getDisplayName() . "\n";
```

Nullable types (prefixed with ?) allow null values in addition to the  
specified type. Use null coalescing (??) for default values and  
strict null checks (=== null) for validation logic.  

## Union types

Using union types for flexible parameter and return types.  

```php
<?php

function format_value(string|int|float $value, string $format = 'default'): string {
    return match($format) {
        'currency' => match(true) {
            is_string($value) => '$' . $value,
            is_numeric($value) => '$' . number_format($value, 2),
        },
        'percent' => match(true) {
            is_string($value) => $value . '%',
            is_numeric($value) => ($value * 100) . '%',
        },
        'padded' => match(true) {
            is_string($value) => str_pad($value, 10, '0', STR_PAD_LEFT),
            is_numeric($value) => str_pad((string)$value, 10, '0', STR_PAD_LEFT),
        },
        default => (string) $value
    };
}

function calculate_total(int|float ...$values): int|float {
    $total = 0;
    foreach ($values as $value) {
        $total += $value;
    }
    
    // Return int if all values were integers and result has no decimal part
    return (floor($total) == $total && 
            array_reduce($values, fn($carry, $val) => $carry && is_int($val), true)) 
            ? (int) $total : $total;
}

function safe_divide(int|float $a, int|float $b): int|float|null {
    if ($b == 0) {
        return null;
    }
    return $a / $b;
}

// Testing union types
$test_values = [123, "456", 78.9, "90.12"];

echo "Union type formatting:\n";
foreach ($test_values as $value) {
    $type = gettype($value);
    echo "Value: $value ($type)\n";
    echo "  Default: " . format_value($value) . "\n";
    echo "  Currency: " . format_value($value, 'currency') . "\n";
    echo "  Percent: " . format_value($value, 'percent') . "\n";
    echo "  Padded: " . format_value($value, 'padded') . "\n\n";
}

echo "Union type calculations:\n";
echo "Total of ints: " . calculate_total(1, 2, 3, 4) . " (type: " . gettype(calculate_total(1, 2, 3, 4)) . ")\n";
echo "Total of floats: " . calculate_total(1.1, 2.2, 3.3) . " (type: " . gettype(calculate_total(1.1, 2.2, 3.3)) . ")\n";
echo "Total mixed: " . calculate_total(1, 2.5, 3) . " (type: " . gettype(calculate_total(1, 2.5, 3)) . ")\n";

echo "\nDivision results:\n";
echo "10 / 3 = " . (safe_divide(10, 3) ?? "undefined") . "\n";
echo "10 / 0 = " . (safe_divide(10, 0) ?? "undefined") . "\n";
```

Union types (type1|type2) allow multiple types for parameters and  
return values. Use match expressions or is_*() functions to handle  
different types appropriately. Combine with nullable syntax for  
maximum flexibility.  

## Mixed type

Working with the mixed type for maximum flexibility.  

```php
<?php

function process_mixed_data(mixed $data): mixed {
    return match(true) {
        is_string($data) => strtoupper($data),
        is_int($data) => $data * 2,
        is_float($data) => round($data, 2),
        is_bool($data) => !$data,
        is_array($data) => count($data),
        is_object($data) => get_class($data),
        is_null($data) => 'NULL_VALUE',
        default => 'UNKNOWN_TYPE'
    };
}

function flexible_storage(): mixed {
    static $counter = 0;
    $counter++;
    
    return match($counter % 6) {
        1 => "String value",
        2 => 42,
        3 => 3.14159,
        4 => true,
        5 => ['array', 'value'],
        0 => null
    };
}

function log_value(mixed $value, string $context = ''): void {
    $type = gettype($value);
    $display = match($type) {
        'string' => "'$value'",
        'array' => '[' . implode(', ', $value) . ']',
        'object' => get_class($value) . ' object',
        'boolean' => $value ? 'true' : 'false',
        'NULL' => 'null',
        default => (string) $value
    };
    
    echo ($context ? "[$context] " : "") . "Value: $display (type: $type)\n";
}

// Testing mixed type functionality
echo "Mixed type processing:\n";

$test_data = [
    "hello world",
    123,
    45.67,
    true,
    false,
    [1, 2, 3],
    new stdClass(),
    null
];

foreach ($test_data as $data) {
    $original_type = gettype($data);
    $processed = process_mixed_data($data);
    $processed_type = gettype($processed);
    
    echo "Input ($original_type) -> Output ($processed_type): ";
    log_value($processed);
}

echo "\nFlexible storage demonstration:\n";
for ($i = 1; $i <= 8; $i++) {
    $value = flexible_storage();
    log_value($value, "Call $i");
}

// Mixed type in class properties
class FlexibleContainer {
    public mixed $data = null;
    
    public function store(mixed $value): void {
        $this->data = $value;
    }
    
    public function retrieve(): mixed {
        return $this->data;
    }
    
    public function getInfo(): array {
        return [
            'has_data' => $this->data !== null,
            'type' => gettype($this->data),
            'size' => match(true) {
                is_string($this->data) => strlen($this->data),
                is_array($this->data) => count($this->data),
                is_object($this->data) => count(get_object_vars($this->data)),
                default => null
            }
        ];
    }
}

echo "\nFlexible container:\n";
$container = new FlexibleContainer();

$values = ["test string", [1, 2, 3, 4], 42];
foreach ($values as $value) {
    $container->store($value);
    $info = $container->getInfo();
    echo "Stored: " . json_encode($info) . "\n";
}
```

The mixed type accepts any value type, providing maximum flexibility.  
Use type checking functions and match expressions to handle different  
types safely. Mixed is useful for generic containers and APIs that  
accept varied input types.  

## Resource type

Working with PHP resources and external connections.  

```php
<?php

// File resource
$filename = '/tmp/test_resource.txt';
$content = "Hello there!\nThis is a test file for resource handling.\n";

echo "File resource operations:\n";

// Open file resource for writing
$file_handle = fopen($filename, 'w');
if (is_resource($file_handle)) {
    echo "File resource created: " . get_resource_type($file_handle) . "\n";
    
    fwrite($file_handle, $content);
    fclose($file_handle);
    echo "Data written and file closed\n";
}

// Open file resource for reading
$file_handle = fopen($filename, 'r');
if (is_resource($file_handle)) {
    echo "Reading from file resource:\n";
    while (!feof($file_handle)) {
        $line = fgets($file_handle);
        if ($line !== false) {
            echo "  Line: " . trim($line) . "\n";
        }
    }
    fclose($file_handle);
}

// Memory resource for temporary data
echo "\nMemory resource operations:\n";
$memory_handle = fopen('php://memory', 'r+');
if (is_resource($memory_handle)) {
    echo "Memory resource created: " . get_resource_type($memory_handle) . "\n";
    
    fwrite($memory_handle, "Temporary data in memory");
    rewind($memory_handle);  // Reset pointer to beginning
    
    $data = fread($memory_handle, 1024);
    echo "Read from memory: $data\n";
    
    fclose($memory_handle);
}

// Process resource (if available on system)
echo "\nProcess resource example:\n";
$cmd = PHP_OS_FAMILY === 'Windows' ? 'dir' : 'ls -la';
$process = popen($cmd, 'r');

if (is_resource($process)) {
    echo "Process resource created: " . get_resource_type($process) . "\n";
    echo "Command output (first 3 lines):\n";
    
    $line_count = 0;
    while (!feof($process) && $line_count < 3) {
        $line = fgets($process);
        if ($line !== false) {
            echo "  " . trim($line) . "\n";
            $line_count++;
        }
    }
    
    $return_value = pclose($process);
    echo "Process completed with return value: $return_value\n";
}

// Resource cleanup demonstration
echo "\nResource lifecycle:\n";
$resources = [];

// Create multiple resources
for ($i = 1; $i <= 3; $i++) {
    $resource = fopen('php://memory', 'r+');
    if (is_resource($resource)) {
        $resources[] = $resource;
        echo "Created resource $i: " . get_resource_type($resource) . "\n";
    }
}

// Show active resources
echo "Active resources: " . count($resources) . "\n";

// Clean up resources
foreach ($resources as $i => $resource) {
    if (is_resource($resource)) {
        fclose($resource);
        echo "Closed resource " . ($i + 1) . "\n";
    }
}

// Clean up test file
if (file_exists($filename)) {
    unlink($filename);
    echo "Test file cleaned up\n";
}
```

Resources represent external connections like files, network sockets,  
or database connections. Always check with is_resource() and use  
get_resource_type() for debugging. Properly close resources to prevent  
memory leaks and connection exhaustion.  

## Callable type

Working with callable types for function references and callbacks.  

```php
<?php

// Function definitions for testing
function simple_greeting(string $name): string {
    return "Hello there, $name!";
}

function calculate_tax(float $amount, float $rate = 0.1): float {
    return $amount * $rate;
}

class MathOperations {
    public static function square(int $number): int {
        return $number * $number;
    }
    
    public function double(int $number): int {
        return $number * 2;
    }
}

// Callable type demonstration
function process_with_callback(array $data, callable $processor): array {
    $result = [];
    foreach ($data as $item) {
        $result[] = $processor($item);
    }
    return $result;
}

function apply_operation(int|float $value, callable $operation): int|float {
    if (!is_callable($operation)) {
        throw new InvalidArgumentException("Operation must be callable");
    }
    return $operation($value);
}

echo "Callable type examples:\n";

// Different types of callables
$callables = [
    'function_name' => 'simple_greeting',
    'static_method' => ['MathOperations', 'square'],
    'static_method_string' => 'MathOperations::square',
    'instance_method' => [new MathOperations(), 'double'],
    'closure' => function(string $text): string {
        return strtoupper($text);
    },
    'arrow_function' => fn(int $x) => $x * 3
];

// Test callable validity
echo "Callable validation:\n";
foreach ($callables as $name => $callable) {
    $is_callable = is_callable($callable);
    echo "  $name: " . ($is_callable ? "valid" : "invalid") . "\n";
}

// Using callables with data processing
$numbers = [1, 2, 3, 4, 5];
$names = ['alice', 'bob', 'charlie'];

echo "\nData processing with callables:\n";

// Process numbers with different operations
$squared = process_with_callback($numbers, ['MathOperations', 'square']);
echo "Squared: " . implode(', ', $squared) . "\n";

$doubled = process_with_callback($numbers, [new MathOperations(), 'double']);
echo "Doubled: " . implode(', ', $doubled) . "\n";

$tripled = process_with_callback($numbers, fn($x) => $x * 3);
echo "Tripled: " . implode(', ', $tripled) . "\n";

// Process names with string operations
$greetings = process_with_callback($names, 'simple_greeting');
echo "Greetings: " . implode(', ', $greetings) . "\n";

$uppercase = process_with_callback($names, function($name) {
    return strtoupper($name);
});
echo "Uppercase: " . implode(', ', $uppercase) . "\n";

// Single value operations
echo "\nSingle value operations:\n";
$test_value = 5;

echo "Original value: $test_value\n";
echo "Squared: " . apply_operation($test_value, ['MathOperations', 'square']) . "\n";
echo "Tax (10%): " . apply_operation(100.0, fn($amount) => calculate_tax($amount, 0.1)) . "\n";

// Callback with closure scope
$multiplier = 4;
$custom_multiply = function(int $value) use ($multiplier): int {
    return $value * $multiplier;
};

echo "Custom multiply: " . apply_operation($test_value, $custom_multiply) . "\n";

// Built-in PHP functions as callbacks
$strings = ['  hello  ', '  WORLD  ', '  test  '];
$trimmed = array_map('trim', $strings);
$lowercased = array_map('strtolower', $trimmed);

echo "\nBuilt-in function callbacks:\n";
echo "Original: " . json_encode($strings) . "\n";
echo "Trimmed: " . json_encode($trimmed) . "\n";
echo "Lowercased: " . json_encode($lowercased) . "\n";
```

Callable types represent functions that can be invoked. This includes  
function names, static methods, instance methods, closures, and arrow  
functions. Use is_callable() to validate and call_user_func() for  
dynamic invocation when needed.  

## Iterable type

Working with iterable types for traversable data structures.  

```php
<?php

function process_iterable(iterable $data, callable $processor = null): array {
    $result = [];
    $processor = $processor ?? fn($item) => $item;
    
    foreach ($data as $key => $value) {
        $processed_value = $processor($value);
        $result[$key] = $processed_value;
    }
    
    return $result;
}

function count_iterable(iterable $data): int {
    $count = 0;
    foreach ($data as $item) {
        $count++;
    }
    return $count;
}

function filter_iterable(iterable $data, callable $filter): \Generator {
    foreach ($data as $key => $value) {
        if ($filter($value)) {
            yield $key => $value;
        }
    }
}

// Custom iterable class
class NumberRange implements \Iterator {
    private int $start;
    private int $end;
    private int $current;
    
    public function __construct(int $start, int $end) {
        $this->start = $start;
        $this->end = $end;
        $this->current = $start;
    }
    
    public function rewind(): void {
        $this->current = $this->start;
    }
    
    public function current(): int {
        return $this->current;
    }
    
    public function key(): int {
        return $this->current - $this->start;
    }
    
    public function next(): void {
        $this->current++;
    }
    
    public function valid(): bool {
        return $this->current <= $this->end;
    }
}

// Generator function
function fibonacci_generator(int $count): \Generator {
    $a = 0;
    $b = 1;
    
    for ($i = 0; $i < $count; $i++) {
        yield $i => $a;
        $temp = $a + $b;
        $a = $b;
        $b = $temp;
    }
}

echo "Iterable type examples:\n";

// Test with different iterable types
$iterables = [
    'array' => [1, 2, 3, 4, 5],
    'associative_array' => ['a' => 1, 'b' => 2, 'c' => 3],
    'range_object' => new NumberRange(10, 15),
    'generator' => fibonacci_generator(8)
];

foreach ($iterables as $name => $iterable) {
    echo "\n$name:\n";
    
    // Count items
    $count = count_iterable($iterable);
    echo "  Item count: $count\n";
    
    // Process with transformation
    $doubled = process_iterable($iterable, fn($x) => $x * 2);
    echo "  Doubled values: " . json_encode($doubled) . "\n";
    
    // Reset generator for filtering (needed because generators are consumed)
    if ($name === 'generator') {
        $iterable = fibonacci_generator(8);
    }
    
    // Filter even numbers
    $even_numbers = iterator_to_array(filter_iterable($iterable, fn($x) => $x % 2 === 0));
    echo "  Even numbers: " . json_encode($even_numbers) . "\n";
}

// Advanced iterable operations
echo "\nAdvanced iterable operations:\n";

// Chaining operations
$data = range(1, 10);
echo "Original data: " . implode(', ', $data) . "\n";

// Chain multiple operations
$result = iterator_to_array(
    filter_iterable(
        process_iterable($data, fn($x) => $x * $x),
        fn($x) => $x > 25
    )
);
echo "Squared and filtered (>25): " . json_encode($result) . "\n";

// Working with nested iterables
$nested_data = [
    'group1' => [1, 2, 3],
    'group2' => [4, 5, 6],
    'group3' => [7, 8, 9]
];

function flatten_iterable(iterable $nested): \Generator {
    foreach ($nested as $group) {
        if (is_iterable($group)) {
            foreach ($group as $item) {
                yield $item;
            }
        } else {
            yield $group;
        }
    }
}

$flattened = iterator_to_array(flatten_iterable($nested_data));
echo "Flattened nested data: " . implode(', ', $flattened) . "\n";

// Performance consideration with large datasets
function large_dataset_generator(int $size): \Generator {
    for ($i = 1; $i <= $size; $i++) {
        yield $i => "Item $i";
    }
}

echo "\nLarge dataset handling:\n";
$large_data = large_dataset_generator(1000000);  // 1M items
$first_five = [];
$count = 0;

foreach ($large_data as $key => $value) {
    if ($count < 5) {
        $first_five[] = $value;
        $count++;
    } else {
        break;
    }
}

echo "First 5 items from 1M dataset: " . implode(', ', $first_five) . "\n";
echo "Memory efficient: Only processed what was needed\n";
```

Iterable types accept both arrays and objects implementing Iterator  
or IteratorAggregate interfaces. Generators provide memory-efficient  
iteration over large datasets. Use iterator_to_array() to convert  
iterators to arrays when needed for array functions.  

## Object type

Working with object types and class instances.  

```php
<?php

// Basic class for demonstration
class Person {
    public function __construct(
        public string $name,
        public int $age,
        public string $email
    ) {}
    
    public function getInfo(): array {
        return [
            'name' => $this->name,
            'age' => $this->age,
            'email' => $this->email,
            'is_adult' => $this->age >= 18
        ];
    }
}

// Generic object processor
function process_object(object $obj): array {
    $reflection = new \ReflectionObject($obj);
    
    return [
        'class_name' => get_class($obj),
        'properties' => get_object_vars($obj),
        'methods' => array_map(
            fn($method) => $method->getName(),
            $reflection->getMethods(\ReflectionMethod::IS_PUBLIC)
        ),
        'object_id' => spl_object_id($obj),
        'object_hash' => spl_object_hash($obj)
    ];
}

function clone_and_modify(object $original): object {
    $cloned = clone $original;
    
    // Modify if it's a known type
    if ($cloned instanceof Person) {
        $cloned->name = "Modified " . $cloned->name;
    }
    
    return $cloned;
}

function compare_objects(object $obj1, object $obj2): array {
    return [
        'same_class' => get_class($obj1) === get_class($obj2),
        'equal_values' => $obj1 == $obj2,
        'identical_reference' => $obj1 === $obj2,
        'same_object_id' => spl_object_id($obj1) === spl_object_id($obj2)
    ];
}

echo "Object type examples:\n";

// Create different types of objects
$person = new Person("Alice Johnson", 28, "alice@example.com");
$std_obj = new stdClass();
$std_obj->name = "Standard Object";
$std_obj->value = 42;

$array_obj = (object) ['key1' => 'value1', 'key2' => 'value2'];

$objects = [
    'person' => $person,
    'std_object' => $std_obj,
    'array_cast' => $array_obj
];

// Process each object
foreach ($objects as $name => $obj) {
    echo "\n$name object:\n";
    $info = process_object($obj);
    
    echo "  Class: " . $info['class_name'] . "\n";
    echo "  Properties: " . json_encode($info['properties']) . "\n";
    echo "  Methods: " . implode(', ', $info['methods']) . "\n";
    echo "  Object ID: " . $info['object_id'] . "\n";
}

// Object cloning
echo "\nObject cloning:\n";
$original_person = new Person("Bob Wilson", 35, "bob@example.com");
$cloned_person = clone_and_modify($original_person);

echo "Original: " . json_encode($original_person->getInfo()) . "\n";
echo "Cloned: " . json_encode($cloned_person->getInfo()) . "\n";

$comparison = compare_objects($original_person, $cloned_person);
echo "Comparison: " . json_encode($comparison) . "\n";

// Object references vs copies
echo "\nObject references:\n";
$ref1 = $person;
$ref2 = clone $person;

echo "Original person age: " . $person->age . "\n";

$ref1->age = 30;  // Modifies original through reference
echo "After ref1 age change: " . $person->age . "\n";

$ref2->age = 25;  // Modifies clone only
echo "After ref2 age change: " . $person->age . "\n";

$ref_comparison1 = compare_objects($person, $ref1);
$ref_comparison2 = compare_objects($person, $ref2);

echo "person vs ref1: " . json_encode($ref_comparison1) . "\n";
echo "person vs ref2: " . json_encode($ref_comparison2) . "\n";

// Anonymous objects
echo "\nAnonymous objects:\n";
$calculator = new class(10) {
    private int $base;
    
    public function __construct(int $base) {
        $this->base = $base;
    }
    
    public function add(int $value): int {
        return $this->base + $value;
    }
    
    public function multiply(int $value): int {
        return $this->base * $value;
    }
};

echo "Anonymous class object:\n";
$calc_info = process_object($calculator);
echo "  Class: " . $calc_info['class_name'] . "\n";
echo "  Methods: " . implode(', ', $calc_info['methods']) . "\n";
echo "  10 + 5 = " . $calculator->add(5) . "\n";
echo "  10 * 3 = " . $calculator->multiply(3) . "\n";

// Object serialization
echo "\nObject serialization:\n";
$serialized = serialize($person);
echo "Serialized length: " . strlen($serialized) . " bytes\n";

$unserialized = unserialize($serialized);
$serial_comparison = compare_objects($person, $unserialized);
echo "Original vs unserialized: " . json_encode($serial_comparison) . "\n";

// JSON representation of objects
$json_person = json_encode($person);
echo "JSON representation: $json_person\n";

$from_json = json_decode($json_person);  // Creates stdClass
echo "Reconstructed from JSON: " . get_class($from_json) . "\n";
echo "JSON object properties: " . json_encode(get_object_vars($from_json)) . "\n";
```

Object types represent instances of classes. Use get_class(),  
get_object_vars(), and reflection for introspection. Objects are  
passed by reference, so use clone for independent copies. Anonymous  
classes provide inline object definitions for specific use cases.  
