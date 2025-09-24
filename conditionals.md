# Conditionals

This chapter covers 25 PHP conditional examples demonstrating various ways  
to control program flow based on conditions. These examples progress from  
basic if statements to advanced conditional patterns using modern PHP 8.4  
features. Each example includes clear explanations to help you understand  
the underlying concepts and apply them effectively in your code.  

## Basic if statement

Making simple decisions in your code.  

```php
<?php

$age = 18;

if ($age >= 18) {
    echo "You are an adult\n";
}
```

The basic `if` statement executes code only when a condition evaluates  
to true. PHP uses comparison operators like `>=` to test conditions.  
Braces are optional for single statements but recommended for clarity.  

## If-else statement

Choosing between two alternatives based on a condition.  

```php
<?php

$temperature = 25;

if ($temperature > 30) {
    echo "It's hot outside\n";
} else {
    echo "It's not too hot\n";
}
```

The `if-else` statement provides two execution paths: one for when the  
condition is true and another for when it's false. This ensures one  
of the two code blocks always executes.  

## If-elseif-else chain

Testing multiple conditions in sequence.  

```php
<?php

$grade = 85;

if ($grade >= 90) {
    echo "Excellent! Grade: A\n";
} elseif ($grade >= 80) {
    echo "Good job! Grade: B\n";
} elseif ($grade >= 70) {
    echo "Fair performance! Grade: C\n";
} else {
    echo "Needs improvement\n";
}
```

The `elseif` chain allows testing multiple conditions sequentially.  
Only the first matching condition executes, making this efficient  
for mutually exclusive conditions like grade ranges.  

## Nested if statements

Placing if statements inside other if statements for complex logic.  

```php
<?php

$user_age = 25;
$has_license = true;
$has_insurance = false;

if ($user_age >= 18) {
    echo "Age requirement met\n";
    if ($has_license) {
        echo "License verified\n";
        if ($has_insurance) {
            echo "Ready to drive!\n";
        } else {
            echo "Insurance required\n";
        }
    } else {
        echo "License required\n";
    }
} else {
    echo "Must be 18 or older\n";
}
```

Nested conditionals allow checking multiple related conditions step by  
step. Each inner condition only executes if its parent condition is  
true, creating a hierarchy of checks.  

## Ternary operator

Concise conditional assignment for simple cases.  

```php
<?php

$score = 75;
$result = ($score >= 60) ? "Pass" : "Fail";
echo "Result: $result\n";

// Multiple ternary operators
$grade = ($score >= 90) ? "A" : (($score >= 80) ? "B" : "C");
echo "Grade: $grade\n";
```

The ternary operator `condition ? value_if_true : value_if_false`  
provides a compact way to assign values based on conditions. Avoid  
nesting too deeply as it reduces readability.  

## Null coalescing operator

Handling null values with default assignments.  

```php
<?php

$username = null;
$config = ['timeout' => null, 'retries' => 3];

// Null coalescing operator
$display_name = $username ?? "Guest";
echo "Welcome, $display_name\n";

// Chaining multiple null coalescing operators
$timeout = $config['timeout'] ?? $_ENV['TIMEOUT'] ?? 30;
echo "Timeout: $timeout seconds\n";

// Null coalescing assignment (PHP 7.4+)
$config['cache'] ??= true;
echo "Cache enabled: " . ($config['cache'] ? 'Yes' : 'No') . "\n";
```

The null coalescing operator `??` returns the right operand when the  
left is null. The assignment version `??=` only assigns if the  
variable is null, preventing overwriting existing values.  

## Match expression

Modern pattern matching for clean conditional logic.  

```php
<?php

$status_code = 404;

$message = match($status_code) {
    200 => "Success",
    201 => "Created",
    400 => "Bad Request",
    401 => "Unauthorized",
    404 => "Not Found",
    500 => "Internal Server Error",
    default => "Unknown Status"
};

echo "Status: $message\n";

// Match with multiple values
$day = "Saturday";
$day_type = match($day) {
    "Monday", "Tuesday", "Wednesday", "Thursday", "Friday" => "Weekday",
    "Saturday", "Sunday" => "Weekend"
};

echo "Day type: $day_type\n";
```

The `match` expression (PHP 8+) provides strict comparison and returns  
values directly. Unlike switch statements, match doesn't fall through  
and supports multiple values per case.  

## Switch statement

