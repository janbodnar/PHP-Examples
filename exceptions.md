# Exceptions

This chapter covers 25 comprehensive PHP exception examples designed to  
demonstrate robust error handling in modern PHP 8.4. These examples  
progress from basic exception handling to advanced patterns including  
custom exceptions, exception chaining, and sophisticated error recovery  
strategies. Each example includes detailed explanations to help you  
master exception handling in your PHP applications.  

## Basic try-catch block

Handling exceptions with fundamental try-catch syntax.  

```php
<?php

try {
    $result = 10 / 0;
    echo "Result: $result\n";
} catch (DivisionByZeroError $e) {
    echo "Error: Cannot divide by zero!\n";
}

// Handling multiple operations
try {
    $data = json_decode('{"invalid": json}');
    if (json_last_error() !== JSON_ERROR_NONE) {
        throw new JsonException("Invalid JSON data");
    }
} catch (JsonException $e) {
    echo "JSON Error: " . $e->getMessage() . "\n";
}
```

The `try` block contains code that might throw exceptions. The `catch`  
block handles specific exception types. PHP automatically throws  
`DivisionByZeroError` for division by zero operations.  

## Multiple catch blocks

Handling different exception types with specific responses.  

```php
<?php

function processUserInput($input) {
    try {
        if (empty($input)) {
            throw new InvalidArgumentException("Input cannot be empty");
        }
        
        $number = (int) $input;
        if ($number < 0) {
            throw new OutOfRangeException("Number must be positive");
        }
        
        return 100 / $number;
    } catch (InvalidArgumentException $e) {
        echo "Validation Error: " . $e->getMessage() . "\n";
    } catch (OutOfRangeException $e) {
        echo "Range Error: " . $e->getMessage() . "\n";
    } catch (DivisionByZeroError $e) {
        echo "Math Error: Division by zero\n";
    }
    return null;
}

processUserInput("");     // Validation Error: Input cannot be empty
processUserInput("-5");   // Range Error: Number must be positive
processUserInput("0");    // Math Error: Division by zero
```

Multiple `catch` blocks allow handling different exception types with  
specific logic. Exception handling follows inheritance - more specific  
exceptions should be caught before their parent classes.  

## Throwing custom exceptions

Creating and throwing your own exception types.  

```php
<?php

function validateEmail($email) {
    if (empty($email)) {
        throw new Exception("Email is required");
    }
    
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        throw new Exception("Invalid email format");
    }
    
    if (strlen($email) > 100) {
        throw new Exception("Email too long (max 100 characters)");
    }
    
    return true;
}

function registerUser($email, $password) {
    try {
        validateEmail($email);
        
        if (strlen($password) < 8) {
            throw new Exception("Password must be at least 8 characters");
        }
        
        echo "User registered successfully: $email\n";
    } catch (Exception $e) {
        echo "Registration failed: " . $e->getMessage() . "\n";
    }
}

registerUser("", "short");           // Registration failed: Email is required
registerUser("invalid-email", "password123"); // Registration failed: Invalid email format
registerUser("test@example.com", "password123"); // User registered successfully
```

Use `throw new Exception()` to create and throw exceptions with custom  
messages. This allows functions to signal errors to calling code  
instead of returning error codes or null values.  

## Finally block

Code that always executes regardless of exceptions.  

```php
<?php

function processFile($filename) {
    $file = null;
    
    try {
        if (!file_exists($filename)) {
            throw new Exception("File not found: $filename");
        }
        
        $file = fopen($filename, 'r');
        if (!$file) {
            throw new Exception("Cannot open file: $filename");
        }
        
        $content = fread($file, 1000);
        echo "File content: " . substr($content, 0, 50) . "...\n";
        
    } catch (Exception $e) {
        echo "Error processing file: " . $e->getMessage() . "\n";
    } finally {
        if ($file) {
            fclose($file);
            echo "File handle closed\n";
        }
        echo "Cleanup completed\n";
    }
}

// Create a test file temporarily
file_put_contents('/tmp/test.txt', 'Hello there! This is test content.');

processFile('/tmp/test.txt');      // Processes file successfully
processFile('/tmp/missing.txt');   // Error but cleanup still runs
```

The `finally` block executes after try/catch blocks regardless of  
whether an exception occurred. Use it for cleanup operations like  
closing files, database connections, or releasing resources.  

## Exception hierarchy

Understanding PHP's built-in exception classes.  

```php
<?php

function demonstrateExceptionTypes() {
    $exceptions = [
        function() { throw new Error("System error"); },
        function() { throw new ArgumentCountError("Wrong argument count"); },
        function() { throw new TypeError("Type mismatch"); },
        function() { throw new ParseError("Parse error"); },
        function() { throw new Exception("General exception"); },
        function() { throw new InvalidArgumentException("Invalid argument"); },
        function() { throw new OutOfBoundsException("Out of bounds"); },
        function() { throw new RuntimeException("Runtime error"); }
    ];
    
    foreach ($exceptions as $index => $thrower) {
        try {
            $thrower();
        } catch (Error $e) {
            echo "Error #$index: " . get_class($e) . " - " . $e->getMessage() . "\n";
        } catch (Exception $e) {
            echo "Exception #$index: " . get_class($e) . " - " . $e->getMessage() . "\n";
        }
    }
}

demonstrateExceptionTypes();
```

PHP has two main exception hierarchies: `Error` for internal errors  
and `Exception` for user-level exceptions. `Error` includes `TypeError`,  
`ArgumentCountError`, etc. `Exception` includes domain-specific types  
like `InvalidArgumentException` and `RuntimeException`.  

## Custom exception classes

Creating specialized exception types for your application.  

```php
<?php

class ValidationException extends Exception {
    private array $errors = [];
    
    public function __construct(string $message, array $errors = []) {
        parent::__construct($message);
        $this->errors = $errors;
    }
    
    public function getErrors(): array {
        return $this->errors;
    }
    
    public function addError(string $field, string $message): void {
        $this->errors[$field] = $message;
    }
}

class DatabaseException extends Exception {
    private string $query;
    
    public function __construct(string $message, string $query = '') {
        parent::__construct($message);
        $this->query = $query;
    }
    
    public function getQuery(): string {
        return $this->query;
    }
}

function validateUser($userData) {
    $errors = [];
    
    if (empty($userData['name'])) {
        $errors['name'] = 'Name is required';
    }
    
    if (empty($userData['email']) || !filter_var($userData['email'], FILTER_VALIDATE_EMAIL)) {
        $errors['email'] = 'Valid email is required';
    }
    
    if (!empty($errors)) {
        throw new ValidationException('User validation failed', $errors);
    }
}

try {
    validateUser(['name' => '', 'email' => 'invalid']);
} catch (ValidationException $e) {
    echo "Validation Error: " . $e->getMessage() . "\n";
    foreach ($e->getErrors() as $field => $error) {
        echo "- $field: $error\n";
    }
}
```

Custom exception classes can store additional context data like  
validation errors or SQL queries. Extend `Exception` and add properties  
and methods specific to your domain needs.  

## Exception chaining

Preserving error context through exception chains.  