Traditional multi-branch conditional logic.  

```php
<?php

$operation = "add";
$a = 10;
$b = 5;

switch ($operation) {
    case "add":
        $result = $a + $b;
        echo "Addition: $result\n";
        break;
    case "subtract":
        $result = $a - $b;
        echo "Subtraction: $result\n";
        break;
    case "multiply":
        $result = $a * $b;
        echo "Multiplication: $result\n";
        break;
    case "divide":
        if ($b != 0) {
            $result = $a / $b;
            echo "Division: $result\n";
        } else {
            echo "Cannot divide by zero\n";
        }
        break;
    default:
        echo "Unknown operation\n";
}
```

Switch statements compare a variable against multiple values using  
loose comparison. The `break` statement prevents fall-through to  
subsequent cases. Always include a `default` case for unexpected values.  

## Switch with fall-through

Using intentional fall-through for grouped cases.  

```php
<?php

$month = 7;

switch ($month) {
    case 12:
    case 1:
    case 2:
        echo "Winter season\n";
        break;
    case 3:
    case 4:
    case 5:
        echo "Spring season\n";
        break;
    case 6:
    case 7:
    case 8:
        echo "Summer season\n";
        break;
    case 9:
    case 10:
    case 11:
        echo "Autumn season\n";
        break;
    default:
        echo "Invalid month\n";
}
```

Intentional fall-through allows multiple cases to execute the same  
code. Omitting `break` statements lets execution continue to the next  
case, useful for grouping related values.  

## Logical operators in conditions

Combining multiple conditions with logical operators.  

```php
<?php

$age = 25;
$has_job = true;
$credit_score = 700;

// AND operator
if ($age >= 21 && $has_job && $credit_score > 650) {
    echo "Loan approved\n";
} else {
    echo "Loan denied\n";
}

// OR operator
if ($age >= 65 || $has_job || $credit_score > 800) {
    echo "Eligible for premium services\n";
}

// NOT operator
if (!$has_job) {
    echo "Job application recommended\n";
} else {
    echo "Employment status: Active\n";
}
```

Logical operators `&&` (AND), `||` (OR), and `!` (NOT) combine boolean  
expressions. PHP also supports `and`, `or`, and `not` keywords with  
different precedence levels.  

## Comparison operators

Testing relationships between values.  

```php
<?php

$x = 10;
$y = "10";
$z = 15;

// Equality (loose comparison)
if ($x == $y) {
    echo "x equals y (loose)\n";
}

// Identity (strict comparison)
if ($x === $y) {
    echo "x identical to y (strict)\n";
} else {
    echo "x not identical to y (different types)\n";
}

// Inequality
if ($x != $z) {
    echo "x not equal to z\n";
}

// Less than, greater than
if ($x < $z) {
    echo "x is less than z\n";
}

if ($z >= $x) {
    echo "z is greater than or equal to x\n";
}
```

PHP provides loose (`==`, `!=`) and strict (`===`, `!==`) comparison  
operators. Strict comparison checks both value and type, preventing  
unexpected type coercion issues.  

## Type checking conditions

Verifying variable types before processing.  

```php
<?php

$data = [1, "hello", 3.14, true, null];

foreach ($data as $item) {
    if (is_int($item)) {
        echo "Integer: $item\n";
    } elseif (is_string($item)) {
        echo "String: $item\n";
    } elseif (is_float($item)) {
        echo "Float: $item\n";
    } elseif (is_bool($item)) {
        echo "Boolean: " . ($item ? "true" : "false") . "\n";
    } elseif (is_null($item)) {
        echo "Null value\n";
    } else {
        echo "Unknown type\n";
    }
}

// Using gettype for dynamic checking
$value = "test";
if (gettype($value) === "string") {
    echo "Confirmed string type\n";
}
```

Type checking functions like `is_int()`, `is_string()`, and `is_null()`  
verify variable types before processing. This prevents type-related  
errors and enables type-specific handling.  

## Empty and isset checks

Testing variable existence and content.  

```php
<?php

$username = "";
$password = null;
$data = [];
$count = 0;

// isset() checks if variable exists and is not null
if (isset($username)) {
    echo "Username variable exists\n";
}

if (!isset($password)) {
    echo "Password is not set\n";
}

// empty() checks for "empty" values
if (empty($username)) {
    echo "Username is empty\n";
}

if (empty($data)) {
    echo "Data array is empty\n";
}

// Combining isset and empty
if (isset($count) && !empty($count)) {
    echo "Count has a non-empty value\n";
} else {
    echo "Count is zero or not set\n";
}

// null coalescing for default values
$display_count = $count ?? "N/A";
echo "Display count: $display_count\n";
```

The `isset()` function checks variable existence and non-null status.  
The `empty()` function returns true for null, false, 0, "", and empty  
arrays. Use both to handle undefined variables safely.  

## String comparisons

Comparing strings with various methods and case sensitivity.  

```php
<?php

$name1 = "Alice";
$name2 = "alice";
$password = "secret123";

// Case-sensitive comparison
if ($name1 === $name2) {
    echo "Names match exactly\n";
} else {
    echo "Names don't match (case-sensitive)\n";
}

// Case-insensitive comparison
if (strcasecmp($name1, $name2) === 0) {
    echo "Names match (case-insensitive)\n";
}

// String length checks
if (strlen($password) >= 8) {
    echo "Password meets minimum length\n";
} else {
    echo "Password too short\n";
}

// Pattern matching
if (str_starts_with($password, "secret")) {
    echo "Password starts with 'secret'\n";
}

if (str_contains($password, "123")) {
    echo "Password contains numbers\n";
}
```

String comparison in PHP is case-sensitive by default. Use functions  
like `strcasecmp()` for case-insensitive comparison and `str_contains()`,  
`str_starts_with()` for pattern matching (PHP 8+).  

## Numeric comparisons

Handling different numeric types and precision issues.  

```php
<?php

$int_val = 42;
$float_val = 42.0;
$string_num = "42";
$precision_test = 0.1 + 0.2;

// Type-aware numeric comparison
if ($int_val == $float_val) {
    echo "Integer equals float (loose comparison)\n";
}

if ($int_val === $float_val) {
    echo "Integer identical to float\n";
} else {
    echo "Integer not identical to float (different types)\n";
}

// String to number conversion
if ($int_val == $string_num) {
    echo "Integer equals string number (with conversion)\n";
}

// Floating point precision handling
if (abs($precision_test - 0.3) < 0.0001) {
    echo "Float precision handled correctly\n";
}

// Range checking
$score = 85;
if ($score >= 0 && $score <= 100) {
    echo "Score is within valid range\n";
}
```

PHP automatically converts strings to numbers in numeric contexts.  
For floating-point comparisons, use absolute difference checks due  
to precision limitations. Always validate numeric ranges.  

## Array condition checks

Testing array properties and contents.  

```php
<?php

$empty_array = [];
$numbers = [1, 2, 3, 4, 5];
$mixed_array = ["name" => "John", "age" => 30, "active" => true];

// Check if array is empty
if (empty($empty_array)) {
    echo "Array is empty\n";
}

// Check array length
if (count($numbers) > 0) {
    echo "Numbers array has " . count($numbers) . " elements\n";
}

// Check if key exists
if (array_key_exists("name", $mixed_array)) {
    echo "Name key exists\n";
}

// Check if value exists
if (in_array(3, $numbers)) {
    echo "Value 3 exists in numbers array\n";
}

// Check for specific conditions in arrays
$all_positive = true;
foreach ($numbers as $num) {
    if ($num <= 0) {
        $all_positive = false;
        break;
    }
}

if ($all_positive) {
    echo "All numbers are positive\n";
}
```

Array condition checks include testing for emptiness with `empty()`,  
size with `count()`, key existence with `array_key_exists()`, and  
value existence with `in_array()`. Use loops for complex conditions.  

## Boolean expressions

Working with boolean values and truthiness.  

```php
<?php

$is_logged_in = true;
$is_admin = false;
$user_count = 0;
$user_name = "";

// Direct boolean checks
if ($is_logged_in) {
    echo "User is logged in\n";
}

if (!$is_admin) {
    echo "User is not an admin\n";
}

// Truthiness evaluation
if ($user_count) {
    echo "There are users\n";
} else {
    echo "No users found\n";
}

if ($user_name) {
    echo "User name provided\n";
} else {
    echo "No user name provided\n";
}

// Boolean function results
function has_permission($role) {
    return $role === "admin" || $role === "moderator";
}

if (has_permission("admin")) {
    echo "Access granted\n";
}

// Converting to boolean
$status = $user_count ? true : false;
echo "Status as boolean: " . ($status ? "true" : "false") . "\n";
```