```php
<?php

class ServiceException extends Exception {}
class ApiException extends Exception {}

function callExternalAPI($url) {
    // Simulate API call failure
    throw new ApiException("API endpoint unavailable: $url");
}

function processData($id) {
    try {
        $url = "https://api.example.com/data/$id";
        return callExternalAPI($url);
    } catch (ApiException $e) {
        // Chain the original exception
        throw new ServiceException(
            "Failed to process data for ID: $id", 
            0, 
            $e
        );
    }
}

function handleRequest($id) {
    try {
        return processData($id);
    } catch (ServiceException $e) {
        echo "Service Error: " . $e->getMessage() . "\n";
        
        // Access the original exception
        $previous = $e->getPrevious();
        if ($previous) {
            echo "Caused by: " . $previous->getMessage() . "\n";
            echo "Original exception type: " . get_class($previous) . "\n";
        }
    }
}

handleRequest(123);
```

Exception chaining preserves the original error context while allowing  
higher-level functions to throw more appropriate exceptions. Use the  
third parameter of the Exception constructor to chain exceptions.  

## Global exception handler

Setting up application-wide exception handling.  

```php
<?php

class ExceptionLogger {
    private string $logFile;
    
    public function __construct(string $logFile = '/tmp/exceptions.log') {
        $this->logFile = $logFile;
    }
    
    public function handleException(Throwable $exception): void {
        $timestamp = date('Y-m-d H:i:s');
        $message = sprintf(
            "[%s] %s: %s in %s:%d\n",
            $timestamp,
            get_class($exception),
            $exception->getMessage(),
            $exception->getFile(),
            $exception->getLine()
        );
        
        // Log to file
        file_put_contents($this->logFile, $message, FILE_APPEND | LOCK_EX);
        
        // Display user-friendly message
        echo "An error occurred. Please try again later.\n";
        echo "Error ID: " . uniqid() . "\n";
    }
}

// Set global exception handler
$logger = new ExceptionLogger();
set_exception_handler([$logger, 'handleException']);

// This will be caught by the global handler
throw new Exception("Unhandled application error");

// The script continues here...
echo "This line won't be reached\n";
```

Global exception handlers catch unhandled exceptions throughout your  
application. Use them for logging, user notifications, and graceful  
error reporting. Set handlers with `set_exception_handler()`.  

## Exception handling in loops

Managing exceptions within iterative operations.  

```php
<?php

function processNumbers(array $numbers): array {
    $results = [];
    $errors = [];
    
    foreach ($numbers as $index => $number) {
        try {
            if (!is_numeric($number)) {
                throw new InvalidArgumentException("Not a number: $number");
            }
            
            if ($number == 0) {
                throw new DivisionByZeroError("Cannot divide by zero");
            }
            
            $result = 100 / $number;
            $results[$index] = round($result, 2);
            
        } catch (InvalidArgumentException $e) {
            $errors[$index] = "Invalid: " . $e->getMessage();
            continue; // Skip to next iteration
        } catch (DivisionByZeroError $e) {
            $errors[$index] = "Math error: " . $e->getMessage();
            continue;
        }
    }
    
    return ['results' => $results, 'errors' => $errors];
}

$input = [10, 'abc', 0, 5, -2, 'xyz', 20];
$output = processNumbers($input);

echo "Results:\n";
foreach ($output['results'] as $index => $result) {
    echo "[$index] $result\n";
}

echo "\nErrors:\n";
foreach ($output['errors'] as $index => $error) {
    echo "[$index] $error\n";
}
```

When processing arrays or collections, catch exceptions to prevent one  
bad item from stopping the entire operation. Collect both successful  
results and errors for comprehensive feedback.  

## Nested try-catch blocks

Handling exceptions at different levels of operation.  

```php
<?php

function readConfigFile($filename) {
    try {
        if (!file_exists($filename)) {
            throw new Exception("Config file not found: $filename");
        }
        
        $content = file_get_contents($filename);
        
        try {
            $config = json_decode($content, true, 512, JSON_THROW_ON_ERROR);
            
            try {
                if (!isset($config['database']['host'])) {
                    throw new Exception("Database host not configured");
                }
                
                return $config;
                
            } catch (Exception $e) {
                echo "Configuration validation error: " . $e->getMessage() . "\n";
                return ['database' => ['host' => 'localhost']]; // Default config
            }
            
        } catch (JsonException $e) {
            echo "JSON parsing error: " . $e->getMessage() . "\n";
            throw new Exception("Invalid configuration format", 0, $e);
        }
        
    } catch (Exception $e) {
        echo "File reading error: " . $e->getMessage() . "\n";
        throw $e; // Re-throw to caller
    }
}

// Create test config file
$testConfig = json_encode(['database' => ['host' => 'db.example.com']]);
file_put_contents('/tmp/config.json', $testConfig);

try {
    $config = readConfigFile('/tmp/config.json');
    echo "Config loaded successfully\n";
} catch (Exception $e) {
    echo "Failed to load configuration\n";
}
```

Nested try-catch blocks allow handling errors at appropriate levels.  
Inner blocks can handle specific errors while outer blocks manage  
broader failure scenarios. Use re-throwing to escalate errors.  

## Exception handling with generators

Managing exceptions in generator functions.  

```php
<?php

function numberGenerator($start, $end) {
    for ($i = $start; $i <= $end; $i++) {
        try {
            if ($i % 7 === 0) {
                throw new Exception("Lucky number 7 multiple: $i");
            }
            
            yield $i;
            
        } catch (Exception $e) {
            echo "Generator exception: " . $e->getMessage() . "\n";
            // Continue with next number
            continue;
        }
    }
}

function safeNumberProcessor($start, $end) {
    $generator = numberGenerator($start, $end);
    
    try {
        foreach ($generator as $number) {
            echo "Processing: $number\n";
            
            // Simulate processing that might fail
            if ($number > 15) {
                throw new RuntimeException("Number too large: $number");
            }
        }
    } catch (RuntimeException $e) {
        echo "Processing stopped: " . $e->getMessage() . "\n";
    }
}

echo "Generator with exception handling:\n";
safeNumberProcessor(10, 20);
```

Generators can throw and catch exceptions like regular functions.  
Exceptions thrown within generators stop iteration unless caught.  
Handle exceptions both inside generators and when consuming them.  

## Validation exception patterns

Implementing comprehensive input validation with exceptions.  