PHP treats various values as "falsy": false, 0, 0.0, "", "0", null,  
and empty arrays. All other values are "truthy". Use explicit  
boolean conversion when the distinction matters.  

## Short-circuit evaluation

Optimizing conditions with lazy evaluation.  

```php
<?php

function expensive_operation() {
    echo "Expensive operation called\n";
    return true;
}

function quick_check() {
    echo "Quick check called\n";
    return false;
}

$user_active = false;
$feature_enabled = true;

// AND short-circuit: if first condition is false, second is not evaluated
echo "Testing AND short-circuit:\n";
if ($user_active && expensive_operation()) {
    echo "Both conditions true\n";
} else {
    echo "First condition false, second not evaluated\n";
}

echo "\nTesting OR short-circuit:\n";
// OR short-circuit: if first condition is true, second is not evaluated
if ($feature_enabled || expensive_operation()) {
    echo "First condition true, second not evaluated\n";
}

echo "\nNo short-circuit example:\n";
if (quick_check() && expensive_operation()) {
    echo "Both conditions checked\n";
}
```

Short-circuit evaluation stops evaluating logical expressions once  
the result is determined. With AND (`&&`), if the first operand is  
false, the second isn't evaluated. With OR (`||`), if the first  
operand is true, the second isn't evaluated.  

## Spaceship operator

Three-way comparison for sorting and ordering.  

```php
<?php

function compare_values($a, $b) {
    $result = $a <=> $b;
    
    if ($result < 0) {
        return "$a is less than $b";
    } elseif ($result > 0) {
        return "$a is greater than $b";
    } else {
        return "$a equals $b";
    }
}

echo compare_values(5, 10) . "\n";
echo compare_values(10, 5) . "\n";
echo compare_values(7, 7) . "\n";

// Using spaceship operator in sorting
$numbers = [3, 1, 4, 1, 5, 9, 2, 6];
usort($numbers, function($a, $b) {
    return $a <=> $b;  // Ascending sort
});

echo "Sorted numbers: " . implode(", ", $numbers) . "\n";

// String comparison with spaceship
$strings = ["banana", "apple", "cherry"];
usort($strings, function($a, $b) {
    return $a <=> $b;
});

echo "Sorted strings: " . implode(", ", $strings) . "\n";
```

The spaceship operator `<=>` returns -1, 0, or 1 for less than,  
equal to, or greater than comparisons. It's particularly useful  
for custom sorting functions and three-way comparisons.  

## Multiple conditions with AND/OR

Combining complex logical expressions effectively.  

```php
<?php

$age = 25;
$income = 50000;
$credit_score = 720;
$employment_years = 3;
$has_cosigner = false;

// Complex AND conditions
if ($age >= 21 && $income >= 30000 && $credit_score > 650 && 
    $employment_years >= 2) {
    echo "Qualifies for standard loan\n";
} elseif ($age >= 18 && ($income >= 25000 || $has_cosigner) && 
         $credit_score > 600) {
    echo "Qualifies for assisted loan\n";
} else {
    echo "Does not qualify for loan\n";
}

// Using variables for readability
$age_requirement = $age >= 21;
$income_requirement = $income >= 30000;
$credit_requirement = $credit_score > 650;
$experience_requirement = $employment_years >= 2;

if ($age_requirement && $income_requirement && $credit_requirement && 
    $experience_requirement) {
    echo "All requirements met (readable version)\n";
}

// Grouping with parentheses
$weekend = true;
$holiday = false;
$urgent = true;

if (($weekend || $holiday) && !$urgent) {
    echo "Work can wait until next business day\n";
} else {
    echo "Work needs attention now\n";
}
```

Complex conditions benefit from clear grouping with parentheses and  
meaningful variable names. Break down complex expressions into  
smaller, named boolean variables for better readability.  

## Complex nested conditions

Managing deeply nested conditional logic.  