```php
<?php

class UserValidationException extends Exception {
    private array $validationErrors;
    
    public function __construct(array $errors) {
        $this->validationErrors = $errors;
        $message = 'Validation failed: ' . implode(', ', array_keys($errors));
        parent::__construct($message);
    }
    
    public function getValidationErrors(): array {
        return $this->validationErrors;
    }
}

class UserValidator {
    public function validate(array $userData): array {
        $errors = [];
        
        // Name validation
        if (empty($userData['name'])) {
            $errors['name'] = 'Name is required';
        } elseif (strlen($userData['name']) < 2) {
            $errors['name'] = 'Name must be at least 2 characters';
        }
        
        // Email validation
        if (empty($userData['email'])) {
            $errors['email'] = 'Email is required';
        } elseif (!filter_var($userData['email'], FILTER_VALIDATE_EMAIL)) {
            $errors['email'] = 'Email format is invalid';
        }
        
        // Age validation
        if (isset($userData['age'])) {
            if (!is_numeric($userData['age']) || $userData['age'] < 0 || $userData['age'] > 120) {
                $errors['age'] = 'Age must be a number between 0 and 120';
            }
        }
        
        if (!empty($errors)) {
            throw new UserValidationException($errors);
        }
        
        return $userData;
    }
}

$validator = new UserValidator();

$testUsers = [
    ['name' => 'Alice', 'email' => 'alice@example.com', 'age' => 30],
    ['name' => '', 'email' => 'invalid-email', 'age' => -5],
    ['name' => 'Bob', 'email' => 'bob@example.com']
];

foreach ($testUsers as $index => $user) {
    try {
        $validatedUser = $validator->validate($user);
        echo "User $index validated successfully: " . $validatedUser['name'] . "\n";
    } catch (UserValidationException $e) {
        echo "User $index validation failed:\n";
        foreach ($e->getValidationErrors() as $field => $error) {
            echo "  - $field: $error\n";
        }
    }
}
```

Validation exceptions consolidate multiple field errors into a single  
exception. This pattern allows collecting all validation failures  
before throwing, providing comprehensive feedback to users.  

## Exception handling in class methods

Implementing error handling in object-oriented code.  

```php
<?php

class BankAccount {
    private float $balance;
    private string $accountNumber;
    private bool $frozen = false;
    
    public function __construct(string $accountNumber, float $initialBalance = 0) {
        if ($initialBalance < 0) {
            throw new InvalidArgumentException("Initial balance cannot be negative");
        }
        $this->accountNumber = $accountNumber;
        $this->balance = $initialBalance;
    }
    
    public function deposit(float $amount): void {
        if ($this->frozen) {
            throw new RuntimeException("Account is frozen");
        }
        
        if ($amount <= 0) {
            throw new InvalidArgumentException("Deposit amount must be positive");
        }
        
        $this->balance += $amount;
        echo "Deposited $amount. New balance: {$this->balance}\n";
    }
    
    public function withdraw(float $amount): void {
        if ($this->frozen) {
            throw new RuntimeException("Account is frozen");
        }
        
        if ($amount <= 0) {
            throw new InvalidArgumentException("Withdrawal amount must be positive");
        }
        
        if ($amount > $this->balance) {
            throw new RuntimeException("Insufficient funds");
        }
        
        $this->balance -= $amount;
        echo "Withdrew $amount. New balance: {$this->balance}\n";
    }
    
    public function freeze(): void {
        $this->frozen = true;
        echo "Account {$this->accountNumber} has been frozen\n";
    }
    
    public function getBalance(): float {
        return $this->balance;
    }
}

try {
    $account = new BankAccount("12345", 1000);
    
    $account->deposit(500);      // Success
    $account->withdraw(200);     // Success
    $account->withdraw(2000);    // Insufficient funds
    
} catch (InvalidArgumentException $e) {
    echo "Invalid operation: " . $e->getMessage() . "\n";
} catch (RuntimeException $e) {
    echo "Operation failed: " . $e->getMessage() . "\n";
}

try {
    $account2 = new BankAccount("67890", 500);
    $account2->freeze();
    $account2->deposit(100);     // Account frozen
} catch (RuntimeException $e) {
    echo "Account error: " . $e->getMessage() . "\n";
}
```

Class methods should validate inputs and throw appropriate exceptions  
for invalid states or operations. Use different exception types to  
distinguish between validation errors and runtime conditions.  

## Error conversion to exceptions

Converting PHP errors into exceptions for consistent handling.  

```php
<?php

class ErrorToExceptionHandler {
    public static function handleError($severity, $message, $file, $line) {
        if (!(error_reporting() & $severity)) {
            return false; // Error reporting is turned off
        }
        
        throw new ErrorException($message, 0, $severity, $file, $line);
    }
}

function demonstrateErrorConversion() {
    // Set error handler to convert errors to exceptions
    set_error_handler([ErrorToExceptionHandler::class, 'handleError']);
    
    try {
        // These operations normally generate warnings/notices
        echo strlen(null) . "\n";           // Warning: strlen expects string
        echo $undefinedVariable;            // Notice: undefined variable
        
    } catch (ErrorException $e) {
        echo "Caught error as exception:\n";
        echo "Severity: " . $e->getSeverity() . "\n";
        echo "Message: " . $e->getMessage() . "\n";
        echo "File: " . $e->getFile() . ":" . $e->getLine() . "\n";
    } finally {
        // Restore default error handler
        restore_error_handler();
    }
}

demonstrateErrorConversion();

// Example with file operations
function safeFileOperation($filename) {
    set_error_handler(function($severity, $message, $file, $line) {
        throw new ErrorException($message, 0, $severity, $file, $line);
    });
    
    try {
        $handle = fopen($filename, 'r');
        if ($handle) {
            $content = fread($handle, 100);
            fclose($handle);
            return $content;
        }
    } catch (ErrorException $e) {
        echo "File operation failed: " . $e->getMessage() . "\n";
    } finally {
        restore_error_handler();
    }
    
    return null;
}

$result = safeFileOperation('/nonexistent/file.txt');
```

Convert PHP errors to exceptions using custom error handlers. This  
provides consistent exception-based error handling throughout your  
application instead of mixed error types.  

## Exception handling in callbacks

Managing exceptions within callback functions and closures.  

```php
<?php

function processItems(array $items, callable $processor) {
    $results = [];
    $errors = [];
    
    foreach ($items as $index => $item) {
        try {
            $result = $processor($item, $index);
            $results[] = $result;
        } catch (Exception $e) {
            $errors[] = [
                'index' => $index,
                'item' => $item,
                'error' => $e->getMessage()
            ];
        }
    }
    
    return ['results' => $results, 'errors' => $errors];
}

// Callback that might throw exceptions
$numberProcessor = function($item, $index) {
    if (!is_numeric($item)) {
        throw new InvalidArgumentException("Item at index $index is not numeric");
    }
    
    $number = (float) $item;
    if ($number === 0.0) {
        throw new DivisionByZeroError("Cannot process zero at index $index");
    }
    
    return 100 / $number;
};

$data = [10, 'abc', 5, 0, 20, 'def', 2.5];
$output = processItems($data, $numberProcessor);

echo "Successful results:\n";
foreach ($output['results'] as $result) {
    echo "- " . round($result, 2) . "\n";
}

echo "\nErrors encountered:\n";
foreach ($output['errors'] as $error) {
    echo "- Index {$error['index']}: {$error['error']}\n";
}

// Using anonymous function with exception handling
$safeProcessor = function($items, $callback) {
    return array_map(function($item) use ($callback) {
        try {
            return $callback($item);
        } catch (Exception $e) {
            return "ERROR: " . $e->getMessage();
        }
    }, $items);
};

$results = $safeProcessor([1, 2, 0, 4], function($x) {
    if ($x === 0) throw new Exception("Zero not allowed");
    return $x * 2;
});

print_r($results);
```

When using callbacks, handle exceptions at the appropriate level.  
Catch exceptions within callback execution to prevent them from  
bubbling up unexpectedly to calling code.  

## Resource management with exceptions

Ensuring proper resource cleanup when exceptions occur.  

```php
<?php

class DatabaseConnection {
    private $connection = null;
    private string $host;
    
    public function __construct(string $host) {
        $this->host = $host;
    }
    
    public function connect(): void {
        try {
            // Simulate database connection
            if ($this->host === 'invalid-host') {
                throw new Exception("Cannot connect to database");
            }
            $this->connection = "connected-to-{$this->host}";
            echo "Connected to database: {$this->host}\n";
        } catch (Exception $e) {
            $this->connection = null;
            throw new Exception("Database connection failed: " . $e->getMessage());
        }
    }
    
    public function query(string $sql): array {
        if (!$this->connection) {
            throw new Exception("Not connected to database");
        }
        
        if (strpos($sql, 'DROP') !== false) {
            throw new Exception("Dangerous query detected");
        }
        
        return ["result" => "Query executed: $sql"];
    }
    
    public function disconnect(): void {
        if ($this->connection) {
            echo "Disconnected from database: {$this->host}\n";
            $this->connection = null;
        }
    }
    
    public function __destruct() {
        $this->disconnect();
    }
}

function performDatabaseOperation(string $host, string $query) {
    $db = null;
    
    try {
        $db = new DatabaseConnection($host);
        $db->connect();
        
        $result = $db->query($query);
        echo "Query result: " . json_encode($result) . "\n";
        
    } catch (Exception $e) {
        echo "Database operation failed: " . $e->getMessage() . "\n";
    } finally {
        if ($db) {
            $db->disconnect();
        }
    }
}

// Successful operation
performDatabaseOperation('localhost', 'SELECT * FROM users');

// Failed connection
performDatabaseOperation('invalid-host', 'SELECT * FROM users');

// Dangerous query
performDatabaseOperation('localhost', 'DROP TABLE users');
```

Use `finally` blocks or destructors to ensure resources are properly  
released when exceptions occur. This prevents resource leaks and  
maintains application stability.  

## Exception handling patterns

Advanced patterns for robust exception management.  

```php
<?php

// Circuit Breaker Pattern
class CircuitBreaker {
    private int $failureCount = 0;
    private int $threshold;
    private int $timeout;
    private ?int $lastFailureTime = null;
    private string $state = 'CLOSED'; // CLOSED, OPEN, HALF_OPEN
    
    public function __construct(int $threshold = 5, int $timeout = 60) {
        $this->threshold = $threshold;
        $this->timeout = $timeout;
    }
    
    public function call(callable $callback) {
        if ($this->state === 'OPEN') {
            if (time() - $this->lastFailureTime >= $this->timeout) {
                $this->state = 'HALF_OPEN';
            } else {
                throw new Exception("Circuit breaker is OPEN");
            }
        }
        
        try {
            $result = $callback();
            $this->onSuccess();
            return $result;
        } catch (Exception $e) {
            $this->onFailure();
            throw $e;
        }
    }
    
    private function onSuccess(): void {
        $this->failureCount = 0;
        $this->state = 'CLOSED';
    }
    
    private function onFailure(): void {
        $this->failureCount++;
        $this->lastFailureTime = time();
        
        if ($this->failureCount >= $this->threshold) {
            $this->state = 'OPEN';
        }
    }
    
    public function getState(): string {
        return $this->state;
    }
}

// Retry Pattern
function withRetry(callable $callback, int $maxRetries = 3, int $delay = 1): mixed {
    $attempt = 0;
    
    while ($attempt < $maxRetries) {
        try {
            return $callback();
        } catch (Exception $e) {
            $attempt++;
            
            if ($attempt >= $maxRetries) {
                throw new Exception("Max retries ($maxRetries) exceeded: " . $e->getMessage());
            }
            
            echo "Attempt $attempt failed, retrying in {$delay}s...\n";
            sleep($delay);
        }
    }
}

// Demonstrate patterns
$circuitBreaker = new CircuitBreaker(3, 10);
$attemptCount = 0;

$unreliableService = function() use (&$attemptCount) {
    $attemptCount++;
    if ($attemptCount <= 4) {
        throw new Exception("Service unavailable");
    }
    return "Service response";
};

// Test circuit breaker
for ($i = 1; $i <= 6; $i++) {
    try {
        $result = $circuitBreaker->call($unreliableService);
        echo "Success: $result (State: {$circuitBreaker->getState()})\n";
    } catch (Exception $e) {
        echo "Failed: {$e->getMessage()} (State: {$circuitBreaker->getState()})\n";
    }
}

// Test retry pattern
try {
    $result = withRetry(function() {
        static $count = 0;
        $count++;
        if ($count < 3) {
            throw new Exception("Temporary failure");
        }
        return "Success after retries";
    }, 3, 1);
    
    echo "Retry result: $result\n";
} catch (Exception $e) {
    echo "Retry failed: " . $e->getMessage() . "\n";
}
```

Advanced exception patterns like Circuit Breaker and Retry provide  
resilience in distributed systems. They prevent cascading failures  
and implement automatic recovery mechanisms.  

## Exception debugging and logging

Comprehensive exception information capture and logging.  

```php
<?php

class ExceptionDebugger {
    private string $logFile;
    
    public function __construct(string $logFile = '/tmp/debug.log') {
        $this->logFile = $logFile;
    }
    
    public function logException(Throwable $exception, array $context = []): void {
        $debugInfo = [
            'timestamp' => date('Y-m-d H:i:s'),
            'type' => get_class($exception),
            'message' => $exception->getMessage(),
            'code' => $exception->getCode(),
            'file' => $exception->getFile(),
            'line' => $exception->getLine(),
            'trace' => $exception->getTraceAsString(),
            'context' => $context
        ];
        
        // Add previous exception if chained
        if ($exception->getPrevious()) {
            $debugInfo['previous'] = [
                'type' => get_class($exception->getPrevious()),
                'message' => $exception->getPrevious()->getMessage(),
                'file' => $exception->getPrevious()->getFile(),
                'line' => $exception->getPrevious()->getLine()
            ];
        }
        
        $logEntry = json_encode($debugInfo, JSON_PRETTY_PRINT) . "\n";
        file_put_contents($this->logFile, $logEntry, FILE_APPEND | LOCK_EX);
        
        echo "Exception logged with debug info\n";
    }
    
    public function getStackTrace(Throwable $exception): array {
        $trace = [];
        foreach ($exception->getTrace() as $frame) {
            $trace[] = sprintf(
                "%s%s%s() called at [%s:%d]",
                isset($frame['class']) ? $frame['class'] : '',
                isset($frame['type']) ? $frame['type'] : '',
                $frame['function'],
                $frame['file'] ?? 'unknown',
                $frame['line'] ?? 0
            );
        }
        return $trace;
    }
}

function buggyFunction($data) {
    if (empty($data)) {
        throw new InvalidArgumentException("Data cannot be empty");
    }
    
    // Simulate nested function calls
    return processBuggyData($data);
}

function processBuggyData($data) {
    if ($data === 'error') {
        throw new RuntimeException("Processing failed for data: $data");
    }
    
    return strtoupper($data);
}

$debugger = new ExceptionDebugger();

// Test exception logging
$testCases = ['', 'error', 'valid'];

foreach ($testCases as $testCase) {
    try {
        $result = buggyFunction($testCase);
        echo "Result: $result\n";
    } catch (Exception $e) {
        $context = [
            'input' => $testCase,
            'user_id' => 12345,
            'request_id' => uniqid()
        ];
        
        $debugger->logException($e, $context);
        
        echo "Stack trace:\n";
        foreach ($debugger->getStackTrace($e) as $frame) {
            echo "  - $frame\n";
        }
    }
}

echo "\nLog file contents:\n";
if (file_exists('/tmp/debug.log')) {
    echo file_get_contents('/tmp/debug.log');
}
```