```php
<?php

$user = [
    'age' => 25,
    'subscription' => 'premium',
    'country' => 'US',
    'verification' => [
        'email' => true,
        'phone' => true,
        'identity' => false
    ],
    'usage' => [
        'last_login' => '2024-01-15',
        'total_hours' => 120
    ]
];

// Complex nested conditions
if ($user['age'] >= 18) {
    if ($user['subscription'] === 'premium' || 
        $user['subscription'] === 'pro') {
        if ($user['country'] === 'US' || $user['country'] === 'CA') {
            if ($user['verification']['email'] && 
                $user['verification']['phone']) {
                if ($user['usage']['total_hours'] >= 100) {
                    echo "Eligible for advanced features\n";
                } else {
                    echo "Need more usage hours for advanced features\n";
                }
            } else {
                echo "Account verification incomplete\n";
            }
        } else {
            echo "Service not available in your country\n";
        }
    } else {
        echo "Premium subscription required\n";
    }
} else {
    echo "Must be 18 or older\n";
}

// Refactored with early returns (better approach)
function check_eligibility($user) {
    if ($user['age'] < 18) {
        return "Must be 18 or older";
    }
    
    if (!in_array($user['subscription'], ['premium', 'pro'])) {
        return "Premium subscription required";
    }
    
    if (!in_array($user['country'], ['US', 'CA'])) {
        return "Service not available in your country";
    }
    
    if (!$user['verification']['email'] || !$user['verification']['phone']) {
        return "Account verification incomplete";
    }
    
    if ($user['usage']['total_hours'] < 100) {
        return "Need more usage hours for advanced features";
    }
    
    return "Eligible for advanced features";
}

echo check_eligibility($user) . "\n";
```

Deep nesting makes code hard to read and maintain. Consider using  
early returns, guard clauses, or extracting conditions into separate  
functions to improve readability and maintainability.  

## Conditional assignment

Assigning values based on conditions efficiently.  

```php
<?php

$user_role = 'admin';
$is_weekend = true;
$server_load = 0.8;

// Ternary operator for simple assignment
$max_users = ($user_role === 'admin') ? 1000 : 100;
echo "Max users: $max_users\n";

// Null coalescing for default values
$timeout = $_GET['timeout'] ?? ($is_weekend ? 60 : 30);
echo "Timeout: $timeout seconds\n";

// Match expression for multiple options
$cache_duration = match($user_role) {
    'guest' => 300,      // 5 minutes
    'user' => 1800,      // 30 minutes
    'premium' => 3600,   // 1 hour
    'admin' => 86400,    // 24 hours
    default => 600       // 10 minutes
};
echo "Cache duration: $cache_duration seconds\n";

// Conditional assignment with complex logic
$priority = 'normal';
if ($server_load > 0.9) {
    $priority = 'critical';
} elseif ($server_load > 0.7) {
    $priority = 'high';
} elseif ($server_load < 0.3) {
    $priority = 'low';
}
echo "System priority: $priority\n";

// Using array for lookup-based assignment
$status_messages = [
    200 => 'Success',
    404 => 'Not Found', 
    500 => 'Server Error'
];
$http_code = 404;
$message = $status_messages[$http_code] ?? 'Unknown Status';
echo "HTTP Status: $message\n";
```

Conditional assignment patterns include ternary operators for simple  
cases, null coalescing for defaults, match expressions for multiple  
options, and array lookups for mapping values.  

## Guard clauses pattern

Using early returns to reduce nesting and improve readability.  

```php
<?php

function process_order($order) {
    // Guard clauses - early returns for invalid conditions
    if (empty($order)) {
        return "Error: Order is empty";
    }
    
    if (!isset($order['customer_id'])) {
        return "Error: Customer ID required";
    }
    
    if (!isset($order['items']) || count($order['items']) === 0) {
        return "Error: Order must contain items";
    }
    
    if ($order['total'] <= 0) {
        return "Error: Order total must be positive";
    }
    
    // Main logic executes only when all conditions are met
    $processed_order = [
        'id' => uniqid('order_'),
        'customer_id' => $order['customer_id'],
        'items' => $order['items'],
        'total' => $order['total'],
        'status' => 'confirmed',
        'timestamp' => time()
    ];
    
    return "Order processed successfully: " . $processed_order['id'];
}

// Test cases
$valid_order = [
    'customer_id' => 'cust_123',
    'items' => ['product_1', 'product_2'],
    'total' => 99.99
];

$invalid_order = [
    'customer_id' => 'cust_456',
    'items' => [],
    'total' => 0
];

echo process_order($valid_order) . "\n";
echo process_order($invalid_order) . "\n";
echo process_order([]) . "\n";
```

Guard clauses use early returns to handle invalid conditions first,  
reducing nesting levels and making the main logic clearer. This  
pattern improves readability and maintainability of complex functions.  