Comprehensive exception logging captures stack traces, context data,  
and chained exceptions. This information is crucial for debugging  
production issues and understanding error patterns.  

## Performance considerations

Optimizing exception handling for better performance.  

```php
<?php

class PerformanceTestRunner {
    public function timeExecution(callable $callback, int $iterations = 1000): float {
        $startTime = microtime(true);
        
        for ($i = 0; $i < $iterations; $i++) {
            $callback();
        }
        
        return microtime(true) - $startTime;
    }
    
    public function compareApproaches(): void {
        $iterations = 10000;
        
        // Approach 1: Exception-based validation
        $exceptionTime = $this->timeExecution(function() {
            try {
                $this->validateWithException('valid@example.com');
            } catch (Exception $e) {
                // Handle error
            }
        }, $iterations);
        
        // Approach 2: Return-based validation
        $returnTime = $this->timeExecution(function() {
            $result = $this->validateWithReturn('valid@example.com');
            if ($result !== true) {
                // Handle error
            }
        }, $iterations);
        
        // Approach 3: Exception with error (expensive)
        $errorTime = $this->timeExecution(function() {
            try {
                $this->validateWithException('invalid-email');
            } catch (Exception $e) {
                // Handle error
            }
        }, $iterations);
        
        echo "Performance comparison ($iterations iterations):\n";
        echo "Exception (valid): " . round($exceptionTime * 1000, 2) . "ms\n";
        echo "Return (valid): " . round($returnTime * 1000, 2) . "ms\n";
        echo "Exception (error): " . round($errorTime * 1000, 2) . "ms\n";
        echo "Exception overhead: " . round(($exceptionTime / $returnTime) * 100, 1) . "%\n";
    }
    
    private function validateWithException(string $email): bool {
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            throw new InvalidArgumentException("Invalid email format");
        }
        return true;
    }
    
    private function validateWithReturn(string $email): bool|string {
        if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
            return "Invalid email format";
        }
        return true;
    }
}

// Exception-heavy vs Exception-light comparison
function heavyExceptionUsage() {
    $data = range(1, 1000);
    
    try {
        foreach ($data as $item) {
            if ($item % 100 === 0) {
                throw new Exception("Checkpoint: $item");
            }
            // Process item
        }
    } catch (Exception $e) {
        echo "Caught: " . $e->getMessage() . "\n";
    }
}

function lightExceptionUsage() {
    $data = range(1, 1000);
    $checkpoints = [];
    
    foreach ($data as $item) {
        if ($item % 100 === 0) {
            $checkpoints[] = "Checkpoint: $item";
        }
        // Process item
    }
    
    if (!empty($checkpoints)) {
        echo "Checkpoints: " . implode(', ', $checkpoints) . "\n";
    }
}

$tester = new PerformanceTestRunner();
$tester->compareApproaches();

echo "\nTesting exception patterns:\n";
$heavyTime = $tester->timeExecution('heavyExceptionUsage', 100);
$lightTime = $tester->timeExecution('lightExceptionUsage', 100);

echo "Heavy exception usage: " . round($heavyTime * 1000, 2) . "ms\n";
echo "Light exception usage: " . round($lightTime * 1000, 2) . "ms\n";
echo "Performance difference: " . round(($heavyTime / $lightTime) * 100, 1) . "%\n";
```

Exceptions have performance overhead, especially when thrown frequently.  
Use exceptions for exceptional conditions, not regular control flow.  
Consider return values for validation in performance-critical code.  

## Exception safety patterns

Writing exception-safe code that maintains consistency.  

```php
<?php

class SafeCounter {
    private int $count = 0;
    private array $history = [];
    
    public function increment(): void {
        $oldCount = $this->count;
        
        try {
            $this->count++;
            $this->addToHistory("increment", $this->count);
            
            // Simulate potential failure after state change
            if ($this->count > 5) {
                throw new Exception("Counter limit exceeded");
            }
            
        } catch (Exception $e) {
            // Rollback on failure
            $this->count = $oldCount;
            array_pop($this->history); // Remove last history entry
            throw $e;
        }
    }
    
    public function batchIncrement(int $amount): void {
        $originalState = [
            'count' => $this->count,
            'history' => $this->history
        ];
        
        try {
            for ($i = 0; $i < $amount; $i++) {
                $this->increment();
            }
        } catch (Exception $e) {
            // Restore original state
            $this->count = $originalState['count'];
            $this->history = $originalState['history'];
            
            throw new Exception("Batch operation failed: " . $e->getMessage());
        }
    }
    
    private function addToHistory(string $operation, int $value): void {
        $this->history[] = [
            'operation' => $operation,
            'value' => $value,
            'timestamp' => time()
        ];
    }
    
    public function getCount(): int {
        return $this->count;
    }
    
    public function getHistory(): array {
        return $this->history;
    }
}

// RAII (Resource Acquisition Is Initialization) pattern
class FileProcessor {
    private $fileHandle = null;
    
    public function __construct(string $filename) {
        $this->fileHandle = fopen($filename, 'w');
        if (!$this->fileHandle) {
            throw new Exception("Cannot open file: $filename");
        }
    }
    
    public function writeData(string $data): void {
        if (!$this->fileHandle) {
            throw new Exception("File not open");
        }
        
        if (fwrite($this->fileHandle, $data) === false) {
            throw new Exception("Failed to write data");
        }
    }
    
    public function __destruct() {
        if ($this->fileHandle) {
            fclose($this->fileHandle);
        }
    }
}

// Test exception safety
$counter = new SafeCounter();

echo "Testing safe counter:\n";
try {
    $counter->increment(); // 1
    $counter->increment(); // 2
    echo "Count after increments: " . $counter->getCount() . "\n";
    
    $counter->batchIncrement(5); // Should fail and rollback
} catch (Exception $e) {
    echo "Exception: " . $e->getMessage() . "\n";
    echo "Count after failed batch: " . $counter->getCount() . "\n";
}

// Test RAII pattern
try {
    $processor = new FileProcessor('/tmp/test_safe.txt');
    $processor->writeData("Hello there!\n");
    $processor->writeData("Exception safety test\n");
    
    echo "File processing completed\n";
    // File automatically closed when $processor goes out of scope
    
} catch (Exception $e) {
    echo "File processing failed: " . $e->getMessage() . "\n";
}

if (file_exists('/tmp/test_safe.txt')) {
    echo "File contents:\n" . file_get_contents('/tmp/test_safe.txt');
}
```