## Early returns

Simplifying function logic with strategic return statements.  

```php
<?php

function calculate_discount($customer_type, $order_total, $is_member) {
    // Early return for invalid input
    if ($order_total <= 0) {
        return 0;
    }
    
    // Early return for VIP customers
    if ($customer_type === 'vip') {
        return min($order_total * 0.3, 500); // Max $500 discount
    }
    
    // Early return for non-members with small orders
    if (!$is_member && $order_total < 100) {
        return 0;
    }
    
    // Early return for premium members
    if ($is_member && $customer_type === 'premium') {
        return $order_total * 0.2;
    }
    
    // Early return for regular members
    if ($is_member) {
        return $order_total * 0.1;
    }
    
    // Default case for non-members with large orders
    return $order_total >= 200 ? $order_total * 0.05 : 0;
}

function validate_password($password) {
    if (strlen($password) < 8) {
        return "Password must be at least 8 characters";
    }
    
    if (!preg_match('/[A-Z]/', $password)) {
        return "Password must contain uppercase letter";
    }
    
    if (!preg_match('/[a-z]/', $password)) {
        return "Password must contain lowercase letter";
    }
    
    if (!preg_match('/[0-9]/', $password)) {
        return "Password must contain number";
    }
    
    if (!preg_match('/[^A-Za-z0-9]/', $password)) {
        return "Password must contain special character";
    }
    
    return "Password is valid";
}

// Test the functions
echo "Discount: $" . calculate_discount('vip', 1000, true) . "\n";
echo "Discount: $" . calculate_discount('regular', 50, false) . "\n";

echo validate_password("weak") . "\n";
echo validate_password("StrongPass123!") . "\n";
```

Early returns eliminate deeply nested conditions by handling special  
cases first. This creates cleaner, more readable code with reduced  
complexity and easier debugging.  

## Exception-based conditionals

Using exceptions for error handling and flow control.  

```php
<?php

class ValidationException extends Exception {}
class InsufficientFundsException extends Exception {}
class AccountLockedException extends Exception {}

function transfer_funds($from_account, $to_account, $amount) {
    try {
        // Validation checks with exceptions
        if ($amount <= 0) {
            throw new ValidationException("Transfer amount must be positive");
        }
        
        if ($from_account['balance'] < $amount) {
            throw new InsufficientFundsException(
                "Insufficient funds in source account"
            );
        }
        
        if ($from_account['locked'] || $to_account['locked']) {
            throw new AccountLockedException("One or both accounts are locked");
        }
        
        // Perform transfer
        $from_account['balance'] -= $amount;
        $to_account['balance'] += $amount;
        
        return [
            'success' => true,
            'message' => "Transfer of $$amount completed successfully",
            'from_balance' => $from_account['balance'],
            'to_balance' => $to_account['balance']
        ];
        
    } catch (ValidationException $e) {
        return [
            'success' => false, 
            'error' => 'Validation Error: ' . $e->getMessage()
        ];
    } catch (InsufficientFundsException $e) {
        return [
            'success' => false, 
            'error' => 'Funds Error: ' . $e->getMessage()
        ];
    } catch (AccountLockedException $e) {
        return [
            'success' => false, 
            'error' => 'Account Error: ' . $e->getMessage()
        ];
    } catch (Exception $e) {
        return [
            'success' => false, 
            'error' => 'Unexpected Error: ' . $e->getMessage()
        ];
    }
}

// Test scenarios
$account_a = ['balance' => 1000, 'locked' => false];
$account_b = ['balance' => 500, 'locked' => false];
$account_c = ['balance' => 100, 'locked' => true];

echo "Test 1: Valid transfer\n";
$result = transfer_funds($account_a, $account_b, 200);
echo $result['success'] ? $result['message'] : $result['error'];
echo "\n\n";

echo "Test 2: Insufficient funds\n";
$result = transfer_funds($account_b, $account_a, 1000);
echo $result['success'] ? $result['message'] : $result['error'];
echo "\n\n";

echo "Test 3: Locked account\n";
$result = transfer_funds($account_a, $account_c, 100);
echo $result['success'] ? $result['message'] : $result['error'];
echo "\n";
```

Exception-based conditionals separate error handling from main logic  
flow. Different exception types enable specific error handling while  
keeping the main code path clean and readable.