Exception safety ensures your program maintains consistent state even  
when exceptions occur. Use rollback mechanisms, RAII patterns, and  
careful state management to prevent corruption during error conditions.  

## Testing exception handling

Writing tests to verify exception behavior.  

```php
<?php

class Calculator {
    public function divide(float $dividend, float $divisor): float {
        if ($divisor === 0.0) {
            throw new InvalidArgumentException("Division by zero");
        }
        
        if (!is_finite($dividend) || !is_finite($divisor)) {
            throw new InvalidArgumentException("Arguments must be finite numbers");
        }
        
        return $dividend / $divisor;
    }
    
    public function factorial(int $n): int {
        if ($n < 0) {
            throw new InvalidArgumentException("Factorial undefined for negative numbers");
        }
        
        if ($n > 20) {
            throw new OverflowException("Factorial too large for integer");
        }
        
        return $n <= 1 ? 1 : $n * $this->factorial($n - 1);
    }
}

class SimpleTestFramework {
    private int $passed = 0;
    private int $failed = 0;
    
    public function expectException(callable $callback, string $expectedExceptionClass): void {
        try {
            $callback();
            $this->failed++;
            echo "FAIL: Expected $expectedExceptionClass but no exception was thrown\n";
        } catch (Exception $e) {
            if (get_class($e) === $expectedExceptionClass) {
                $this->passed++;
                echo "PASS: Caught expected $expectedExceptionClass\n";
            } else {
                $this->failed++;
                echo "FAIL: Expected $expectedExceptionClass but got " . get_class($e) . "\n";
            }
        }
    }
    
    public function expectNoException(callable $callback): void {
        try {
            $result = $callback();
            $this->passed++;
            echo "PASS: No exception thrown\n";
        } catch (Exception $e) {
            $this->failed++;
            echo "FAIL: Unexpected exception: " . get_class($e) . " - " . $e->getMessage() . "\n";
        }
    }
    
    public function expectExceptionMessage(callable $callback, string $expectedMessage): void {
        try {
            $callback();
            $this->failed++;
            echo "FAIL: Expected exception with message '$expectedMessage' but no exception was thrown\n";
        } catch (Exception $e) {
            if ($e->getMessage() === $expectedMessage) {
                $this->passed++;
                echo "PASS: Exception message matches expected\n";
            } else {
                $this->failed++;
                echo "FAIL: Expected message '$expectedMessage' but got '{$e->getMessage()}'\n";
            }
        }
    }
    
    public function getSummary(): array {
        return [
            'passed' => $this->passed,
            'failed' => $this->failed,
            'total' => $this->passed + $this->failed
        ];
    }
}

// Run tests
$calculator = new Calculator();
$test = new SimpleTestFramework();

echo "Testing Calculator exception handling:\n\n";

// Test division by zero
$test->expectException(
    fn() => $calculator->divide(10, 0),
    InvalidArgumentException::class
);

// Test division with infinity
$test->expectException(
    fn() => $calculator->divide(INF, 5),
    InvalidArgumentException::class
);

// Test valid division
$test->expectNoException(
    fn() => $calculator->divide(10, 2)
);

// Test negative factorial
$test->expectExceptionMessage(
    fn() => $calculator->factorial(-1),
    "Factorial undefined for negative numbers"
);

// Test large factorial
$test->expectException(
    fn() => $calculator->factorial(25),
    OverflowException::class
);

// Test valid factorial
$test->expectNoException(
    fn() => $calculator->factorial(5)
);

$summary = $test->getSummary();
echo "\nTest Summary:\n";
echo "Passed: {$summary['passed']}\n";
echo "Failed: {$summary['failed']}\n";
echo "Total: {$summary['total']}\n";
```

Testing exception handling verifies that your code throws appropriate  
exceptions for invalid inputs and error conditions. Test both positive  
cases (no exceptions) and negative cases (expected exceptions).  

## Exception handling with interfaces

Using interfaces to define exception handling contracts.  

```php
<?php

interface LoggerInterface {
    public function log(string $message): void;
}

interface ExceptionHandlerInterface {
    public function handle(Throwable $exception): void;
    public function canHandle(Throwable $exception): bool;
}

class FileLogger implements LoggerInterface {
    private string $logFile;
    
    public function __construct(string $logFile = '/tmp/app.log') {
        $this->logFile = $logFile;
    }
    
    public function log(string $message): void {
        $timestamp = date('Y-m-d H:i:s');
        file_put_contents($this->logFile, "[$timestamp] $message\n", FILE_APPEND);
    }
}

class ValidationExceptionHandler implements ExceptionHandlerInterface {
    private LoggerInterface $logger;
    
    public function __construct(LoggerInterface $logger) {
        $this->logger = $logger;
    }
    
    public function canHandle(Throwable $exception): bool {
        return $exception instanceof InvalidArgumentException;
    }
    
    public function handle(Throwable $exception): void {
        $this->logger->log("Validation error: " . $exception->getMessage());
        echo "Please check your input and try again.\n";
    }
}

class DatabaseExceptionHandler implements ExceptionHandlerInterface {
    private LoggerInterface $logger;
    
    public function __construct(LoggerInterface $logger) {
        $this->logger = $logger;
    }
    
    public function canHandle(Throwable $exception): bool {
        return $exception instanceof RuntimeException;
    }
    
    public function handle(Throwable $exception): void {
        $this->logger->log("Database error: " . $exception->getMessage());
        echo "Service temporarily unavailable. Please try again later.\n";
    }
}

class ExceptionDispatcher {
    private array $handlers = [];
    
    public function addHandler(ExceptionHandlerInterface $handler): void {
        $this->handlers[] = $handler;
    }
    
    public function dispatch(Throwable $exception): void {
        foreach ($this->handlers as $handler) {
            if ($handler->canHandle($exception)) {
                $handler->handle($exception);
                return;
            }
        }
        
        // Default handling
        echo "Unhandled exception: " . $exception->getMessage() . "\n";
    }
}

// Usage example
$logger = new FileLogger();
$dispatcher = new ExceptionDispatcher();
$dispatcher->addHandler(new ValidationExceptionHandler($logger));
$dispatcher->addHandler(new DatabaseExceptionHandler($logger));

// Test different exception types
try {
    throw new InvalidArgumentException("Invalid user input");
} catch (Throwable $e) {
    $dispatcher->dispatch($e);
}

try {
    throw new RuntimeException("Database connection failed");
} catch (Throwable $e) {
    $dispatcher->dispatch($e);
}
```

Interfaces provide contracts for exception handling components, enabling  
flexible and testable error handling systems. This pattern allows  
different handlers for different exception types while maintaining  
consistent behavior across the application.  

## Context-aware exceptions

Adding contextual information to exceptions for better debugging.  

```php
<?php

class ContextAwareException extends Exception {
    private array $context;
    private string $operation;
    private array $metadata;
    
    public function __construct(
        string $message,
        string $operation = '',
        array $context = [],
        array $metadata = [],
        int $code = 0,
        ?Throwable $previous = null
    ) {
        parent::__construct($message, $code, $previous);
        $this->operation = $operation;
        $this->context = $context;
        $this->metadata = $metadata;
    }
    
    public function getOperation(): string {
        return $this->operation;
    }
    
    public function getContext(): array {
        return $this->context;
    }
    
    public function getMetadata(): array {
        return $this->metadata;
    }
    
    public function addContext(string $key, mixed $value): self {
        $this->context[$key] = $value;
        return $this;
    }
    
    public function addMetadata(string $key, mixed $value): self {
        $this->metadata[$key] = $value;
        return $this;
    }
    
    public function getFullContext(): array {
        return [
            'message' => $this->getMessage(),
            'operation' => $this->operation,
            'context' => $this->context,
            'metadata' => $this->metadata,
            'file' => $this->getFile(),
            'line' => $this->getLine(),
            'timestamp' => date('Y-m-d H:i:s')
        ];
    }
}

class UserService {
    private array $users = [];
    
    public function createUser(array $userData): array {
        try {
            $this->validateUserData($userData);
            
            if (isset($this->users[$userData['email']])) {
                throw new ContextAwareException(
                    "User already exists",
                    "user_creation",
                    ['email' => $userData['email']],
                    ['user_count' => count($this->users)]
                );
            }
            
            $user = [
                'id' => uniqid(),
                'name' => $userData['name'],
                'email' => $userData['email'],
                'created_at' => date('Y-m-d H:i:s')
            ];
            
            $this->users[$userData['email']] = $user;
            return $user;
            
        } catch (ContextAwareException $e) {
            // Add additional context
            $e->addContext('total_users', count($this->users))
              ->addMetadata('request_ip', '192.168.1.1')
              ->addMetadata('user_agent', 'PHP-Client/1.0');
            throw $e;
        }
    }
    
    private function validateUserData(array $userData): void {
        $errors = [];
        
        if (empty($userData['name'])) {
            $errors[] = 'Name is required';
        }
        
        if (empty($userData['email']) || !filter_var($userData['email'], FILTER_VALIDATE_EMAIL)) {
            $errors[] = 'Valid email is required';
        }
        
        if (!empty($errors)) {
            throw new ContextAwareException(
                "User validation failed",
                "user_validation",
                ['validation_errors' => $errors, 'provided_data' => array_keys($userData)],
                ['validation_rules' => ['name' => 'required', 'email' => 'required|email']]
            );
        }
    }
}

// Test context-aware exceptions
$userService = new UserService();

$testUsers = [
    ['name' => 'John Doe', 'email' => 'john@example.com'],
    ['name' => '', 'email' => 'invalid-email'],
    ['name' => 'Jane Smith', 'email' => 'john@example.com'] // Duplicate
];

foreach ($testUsers as $index => $userData) {
    try {
        $user = $userService->createUser($userData);
        echo "User created: " . $user['name'] . " ({$user['id']})\n";
    } catch (ContextAwareException $e) {
        echo "Error creating user #$index:\n";
        echo "Operation: " . $e->getOperation() . "\n";
        echo "Message: " . $e->getMessage() . "\n";
        echo "Context: " . json_encode($e->getContext(), JSON_PRETTY_PRINT) . "\n";
        echo "Metadata: " . json_encode($e->getMetadata(), JSON_PRETTY_PRINT) . "\n\n";
    }
}
```

Context-aware exceptions capture environmental and operational data  
at the point of failure. This information helps developers understand  
not just what went wrong, but the circumstances that led to the error.  

## Exception handling with decorators

Using the decorator pattern for exception handling enhancement.  

```php
<?php

interface ServiceInterface {
    public function processData(array $data): array;
}

class DataProcessor implements ServiceInterface {
    public function processData(array $data): array {
        // Simulate data processing that might fail
        if (empty($data)) {
            throw new InvalidArgumentException("Data cannot be empty");
        }
        
        if (isset($data['error'])) {
            throw new RuntimeException("Processing error: " . $data['error']);
        }
        
        return array_map('strtoupper', $data);
    }
}

abstract class ServiceDecorator implements ServiceInterface {
    protected ServiceInterface $service;
    
    public function __construct(ServiceInterface $service) {
        $this->service = $service;
    }
    
    public function processData(array $data): array {
        return $this->service->processData($data);
    }
}

class ExceptionLoggingDecorator extends ServiceDecorator {
    private string $logFile;
    
    public function __construct(ServiceInterface $service, string $logFile = '/tmp/service.log') {
        parent::__construct($service);
        $this->logFile = $logFile;
    }
    
    public function processData(array $data): array {
        try {
            return $this->service->processData($data);
        } catch (Throwable $e) {
            $this->logException($e, $data);
            throw $e;
        }
    }
    
    private function logException(Throwable $e, array $data): void {
        $logEntry = [
            'timestamp' => date('Y-m-d H:i:s'),
            'exception' => get_class($e),
            'message' => $e->getMessage(),
            'input_data' => $data,
            'file' => $e->getFile(),
            'line' => $e->getLine()
        ];
        
        file_put_contents($this->logFile, json_encode($logEntry) . "\n", FILE_APPEND);
    }
}

class ExceptionRetryDecorator extends ServiceDecorator {
    private int $maxRetries;
    private int $delay;
    
    public function __construct(ServiceInterface $service, int $maxRetries = 3, int $delay = 1) {
        parent::__construct($service);
        $this->maxRetries = $maxRetries;
        $this->delay = $delay;
    }
    
    public function processData(array $data): array {
        $attempt = 0;
        
        while ($attempt < $this->maxRetries) {
            try {
                return $this->service->processData($data);
            } catch (RuntimeException $e) {
                $attempt++;
                
                if ($attempt >= $this->maxRetries) {
                    throw new Exception(
                        "Service failed after {$this->maxRetries} attempts: " . $e->getMessage(),
                        0,
                        $e
                    );
                }
                
                echo "Attempt $attempt failed, retrying in {$this->delay}s...\n";
                sleep($this->delay);
            } catch (InvalidArgumentException $e) {
                // Don't retry validation errors
                throw $e;
            }
        }
    }
}

class ExceptionMetricsDecorator extends ServiceDecorator {
    private array $metrics = [
        'total_calls' => 0,
        'successful_calls' => 0,
        'failed_calls' => 0,
        'exceptions_by_type' => []
    ];
    
    public function processData(array $data): array {
        $this->metrics['total_calls']++;
        
        try {
            $result = $this->service->processData($data);
            $this->metrics['successful_calls']++;
            return $result;
        } catch (Throwable $e) {
            $this->metrics['failed_calls']++;
            $exceptionType = get_class($e);
            $this->metrics['exceptions_by_type'][$exceptionType] = 
                ($this->metrics['exceptions_by_type'][$exceptionType] ?? 0) + 1;
            throw $e;
        }
    }
    
    public function getMetrics(): array {
        return $this->metrics;
    }
}

// Usage example with multiple decorators
$baseService = new DataProcessor();
$serviceWithLogging = new ExceptionLoggingDecorator($baseService);
$serviceWithRetry = new ExceptionRetryDecorator($serviceWithLogging);
$serviceWithMetrics = new ExceptionMetricsDecorator($serviceWithRetry);

$testData = [
    ['name' => 'test', 'value' => 'data'],
    [],
    ['error' => 'simulated failure'],
    ['success' => 'final test']
];

foreach ($testData as $index => $data) {
    try {
        $result = $serviceWithMetrics->processData($data);
        echo "Test $index succeeded: " . json_encode($result) . "\n";
    } catch (Throwable $e) {
        echo "Test $index failed: " . $e->getMessage() . "\n";
    }
}

echo "\nService Metrics:\n";
print_r($serviceWithMetrics->getMetrics());
```

Decorator pattern allows adding exception handling capabilities to  
existing services without modifying their core logic. Each decorator  
can add specific behavior like logging, retries, or metrics collection.  

## Exception handling with middleware

Implementing middleware pattern for request-response exception handling.  

```php
<?php

interface RequestInterface {
    public function getData(): array;
    public function getPath(): string;
}

interface ResponseInterface {
    public function setData(array $data): void;
    public function getData(): array;
    public function setStatus(int $status): void;
    public function getStatus(): int;
}

interface MiddlewareInterface {
    public function process(RequestInterface $request, ResponseInterface $response, callable $next): ResponseInterface;
}

class Request implements RequestInterface {
    private array $data;
    private string $path;
    
    public function __construct(string $path, array $data = []) {
        $this->path = $path;
        $this->data = $data;
    }
    
    public function getData(): array {
        return $this->data;
    }
    
    public function getPath(): string {
        return $this->path;
    }
}

class Response implements ResponseInterface {
    private array $data = [];
    private int $status = 200;
    
    public function setData(array $data): void {
        $this->data = $data;
    }
    
    public function getData(): array {
        return $this->data;
    }
    
    public function setStatus(int $status): void {
        $this->status = $status;
    }
    
    public function getStatus(): int {
        return $this->status;
    }
}

class ExceptionHandlingMiddleware implements MiddlewareInterface {
    private array $handlers = [];
    
    public function addHandler(string $exceptionClass, callable $handler): void {
        $this->handlers[$exceptionClass] = $handler;
    }
    
    public function process(RequestInterface $request, ResponseInterface $response, callable $next): ResponseInterface {
        try {
            return $next($request, $response);
        } catch (Throwable $e) {
            return $this->handleException($e, $request, $response);
        }
    }
    
    private function handleException(Throwable $e, RequestInterface $request, ResponseInterface $response): ResponseInterface {
        $exceptionClass = get_class($e);
        
        if (isset($this->handlers[$exceptionClass])) {
            return $this->handlers[$exceptionClass]($e, $request, $response);
        }
        
        // Default error handling
        $response->setStatus(500);
        $response->setData([
            'error' => 'Internal Server Error',
            'message' => 'An unexpected error occurred',
            'path' => $request->getPath()
        ]);
        
        return $response;
    }
}

class LoggingMiddleware implements MiddlewareInterface {
    private string $logFile;
    
    public function __construct(string $logFile = '/tmp/requests.log') {
        $this->logFile = $logFile;
    }
    
    public function process(RequestInterface $request, ResponseInterface $response, callable $next): ResponseInterface {
        $startTime = microtime(true);
        
        try {
            $response = $next($request, $response);
            $this->logRequest($request, $response, microtime(true) - $startTime);
            return $response;
        } catch (Throwable $e) {
            $this->logException($request, $e, microtime(true) - $startTime);
            throw $e;
        }
    }
    
    private function logRequest(RequestInterface $request, ResponseInterface $response, float $duration): void {
        $logEntry = sprintf(
            "[%s] %s - Status: %d - Duration: %.2fms\n",
            date('Y-m-d H:i:s'),
            $request->getPath(),
            $response->getStatus(),
            $duration * 1000
        );
        
        file_put_contents($this->logFile, $logEntry, FILE_APPEND);
    }
    
    private function logException(RequestInterface $request, Throwable $e, float $duration): void {
        $logEntry = sprintf(
            "[%s] %s - Exception: %s - Duration: %.2fms\n",
            date('Y-m-d H:i:s'),
            $request->getPath(),
            $e->getMessage(),
            $duration * 1000
        );
        
        file_put_contents($this->logFile, $logEntry, FILE_APPEND);
    }
}

class Application {
    private array $middleware = [];
    private array $routes = [];
    
    public function addMiddleware(MiddlewareInterface $middleware): void {
        $this->middleware[] = $middleware;
    }
    
    public function addRoute(string $path, callable $handler): void {
        $this->routes[$path] = $handler;
    }
    
    public function handle(RequestInterface $request): ResponseInterface {
        $response = new Response();
        $middlewareStack = $this->middleware;
        
        $next = function($request, $response) use (&$middlewareStack, &$next) {
            if (empty($middlewareStack)) {
                return $this->executeRoute($request, $response);
            }
            
            $middleware = array_shift($middlewareStack);
            return $middleware->process($request, $response, $next);
        };
        
        return $next($request, $response);
    }
    
    private function executeRoute(RequestInterface $request, ResponseInterface $response): ResponseInterface {
        $path = $request->getPath();
        
        if (!isset($this->routes[$path])) {
            throw new RuntimeException("Route not found: $path");
        }
        
        return $this->routes[$path]($request, $response);
    }
}

// Set up application
$app = new Application();

// Add exception handling middleware
$exceptionMiddleware = new ExceptionHandlingMiddleware();
$exceptionMiddleware->addHandler(InvalidArgumentException::class, function($e, $request, $response) {
    $response->setStatus(400);
    $response->setData([
        'error' => 'Bad Request',
        'message' => $e->getMessage(),
        'path' => $request->getPath()
    ]);
    return $response;
});

$exceptionMiddleware->addHandler(RuntimeException::class, function($e, $request, $response) {
    $response->setStatus(404);
    $response->setData([
        'error' => 'Not Found',
        'message' => $e->getMessage(),
        'path' => $request->getPath()
    ]);
    return $response;
});

$app->addMiddleware(new LoggingMiddleware());
$app->addMiddleware($exceptionMiddleware);

// Add routes
$app->addRoute('/users', function($request, $response) {
    $data = $request->getData();
    
    if (empty($data['name'])) {
        throw new InvalidArgumentException("Name is required");
    }
    
    $response->setData(['user' => ['id' => 1, 'name' => $data['name']]]);
    return $response;
});

// Test requests
$requests = [
    new Request('/users', ['name' => 'John Doe']),
    new Request('/users', []), // Missing name
    new Request('/posts', ['title' => 'Test']), // Route not found
];

foreach ($requests as $index => $request) {
    echo "Processing request $index: {$request->getPath()}\n";
    $response = $app->handle($request);
    echo "Status: {$response->getStatus()}\n";
    echo "Data: " . json_encode($response->getData()) . "\n\n";
}
```

Middleware pattern provides a clean way to handle exceptions across  
different layers of your application. Each middleware can transform  
exceptions into appropriate HTTP responses while maintaining separation  
of concerns between different aspects of request processing.  