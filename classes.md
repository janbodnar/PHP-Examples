# Classes

This chapter covers 30 comprehensive PHP class examples designed to  
demonstrate the full capabilities of object-oriented programming in modern  
PHP 8.4. These examples progress from basic class definitions to advanced  
OOP patterns and design principles. Each example includes clear explanations  
to help you master PHP class development and create well-structured,  
maintainable applications.  

## Basic class definition

Creating simple classes with properties and methods.  

```php
<?php

class Person {
    public string $name;
    public int $age;
    
    public function setDetails(string $name, int $age): void {
        $this->name = $name;
        $this->age = $age;
    }
    
    public function introduce(): string {
        return "Hello there! I'm {$this->name} and I'm {$this->age} years old";
    }
}

$person = new Person();
$person->setDetails("Alice", 30);
echo $person->introduce() . "\n";     // Hello there! I'm Alice and I'm 30 years old
echo "Name: " . $person->name . "\n"; // Name: Alice
```

Classes encapsulate data (properties) and behavior (methods) together.  
The `$this` keyword refers to the current object instance. Properties  
and methods can have visibility modifiers: public, private, or protected.  

## Constructor and property promotion

Modern PHP constructor features for cleaner code.  

```php
<?php

class Product {
    public function __construct(
        public readonly string $name,
        public readonly float $price,
        private array $tags = []
    ) {}
    
    public function addTag(string $tag): self {
        $this->tags[] = $tag;
        return $this;
    }
    
    public function getTags(): array {
        return $this->tags;
    }
    
    public function getFormattedPrice(): string {
        return "$" . number_format($this->price, 2);
    }
}

$laptop = new Product("Gaming Laptop", 1299.99);
$laptop->addTag("electronics")->addTag("gaming");

echo "Product: " . $laptop->name . "\n";           // Product: Gaming Laptop
echo "Price: " . $laptop->getFormattedPrice() . "\n"; // Price: $1,299.99
print_r($laptop->getTags());                      // Array with tags
```

PHP 8.0+ constructor property promotion allows declaring and initializing  
properties directly in the constructor parameters. The `readonly` keyword  
makes properties immutable after initialization. Method chaining returns  
`self` to enable fluent interfaces.  

## Public, private, protected visibility

Understanding different levels of property and method access.  

```php
<?php

class BankAccount {
    private float $balance = 0.0;
    protected string $accountType;
    public string $accountHolder;
    
    public function __construct(string $holder, string $type = "checking") {
        $this->accountHolder = $holder;
        $this->accountType = $type;
    }
    
    public function deposit(float $amount): void {
        if ($amount > 0) {
            $this->balance += $amount;
            $this->logTransaction("deposit", $amount);
        }
    }
    
    public function getBalance(): float {
        return $this->balance;
    }
    
    protected function logTransaction(string $type, float $amount): void {
        echo "Transaction: {$type} of $" . number_format($amount, 2) . "\n";
    }
    
    private function validateAmount(float $amount): bool {
        return $amount > 0 && $amount <= 10000;
    }
}

class SavingsAccount extends BankAccount {
    public function addInterest(float $rate): void {
        $interest = $this->getBalance() * $rate;
        $this->deposit($interest);
        echo "Interest added: $" . number_format($interest, 2) . "\n";
    }
    
    public function getAccountInfo(): string {
        return "{$this->accountHolder} - {$this->accountType} account";
    }
}

$account = new SavingsAccount("Bob Smith", "savings");
$account->deposit(1000);
$account->addInterest(0.02); // 2% interest

echo $account->getAccountInfo() . "\n";
echo "Balance: $" . number_format($account->getBalance(), 2) . "\n";
```

Visibility controls access to class members. `private` members are only  
accessible within the same class, `protected` members can be accessed by  
child classes, and `public` members are accessible everywhere. This  
encapsulation is fundamental to object-oriented design.  

## Static properties and methods

Using class-level data and methods without object instances.  

```php
<?php

class Counter {
    private static int $count = 0;
    private static array $instances = [];
    public string $name;
    
    public function __construct(string $name) {
        $this->name = $name;
        self::$count++;
        self::$instances[] = $this;
    }
    
    public static function getCount(): int {
        return self::$count;
    }
    
    public static function getAllInstances(): array {
        return self::$instances;
    }
    
    public static function reset(): void {
        self::$count = 0;
        self::$instances = [];
    }
    
    public function getInstanceInfo(): string {
        return "Instance '{$this->name}' (#" . self::$count . ")";
    }
}

class DatabaseConfig {
    private static ?self $instance = null;
    private array $config;
    
    private function __construct(array $config) {
        $this->config = $config;
    }
    
    public static function getInstance(array $config = []): self {
        if (self::$instance === null) {
            self::$instance = new self($config);
        }
        return self::$instance;
    }
    
    public function get(string $key): mixed {
        return $this->config[$key] ?? null;
    }
}

$counter1 = new Counter("First");
$counter2 = new Counter("Second");
$counter3 = new Counter("Third");

echo "Total counters: " . Counter::getCount() . "\n";        // Total counters: 3
echo $counter2->getInstanceInfo() . "\n";                    // Instance 'Second' (#3)

$db = DatabaseConfig::getInstance(["host" => "localhost", "port" => 3306]);
echo "Database host: " . $db->get("host") . "\n";           // Database host: localhost
```

Static members belong to the class itself, not to individual instances.  
Use `self::` to reference static members within the class, or `ClassName::`  
from outside. Static methods can't access instance properties or methods  
directly. The singleton pattern uses static properties to ensure single  
instance creation.  

## Class constants

Defining immutable values associated with classes.  

```php
<?php

class HttpStatus {
    public const OK = 200;
    public const NOT_FOUND = 404;
    public const SERVER_ERROR = 500;
    
    private const MESSAGES = [
        self::OK => "OK",
        self::NOT_FOUND => "Not Found", 
        self::SERVER_ERROR => "Internal Server Error"
    ];
    
    public static function getMessage(int $code): string {
        return self::MESSAGES[$code] ?? "Unknown Status";
    }
    
    public static function getAllStatuses(): array {
        return self::MESSAGES;
    }
}

class MathConstants {
    public const PI = 3.14159265359;
    public const E = 2.71828182846;
    protected const GOLDEN_RATIO = 1.61803398875;
    
    public static function circleArea(float $radius): float {
        return self::PI * $radius * $radius;
    }
    
    public static function getGoldenRatio(): float {
        return self::GOLDEN_RATIO;
    }
}

class ExtendedMath extends MathConstants {
    public const TAU = 2 * parent::PI; // Using parent:: to access parent constants
    
    public static function getParentPi(): float {
        return parent::PI;
    }
}

echo "HTTP OK: " . HttpStatus::OK . " - " . HttpStatus::getMessage(HttpStatus::OK) . "\n";
echo "Circle area (r=5): " . MathConstants::circleArea(5) . "\n";
echo "TAU: " . ExtendedMath::TAU . "\n";
echo "Golden Ratio: " . MathConstants::getGoldenRatio() . "\n";

foreach (HttpStatus::getAllStatuses() as $code => $message) {
    echo "Status $code: $message\n";
}
```

Class constants are immutable values that belong to the class. They're  
accessible using the scope resolution operator (::). Constants can have  
visibility modifiers and can be referenced in child classes using  
`parent::CONSTANT_NAME`. They're useful for configuration values and  
enumerations.  

## Inheritance

Extending classes to create specialized versions with additional features.  

```php
<?php

class Vehicle {
    protected string $make;
    protected string $model;
    protected int $year;
    
    public function __construct(string $make, string $model, int $year) {
        $this->make = $make;
        $this->model = $model;
        $this->year = $year;
    }
    
    public function getInfo(): string {
        return "{$this->year} {$this->make} {$this->model}";
    }
    
    public function start(): string {
        return "Starting the vehicle...";
    }
    
    protected function getAge(): int {
        return date('Y') - $this->year;
    }
}

class Car extends Vehicle {
    private int $doors;
    private string $fuelType;
    
    public function __construct(string $make, string $model, int $year, int $doors, string $fuelType = "gasoline") {
        parent::__construct($make, $model, $year);
        $this->doors = $doors;
        $this->fuelType = $fuelType;
    }
    
    public function start(): string {
        return parent::start() . " Car engine running.";
    }
    
    public function getInfo(): string {
        return parent::getInfo() . " ({$this->doors} doors, {$this->fuelType})";
    }
    
    public function honk(): string {
        return "Beep beep!";
    }
}

class ElectricCar extends Car {
    private int $batteryLevel = 100;
    
    public function __construct(string $make, string $model, int $year, int $doors) {
        parent::__construct($make, $model, $year, $doors, "electric");
    }
    
    public function start(): string {
        if ($this->batteryLevel > 10) {
            return "Electric motor activated silently.";
        }
        return "Battery too low to start.";
    }
    
    public function charge(): string {
        $this->batteryLevel = 100;
        return "Battery fully charged.";
    }
    
    public function getBatteryLevel(): int {
        return $this->batteryLevel;
    }
}

$car = new Car("Toyota", "Camry", 2020, 4);
$tesla = new ElectricCar("Tesla", "Model 3", 2023, 4);

echo $car->getInfo() . "\n";      // 2020 Toyota Camry (4 doors, gasoline)
echo $car->start() . "\n";        // Starting the vehicle... Car engine running.
echo $car->honk() . "\n";         // Beep beep!

echo "\n" . $tesla->getInfo() . "\n";    // 2023 Tesla Model 3 (4 doors, electric)
echo $tesla->start() . "\n";             // Electric motor activated silently.
echo $tesla->charge() . "\n";            // Battery fully charged.
```

Inheritance allows creating specialized classes that extend base class  
functionality. Child classes inherit all public and protected members  
from parents. Use `parent::` to call parent methods. Method overriding  
lets child classes provide specific implementations while maintaining  
the same interface.  

## Abstract classes

Defining blueprint classes that cannot be instantiated directly.  

```php
<?php

abstract class Shape {
    protected float $x;
    protected float $y;
    
    public function __construct(float $x, float $y) {
        $this->x = $x;
        $this->y = $y;
    }
    
    abstract public function area(): float;
    abstract public function perimeter(): float;
    
    public function getPosition(): array {
        return ['x' => $this->x, 'y' => $this->y];
    }
    
    public function move(float $deltaX, float $deltaY): void {
        $this->x += $deltaX;
        $this->y += $deltaY;
    }
    
    public function describe(): string {
        return static::class . " at ({$this->x}, {$this->y})";
    }
}

class Circle extends Shape {
    private float $radius;
    
    public function __construct(float $x, float $y, float $radius) {
        parent::__construct($x, $y);
        $this->radius = $radius;
    }
    
    public function area(): float {
        return M_PI * $this->radius * $this->radius;
    }
    
    public function perimeter(): float {
        return 2 * M_PI * $this->radius;
    }
    
    public function getRadius(): float {
        return $this->radius;
    }
}

class Rectangle extends Shape {
    private float $width;
    private float $height;
    
    public function __construct(float $x, float $y, float $width, float $height) {
        parent::__construct($x, $y);
        $this->width = $width;
        $this->height = $height;
    }
    
    public function area(): float {
        return $this->width * $this->height;
    }
    
    public function perimeter(): float {
        return 2 * ($this->width + $this->height);
    }
    
    public function getDimensions(): array {
        return ['width' => $this->width, 'height' => $this->height];
    }
}

$circle = new Circle(10, 20, 5);
$rectangle = new Rectangle(0, 0, 10, 8);

echo $circle->describe() . "\n";                    // Circle at (10, 20)
echo "Area: " . round($circle->area(), 2) . "\n";   // Area: 78.54
echo "Perimeter: " . round($circle->perimeter(), 2) . "\n"; // Perimeter: 31.42

$rectangle->move(5, 5);
echo "\n" . $rectangle->describe() . "\n";          // Rectangle at (5, 5)
echo "Area: " . $rectangle->area() . "\n";          // Area: 80
echo "Perimeter: " . $rectangle->perimeter() . "\n"; // Perimeter: 36
```

Abstract classes define common structure and behavior for related classes  
but cannot be instantiated directly. They can contain both concrete and  
abstract methods. Child classes must implement all abstract methods.  
Abstract classes enforce consistent interfaces while allowing shared  
implementation.  

## Interfaces

Defining contracts that classes must implement.  

```php
<?php

interface Drawable {
    public function draw(): string;
    public function getArea(): float;
}

interface Colorable {
    public function setColor(string $color): void;
    public function getColor(): string;
}

interface Resizable {
    public function resize(float $factor): void;
    public function getSize(): array;
}

class Square implements Drawable, Colorable, Resizable {
    private float $side;
    private string $color = 'black';
    
    public function __construct(float $side) {
        $this->side = $side;
    }
    
    public function draw(): string {
        return "Drawing a {$this->color} square with side {$this->side}";
    }
    
    public function getArea(): float {
        return $this->side * $this->side;
    }
    
    public function setColor(string $color): void {
        $this->color = $color;
    }
    
    public function getColor(): string {
        return $this->color;
    }
    
    public function resize(float $factor): void {
        $this->side *= $factor;
    }
    
    public function getSize(): array {
        return ['side' => $this->side];
    }
}

class Canvas {
    private array $shapes = [];
    
    public function addShape(Drawable $shape): void {
        $this->shapes[] = $shape;
    }
    
    public function drawAll(): array {
        return array_map(fn(Drawable $shape) => $shape->draw(), $this->shapes);
    }
    
    public function getTotalArea(): float {
        return array_sum(array_map(fn(Drawable $shape) => $shape->getArea(), $this->shapes));
    }
    
    public function colorizeAll(string $color): void {
        foreach ($this->shapes as $shape) {
            if ($shape instanceof Colorable) {
                $shape->setColor($color);
            }
        }
    }
}

$square1 = new Square(5);
$square2 = new Square(3);

$canvas = new Canvas();
$canvas->addShape($square1);
$canvas->addShape($square2);

$square1->setColor('red');
$square2->setColor('blue');

echo "Before resizing:\n";
foreach ($canvas->drawAll() as $drawing) {
    echo $drawing . "\n";
}

$square1->resize(2);
echo "\nAfter resizing square1:\n";
echo $square1->draw() . "\n";
echo "New area: " . $square1->getArea() . "\n";

echo "\nTotal canvas area: " . $canvas->getTotalArea() . "\n";
```

Interfaces define contracts that implementing classes must follow. They  
specify method signatures without implementation. Classes can implement  
multiple interfaces, enabling flexible design. Use interface type hints  
to accept any object implementing specific behavior, promoting loose  
coupling and testability.  

## Traits

Reusing code across multiple classes through horizontal composition.  

```php
<?php

trait Timestampable {
    private ?\DateTime $createdAt = null;
    private ?\DateTime $updatedAt = null;
    
    public function initTimestamps(): void {
        $this->createdAt = new \DateTime();
        $this->updatedAt = new \DateTime();
    }
    
    public function updateTimestamp(): void {
        $this->updatedAt = new \DateTime();
    }
    
    public function getCreatedAt(): ?\DateTime {
        return $this->createdAt;
    }
    
    public function getUpdatedAt(): ?\DateTime {
        return $this->updatedAt;
    }
    
    public function getAge(): string {
        if (!$this->createdAt) return "Unknown";
        
        $diff = (new \DateTime())->diff($this->createdAt);
        return $diff->format('%a days, %h hours, %i minutes');
    }
}

trait Cacheable {
    private array $cache = [];
    
    protected function getCacheKey(string $method, array $params = []): string {
        return $method . '_' . md5(serialize($params));
    }
    
    protected function getFromCache(string $key): mixed {
        return $this->cache[$key] ?? null;
    }
    
    protected function setCache(string $key, mixed $value): void {
        $this->cache[$key] = $value;
    }
    
    protected function clearCache(): void {
        $this->cache = [];
    }
    
    public function getCacheStats(): array {
        return [
            'entries' => count($this->cache),
            'keys' => array_keys($this->cache),
            'memory' => strlen(serialize($this->cache))
        ];
    }
}

trait Loggable {
    private array $logs = [];
    
    protected function log(string $level, string $message): void {
        $this->logs[] = [
            'timestamp' => new \DateTime(),
            'level' => $level,
            'message' => $message
        ];
    }
    
    public function getLogs(string $level = null): array {
        if ($level === null) {
            return $this->logs;
        }
        
        return array_filter($this->logs, fn($log) => $log['level'] === $level);
    }
    
    public function clearLogs(): void {
        $this->logs = [];
    }
}

class BlogPost {
    use Timestampable, Cacheable, Loggable;
    
    private string $title;
    private string $content;
    private bool $published = false;
    
    public function __construct(string $title, string $content) {
        $this->title = $title;
        $this->content = $content;
        $this->initTimestamps();
        $this->log('info', 'Blog post created');
    }
    
    public function publish(): void {
        $this->published = true;
        $this->updateTimestamp();
        $this->clearCache(); // Clear cache when publishing
        $this->log('info', 'Blog post published');
    }
    
    public function getWordCount(): int {
        $cacheKey = $this->getCacheKey('wordCount');
        $cached = $this->getFromCache($cacheKey);
        
        if ($cached !== null) {
            $this->log('debug', 'Word count retrieved from cache');
            return $cached;
        }
        
        $count = str_word_count($this->content);
        $this->setCache($cacheKey, $count);
        $this->log('debug', 'Word count calculated and cached');
        
        return $count;
    }
    
    public function getInfo(): array {
        return [
            'title' => $this->title,
            'published' => $this->published,
            'word_count' => $this->getWordCount(),
            'age' => $this->getAge(),
            'cache_stats' => $this->getCacheStats()
        ];
    }
}

$post = new BlogPost("Learning PHP Traits", "Traits are a great way to share code between classes without using inheritance. They provide horizontal code reuse...");

echo "Initial info:\n";
print_r($post->getInfo());

echo "\nWord count (first call): " . $post->getWordCount() . "\n";
echo "Word count (cached call): " . $post->getWordCount() . "\n";

$post->publish();

echo "\nAfter publishing:\n";
print_r($post->getLogs());
```

Traits provide a mechanism for code reuse in single inheritance languages.  
They allow mixing behavior into classes without inheritance relationships.  
Traits can include properties, methods, and even other traits. They solve  
the diamond problem and enable clean separation of concerns through  
horizontal composition.  

## Method chaining

Creating fluent interfaces for improved code readability.  

```php
<?php

class QueryBuilder {
    private string $table = '';
    private array $select = [];
    private array $where = [];
    private array $orderBy = [];
    private ?int $limit = null;
    
    public function from(string $table): self {
        $this->table = $table;
        return $this;
    }
    
    public function select(string ...$columns): self {
        $this->select = array_merge($this->select, $columns);
        return $this;
    }
    
    public function where(string $column, string $operator, mixed $value): self {
        $this->where[] = "{$column} {$operator} " . (is_string($value) ? "'{$value}'" : $value);
        return $this;
    }
    
    public function orderBy(string $column, string $direction = 'ASC'): self {
        $this->orderBy[] = "{$column} {$direction}";
        return $this;
    }
    
    public function limit(int $count): self {
        $this->limit = $count;
        return $this;
    }
    
    public function build(): string {
        $sql = "SELECT " . (empty($this->select) ? '*' : implode(', ', $this->select));
        $sql .= " FROM {$this->table}";
        
        if (!empty($this->where)) {
            $sql .= " WHERE " . implode(' AND ', $this->where);
        }
        
        if (!empty($this->orderBy)) {
            $sql .= " ORDER BY " . implode(', ', $this->orderBy);
        }
        
        if ($this->limit !== null) {
            $sql .= " LIMIT {$this->limit}";
        }
        
        return $sql;
    }
    
    public function reset(): self {
        $this->table = '';
        $this->select = [];
        $this->where = [];
        $this->orderBy = [];
        $this->limit = null;
        return $this;
    }
}

class StringBuilder {
    private string $content = '';
    
    public function append(string $text): self {
        $this->content .= $text;
        return $this;
    }
    
    public function prepend(string $text): self {
        $this->content = $text . $this->content;
        return $this;
    }
    
    public function upper(): self {
        $this->content = strtoupper($this->content);
        return $this;
    }
    
    public function lower(): self {
        $this->content = strtolower($this->content);
        return $this;
    }
    
    public function replace(string $search, string $replace): self {
        $this->content = str_replace($search, $replace, $this->content);
        return $this;
    }
    
    public function trim(): self {
        $this->content = trim($this->content);
        return $this;
    }
    
    public function toString(): string {
        return $this->content;
    }
    
    public function length(): int {
        return strlen($this->content);
    }
}

// Query builder example
$query = (new QueryBuilder())
    ->select('name', 'email', 'age')
    ->from('users')
    ->where('age', '>', 18)
    ->where('active', '=', 1)
    ->orderBy('name', 'ASC')
    ->limit(10)
    ->build();

echo "Generated SQL:\n" . $query . "\n\n";

// String builder example
$message = (new StringBuilder())
    ->append('Hello')
    ->append(' ')
    ->append('World')
    ->append('!')
    ->upper()
    ->replace('WORLD', 'PHP')
    ->toString();

echo "Built message: " . $message . "\n";  // Built message: HELLO PHP!
```

Method chaining creates fluent interfaces by returning `$this` from methods.  
This enables multiple method calls in a single expression, improving  
readability for configuration and building operations. The pattern is  
common in query builders, validators, and object configuration scenarios.  

## Magic methods

Implementing special behavior through PHP's magic method hooks.  

```php
<?php

class SmartArray {
    private array $data = [];
    private array $accessLog = [];
    
    public function __construct(array $data = []) {
        $this->data = $data;
    }
    
    public function __get(string $key): mixed {
        $this->accessLog[] = "get: {$key}";
        return $this->data[$key] ?? null;
    }
    
    public function __set(string $key, mixed $value): void {
        $this->accessLog[] = "set: {$key}";
        $this->data[$key] = $value;
    }
    
    public function __isset(string $key): bool {
        return isset($this->data[$key]);
    }
    
    public function __unset(string $key): void {
        $this->accessLog[] = "unset: {$key}";
        unset($this->data[$key]);
    }
    
    public function __toString(): string {
        return json_encode($this->data, JSON_PRETTY_PRINT);
    }
    
    public function __invoke(string $operation, ...$args): mixed {
        return match($operation) {
            'count' => count($this->data),
            'keys' => array_keys($this->data),
            'values' => array_values($this->data),
            'clear' => $this->data = [],
            'log' => $this->accessLog,
            default => "Unknown operation: {$operation}"
        };
    }
    
    public function __call(string $name, array $arguments): mixed {
        if (str_starts_with($name, 'get') && strlen($name) > 3) {
            $property = strtolower(substr($name, 3));
            return $this->data[$property] ?? null;
        }
        
        if (str_starts_with($name, 'set') && strlen($name) > 3) {
            $property = strtolower(substr($name, 3));
            $this->data[$property] = $arguments[0] ?? null;
            return $this;
        }
        
        throw new BadMethodCallException("Method {$name} not found");
    }
    
    public function __clone(): void {
        // Deep clone the data array
        $this->data = unserialize(serialize($this->data));
        $this->accessLog = []; // Reset access log for clone
    }
    
    public function __debugInfo(): array {
        return [
            'data' => $this->data,
            'size' => count($this->data),
            'access_log_count' => count($this->accessLog)
        ];
    }
}

class Person {
    public function __construct(
        private string $name,
        private int $age,
        private array $skills = []
    ) {}
    
    public function __toString(): string {
        return "{$this->name} ({$this->age} years old)";
    }
    
    public function __sleep(): array {
        // Only serialize specific properties
        echo "Preparing {$this->name} for serialization\n";
        return ['name', 'age']; // Don't serialize skills
    }
    
    public function __wakeup(): void {
        echo "Waking up {$this->name} from serialization\n";
        $this->skills = []; // Initialize skills after unserialization
    }
}

// SmartArray usage
$arr = new SmartArray(['name' => 'Alice', 'age' => 30]);

$arr->city = 'New York';  // Uses __set
echo $arr->name . "\n";   // Uses __get: Alice
echo isset($arr->age) ? "Age exists\n" : "Age missing\n"; // Uses __isset

echo "Array contents:\n" . $arr . "\n"; // Uses __toString

echo "Count: " . $arr('count') . "\n";   // Uses __invoke
print_r($arr('keys'));                   // Uses __invoke

// Magic method calls
$arr->setCountry('USA');  // Uses __call (setCountry -> country)
echo "Country: " . $arr->getCountry() . "\n"; // Uses __call

// Person serialization
$person = new Person('Bob', 25, ['PHP', 'JavaScript']);
echo $person . "\n";      // Uses __toString

$serialized = serialize($person);
$unserialized = unserialize($serialized);
echo $unserialized . "\n";

var_dump($arr); // Uses __debugInfo
```

Magic methods provide hooks for common object operations. `__get` and  
`__set` handle property access, `__toString` defines string conversion,  
`__invoke` makes objects callable, and `__call` handles undefined method  
calls. These methods enable dynamic behavior and can create more intuitive  
APIs, but should be used judiciously to maintain code clarity.  

## Type declarations and return types

Using PHP's type system for better code reliability and documentation.  

```php
<?php

class TypedCalculator {
    private float $precision = 2;
    
    public function __construct(int|float $precision = 2) {
        $this->precision = (float) $precision;
    }
    
    public function add(int|float $a, int|float $b): int|float {
        return $a + $b;
    }
    
    public function divide(int|float $dividend, int|float $divisor): float {
        if ($divisor == 0) {
            throw new InvalidArgumentException("Division by zero");
        }
        return round($dividend / $divisor, (int) $this->precision);
    }
    
    public function factorial(int $n): int {
        if ($n < 0) {
            throw new InvalidArgumentException("Factorial undefined for negative numbers");
        }
        return $n <= 1 ? 1 : $n * $this->factorial($n - 1);
    }
    
    public function processNumbers(array $numbers): array {
        return array_map(fn(int|float $num): float => round($num * 1.1, 2), $numbers);
    }
    
    public function findInRange(array $numbers, int|float $min, int|float $max): ?array {
        $result = array_filter($numbers, fn($num) => $num >= $min && $num <= $max);
        return empty($result) ? null : array_values($result);
    }
}

class DataProcessor {
    public function processUser(object $user): stdClass {
        if (!isset($user->name, $user->email)) {
            throw new InvalidArgumentException("User must have name and email");
        }
        
        $processed = new stdClass();
        $processed->name = ucwords(strtolower($user->name));
        $processed->email = strtolower($user->email);
        $processed->slug = $this->createSlug($user->name);
        $processed->processed_at = new DateTime();
        
        return $processed;
    }
    
    private function createSlug(string $text): string {
        return strtolower(preg_replace('/[^A-Za-z0-9-]+/', '-', $text));
    }
    
    public function aggregateData(array $items): array {
        $counts = [];
        $sums = [];
        
        foreach ($items as $item) {
            if (!is_array($item) && !is_object($item)) continue;
            
            $data = is_array($item) ? $item : get_object_vars($item);
            
            foreach ($data as $key => $value) {
                if (is_numeric($value)) {
                    $counts[$key] = ($counts[$key] ?? 0) + 1;
                    $sums[$key] = ($sums[$key] ?? 0) + $value;
                }
            }
        }
        
        $averages = [];
        foreach ($sums as $key => $sum) {
            $averages[$key] = $sum / $counts[$key];
        }
        
        return ['counts' => $counts, 'sums' => $sums, 'averages' => $averages];
    }
    
    public function validateAndTransform(mixed $data, string $type): mixed {
        return match($type) {
            'int' => filter_var($data, FILTER_VALIDATE_INT) !== false ? (int) $data : null,
            'float' => filter_var($data, FILTER_VALIDATE_FLOAT) !== false ? (float) $data : null,
            'email' => filter_var($data, FILTER_VALIDATE_EMAIL) ?: null,
            'url' => filter_var($data, FILTER_VALIDATE_URL) ?: null,
            'bool' => filter_var($data, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE),
            'string' => is_scalar($data) ? (string) $data : null,
            default => throw new InvalidArgumentException("Unsupported type: {$type}")
        };
    }
}

// Usage examples
$calc = new TypedCalculator(3);

echo "Addition: " . $calc->add(10, 5.5) . "\n";      // Addition: 15.5
echo "Division: " . $calc->divide(10, 3) . "\n";     // Division: 3.333
echo "Factorial: " . $calc->factorial(5) . "\n";     // Factorial: 120

$numbers = [1.234, 2.567, 3.891, 4.123];
$processed = $calc->processNumbers($numbers);
echo "Processed: " . json_encode($processed) . "\n";

$inRange = $calc->findInRange([1, 5, 10, 15, 20], 5, 15);
echo "In range: " . json_encode($inRange) . "\n";    // [5, 10, 15]

// Data processor example
$processor = new DataProcessor();

$user = new stdClass();
$user->name = 'john doe';
$user->email = 'JOHN@EXAMPLE.COM';

$processed = $processor->processUser($user);
echo "Processed user: " . json_encode($processed, JSON_PRETTY_PRINT) . "\n";

// Validation examples
echo "Validate int: " . var_export($processor->validateAndTransform('123', 'int'), true) . "\n";
echo "Validate email: " . var_export($processor->validateAndTransform('test@example.com', 'email'), true) . "\n";
echo "Validate bool: " . var_export($processor->validateAndTransform('true', 'bool'), true) . "\n";
```

Type declarations improve code reliability by enforcing parameter and return  
value types. PHP 8 introduced union types (int|float), nullable types (?string),  
and mixed type. Type hints catch errors early, improve IDE support, and serve  
as documentation. Use them consistently to build more maintainable applications.  

## Readonly properties

Creating immutable object properties for enhanced data integrity.  

```php
<?php

class Point {
    public function __construct(
        public readonly float $x,
        public readonly float $y
    ) {}
    
    public function distanceTo(Point $other): float {
        $dx = $this->x - $other->x;
        $dy = $this->y - $other->y;
        return sqrt($dx * $dx + $dy * $dy);
    }
    
    public function translate(float $dx, float $dy): self {
        // Since properties are readonly, we return a new instance
        return new self($this->x + $dx, $this->y + $dy);
    }
    
    public function __toString(): string {
        return "({$this->x}, {$this->y})";
    }
}

class Money {
    public function __construct(
        public readonly int $amount,     // Amount in cents
        public readonly string $currency
    ) {
        if ($amount < 0) {
            throw new InvalidArgumentException("Amount cannot be negative");
        }
        
        if (strlen($currency) !== 3) {
            throw new InvalidArgumentException("Currency must be 3 characters");
        }
    }
    
    public function add(Money $other): self {
        if ($this->currency !== $other->currency) {
            throw new InvalidArgumentException("Cannot add different currencies");
        }
        
        return new self($this->amount + $other->amount, $this->currency);
    }
    
    public function multiply(float $factor): self {
        return new self((int) round($this->amount * $factor), $this->currency);
    }
    
    public function getFormattedAmount(): string {
        $dollars = $this->amount / 100;
        return number_format($dollars, 2) . ' ' . $this->currency;
    }
    
    public function equals(Money $other): bool {
        return $this->amount === $other->amount && $this->currency === $other->currency;
    }
}

readonly class Configuration {
    public function __construct(
        public string $appName,
        public string $version,
        public array $features,
        public bool $debugMode = false
    ) {}
    
    public function isFeatureEnabled(string $feature): bool {
        return in_array($feature, $this->features);
    }
    
    public function withFeature(string $feature): self {
        if ($this->isFeatureEnabled($feature)) {
            return $this;
        }
        
        return new self(
            $this->appName,
            $this->version,
            [...$this->features, $feature],
            $this->debugMode
        );
    }
    
    public function withoutFeature(string $feature): self {
        return new self(
            $this->appName,
            $this->version,
            array_values(array_filter($this->features, fn($f) => $f !== $feature)),
            $this->debugMode
        );
    }
    
    public function enableDebug(): self {
        return new self($this->appName, $this->version, $this->features, true);
    }
}

// Point usage
$p1 = new Point(0, 0);
$p2 = new Point(3, 4);

echo "Point 1: " . $p1 . "\n";                        // Point 1: (0, 0)
echo "Point 2: " . $p2 . "\n";                        // Point 2: (3, 4)
echo "Distance: " . $p1->distanceTo($p2) . "\n";      // Distance: 5

$p3 = $p1->translate(10, 10);
echo "Translated point: " . $p3 . "\n";               // Translated point: (10, 10)

// Money usage
$price1 = new Money(2500, 'USD');  // $25.00
$price2 = new Money(1500, 'USD');  // $15.00

echo "Price 1: " . $price1->getFormattedAmount() . "\n";
echo "Price 2: " . $price2->getFormattedAmount() . "\n";

$total = $price1->add($price2);
echo "Total: " . $total->getFormattedAmount() . "\n"; // Total: 40.00 USD

$discounted = $total->multiply(0.9);
echo "10% discount: " . $discounted->getFormattedAmount() . "\n";

// Configuration usage
$config = new Configuration(
    'My App',
    '1.0.0',
    ['user_auth', 'caching']
);

echo "Features: " . json_encode($config->features) . "\n";
echo "Has caching: " . ($config->isFeatureEnabled('caching') ? 'Yes' : 'No') . "\n";

$newConfig = $config->withFeature('logging')->enableDebug();
echo "New config features: " . json_encode($newConfig->features) . "\n";
echo "Debug mode: " . ($newConfig->debugMode ? 'Enabled' : 'Disabled') . "\n";
```

Readonly properties can only be written during object construction,  
making objects immutable after creation. This prevents accidental  
modification and enables safe sharing across threads. When modifications  
are needed, create new instances rather than mutating existing ones,  
following functional programming principles for better reliability.  

## Enumerations

Using PHP 8.1 enums for type-safe value sets and behavior.  

```php
<?php

enum Status {
    case Pending;
    case Processing;
    case Completed;
    case Failed;
    
    public function getLabel(): string {
        return match($this) {
            Status::Pending => 'Pending',
            Status::Processing => 'Processing',
            Status::Completed => 'Completed',
            Status::Failed => 'Failed',
        };
    }
    
    public function canTransitionTo(Status $newStatus): bool {
        return match($this) {
            Status::Pending => in_array($newStatus, [Status::Processing, Status::Failed]),
            Status::Processing => in_array($newStatus, [Status::Completed, Status::Failed]),
            Status::Completed => false,
            Status::Failed => $newStatus === Status::Pending,
        };
    }
    
    public function getColor(): string {
        return match($this) {
            Status::Pending => 'yellow',
            Status::Processing => 'blue',
            Status::Completed => 'green',
            Status::Failed => 'red',
        };
    }
}

enum Priority: int {
    case Low = 1;
    case Medium = 2;
    case High = 3;
    case Critical = 4;
    
    public function getWeight(): int {
        return $this->value;
    }
    
    public function getDescription(): string {
        return match($this) {
            Priority::Low => 'Low priority - can wait',
            Priority::Medium => 'Medium priority - normal processing',
            Priority::High => 'High priority - expedited processing',
            Priority::Critical => 'Critical priority - immediate attention required',
        };
    }
    
    public static function fromString(string $priority): ?self {
        return match(strtolower($priority)) {
            'low', '1' => self::Low,
            'medium', '2' => self::Medium,
            'high', '3' => self::High,
            'critical', '4' => self::Critical,
            default => null,
        };
    }
    
    public function isHigherThan(Priority $other): bool {
        return $this->value > $other->value;
    }
}

enum ResponseFormat: string {
    case JSON = 'json';
    case XML = 'xml';
    case CSV = 'csv';
    case HTML = 'html';
    
    public function getMimeType(): string {
        return match($this) {
            ResponseFormat::JSON => 'application/json',
            ResponseFormat::XML => 'application/xml',
            ResponseFormat::CSV => 'text/csv',
            ResponseFormat::HTML => 'text/html',
        };
    }
    
    public function getFileExtension(): string {
        return $this->value;
    }
    
    public static function fromMimeType(string $mimeType): ?self {
        return match($mimeType) {
            'application/json' => self::JSON,
            'application/xml', 'text/xml' => self::XML,
            'text/csv' => self::CSV,
            'text/html' => self::HTML,
            default => null,
        };
    }
}

class Task {
    private Status $status = Status::Pending;
    
    public function __construct(
        private string $title,
        private Priority $priority
    ) {}
    
    public function getStatus(): Status {
        return $this->status;
    }
    
    public function changeStatus(Status $newStatus): bool {
        if ($this->status->canTransitionTo($newStatus)) {
            $this->status = $newStatus;
            return true;
        }
        return false;
    }
    
    public function getPriority(): Priority {
        return $this->priority;
    }
    
    public function getInfo(): array {
        return [
            'title' => $this->title,
            'status' => $this->status->getLabel(),
            'status_color' => $this->status->getColor(),
            'priority' => $this->priority->getDescription(),
            'priority_weight' => $this->priority->getWeight(),
        ];
    }
}

class ApiResponse {
    public function __construct(
        private array $data,
        private ResponseFormat $format = ResponseFormat::JSON
    ) {}
    
    public function render(): string {
        return match($this->format) {
            ResponseFormat::JSON => json_encode($this->data, JSON_PRETTY_PRINT),
            ResponseFormat::XML => $this->arrayToXml($this->data),
            ResponseFormat::CSV => $this->arrayToCsv($this->data),
            ResponseFormat::HTML => $this->arrayToHtml($this->data),
        };
    }
    
    public function getHeaders(): array {
        return [
            'Content-Type' => $this->format->getMimeType(),
        ];
    }
    
    private function arrayToXml(array $data, string $rootElement = 'data'): string {
        $xml = "<{$rootElement}>";
        foreach ($data as $key => $value) {
            if (is_array($value)) {
                $xml .= "<{$key}>" . $this->arrayToXml($value, 'item') . "</{$key}>";
            } else {
                $xml .= "<{$key}>" . htmlspecialchars((string) $value) . "</{$key}>";
            }
        }
        $xml .= "</{$rootElement}>";
        return $xml;
    }
    
    private function arrayToCsv(array $data): string {
        if (empty($data)) return '';
        
        $output = '';
        $headers = array_keys($data[0] ?? $data);
        $output .= implode(',', $headers) . "\n";
        
        foreach ($data as $row) {
            $values = array_map(fn($v) => '"' . str_replace('"', '""', (string) $v) . '"', 
                               is_array($row) ? $row : [$row]);
            $output .= implode(',', $values) . "\n";
        }
        
        return $output;
    }
    
    private function arrayToHtml(array $data): string {
        $html = '<table border="1"><thead><tr>';
        if (!empty($data)) {
            $headers = array_keys($data[0] ?? $data);
            foreach ($headers as $header) {
                $html .= '<th>' . htmlspecialchars($header) . '</th>';
            }
        }
        $html .= '</tr></thead><tbody>';
        
        foreach ($data as $row) {
            $html .= '<tr>';
            if (is_array($row)) {
                foreach ($row as $value) {
                    $html .= '<td>' . htmlspecialchars((string) $value) . '</td>';
                }
            } else {
                $html .= '<td>' . htmlspecialchars((string) $row) . '</td>';
            }
            $html .= '</tr>';
        }
        
        $html .= '</tbody></table>';
        return $html;
    }
}

// Usage examples
$task = new Task("Implement user authentication", Priority::High);

echo "Task created:\n";
print_r($task->getInfo());

echo "\nTrying to change status from Pending to Processing: ";
echo $task->changeStatus(Status::Processing) ? "Success" : "Failed";
echo "\n";

echo "Current status: " . $task->getStatus()->getLabel() . "\n";
echo "Status color: " . $task->getStatus()->getColor() . "\n";

echo "\nTrying to change status from Processing to Pending: ";
echo $task->changeStatus(Status::Pending) ? "Success" : "Failed";
echo "\n";

// Priority comparison
$lowPriority = Priority::Low;
$highPriority = Priority::High;

echo "\nIs High priority higher than Low? " . 
     ($highPriority->isHigherThan($lowPriority) ? "Yes" : "No") . "\n";

// Priority from string
$priority = Priority::fromString('critical');
echo "Priority from string 'critical': " . 
     ($priority ? $priority->getDescription() : "Not found") . "\n";

// Response format example
$data = [
    ['name' => 'Alice', 'age' => 30, 'city' => 'New York'],
    ['name' => 'Bob', 'age' => 25, 'city' => 'San Francisco']
];

$jsonResponse = new ApiResponse($data, ResponseFormat::JSON);
echo "\nJSON Response:\n" . $jsonResponse->render() . "\n";

$xmlResponse = new ApiResponse($data, ResponseFormat::XML);
echo "\nXML Response:\n" . $xmlResponse->render() . "\n";
```

Enumerations provide type-safe sets of predefined values with associated  
behavior. Backed enums can have scalar values (int, string), while pure  
enums represent discrete cases. Enums support methods, implement interfaces,  
and work with match expressions. They replace class constants and magic  
strings with compile-time checked values.  

## Anonymous classes

Creating one-off classes for specific use cases without formal declaration.  

```php
<?php

// Anonymous class for event handling
$logger = new class {
    private array $logs = [];
    
    public function log(string $level, string $message): void {
        $this->logs[] = [
            'timestamp' => date('Y-m-d H:i:s'),
            'level' => strtoupper($level),
            'message' => $message
        ];
    }
    
    public function getLogs(): array {
        return $this->logs;
    }
    
    public function getLastLog(): ?array {
        return end($this->logs) ?: null;
    }
};

function processWithCallback(array $items, object $processor): array {
    return array_map([$processor, 'process'], $items);
}

// Anonymous class implementing a simple processor
$results = processWithCallback([1, 2, 3, 4, 5], new class {
    public function process(int $item): array {
        return [
            'original' => $item,
            'squared' => $item * $item,
            'doubled' => $item * 2,
            'is_even' => $item % 2 === 0
        ];
    }
});

// Anonymous class extending a base class
abstract class Validator {
    abstract public function validate(mixed $value): bool;
    
    public function getErrorMessage(): string {
        return "Validation failed";
    }
}

$emailValidator = new class extends Validator {
    public function validate(mixed $value): bool {
        return is_string($value) && filter_var($value, FILTER_VALIDATE_EMAIL) !== false;
    }
    
    public function getErrorMessage(): string {
        return "Invalid email format";
    }
};

$rangeValidator = new class(1, 100) extends Validator {
    private int $min;
    private int $max;
    
    public function __construct(int $min, int $max) {
        $this->min = $min;
        $this->max = $max;
    }
    
    public function validate(mixed $value): bool {
        return is_numeric($value) && $value >= $this->min && $value <= $this->max;
    }
    
    public function getErrorMessage(): string {
        return "Value must be between {$this->min} and {$this->max}";
    }
};

// Anonymous class implementing interface
interface Renderable {
    public function render(): string;
}

$card = new class('John Doe', 'Software Engineer') implements Renderable {
    private string $name;
    private string $title;
    
    public function __construct(string $name, string $title) {
        $this->name = $name;
        $this->title = $title;
    }
    
    public function render(): string {
        return "<div class='card'><h3>{$this->name}</h3><p>{$this->title}</p></div>";
    }
};

// Usage examples
$logger->log('info', 'Application started');
$logger->log('error', 'Database connection failed');
$logger->log('info', 'Retrying connection...');

echo "Last log entry:\n";
print_r($logger->getLastLog());

echo "\nProcessed results:\n";
print_r($results[0]); // Show first result

echo "\nEmail validation:\n";
$testEmails = ['test@example.com', 'invalid-email', 'user@domain.org'];
foreach ($testEmails as $email) {
    $isValid = $emailValidator->validate($email);
    echo "$email: " . ($isValid ? 'Valid' : $emailValidator->getErrorMessage()) . "\n";
}

echo "\nRange validation (1-100):\n";
$testValues = [50, 150, -10, 75];
foreach ($testValues as $value) {
    $isValid = $rangeValidator->validate($value);
    echo "$value: " . ($isValid ? 'Valid' : $rangeValidator->getErrorMessage()) . "\n";
}

echo "\nCard rendering:\n";
echo $card->render() . "\n";
```

Anonymous classes are useful for one-off implementations, callbacks, and  
testing. They can extend classes, implement interfaces, and have constructors.  
Use them when you need a simple, single-use class without polluting the  
global namespace. They're particularly useful for event handlers, validators,  
and temporary object creation.  

## Object cloning

Creating copies of objects with control over the cloning process.  

```php
<?php

class Address {
    public function __construct(
        public string $street,
        public string $city,
        public string $country
    ) {}
    
    public function __toString(): string {
        return "{$this->street}, {$this->city}, {$this->country}";
    }
}

class Person {
    private array $friends = [];
    
    public function __construct(
        public string $name,
        public int $age,
        public Address $address
    ) {}
    
    public function addFriend(Person $friend): void {
        $this->friends[] = $friend;
    }
    
    public function getFriends(): array {
        return $this->friends;
    }
    
    public function __clone(): void {
        // Deep clone the address
        $this->address = clone $this->address;
        
        // Deep clone friends array (but not the friend objects themselves)
        $this->friends = array_map(fn($friend) => clone $friend, $this->friends);
    }
    
    public function __toString(): string {
        $friendCount = count($this->friends);
        return "{$this->name} ({$this->age}) at {$this->address} - {$friendCount} friends";
    }
}

class BankAccount {
    private static int $nextId = 1000;
    private int $id;
    private float $balance;
    private array $transactions = [];
    
    public function __construct(
        public readonly string $owner,
        float $initialBalance = 0
    ) {
        $this->id = self::$nextId++;
        $this->balance = $initialBalance;
        $this->addTransaction('Initial deposit', $initialBalance);
    }
    
    public function deposit(float $amount): void {
        $this->balance += $amount;
        $this->addTransaction('Deposit', $amount);
    }
    
    public function withdraw(float $amount): bool {
        if ($amount > $this->balance) {
            return false;
        }
        
        $this->balance -= $amount;
        $this->addTransaction('Withdrawal', -$amount);
        return true;
    }
    
    private function addTransaction(string $type, float $amount): void {
        $this->transactions[] = [
            'date' => new DateTime(),
            'type' => $type,
            'amount' => $amount,
            'balance' => $this->balance
        ];
    }
    
    public function getBalance(): float {
        return $this->balance;
    }
    
    public function getId(): int {
        return $this->id;
    }
    
    public function getTransactionCount(): int {
        return count($this->transactions);
    }
    
    public function __clone(): void {
        // Generate new ID for cloned account
        $this->id = self::$nextId++;
        
        // Reset balance and transactions for the clone
        $this->balance = 0;
        $this->transactions = [];
        $this->addTransaction('Account cloned', 0);
    }
    
    public function __toString(): string {
        return "Account #{$this->id} ({$this->owner}): $" . number_format($this->balance, 2);
    }
}

class Configuration {
    private array $settings = [];
    private bool $readOnly = false;
    
    public function set(string $key, mixed $value): self {
        if ($this->readOnly) {
            throw new RuntimeException("Configuration is read-only");
        }
        
        $this->settings[$key] = $value;
        return $this;
    }
    
    public function get(string $key): mixed {
        return $this->settings[$key] ?? null;
    }
    
    public function makeReadOnly(): self {
        $this->readOnly = true;
        return $this;
    }
    
    public function isReadOnly(): bool {
        return $this->readOnly;
    }
    
    public function getAll(): array {
        return $this->settings;
    }
    
    public function __clone(): void {
        // Cloned configuration should be writable
        $this->readOnly = false;
        // Deep clone nested arrays/objects in settings
        $this->settings = unserialize(serialize($this->settings));
    }
}

// Person cloning example
$address = new Address('123 Main St', 'New York', 'USA');
$john = new Person('John', 30, $address);

$friend = new Person('Alice', 28, new Address('456 Oak Ave', 'Boston', 'USA'));
$john->addFriend($friend);

echo "Original John: " . $john . "\n";

// Clone John
$johnClone = clone $john;
$johnClone->name = 'John Jr.';
$johnClone->age = 5;
$johnClone->address->street = '789 Pine St';

echo "Cloned John: " . $johnClone . "\n";
echo "Original John after cloning: " . $john . "\n"; // Original unchanged

// Bank account cloning
$account1 = new BankAccount('Alice Smith', 1000);
$account1->deposit(500);
$account1->withdraw(200);

echo "\nOriginal account: " . $account1 . "\n";
echo "Transactions: " . $account1->getTransactionCount() . "\n";

$account2 = clone $account1;
echo "Cloned account: " . $account2 . "\n";
echo "Cloned transactions: " . $account2->getTransactionCount() . "\n";

// Configuration cloning
$config = new Configuration();
$config->set('database', ['host' => 'localhost', 'port' => 3306])
       ->set('debug', true)
       ->makeReadOnly();

echo "\nOriginal config read-only: " . ($config->isReadOnly() ? 'Yes' : 'No') . "\n";

$configCopy = clone $config;
echo "Cloned config read-only: " . ($configCopy->isReadOnly() ? 'Yes' : 'No') . "\n";

$configCopy->set('cache', true); // This works because clone is writable
echo "Cloned config settings: " . json_encode($configCopy->getAll()) . "\n";
```

Object cloning creates copies of objects using the `clone` keyword. The  
`__clone()` magic method controls the cloning process, enabling deep copies  
of nested objects and custom initialization of cloned instances. Use cloning  
when you need independent copies of complex objects while maintaining their  
structure and relationships.  

## Late static binding

Using static references that resolve at runtime for inheritance scenarios.  

```php
<?php

class DatabaseModel {
    protected static string $table = 'models';
    protected static array $columns = [];
    
    public static function getTable(): string {
        return static::$table; // Late static binding
    }
    
    public static function create(array $data): static {
        $instance = new static(); // Creates instance of calling class
        foreach ($data as $key => $value) {
            if (property_exists($instance, $key)) {
                $instance->$key = $value;
            }
        }
        return $instance;
    }
    
    public static function find(int $id): ?static {
        // Simulated database lookup
        echo "SELECT * FROM " . static::getTable() . " WHERE id = $id\n";
        
        // Return mock data
        return static::create(['id' => $id]);
    }
    
    public static function all(): array {
        echo "SELECT * FROM " . static::getTable() . "\n";
        return []; // Mock implementation
    }
    
    public function save(): bool {
        $table = static::getTable();
        $class = static::class; // Gets the actual class name
        
        echo "Saving {$class} to {$table}\n";
        return true;
    }
    
    public static function getModelInfo(): array {
        return [
            'class' => static::class,
            'table' => static::$table,
            'columns' => static::$columns
        ];
    }
}

class User extends DatabaseModel {
    protected static string $table = 'users';
    protected static array $columns = ['name', 'email', 'password'];
    
    public string $name = '';
    public string $email = '';
    protected string $password = '';
    
    public static function findByEmail(string $email): ?static {
        echo "SELECT * FROM " . static::getTable() . " WHERE email = '$email'\n";
        return static::create(['email' => $email]);
    }
    
    public function setPassword(string $password): void {
        $this->password = password_hash($password, PASSWORD_DEFAULT);
    }
    
    public function verifyPassword(string $password): bool {
        return password_verify($password, $this->password);
    }
}

class Product extends DatabaseModel {
    protected static string $table = 'products';
    protected static array $columns = ['name', 'price', 'category'];
    
    public string $name = '';
    public float $price = 0.0;
    public string $category = '';
    
    public static function findByCategory(string $category): array {
        echo "SELECT * FROM " . static::getTable() . " WHERE category = '$category'\n";
        return [
            static::create(['name' => 'Sample Product 1', 'category' => $category]),
            static::create(['name' => 'Sample Product 2', 'category' => $category])
        ];
    }
    
    public function applyDiscount(float $percentage): static {
        $newPrice = $this->price * (1 - $percentage / 100);
        return static::create([
            'name' => $this->name,
            'price' => $newPrice,
            'category' => $this->category
        ]);
    }
}

abstract class EventLogger {
    protected static string $logFile = 'general.log';
    
    public static function log(string $message): void {
        $timestamp = date('Y-m-d H:i:s');
        $class = static::class;
        $file = static::$logFile;
        
        echo "[{$timestamp}] [{$class}] Writing to {$file}: {$message}\n";
    }
    
    public static function getLoggerInfo(): array {
        return [
            'class' => static::class,
            'log_file' => static::$logFile
        ];
    }
}

class UserLogger extends EventLogger {
    protected static string $logFile = 'users.log';
}

class OrderLogger extends EventLogger {
    protected static string $logFile = 'orders.log';
}

class ModelFactory {
    public static function createModel(string $type, array $data = []): DatabaseModel {
        return match(strtolower($type)) {
            'user' => User::create($data),
            'product' => Product::create($data),
            default => throw new InvalidArgumentException("Unknown model type: {$type}")
        };
    }
    
    public static function getAvailableModels(): array {
        return [
            'user' => User::getModelInfo(),
            'product' => Product::getModelInfo()
        ];
    }
}

// Usage examples
echo "=== Late Static Binding Examples ===\n\n";

// Database model examples
echo "User table: " . User::getTable() . "\n";
echo "Product table: " . Product::getTable() . "\n";

$user = User::create(['name' => 'Alice', 'email' => 'alice@example.com']);
$user->save(); // Shows "Saving User to users"

$product = Product::create(['name' => 'Laptop', 'price' => 999.99, 'category' => 'Electronics']);
$product->save(); // Shows "Saving Product to products"

// Late static binding in action
$foundUser = User::find(1);        // Uses User::$table
$foundProduct = Product::find(1);  // Uses Product::$table

echo "\nModel info:\n";
print_r(User::getModelInfo());
print_r(Product::getModelInfo());

// Custom methods using late static binding
$emailUser = User::findByEmail('test@example.com');
$electronics = Product::findByCategory('Electronics');

echo "\nFound " . count($electronics) . " products in Electronics category\n";

// Logging with late static binding
UserLogger::log('User logged in');
OrderLogger::log('Order placed');

echo "\nLogger info:\n";
print_r(UserLogger::getLoggerInfo());
print_r(OrderLogger::getLoggerInfo());

// Factory pattern with late static binding
$models = ModelFactory::getAvailableModels();
echo "\nAvailable models:\n";
foreach ($models as $type => $info) {
    echo "- {$type}: table '{$info['table']}', columns: " . 
         implode(', ', $info['columns']) . "\n";
}

$factoryUser = ModelFactory::createModel('user', ['name' => 'Bob']);
echo "\nFactory created user: " . get_class($factoryUser) . "\n";
```

Late static binding resolves `static::` references at runtime based on the  
calling class, not the class where the method is defined. This enables  
proper inheritance behavior for static methods and properties. Use `static::`  
instead of `self::` when you want child classes to override static behavior.  
Common in ORM patterns and factory methods.  

## Dependency injection

Managing object dependencies through constructor injection and interfaces.  

```php
<?php

interface LoggerInterface {
    public function log(string $level, string $message): void;
    public function info(string $message): void;
    public function error(string $message): void;
}

interface CacheInterface {
    public function get(string $key): mixed;
    public function set(string $key, mixed $value, int $ttl = 3600): bool;
    public function delete(string $key): bool;
    public function clear(): bool;
}

interface DatabaseInterface {
    public function query(string $sql, array $params = []): array;
    public function insert(string $table, array $data): int;
    public function update(string $table, array $data, array $where): int;
}

class FileLogger implements LoggerInterface {
    private string $logFile;
    
    public function __construct(string $logFile = 'app.log') {
        $this->logFile = $logFile;
    }
    
    public function log(string $level, string $message): void {
        $timestamp = date('Y-m-d H:i:s');
        $entry = "[{$timestamp}] [{$level}] {$message}\n";
        echo "Writing to {$this->logFile}: {$entry}";
    }
    
    public function info(string $message): void {
        $this->log('INFO', $message);
    }
    
    public function error(string $message): void {
        $this->log('ERROR', $message);
    }
}

class MemoryCache implements CacheInterface {
    private array $cache = [];
    private array $expiry = [];
    
    public function get(string $key): mixed {
        if (isset($this->expiry[$key]) && $this->expiry[$key] < time()) {
            unset($this->cache[$key], $this->expiry[$key]);
            return null;
        }
        
        return $this->cache[$key] ?? null;
    }
    
    public function set(string $key, mixed $value, int $ttl = 3600): bool {
        $this->cache[$key] = $value;
        $this->expiry[$key] = time() + $ttl;
        return true;
    }
    
    public function delete(string $key): bool {
        unset($this->cache[$key], $this->expiry[$key]);
        return true;
    }
    
    public function clear(): bool {
        $this->cache = [];
        $this->expiry = [];
        return true;
    }
}

class MockDatabase implements DatabaseInterface {
    private array $data = [];
    private int $lastInsertId = 0;
    
    public function query(string $sql, array $params = []): array {
        echo "Executing query: {$sql}\n";
        return $this->data[$sql] ?? [];
    }
    
    public function insert(string $table, array $data): int {
        $this->lastInsertId++;
        echo "Inserting into {$table}: " . json_encode($data) . "\n";
        return $this->lastInsertId;
    }
    
    public function update(string $table, array $data, array $where): int {
        echo "Updating {$table} with " . json_encode($data) . 
             " where " . json_encode($where) . "\n";
        return 1; // Mock: one row affected
    }
}

class UserService {
    public function __construct(
        private DatabaseInterface $database,
        private LoggerInterface $logger,
        private CacheInterface $cache
    ) {}
    
    public function createUser(string $name, string $email): int {
        $this->logger->info("Creating user: {$name}");
        
        // Check if user already exists
        $existing = $this->findUserByEmail($email);
        if ($existing) {
            $this->logger->error("User with email {$email} already exists");
            throw new InvalidArgumentException("User already exists");
        }
        
        // Insert user
        $userId = $this->database->insert('users', [
            'name' => $name,
            'email' => $email,
            'created_at' => date('Y-m-d H:i:s')
        ]);
        
        // Cache user data
        $userData = ['id' => $userId, 'name' => $name, 'email' => $email];
        $this->cache->set("user:{$userId}", $userData);
        $this->cache->set("user:email:{$email}", $userData);
        
        $this->logger->info("User created with ID: {$userId}");
        return $userId;
    }
    
    public function findUser(int $userId): ?array {
        // Try cache first
        $cacheKey = "user:{$userId}";
        $user = $this->cache->get($cacheKey);
        
        if ($user) {
            $this->logger->info("User {$userId} loaded from cache");
            return $user;
        }
        
        // Load from database
        $users = $this->database->query("SELECT * FROM users WHERE id = ?", [$userId]);
        $user = $users[0] ?? null;
        
        if ($user) {
            $this->cache->set($cacheKey, $user);
            $this->logger->info("User {$userId} loaded from database");
        } else {
            $this->logger->info("User {$userId} not found");
        }
        
        return $user;
    }
    
    public function findUserByEmail(string $email): ?array {
        $cacheKey = "user:email:{$email}";
        $user = $this->cache->get($cacheKey);
        
        if ($user) {
            return $user;
        }
        
        $users = $this->database->query("SELECT * FROM users WHERE email = ?", [$email]);
        $user = $users[0] ?? null;
        
        if ($user) {
            $this->cache->set($cacheKey, $user);
        }
        
        return $user;
    }
    
    public function updateUser(int $userId, array $data): bool {
        $this->logger->info("Updating user {$userId}");
        
        $affectedRows = $this->database->update('users', $data, ['id' => $userId]);
        
        if ($affectedRows > 0) {
            // Invalidate cache
            $this->cache->delete("user:{$userId}");
            $this->logger->info("User {$userId} updated successfully");
            return true;
        }
        
        $this->logger->error("Failed to update user {$userId}");
        return false;
    }
}

class Container {
    private array $bindings = [];
    private array $instances = [];
    
    public function bind(string $abstract, callable $concrete): void {
        $this->bindings[$abstract] = $concrete;
    }
    
    public function singleton(string $abstract, callable $concrete): void {
        $this->bind($abstract, function() use ($concrete, $abstract) {
            if (!isset($this->instances[$abstract])) {
                $this->instances[$abstract] = $concrete();
            }
            return $this->instances[$abstract];
        });
    }
    
    public function resolve(string $abstract): mixed {
        if (isset($this->bindings[$abstract])) {
            return $this->bindings[$abstract]();
        }
        
        // Auto-resolution for classes
        if (class_exists($abstract)) {
            return $this->createInstance($abstract);
        }
        
        throw new InvalidArgumentException("Unable to resolve: {$abstract}");
    }
    
    private function createInstance(string $class): object {
        $reflection = new ReflectionClass($class);
        $constructor = $reflection->getConstructor();
        
        if (!$constructor) {
            return new $class();
        }
        
        $parameters = $constructor->getParameters();
        $dependencies = [];
        
        foreach ($parameters as $parameter) {
            $type = $parameter->getType();
            if ($type && !$type->isBuiltin()) {
                $dependencies[] = $this->resolve($type->getName());
            }
        }
        
        return $reflection->newInstanceArgs($dependencies);
    }
}

// Dependency injection setup
$container = new Container();

// Bind interfaces to implementations
$container->singleton(LoggerInterface::class, fn() => new FileLogger('app.log'));
$container->singleton(CacheInterface::class, fn() => new MemoryCache());
$container->singleton(DatabaseInterface::class, fn() => new MockDatabase());

// Bind UserService with automatic dependency injection
$container->bind(UserService::class, fn() => new UserService(
    $container->resolve(DatabaseInterface::class),
    $container->resolve(LoggerInterface::class),
    $container->resolve(CacheInterface::class)
));

// Usage example
echo "=== Dependency Injection Example ===\n\n";

$userService = $container->resolve(UserService::class);

// Create a user
try {
    $userId = $userService->createUser('Alice Johnson', 'alice@example.com');
    echo "Created user with ID: {$userId}\n\n";
    
    // Find the user (will hit cache)
    $user = $userService->findUser($userId);
    echo "Found user: " . json_encode($user) . "\n\n";
    
    // Update the user
    $userService->updateUser($userId, ['name' => 'Alice Smith']);
    
    // Find again (cache invalidated, will hit database)
    $user = $userService->findUser($userId);
    
} catch (Exception $e) {
    echo "Error: " . $e->getMessage() . "\n";
}
```

Dependency injection promotes loose coupling by injecting dependencies rather  
than creating them internally. Classes depend on interfaces, not concrete  
implementations. This makes code more testable, flexible, and maintainable.  
Use constructor injection for required dependencies and consider using a  
container for complex dependency graphs.  

## Factory patterns

Creating objects through factory methods and abstract factories.  

```php
<?php

// Product hierarchy
abstract class Vehicle {
    protected string $make;
    protected string $model;
    
    public function __construct(string $make, string $model) {
        $this->make = $make;
        $this->model = $model;
    }
    
    abstract public function start(): string;
    abstract public function getType(): string;
    
    public function getInfo(): string {
        return "{$this->make} {$this->model} ({$this->getType()})";
    }
}

class Car extends Vehicle {
    public function start(): string {
        return "Starting car engine...";
    }
    
    public function getType(): string {
        return "Car";
    }
}

class Motorcycle extends Vehicle {
    public function start(): string {
        return "Starting motorcycle engine...";
    }
    
    public function getType(): string {
        return "Motorcycle";
    }
}

class Truck extends Vehicle {
    public function start(): string {
        return "Starting truck engine...";
    }
    
    public function getType(): string {
        return "Truck";
    }
}

// Simple Factory
class VehicleFactory {
    public static function create(string $type, string $make, string $model): Vehicle {
        return match(strtolower($type)) {
            'car' => new Car($make, $model),
            'motorcycle' => new Motorcycle($make, $model),
            'truck' => new Truck($make, $model),
            default => throw new InvalidArgumentException("Unknown vehicle type: {$type}")
        };
    }
    
    public static function getSupportedTypes(): array {
        return ['car', 'motorcycle', 'truck'];
    }
}

// Factory Method Pattern
abstract class VehicleManufacturer {
    protected string $companyName;
    
    public function __construct(string $companyName) {
        $this->companyName = $companyName;
    }
    
    // Factory method - to be implemented by subclasses
    abstract protected function createVehicle(string $model): Vehicle;
    
    public function produceVehicle(string $model): Vehicle {
        $vehicle = $this->createVehicle($model);
        $this->registerVehicle($vehicle);
        return $vehicle;
    }
    
    protected function registerVehicle(Vehicle $vehicle): void {
        echo "Registering {$vehicle->getInfo()} manufactured by {$this->companyName}\n";
    }
    
    public function getCompanyName(): string {
        return $this->companyName;
    }
}

class CarManufacturer extends VehicleManufacturer {
    protected function createVehicle(string $model): Vehicle {
        return new Car($this->companyName, $model);
    }
}

class MotorcycleManufacturer extends VehicleManufacturer {
    protected function createVehicle(string $model): Vehicle {
        return new Motorcycle($this->companyName, $model);
    }
}

// Abstract Factory Pattern
interface UIFactory {
    public function createButton(): Button;
    public function createTextField(): TextField;
    public function createDialog(): Dialog;
}

abstract class Button {
    abstract public function render(): string;
    abstract public function onClick(): string;
}

abstract class TextField {
    abstract public function render(): string;
    abstract public function getValue(): string;
}

abstract class Dialog {
    abstract public function render(): string;
    abstract public function show(): string;
}

// Windows UI Components
class WindowsButton extends Button {
    public function render(): string {
        return "<button class='windows-button'>Windows Button</button>";
    }
    
    public function onClick(): string {
        return "Windows button clicked with system sound";
    }
}

class WindowsTextField extends TextField {
    private string $value = '';
    
    public function render(): string {
        return "<input type='text' class='windows-textfield' />";
    }
    
    public function getValue(): string {
        return $this->value;
    }
}

class WindowsDialog extends Dialog {
    public function render(): string {
        return "<div class='windows-dialog'>Windows Dialog</div>";
    }
    
    public function show(): string {
        return "Showing Windows-style dialog with system animation";
    }
}

// macOS UI Components
class MacButton extends Button {
    public function render(): string {
        return "<button class='mac-button'>Mac Button</button>";
    }
    
    public function onClick(): string {
        return "Mac button clicked with smooth animation";
    }
}

class MacTextField extends TextField {
    private string $value = '';
    
    public function render(): string {
        return "<input type='text' class='mac-textfield' />";
    }
    
    public function getValue(): string {
        return $this->value;
    }
}

class MacDialog extends Dialog {
    public function render(): string {
        return "<div class='mac-dialog'>Mac Dialog</div>";
    }
    
    public function show(): string {
        return "Showing Mac-style dialog with fade animation";
    }
}

// Concrete Factories
class WindowsUIFactory implements UIFactory {
    public function createButton(): Button {
        return new WindowsButton();
    }
    
    public function createTextField(): TextField {
        return new WindowsTextField();
    }
    
    public function createDialog(): Dialog {
        return new WindowsDialog();
    }
}

class MacUIFactory implements UIFactory {
    public function createButton(): Button {
        return new MacButton();
    }
    
    public function createTextField(): TextField {
        return new MacTextField();
    }
    
    public function createDialog(): Dialog {
        return new MacDialog();
    }
}

// Application using Abstract Factory
class Application {
    private UIFactory $uiFactory;
    private Button $button;
    private TextField $textField;
    private Dialog $dialog;
    
    public function __construct(UIFactory $uiFactory) {
        $this->uiFactory = $uiFactory;
        $this->createUI();
    }
    
    private function createUI(): void {
        $this->button = $this->uiFactory->createButton();
        $this->textField = $this->uiFactory->createTextField();
        $this->dialog = $this->uiFactory->createDialog();
    }
    
    public function render(): string {
        return implode("\n", [
            $this->button->render(),
            $this->textField->render(),
            $this->dialog->render()
        ]);
    }
    
    public function interact(): array {
        return [
            'button_click' => $this->button->onClick(),
            'dialog_show' => $this->dialog->show()
        ];
    }
}

class UIFactoryProvider {
    public static function getFactory(string $platform): UIFactory {
        return match(strtolower($platform)) {
            'windows' => new WindowsUIFactory(),
            'mac', 'macos' => new MacUIFactory(),
            default => throw new InvalidArgumentException("Unsupported platform: {$platform}")
        };
    }
}

// Usage examples
echo "=== Factory Pattern Examples ===\n\n";

// Simple Factory
echo "Simple Factory:\n";
$car = VehicleFactory::create('car', 'Toyota', 'Camry');
$bike = VehicleFactory::create('motorcycle', 'Harley', 'Sportster');

echo $car->getInfo() . " - " . $car->start() . "\n";
echo $bike->getInfo() . " - " . $bike->start() . "\n";

echo "Supported types: " . implode(', ', VehicleFactory::getSupportedTypes()) . "\n\n";

// Factory Method
echo "Factory Method:\n";
$toyotaFactory = new CarManufacturer('Toyota');
$hondaFactory = new MotorcycleManufacturer('Honda');

$prius = $toyotaFactory->produceVehicle('Prius');
$cbr = $hondaFactory->produceVehicle('CBR600');

echo $prius->getInfo() . "\n";
echo $cbr->getInfo() . "\n\n";

// Abstract Factory
echo "Abstract Factory:\n";
$windowsApp = new Application(UIFactoryProvider::getFactory('windows'));
$macApp = new Application(UIFactoryProvider::getFactory('mac'));

echo "Windows UI:\n" . $windowsApp->render() . "\n";
echo "\nMac UI:\n" . $macApp->render() . "\n";

echo "\nWindows interactions:\n";
print_r($windowsApp->interact());

echo "\nMac interactions:\n";
print_r($macApp->interact());
```

Factory patterns encapsulate object creation logic and promote loose coupling.  
Simple factories centralize object creation, factory methods allow subclasses  
to decide which objects to create, and abstract factories provide interfaces  
for creating families of related objects. Use factories when object creation  
is complex or when you need to switch between different implementations.  

## Observer pattern

Implementing the observer pattern for event-driven programming.  

```php
<?php

interface ObserverInterface {
    public function update(string $event, mixed $data): void;
}

interface SubjectInterface {
    public function attach(ObserverInterface $observer): void;
    public function detach(ObserverInterface $observer): void;
    public function notify(string $event, mixed $data = null): void;
}

trait Observable {
    private array $observers = [];
    
    public function attach(ObserverInterface $observer): void {
        $this->observers[] = $observer;
    }
    
    public function detach(ObserverInterface $observer): void {
        $this->observers = array_filter(
            $this->observers, 
            fn($obs) => $obs !== $observer
        );
    }
    
    public function notify(string $event, mixed $data = null): void {
        foreach ($this->observers as $observer) {
            $observer->update($event, $data);
        }
    }
}

class User implements SubjectInterface {
    use Observable;
    
    private string $name;
    private string $email;
    private string $status = 'offline';
    private array $orders = [];
    
    public function __construct(string $name, string $email) {
        $this->name = $name;
        $this->email = $email;
    }
    
    public function getName(): string {
        return $this->name;
    }
    
    public function getEmail(): string {
        return $this->email;
    }
    
    public function setStatus(string $status): void {
        $oldStatus = $this->status;
        $this->status = $status;
        
        $this->notify('status_changed', [
            'user' => $this,
            'old_status' => $oldStatus,
            'new_status' => $status
        ]);
    }
    
    public function getStatus(): string {
        return $this->status;
    }
    
    public function placeOrder(array $orderData): void {
        $order = [
            'id' => count($this->orders) + 1,
            'user' => $this->name,
            'items' => $orderData['items'],
            'total' => $orderData['total'],
            'timestamp' => new DateTime()
        ];
        
        $this->orders[] = $order;
        
        $this->notify('order_placed', [
            'user' => $this,
            'order' => $order
        ]);
    }
    
    public function getOrders(): array {
        return $this->orders;
    }
}

class EmailNotifier implements ObserverInterface {
    private string $service;
    
    public function __construct(string $service = 'default') {
        $this->service = $service;
    }
    
    public function update(string $event, mixed $data): void {
        match($event) {
            'status_changed' => $this->sendStatusEmail($data),
            'order_placed' => $this->sendOrderConfirmation($data),
            default => null
        };
    }
    
    private function sendStatusEmail(array $data): void {
        $user = $data['user'];
        $status = $data['new_status'];
        
        echo "[{$this->service} Email] Status update for {$user->getName()}: {$status}\n";
    }
    
    private function sendOrderConfirmation(array $data): void {
        $user = $data['user'];
        $order = $data['order'];
        
        echo "[{$this->service} Email] Order #{$order['id']} confirmed for {$user->getName()} - Total: \${$order['total']}\n";
    }
}

class SMSNotifier implements ObserverInterface {
    private string $provider;
    
    public function __construct(string $provider = 'default') {
        $this->provider = $provider;
    }
    
    public function update(string $event, mixed $data): void {
        match($event) {
            'order_placed' => $this->sendOrderSMS($data),
            default => null // SMS only for orders in this example
        };
    }
    
    private function sendOrderSMS(array $data): void {
        $user = $data['user'];
        $order = $data['order'];
        
        echo "[{$this->provider} SMS] Order #{$order['id']} received. Total: \${$order['total']}\n";
    }
}

class AnalyticsTracker implements ObserverInterface {
    private array $events = [];
    
    public function update(string $event, mixed $data): void {
        $this->events[] = [
            'timestamp' => new DateTime(),
            'event' => $event,
            'data' => $data
        ];
        
        echo "[Analytics] Tracked event: {$event}\n";
    }
    
    public function getEventCount(string $event = null): int {
        if ($event === null) {
            return count($this->events);
        }
        
        return count(array_filter($this->events, fn($e) => $e['event'] === $event));
    }
    
    public function getEvents(): array {
        return $this->events;
    }
}

// Usage example
echo "=== Observer Pattern Example ===\n\n";

$user = new User("Alice Johnson", "alice@example.com");

// Create observers
$emailNotifier = new EmailNotifier("SendGrid");
$smsNotifier = new SMSNotifier("Twilio");
$analytics = new AnalyticsTracker();

// Attach observers
$user->attach($emailNotifier);
$user->attach($smsNotifier);
$user->attach($analytics);

// User actions that trigger notifications
$user->setStatus('online');

$user->placeOrder([
    'items' => ['Laptop', 'Mouse'],
    'total' => 1299.99
]);

$user->setStatus('offline');

$user->placeOrder([
    'items' => ['Book'],
    'total' => 29.99
]);

echo "\nAnalytics Summary:\n";
echo "Total events: " . $analytics->getEventCount() . "\n";
echo "Status changes: " . $analytics->getEventCount('status_changed') . "\n";
echo "Orders placed: " . $analytics->getEventCount('order_placed') . "\n";

// Detach email notifier
$user->detach($emailNotifier);
echo "\nEmail notifier detached.\n";

$user->setStatus('busy'); // Only SMS and analytics will be notified
```

The Observer pattern defines a one-to-many dependency between objects so  
when one object changes state, all dependents are notified automatically.  
It promotes loose coupling between subjects and observers, enabling flexible  
event-driven architectures. Use it for implementing event systems, model-view  
separation, and distributed event handling.  

## Serialization

Converting objects to and from serializable formats for storage or transmission.  

```php
<?php

class SerializableUser {
    private int $id;
    private string $name;
    private string $email;
    private array $preferences;
    private DateTime $createdAt;
    private ?string $temporaryToken = null; // Won't be serialized
    
    public function __construct(int $id, string $name, string $email) {
        $this->id = $id;
        $this->name = $name;
        $this->email = $email;
        $this->preferences = [];
        $this->createdAt = new DateTime();
    }
    
    public function setPreference(string $key, mixed $value): void {
        $this->preferences[$key] = $value;
    }
    
    public function getPreference(string $key): mixed {
        return $this->preferences[$key] ?? null;
    }
    
    public function setTemporaryToken(string $token): void {
        $this->temporaryToken = $token;
    }
    
    public function getTemporaryToken(): ?string {
        return $this->temporaryToken;
    }
    
    // Control what gets serialized
    public function __sleep(): array {
        echo "Preparing user '{$this->name}' for serialization\n";
        // Don't serialize the temporary token
        return ['id', 'name', 'email', 'preferences', 'createdAt'];
    }
    
    // Handle post-unserialization initialization
    public function __wakeup(): void {
        echo "Waking up user '{$this->name}' from serialization\n";
        // Reset temporary token after unserialization
        $this->temporaryToken = null;
    }
    
    public function getInfo(): array {
        return [
            'id' => $this->id,
            'name' => $this->name,
            'email' => $this->email,
            'preferences_count' => count($this->preferences),
            'created_at' => $this->createdAt->format('Y-m-d H:i:s'),
            'has_token' => $this->temporaryToken !== null
        ];
    }
}

class JsonSerializable implements \JsonSerializable {
    public function __construct(
        private string $title,
        private array $data,
        private DateTime $timestamp
    ) {}
    
    public function jsonSerialize(): mixed {
        return [
            'title' => $this->title,
            'data' => $this->data,
            'timestamp' => $this->timestamp->format('c'), // ISO 8601
            'serialized_at' => (new DateTime())->format('c')
        ];
    }
    
    public static function fromJson(string $json): self {
        $data = json_decode($json, true);
        
        return new self(
            $data['title'],
            $data['data'],
            new DateTime($data['timestamp'])
        );
    }
    
    public function getTitle(): string {
        return $this->title;
    }
    
    public function getData(): array {
        return $this->data;
    }
    
    public function getTimestamp(): DateTime {
        return $this->timestamp;
    }
}

class CacheableObject {
    private static array $cache = [];
    
    public function __construct(
        private string $key,
        private mixed $value,
        private int $ttl = 3600
    ) {}
    
    public function save(): void {
        $serialized = serialize($this);
        $expiry = time() + $this->ttl;
        
        self::$cache[$this->key] = [
            'data' => $serialized,
            'expiry' => $expiry
        ];
        
        echo "Cached object with key: {$this->key}\n";
    }
    
    public static function load(string $key): ?self {
        if (!isset(self::$cache[$key])) {
            return null;
        }
        
        $cached = self::$cache[$key];
        
        if ($cached['expiry'] < time()) {
            unset(self::$cache[$key]);
            return null;
        }
        
        echo "Loading cached object with key: {$key}\n";
        return unserialize($cached['data']);
    }
    
    public function getValue(): mixed {
        return $this->value;
    }
    
    public function getKey(): string {
        return $this->key;
    }
    
    public static function getCacheStats(): array {
        return [
            'total_items' => count(self::$cache),
            'items' => array_keys(self::$cache)
        ];
    }
    
    public static function clearCache(): void {
        self::$cache = [];
        echo "Cache cleared\n";
    }
}

class ConfigurationData {
    public function __construct(
        private array $settings = [],
        private string $environment = 'development'
    ) {}
    
    public function set(string $key, mixed $value): void {
        $this->settings[$key] = $value;
    }
    
    public function get(string $key): mixed {
        return $this->settings[$key] ?? null;
    }
    
    public function toArray(): array {
        return [
            'settings' => $this->settings,
            'environment' => $this->environment
        ];
    }
    
    public function exportToFile(string $filename): void {
        $data = serialize($this->toArray());
        file_put_contents($filename, $data);
        echo "Configuration exported to {$filename}\n";
    }
    
    public static function importFromFile(string $filename): self {
        if (!file_exists($filename)) {
            throw new InvalidArgumentException("File not found: {$filename}");
        }
        
        $data = unserialize(file_get_contents($filename));
        $config = new self($data['settings'], $data['environment']);
        
        echo "Configuration imported from {$filename}\n";
        return $config;
    }
    
    public function toJson(): string {
        return json_encode($this->toArray(), JSON_PRETTY_PRINT);
    }
    
    public static function fromJson(string $json): self {
        $data = json_decode($json, true);
        return new self($data['settings'] ?? [], $data['environment'] ?? 'development');
    }
}

// Usage examples
echo "=== Serialization Examples ===\n\n";

// PHP native serialization
$user = new SerializableUser(1, 'Alice', 'alice@example.com');
$user->setPreference('theme', 'dark');
$user->setPreference('notifications', true);
$user->setTemporaryToken('temp_123');

echo "Before serialization:\n";
print_r($user->getInfo());
echo "Temporary token: " . ($user->getTemporaryToken() ?? 'null') . "\n\n";

$serialized = serialize($user);
echo "Serialized data length: " . strlen($serialized) . " bytes\n\n";

$unserialized = unserialize($serialized);
echo "After unserialization:\n";
print_r($unserialized->getInfo());
echo "Temporary token: " . ($unserialized->getTemporaryToken() ?? 'null') . "\n\n";

// JSON serialization
$jsonObj = new JsonSerializable(
    'Test Data',
    ['key1' => 'value1', 'key2' => 42],
    new DateTime()
);

$json = json_encode($jsonObj, JSON_PRETTY_PRINT);
echo "JSON serialization:\n{$json}\n\n";

$fromJson = JsonSerializable::fromJson($json);
echo "Deserialized from JSON:\n";
echo "Title: " . $fromJson->getTitle() . "\n";
echo "Data: " . json_encode($fromJson->getData()) . "\n\n";

// Cacheable object
$obj1 = new CacheableObject('config', ['host' => 'localhost', 'port' => 3306]);
$obj1->save();

$obj2 = new CacheableObject('session', ['user_id' => 123, 'role' => 'admin']);
$obj2->save();

echo "Cache stats: " . json_encode(CacheableObject::getCacheStats()) . "\n\n";

$loaded = CacheableObject::load('config');
if ($loaded) {
    echo "Loaded value: " . json_encode($loaded->getValue()) . "\n";
}

// Configuration serialization
$config = new ConfigurationData([
    'database' => ['host' => 'localhost', 'name' => 'myapp'],
    'debug' => true,
    'cache_ttl' => 3600
], 'production');

echo "\nConfiguration as JSON:\n";
echo $config->toJson() . "\n";

// Create temporary file for demo
$tempFile = tempnam(sys_get_temp_dir(), 'config_');
$config->exportToFile($tempFile);

$importedConfig = ConfigurationData::importFromFile($tempFile);
echo "Imported config debug setting: " . var_export($importedConfig->get('debug'), true) . "\n";

// Clean up
unlink($tempFile);
CacheableObject::clearCache();
```

Serialization converts objects into storable formats and back. PHP's native  
`serialize()` works with all data types but isn't human-readable. JSON  
serialization is portable but limited to basic data types. Custom  
serialization via `__sleep()` and `__wakeup()` controls what gets serialized.  
Use serialization for caching, session storage, and data persistence.  

## Iterator implementation

Creating objects that can be iterated using foreach loops.  

```php
<?php

class NumberRange implements Iterator {
    private int $start;
    private int $end;
    private int $step;
    private int $current;
    
    public function __construct(int $start, int $end, int $step = 1) {
        if ($step === 0) {
            throw new InvalidArgumentException("Step cannot be zero");
        }
        
        $this->start = $start;
        $this->end = $end;
        $this->step = $step;
        $this->current = $start;
    }
    
    public function current(): int {
        return $this->current;
    }
    
    public function key(): int {
        return ($this->current - $this->start) / $this->step;
    }
    
    public function next(): void {
        $this->current += $this->step;
    }
    
    public function rewind(): void {
        $this->current = $this->start;
    }
    
    public function valid(): bool {
        return $this->step > 0 ? $this->current <= $this->end : $this->current >= $this->end;
    }
}

class FileLineReader implements Iterator {
    private $handle;
    private string $currentLine;
    private int $lineNumber;
    private bool $valid;
    
    public function __construct(string $filename) {
        $this->handle = fopen($filename, 'r');
        if (!$this->handle) {
            throw new InvalidArgumentException("Cannot open file: {$filename}");
        }
    }
    
    public function __destruct() {
        if ($this->handle) {
            fclose($this->handle);
        }
    }
    
    public function current(): string {
        return $this->currentLine;
    }
    
    public function key(): int {
        return $this->lineNumber;
    }
    
    public function next(): void {
        $this->currentLine = fgets($this->handle);
        $this->lineNumber++;
        $this->valid = $this->currentLine !== false;
    }
    
    public function rewind(): void {
        rewind($this->handle);
        $this->lineNumber = 0;
        $this->next(); // Load first line
    }
    
    public function valid(): bool {
        return $this->valid;
    }
}

class Collection implements Iterator, Countable {
    private array $items = [];
    private int $position = 0;
    
    public function add(mixed $item): void {
        $this->items[] = $item;
    }
    
    public function remove(int $index): bool {
        if (isset($this->items[$index])) {
            unset($this->items[$index]);
            $this->items = array_values($this->items); // Re-index
            return true;
        }
        return false;
    }
    
    public function get(int $index): mixed {
        return $this->items[$index] ?? null;
    }
    
    public function indexOf(mixed $item): int|false {
        return array_search($item, $this->items, true);
    }
    
    public function contains(mixed $item): bool {
        return in_array($item, $this->items, true);
    }
    
    public function toArray(): array {
        return $this->items;
    }
    
    // Iterator implementation
    public function current(): mixed {
        return $this->items[$this->position] ?? null;
    }
    
    public function key(): int {
        return $this->position;
    }
    
    public function next(): void {
        $this->position++;
    }
    
    public function rewind(): void {
        $this->position = 0;
    }
    
    public function valid(): bool {
        return isset($this->items[$this->position]);
    }
    
    // Countable implementation
    public function count(): int {
        return count($this->items);
    }
}

class TreeNode implements Iterator {
    private mixed $value;
    private array $children = [];
    private int $iteratorPosition = 0;
    
    public function __construct(mixed $value) {
        $this->value = $value;
    }
    
    public function getValue(): mixed {
        return $this->value;
    }
    
    public function addChild(TreeNode $child): void {
        $this->children[] = $child;
    }
    
    public function getChildren(): array {
        return $this->children;
    }
    
    public function hasChildren(): bool {
        return !empty($this->children);
    }
    
    // Iterator implementation - iterates over children
    public function current(): TreeNode {
        return $this->children[$this->iteratorPosition];
    }
    
    public function key(): int {
        return $this->iteratorPosition;
    }
    
    public function next(): void {
        $this->iteratorPosition++;
    }
    
    public function rewind(): void {
        $this->iteratorPosition = 0;
    }
    
    public function valid(): bool {
        return isset($this->children[$this->iteratorPosition]);
    }
    
    // Recursive iteration helper
    public function getAllValues(): array {
        $values = [$this->value];
        
        foreach ($this->children as $child) {
            $values = array_merge($values, $child->getAllValues());
        }
        
        return $values;
    }
}

class FilterIterator implements Iterator {
    private Iterator $iterator;
    private callable $filter;
    
    public function __construct(Iterator $iterator, callable $filter) {
        $this->iterator = $iterator;
        $this->filter = $filter;
    }
    
    public function current(): mixed {
        return $this->iterator->current();
    }
    
    public function key(): mixed {
        return $this->iterator->key();
    }
    
    public function next(): void {
        $this->iterator->next();
        $this->skipInvalid();
    }
    
    public function rewind(): void {
        $this->iterator->rewind();
        $this->skipInvalid();
    }
    
    public function valid(): bool {
        return $this->iterator->valid();
    }
    
    private function skipInvalid(): void {
        while ($this->iterator->valid()) {
            $filter = $this->filter;
            if ($filter($this->iterator->current())) {
                break;
            }
            $this->iterator->next();
        }
    }
}

// Usage examples
echo "=== Iterator Implementation Examples ===\n\n";

// Number range iterator
echo "Number range (1 to 10, step 2):\n";
$range = new NumberRange(1, 10, 2);
foreach ($range as $key => $value) {
    echo "Index $key: $value\n";
}

echo "\nReversed range (10 to 1, step -3):\n";
$reverseRange = new NumberRange(10, 1, -3);
foreach ($reverseRange as $value) {
    echo "$value ";
}
echo "\n\n";

// Collection iterator
$collection = new Collection();
$collection->add('apple');
$collection->add('banana');
$collection->add('cherry');

echo "Collection items (count: " . count($collection) . "):\n";
foreach ($collection as $index => $item) {
    echo "$index: $item\n";
}

echo "Contains 'banana': " . ($collection->contains('banana') ? 'Yes' : 'No') . "\n";
$collection->remove(1); // Remove banana

echo "After removing banana:\n";
foreach ($collection as $index => $item) {
    echo "$index: $item\n";
}
echo "\n";

// Tree iterator
$root = new TreeNode('Root');
$child1 = new TreeNode('Child 1');
$child2 = new TreeNode('Child 2');
$grandchild1 = new TreeNode('Grandchild 1');
$grandchild2 = new TreeNode('Grandchild 2');

$child1->addChild($grandchild1);
$child2->addChild($grandchild2);
$root->addChild($child1);
$root->addChild($child2);

echo "Tree children (direct children only):\n";
foreach ($root as $child) {
    echo "- " . $child->getValue() . "\n";
}

echo "\nAll tree values (recursive):\n";
foreach ($root->getAllValues() as $value) {
    echo "- $value\n";
}
echo "\n";

// Filter iterator
$numbers = new Collection();
for ($i = 1; $i <= 20; $i++) {
    $numbers->add($i);
}

$evenNumbers = new FilterIterator($numbers, fn($n) => $n % 2 === 0);

echo "Even numbers from 1-20:\n";
foreach ($evenNumbers as $number) {
    echo "$number ";
}
echo "\n\n";

// Create a temporary file for FileLineReader demo
$tempFile = tempnam(sys_get_temp_dir(), 'lines_');
file_put_contents($tempFile, "Line 1\nLine 2\nLine 3\nLine 4\n");

echo "File contents:\n";
$fileReader = new FileLineReader($tempFile);
foreach ($fileReader as $lineNum => $line) {
    echo "Line $lineNum: " . trim($line) . "\n";
}

// Clean up
unlink($tempFile);
```

Iterator implementation allows objects to be used in foreach loops and with  
iterator functions. Implement the Iterator interface methods: current(),  
key(), next(), rewind(), and valid(). This enables custom iteration logic  
over object data. Combine with Countable for count() support and create  
filter iterators for advanced iteration patterns.  

## Countable interface

Making objects countable with the count() function.  

```php
<?php

class Playlist implements Countable, Iterator {
    private array $songs = [];
    private int $position = 0;
    private string $name;
    private int $totalDuration = 0; // in seconds
    
    public function __construct(string $name) {
        $this->name = $name;
    }
    
    public function addSong(string $title, string $artist, int $duration): void {
        $song = [
            'title' => $title,
            'artist' => $artist,
            'duration' => $duration,
            'added_at' => new DateTime()
        ];
        
        $this->songs[] = $song;
        $this->totalDuration += $duration;
    }
    
    public function removeSong(int $index): bool {
        if (isset($this->songs[$index])) {
            $this->totalDuration -= $this->songs[$index]['duration'];
            unset($this->songs[$index]);
            $this->songs = array_values($this->songs); // Re-index
            return true;
        }
        return false;
    }
    
    public function getName(): string {
        return $this->name;
    }
    
    public function getTotalDuration(): string {
        $hours = floor($this->totalDuration / 3600);
        $minutes = floor(($this->totalDuration % 3600) / 60);
        $seconds = $this->totalDuration % 60;
        
        if ($hours > 0) {
            return sprintf('%d:%02d:%02d', $hours, $minutes, $seconds);
        }
        return sprintf('%d:%02d', $minutes, $seconds);
    }
    
    public function getSong(int $index): ?array {
        return $this->songs[$index] ?? null;
    }
    
    public function shuffle(): void {
        shuffle($this->songs);
    }
    
    public function sortByTitle(): void {
        usort($this->songs, fn($a, $b) => strcasecmp($a['title'], $b['title']));
    }
    
    public function sortByArtist(): void {
        usort($this->songs, fn($a, $b) => strcasecmp($a['artist'], $b['artist']));
    }
    
    // Countable interface
    public function count(): int {
        return count($this->songs);
    }
    
    // Iterator interface
    public function current(): array {
        return $this->songs[$this->position];
    }
    
    public function key(): int {
        return $this->position;
    }
    
    public function next(): void {
        $this->position++;
    }
    
    public function rewind(): void {
        $this->position = 0;
    }
    
    public function valid(): bool {
        return isset($this->songs[$this->position]);
    }
    
    public function getPlaylistInfo(): array {
        return [
            'name' => $this->name,
            'song_count' => $this->count(),
            'total_duration' => $this->getTotalDuration(),
            'average_duration' => $this->count() > 0 ? 
                round($this->totalDuration / $this->count()) . 's' : '0s'
        ];
    }
}

class BookCollection implements Countable {
    private array $books = [];
    private array $categories = [];
    
    public function addBook(string $title, string $author, string $category, int $pages): void {
        $book = [
            'title' => $title,
            'author' => $author,
            'category' => $category,
            'pages' => $pages,
            'added_at' => new DateTime()
        ];
        
        $this->books[] = $book;
        
        if (!isset($this->categories[$category])) {
            $this->categories[$category] = [];
        }
        $this->categories[$category][] = count($this->books) - 1;
    }
    
    public function count(): int {
        return count($this->books);
    }
    
    public function countByCategory(string $category): int {
        return count($this->categories[$category] ?? []);
    }
    
    public function getCategories(): array {
        return array_keys($this->categories);
    }
    
    public function getBooksByCategory(string $category): array {
        if (!isset($this->categories[$category])) {
            return [];
        }
        
        return array_map(
            fn($index) => $this->books[$index],
            $this->categories[$category]
        );
    }
    
    public function getTotalPages(): int {
        return array_sum(array_column($this->books, 'pages'));
    }
    
    public function getAveragePages(): float {
        return $this->count() > 0 ? $this->getTotalPages() / $this->count() : 0;
    }
    
    public function getCollectionStats(): array {
        $stats = [
            'total_books' => $this->count(),
            'total_pages' => $this->getTotalPages(),
            'average_pages' => round($this->getAveragePages(), 1),
            'categories' => []
        ];
        
        foreach ($this->categories as $category => $bookIndices) {
            $stats['categories'][$category] = count($bookIndices);
        }
        
        return $stats;
    }
}

class InventoryItem {
    public function __construct(
        public string $name,
        public int $quantity,
        public float $price
    ) {}
    
    public function getValue(): float {
        return $this->quantity * $this->price;
    }
}

class Inventory implements Countable {
    private array $items = [];
    
    public function addItem(string $name, int $quantity, float $price): void {
        $key = strtolower($name);
        
        if (isset($this->items[$key])) {
            $this->items[$key]->quantity += $quantity;
        } else {
            $this->items[$key] = new InventoryItem($name, $quantity, $price);
        }
    }
    
    public function removeItem(string $name, int $quantity = null): bool {
        $key = strtolower($name);
        
        if (!isset($this->items[$key])) {
            return false;
        }
        
        if ($quantity === null) {
            unset($this->items[$key]);
        } else {
            $this->items[$key]->quantity -= $quantity;
            if ($this->items[$key]->quantity <= 0) {
                unset($this->items[$key]);
            }
        }
        
        return true;
    }
    
    public function getItem(string $name): ?InventoryItem {
        $key = strtolower($name);
        return $this->items[$key] ?? null;
    }
    
    public function count(): int {
        return count($this->items);
    }
    
    public function getTotalQuantity(): int {
        return array_sum(array_map(fn($item) => $item->quantity, $this->items));
    }
    
    public function getTotalValue(): float {
        return array_sum(array_map(fn($item) => $item->getValue(), $this->items));
    }
    
    public function getLowStockItems(int $threshold = 10): array {
        return array_filter(
            $this->items,
            fn($item) => $item->quantity < $threshold
        );
    }
    
    public function getInventoryReport(): array {
        return [
            'unique_items' => $this->count(),
            'total_quantity' => $this->getTotalQuantity(),
            'total_value' => round($this->getTotalValue(), 2),
            'low_stock_count' => count($this->getLowStockItems())
        ];
    }
    
    public function getAllItems(): array {
        return $this->items;
    }
}

// Usage examples
echo "=== Countable Interface Examples ===\n\n";

// Playlist example
$playlist = new Playlist("My Favorites");
$playlist->addSong("Bohemian Rhapsody", "Queen", 354);
$playlist->addSong("Hotel California", "Eagles", 391);
$playlist->addSong("Stairway to Heaven", "Led Zeppelin", 482);
$playlist->addSong("Sweet Child O' Mine", "Guns N' Roses", 356);

echo "Playlist: " . $playlist->getName() . "\n";
echo "Songs count: " . count($playlist) . "\n";
print_r($playlist->getPlaylistInfo());

echo "\nPlaylist songs:\n";
foreach ($playlist as $index => $song) {
    $minutes = floor($song['duration'] / 60);
    $seconds = $song['duration'] % 60;
    echo ($index + 1) . ". {$song['title']} by {$song['artist']} ({$minutes}:{$seconds:02d})\n";
}

$playlist->sortByArtist();
echo "\nAfter sorting by artist:\n";
foreach ($playlist as $index => $song) {
    echo ($index + 1) . ". {$song['title']} by {$song['artist']}\n";
}

// Book collection example
$books = new BookCollection();
$books->addBook("The Great Gatsby", "F. Scott Fitzgerald", "Fiction", 180);
$books->addBook("To Kill a Mockingbird", "Harper Lee", "Fiction", 281);
$books->addBook("1984", "George Orwell", "Fiction", 328);
$books->addBook("A Brief History of Time", "Stephen Hawking", "Science", 256);
$books->addBook("The Selfish Gene", "Richard Dawkins", "Science", 360);

echo "\n\nBook Collection:\n";
echo "Total books: " . count($books) . "\n";
print_r($books->getCollectionStats());

echo "Fiction books: " . $books->countByCategory('Fiction') . "\n";
echo "Science books: " . $books->countByCategory('Science') . "\n";

// Inventory example
$inventory = new Inventory();
$inventory->addItem("Laptop", 5, 999.99);
$inventory->addItem("Mouse", 25, 29.99);
$inventory->addItem("Keyboard", 15, 79.99);
$inventory->addItem("Monitor", 8, 299.99);

echo "\n\nInventory:\n";
echo "Unique items: " . count($inventory) . "\n";
print_r($inventory->getInventoryReport());

echo "\nLow stock items (< 10):\n";
foreach ($inventory->getLowStockItems() as $name => $item) {
    echo "- {$item->name}: {$item->quantity} units\n";
}

$inventory->removeItem("Mouse", 20);
echo "\nAfter removing 20 mice:\n";
echo "Mouse stock: " . ($inventory->getItem("Mouse")?->quantity ?? 0) . "\n";
echo "Total value: $" . $inventory->getTotalValue() . "\n";
```

The Countable interface allows objects to work with the count() function.  
Implement the single count() method to return the number of elements.  
This is useful for collections, containers, and aggregate objects where  
counting elements makes sense. Combine with Iterator for full collection  
functionality that integrates seamlessly with PHP's built-in functions.  

## ArrayAccess interface

Making objects behave like arrays with bracket notation.  

```php
<?php

class Dictionary implements ArrayAccess, Countable, Iterator {
    private array $data = [];
    private array $keys = [];
    private int $position = 0;
    
    public function __construct(array $initialData = []) {
        foreach ($initialData as $key => $value) {
            $this->offsetSet($key, $value);
        }
    }
    
    // ArrayAccess interface
    public function offsetExists(mixed $offset): bool {
        return array_key_exists($offset, $this->data);
    }
    
    public function offsetGet(mixed $offset): mixed {
        return $this->data[$offset] ?? null;
    }
    
    public function offsetSet(mixed $offset, mixed $value): void {
        if ($offset === null) {
            $offset = count($this->data);
        }
        
        if (!array_key_exists($offset, $this->data)) {
            $this->keys[] = $offset;
        }
        
        $this->data[$offset] = $value;
    }
    
    public function offsetUnset(mixed $offset): void {
        if (array_key_exists($offset, $this->data)) {
            unset($this->data[$offset]);
            $this->keys = array_values(array_filter($this->keys, fn($k) => $k !== $offset));
        }
    }
    
    // Countable interface
    public function count(): int {
        return count($this->data);
    }
    
    // Iterator interface
    public function current(): mixed {
        $key = $this->keys[$this->position];
        return $this->data[$key];
    }
    
    public function key(): mixed {
        return $this->keys[$this->position];
    }
    
    public function next(): void {
        $this->position++;
    }
    
    public function rewind(): void {
        $this->position = 0;
    }
    
    public function valid(): bool {
        return isset($this->keys[$this->position]);
    }
    
    // Additional utility methods
    public function keys(): array {
        return $this->keys;
    }
    
    public function values(): array {
        return array_values($this->data);
    }
    
    public function toArray(): array {
        return $this->data;
    }
    
    public function has(mixed $key): bool {
        return $this->offsetExists($key);
    }
    
    public function get(mixed $key, mixed $default = null): mixed {
        return $this->offsetExists($key) ? $this->offsetGet($key) : $default;
    }
    
    public function set(mixed $key, mixed $value): self {
        $this->offsetSet($key, $value);
        return $this;
    }
    
    public function remove(mixed $key): bool {
        if ($this->offsetExists($key)) {
            $this->offsetUnset($key);
            return true;
        }
        return false;
    }
    
    public function clear(): void {
        $this->data = [];
        $this->keys = [];
        $this->position = 0;
    }
}

class Matrix implements ArrayAccess {
    private array $data;
    private int $rows;
    private int $cols;
    
    public function __construct(int $rows, int $cols, mixed $initialValue = 0) {
        $this->rows = $rows;
        $this->cols = $cols;
        
        $this->data = [];
        for ($i = 0; $i < $rows; $i++) {
            for ($j = 0; $j < $cols; $j++) {
                $this->data[$i][$j] = $initialValue;
            }
        }
    }
    
    public function offsetExists(mixed $offset): bool {
        if (is_array($offset) && count($offset) === 2) {
            [$row, $col] = $offset;
            return $row >= 0 && $row < $this->rows && $col >= 0 && $col < $this->cols;
        }
        return isset($this->data[$offset]);
    }
    
    public function offsetGet(mixed $offset): mixed {
        if (is_array($offset) && count($offset) === 2) {
            [$row, $col] = $offset;
            if ($this->offsetExists($offset)) {
                return $this->data[$row][$col];
            }
            throw new OutOfBoundsException("Matrix index [$row, $col] out of bounds");
        }
        
        return $this->data[$offset] ?? null;
    }
    
    public function offsetSet(mixed $offset, mixed $value): void {
        if (is_array($offset) && count($offset) === 2) {
            [$row, $col] = $offset;
            if ($this->offsetExists($offset)) {
                $this->data[$row][$col] = $value;
            } else {
                throw new OutOfBoundsException("Matrix index [$row, $col] out of bounds");
            }
        } else {
            throw new InvalidArgumentException("Matrix requires [row, col] array for indexing");
        }
    }
    
    public function offsetUnset(mixed $offset): void {
        throw new BadMethodCallException("Cannot unset matrix elements");
    }
    
    public function getDimensions(): array {
        return ['rows' => $this->rows, 'cols' => $this->cols];
    }
    
    public function getRow(int $row): array {
        if ($row < 0 || $row >= $this->rows) {
            throw new OutOfBoundsException("Row $row out of bounds");
        }
        return $this->data[$row];
    }
    
    public function setRow(int $row, array $values): void {
        if ($row < 0 || $row >= $this->rows) {
            throw new OutOfBoundsException("Row $row out of bounds");
        }
        if (count($values) !== $this->cols) {
            throw new InvalidArgumentException("Row must have exactly {$this->cols} values");
        }
        $this->data[$row] = array_values($values);
    }
    
    public function transpose(): Matrix {
        $transposed = new Matrix($this->cols, $this->rows);
        
        for ($i = 0; $i < $this->rows; $i++) {
            for ($j = 0; $j < $this->cols; $j++) {
                $transposed[[j, i]] = $this[[i, j]];
            }
        }
        
        return $transposed;
    }
    
    public function toArray(): array {
        return $this->data;
    }
    
    public function __toString(): string {
        $result = "Matrix ({$this->rows}x{$this->cols}):\n";
        foreach ($this->data as $row) {
            $result .= "[" . implode(", ", array_map(fn($v) => str_pad($v, 4), $row)) . "]\n";
        }
        return $result;
    }
}

class Configuration implements ArrayAccess {
    private array $config = [];
    private bool $readOnly = false;
    
    public function __construct(array $initialConfig = []) {
        $this->config = $initialConfig;
    }
    
    public function offsetExists(mixed $offset): bool {
        return $this->hasPath($offset);
    }
    
    public function offsetGet(mixed $offset): mixed {
        return $this->getPath($offset);
    }
    
    public function offsetSet(mixed $offset, mixed $value): void {
        if ($this->readOnly) {
            throw new BadMethodCallException("Configuration is read-only");
        }
        $this->setPath($offset, $value);
    }
    
    public function offsetUnset(mixed $offset): void {
        if ($this->readOnly) {
            throw new BadMethodCallException("Configuration is read-only");
        }
        $this->unsetPath($offset);
    }
    
    private function hasPath(string $path): bool {
        $keys = explode('.', $path);
        $current = $this->config;
        
        foreach ($keys as $key) {
            if (!is_array($current) || !array_key_exists($key, $current)) {
                return false;
            }
            $current = $current[$key];
        }
        
        return true;
    }
    
    private function getPath(string $path): mixed {
        $keys = explode('.', $path);
        $current = $this->config;
        
        foreach ($keys as $key) {
            if (!is_array($current) || !array_key_exists($key, $current)) {
                return null;
            }
            $current = $current[$key];
        }
        
        return $current;
    }
    
    private function setPath(string $path, mixed $value): void {
        $keys = explode('.', $path);
        $current = &$this->config;
        
        foreach ($keys as $i => $key) {
            if ($i === count($keys) - 1) {
                $current[$key] = $value;
            } else {
                if (!isset($current[$key]) || !is_array($current[$key])) {
                    $current[$key] = [];
                }
                $current = &$current[$key];
            }
        }
    }
    
    private function unsetPath(string $path): void {
        $keys = explode('.', $path);
        $current = &$this->config;
        $parents = [];
        
        // Navigate to parent of target
        foreach (array_slice($keys, 0, -1) as $key) {
            if (!is_array($current) || !array_key_exists($key, $current)) {
                return; // Path doesn't exist
            }
            $parents[] = &$current;
            $current = &$current[$key];
        }
        
        $lastKey = end($keys);
        if (is_array($current) && array_key_exists($lastKey, $current)) {
            unset($current[$lastKey]);
        }
    }
    
    public function makeReadOnly(): self {
        $this->readOnly = true;
        return $this;
    }
    
    public function isReadOnly(): bool {
        return $this->readOnly;
    }
    
    public function toArray(): array {
        return $this->config;
    }
    
    public function merge(array $config): self {
        if ($this->readOnly) {
            throw new BadMethodCallException("Configuration is read-only");
        }
        
        $this->config = array_merge_recursive($this->config, $config);
        return $this;
    }
}

// Usage examples
echo "=== ArrayAccess Interface Examples ===\n\n";

// Dictionary example
$dict = new Dictionary(['name' => 'Alice', 'age' => 30]);

// Array-like access
$dict['city'] = 'New York';
$dict['country'] = 'USA';

echo "Dictionary using array notation:\n";
echo "Name: " . $dict['name'] . "\n";
echo "Age: " . $dict['age'] . "\n";
echo "City: " . $dict['city'] . "\n";

echo "Count: " . count($dict) . "\n";
echo "Has 'email': " . (isset($dict['email']) ? 'Yes' : 'No') . "\n";

echo "\nIterating through dictionary:\n";
foreach ($dict as $key => $value) {
    echo "$key: $value\n";
}

unset($dict['age']);
echo "\nAfter removing 'age':\n";
echo "Keys: " . implode(', ', $dict->keys()) . "\n";

// Matrix example
echo "\nMatrix example:\n";
$matrix = new Matrix(3, 3);

// Fill matrix with some values
for ($i = 0; $i < 3; $i++) {
    for ($j = 0; $j < 3; $j++) {
        $matrix[[$i, $j]] = ($i + 1) * ($j + 1);
    }
}

echo $matrix;

echo "Element at [1,2]: " . $matrix[[1, 2]] . "\n";
$matrix[[2, 1]] = 99;
echo "After setting [2,1] to 99:\n";
echo $matrix;

// Configuration example
echo "Configuration example:\n";
$config = new Configuration([
    'database' => [
        'host' => 'localhost',
        'port' => 3306,
        'credentials' => [
            'username' => 'root',
            'password' => 'secret'
        ]
    ],
    'debug' => true
]);

// Dot notation access
echo "DB Host: " . $config['database.host'] . "\n";
echo "DB Username: " . $config['database.credentials.username'] . "\n";
echo "Debug mode: " . ($config['debug'] ? 'Yes' : 'No') . "\n";

// Set nested value
$config['app.name'] = 'My Application';
$config['app.version'] = '1.0.0';

echo "\nApp name: " . $config['app.name'] . "\n";
echo "Has timezone setting: " . (isset($config['app.timezone']) ? 'Yes' : 'No') . "\n";

echo "\nFull configuration:\n";
print_r($config->toArray());

// Make read-only
$config->makeReadOnly();
echo "Configuration is now read-only: " . ($config->isReadOnly() ? 'Yes' : 'No') . "\n";

try {
    $config['new.setting'] = 'value';
} catch (BadMethodCallException $e) {
    echo "Error: " . $e->getMessage() . "\n";
}
```

ArrayAccess enables objects to be used with array syntax ([], isset(), unset()).  
Implement offsetExists(), offsetGet(), offsetSet(), and offsetUnset() methods.  
This is powerful for creating array-like containers, configurations with dot  
notation, matrices with multi-dimensional indexing, and other scenarios where  
array syntax improves usability and readability.  

## Exception classes

Creating custom exception classes for specific error handling scenarios.  

```php
<?php

// Base application exception
abstract class AppException extends Exception {
    protected array $context = [];
    
    public function __construct(string $message = "", int $code = 0, ?Throwable $previous = null, array $context = []) {
        parent::__construct($message, $code, $previous);
        $this->context = $context;
    }
    
    public function getContext(): array {
        return $this->context;
    }
    
    public function setContext(array $context): self {
        $this->context = $context;
        return $this;
    }
    
    public function addContext(string $key, mixed $value): self {
        $this->context[$key] = $value;
        return $this;
    }
    
    abstract public function getErrorType(): string;
    
    public function toArray(): array {
        return [
            'type' => $this->getErrorType(),
            'message' => $this->getMessage(),
            'code' => $this->getCode(),
            'file' => $this->getFile(),
            'line' => $this->getLine(),
            'context' => $this->context,
            'trace' => $this->getTraceAsString()
        ];
    }
}

class ValidationException extends AppException {
    private array $errors = [];
    
    public function __construct(string $message = "Validation failed", array $errors = [], int $code = 400) {
        parent::__construct($message, $code);
        $this->errors = $errors;
    }
    
    public function getErrorType(): string {
        return 'validation_error';
    }
    
    public function getErrors(): array {
        return $this->errors;
    }
    
    public function addError(string $field, string $message): self {
        if (!isset($this->errors[$field])) {
            $this->errors[$field] = [];
        }
        $this->errors[$field][] = $message;
        return $this;
    }
    
    public function hasErrors(): bool {
        return !empty($this->errors);
    }
    
    public function getFirstError(): ?string {
        foreach ($this->errors as $fieldErrors) {
            if (!empty($fieldErrors)) {
                return $fieldErrors[0];
            }
        }
        return null;
    }
    
    public function toArray(): array {
        return array_merge(parent::toArray(), [
            'errors' => $this->errors,
            'error_count' => count($this->errors)
        ]);
    }
}

class DatabaseException extends AppException {
    private ?string $query = null;
    private array $bindings = [];
    
    public function __construct(string $message, ?string $query = null, array $bindings = [], int $code = 500) {
        parent::__construct($message, $code);
        $this->query = $query;
        $this->bindings = $bindings;
    }
    
    public function getErrorType(): string {
        return 'database_error';
    }
    
    public function getQuery(): ?string {
        return $this->query;
    }
    
    public function getBindings(): array {
        return $this->bindings;
    }
    
    public function toArray(): array {
        return array_merge(parent::toArray(), [
            'query' => $this->query,
            'bindings' => $this->bindings
        ]);
    }
}

class AuthorizationException extends AppException {
    private string $requiredPermission;
    private array $userPermissions;
    
    public function __construct(string $requiredPermission, array $userPermissions = [], string $message = "") {
        $this->requiredPermission = $requiredPermission;
        $this->userPermissions = $userPermissions;
        
        if (empty($message)) {
            $message = "Access denied. Required permission: {$requiredPermission}";
        }
        
        parent::__construct($message, 403);
    }
    
    public function getErrorType(): string {
        return 'authorization_error';
    }
    
    public function getRequiredPermission(): string {
        return $this->requiredPermission;
    }
    
    public function getUserPermissions(): array {
        return $this->userPermissions;
    }
    
    public function toArray(): array {
        return array_merge(parent::toArray(), [
            'required_permission' => $this->requiredPermission,
            'user_permissions' => $this->userPermissions
        ]);
    }
}

class RateLimitException extends AppException {
    private int $limit;
    private int $remaining;
    private int $resetTime;
    
    public function __construct(int $limit, int $remaining = 0, int $resetTime = 0) {
        $this->limit = $limit;
        $this->remaining = $remaining;
        $this->resetTime = $resetTime ?: time() + 3600; // Default 1 hour
        
        $resetTimeFormatted = date('Y-m-d H:i:s', $this->resetTime);
        $message = "Rate limit exceeded. Limit: {$limit}, Remaining: {$remaining}, Resets at: {$resetTimeFormatted}";
        
        parent::__construct($message, 429);
    }
    
    public function getErrorType(): string {
        return 'rate_limit_error';
    }
    
    public function getLimit(): int {
        return $this->limit;
    }
    
    public function getRemaining(): int {
        return $this->remaining;
    }
    
    public function getResetTime(): int {
        return $this->resetTime;
    }
    
    public function getTimeUntilReset(): int {
        return max(0, $this->resetTime - time());
    }
    
    public function toArray(): array {
        return array_merge(parent::toArray(), [
            'limit' => $this->limit,
            'remaining' => $this->remaining,
            'reset_time' => $this->resetTime,
            'time_until_reset' => $this->getTimeUntilReset()
        ]);
    }
}

// Exception handler and logger
class ExceptionHandler {
    private array $handlers = [];
    private bool $debug = false;
    
    public function __construct(bool $debug = false) {
        $this->debug = $debug;
    }
    
    public function addHandler(string $exceptionClass, callable $handler): void {
        $this->handlers[$exceptionClass] = $handler;
    }
    
    public function handle(Throwable $exception): void {
        $exceptionClass = get_class($exception);
        
        // Look for specific handler
        if (isset($this->handlers[$exceptionClass])) {
            $this->handlers[$exceptionClass]($exception);
            return;
        }
        
        // Look for parent class handlers
        foreach ($this->handlers as $class => $handler) {
            if ($exception instanceof $class) {
                $handler($exception);
                return;
            }
        }
        
        // Default handling
        $this->defaultHandler($exception);
    }
    
    private function defaultHandler(Throwable $exception): void {
        $errorData = [
            'type' => get_class($exception),
            'message' => $exception->getMessage(),
            'code' => $exception->getCode(),
            'file' => $exception->getFile(),
            'line' => $exception->getLine()
        ];
        
        if ($this->debug) {
            $errorData['trace'] = $exception->getTraceAsString();
        }
        
        if ($exception instanceof AppException) {
            $errorData = array_merge($errorData, $exception->toArray());
        }
        
        echo "Error logged: " . json_encode($errorData, JSON_PRETTY_PRINT) . "\n";
    }
}

// Usage examples
echo "=== Exception Classes Examples ===\n\n";

$handler = new ExceptionHandler(true);

// Add specific handlers
$handler->addHandler(ValidationException::class, function(ValidationException $e) {
    echo "Validation Error Handler:\n";
    echo "Message: " . $e->getMessage() . "\n";
    echo "Errors: " . json_encode($e->getErrors()) . "\n\n";
});

$handler->addHandler(AuthorizationException::class, function(AuthorizationException $e) {
    echo "Authorization Error Handler:\n";
    echo "Required: " . $e->getRequiredPermission() . "\n";
    echo "User has: " . implode(', ', $e->getUserPermissions()) . "\n\n";
});

// Validation exception
try {
    $validation = new ValidationException("User data is invalid");
    $validation->addError('email', 'Invalid email format')
               ->addError('email', 'Email already exists')
               ->addError('password', 'Password too short')
               ->addContext('user_id', 123)
               ->addContext('ip_address', '192.168.1.1');
    
    throw $validation;
} catch (ValidationException $e) {
    $handler->handle($e);
}

// Database exception
try {
    $dbException = new DatabaseException(
        "Connection failed",
        "SELECT * FROM users WHERE id = ?",
        [123]
    );
    
    throw $dbException;
} catch (DatabaseException $e) {
    $handler->handle($e);
}

// Authorization exception
try {
    throw new AuthorizationException(
        'admin.delete_user',
        ['user.read', 'user.write']
    );
} catch (AuthorizationException $e) {
    $handler->handle($e);
}

// Rate limit exception
try {
    throw new RateLimitException(100, 0, time() + 1800); // 30 minutes
} catch (RateLimitException $e) {
    echo "Rate Limit Error:\n";
    echo "Message: " . $e->getMessage() . "\n";
    echo "Time until reset: " . $e->getTimeUntilReset() . " seconds\n\n";
}

// Exception chaining
try {
    try {
        throw new DatabaseException("Primary database unavailable");
    } catch (DatabaseException $e) {
        throw new AppException("Service temporarily unavailable", 503, $e);
    }
} catch (AppException $e) {
    echo "Chained Exception:\n";
    echo "Current: " . $e->getMessage() . "\n";
    echo "Previous: " . $e->getPrevious()->getMessage() . "\n";
}
```

Custom exception classes provide structured error handling with context and  
type-specific information. Extend base exceptions to add domain-specific  
properties and methods. Use exception chaining to preserve error context  
while transforming exceptions. This enables comprehensive error logging,  
user-friendly error messages, and proper error recovery strategies.  

## Reflection

Using reflection to inspect and manipulate classes, methods, and properties at runtime.  

```php
<?php

class ReflectionExample {
    public const VERSION = '1.0.0';
    
    private string $title;
    protected array $data = [];
    public static int $instanceCount = 0;
    
    public function __construct(string $title, array $data = []) {
        $this->title = $title;
        $this->data = $data;
        self::$instanceCount++;
    }
    
    public function getTitle(): string {
        return $this->title;
    }
    
    private function processData(): array {
        return array_map('strtoupper', $this->data);
    }
    
    protected function validateData(array $data): bool {
        return !empty($data);
    }
    
    public static function getInstanceCount(): int {
        return self::$instanceCount;
    }
    
    public function getData(): array {
        return $this->data;
    }
    
    public function setData(array $data): void {
        if ($this->validateData($data)) {
            $this->data = $data;
        }
    }
}

class ReflectionAnalyzer {
    private ReflectionClass $reflection;
    
    public function __construct(string $className) {
        $this->reflection = new ReflectionClass($className);
    }
    
    public function getClassInfo(): array {
        return [
            'name' => $this->reflection->getName(),
            'short_name' => $this->reflection->getShortName(),
            'namespace' => $this->reflection->getNamespaceName(),
            'filename' => $this->reflection->getFileName(),
            'is_abstract' => $this->reflection->isAbstract(),
            'is_final' => $this->reflection->isFinal(),
            'is_interface' => $this->reflection->isInterface(),
            'is_trait' => $this->reflection->isTrait(),
            'parent_class' => $this->reflection->getParentClass()?->getName(),
            'interfaces' => array_keys($this->reflection->getInterfaces()),
            'traits' => array_keys($this->reflection->getTraits())
        ];
    }
    
    public function getConstants(): array {
        return $this->reflection->getConstants();
    }
    
    public function getProperties(): array {
        $properties = [];
        
        foreach ($this->reflection->getProperties() as $property) {
            $properties[] = [
                'name' => $property->getName(),
                'visibility' => $this->getVisibility($property),
                'is_static' => $property->isStatic(),
                'is_readonly' => $property->isReadOnly(),
                'type' => $property->getType()?->getName(),
                'default_value' => $this->getDefaultValue($property),
                'doc_comment' => $property->getDocComment()
            ];
        }
        
        return $properties;
    }
    
    public function getMethods(): array {
        $methods = [];
        
        foreach ($this->reflection->getMethods() as $method) {
            $parameters = [];
            foreach ($method->getParameters() as $param) {
                $parameters[] = [
                    'name' => $param->getName(),
                    'type' => $param->getType()?->getName(),
                    'is_optional' => $param->isOptional(),
                    'default_value' => $param->isDefaultValueAvailable() ? 
                        $param->getDefaultValue() : null,
                    'allows_null' => $param->allowsNull()
                ];
            }
            
            $methods[] = [
                'name' => $method->getName(),
                'visibility' => $this->getVisibility($method),
                'is_static' => $method->isStatic(),
                'is_abstract' => $method->isAbstract(),
                'is_final' => $method->isFinal(),
                'return_type' => $method->getReturnType()?->getName(),
                'parameters' => $parameters,
                'doc_comment' => $method->getDocComment()
            ];
        }
        
        return $methods;
    }
    
    private function getVisibility(ReflectionMethod|ReflectionProperty $member): string {
        if ($member->isPublic()) return 'public';
        if ($member->isProtected()) return 'protected';
        if ($member->isPrivate()) return 'private';
        return 'unknown';
    }
    
    private function getDefaultValue(ReflectionProperty $property): mixed {
        $defaults = $this->reflection->getDefaultProperties();
        return $defaults[$property->getName()] ?? null;
    }
    
    public function createInstance(array $args = []): object {
        return $this->reflection->newInstanceArgs($args);
    }
    
    public function invokeMethod(object $instance, string $methodName, array $args = []): mixed {
        $method = $this->reflection->getMethod($methodName);
        $method->setAccessible(true); // Allow access to private/protected methods
        return $method->invokeArgs($instance, $args);
    }
    
    public function getPropertyValue(object $instance, string $propertyName): mixed {
        $property = $this->reflection->getProperty($propertyName);
        $property->setAccessible(true);
        return $property->getValue($instance);
    }
    
    public function setPropertyValue(object $instance, string $propertyName, mixed $value): void {
        $property = $this->reflection->getProperty($propertyName);
        $property->setAccessible(true);
        $property->setValue($instance, $value);
    }
}

class ObjectCloner {
    public static function deepClone(object $object): object {
        $reflection = new ReflectionClass($object);
        $clone = $reflection->newInstanceWithoutConstructor();
        
        foreach ($reflection->getProperties() as $property) {
            $property->setAccessible(true);
            $value = $property->getValue($object);
            
            if (is_object($value)) {
                $value = self::deepClone($value);
            } elseif (is_array($value)) {
                $value = self::deepCloneArray($value);
            }
            
            $property->setValue($clone, $value);
        }
        
        return $clone;
    }
    
    private static function deepCloneArray(array $array): array {
        $result = [];
        
        foreach ($array as $key => $value) {
            if (is_object($value)) {
                $result[$key] = self::deepClone($value);
            } elseif (is_array($value)) {
                $result[$key] = self::deepCloneArray($value);
            } else {
                $result[$key] = $value;
            }
        }
        
        return $result;
    }
}

class AttributeReader {
    public static function getClassAttributes(string $className): array {
        $reflection = new ReflectionClass($className);
        $attributes = [];
        
        foreach ($reflection->getAttributes() as $attribute) {
            $attributes[] = [
                'name' => $attribute->getName(),
                'arguments' => $attribute->getArguments(),
                'instance' => $attribute->newInstance()
            ];
        }
        
        return $attributes;
    }
    
    public static function getMethodAttributes(string $className, string $methodName): array {
        $reflection = new ReflectionMethod($className, $methodName);
        $attributes = [];
        
        foreach ($reflection->getAttributes() as $attribute) {
            $attributes[] = [
                'name' => $attribute->getName(),
                'arguments' => $attribute->getArguments()
            ];
        }
        
        return $attributes;
    }
}

// Usage examples
echo "=== Reflection Examples ===\n\n";

$analyzer = new ReflectionAnalyzer(ReflectionExample::class);

// Class information
echo "Class Information:\n";
print_r($analyzer->getClassInfo());

echo "\nConstants:\n";
print_r($analyzer->getConstants());

echo "\nProperties:\n";
foreach ($analyzer->getProperties() as $property) {
    echo "- {$property['visibility']} " . 
         ($property['is_static'] ? 'static ' : '') . 
         ($property['type'] ? $property['type'] . ' ' : '') .
         "\${$property['name']}" . 
         ($property['default_value'] !== null ? " = " . var_export($property['default_value'], true) : '') . "\n";
}

echo "\nMethods:\n";
foreach ($analyzer->getMethods() as $method) {
    $params = array_map(function($p) {
        return ($p['type'] ? $p['type'] . ' ' : '') . 
               '$' . $p['name'] . 
               ($p['is_optional'] ? ' = ' . var_export($p['default_value'], true) : '');
    }, $method['parameters']);
    
    echo "- {$method['visibility']} " . 
         ($method['is_static'] ? 'static ' : '') . 
         ($method['return_type'] ? $method['return_type'] . ' ' : '') .
         "{$method['name']}(" . implode(', ', $params) . ")\n";
}

// Create instance and manipulate
$instance = $analyzer->createInstance(['Test Title', ['a', 'b', 'c']]);

echo "\nInstance manipulation:\n";
echo "Title: " . $analyzer->getPropertyValue($instance, 'title') . "\n";
echo "Data: " . json_encode($analyzer->getPropertyValue($instance, 'data')) . "\n";

// Call private method
$processedData = $analyzer->invokeMethod($instance, 'processData');
echo "Processed data: " . json_encode($processedData) . "\n";

// Modify private property
$analyzer->setPropertyValue($instance, 'title', 'Modified Title');
echo "Modified title: " . $analyzer->getPropertyValue($instance, 'title') . "\n";

// Check static property
echo "Instance count: " . $analyzer->getPropertyValue(null, 'instanceCount') . "\n";

// Object cloning
echo "\nDeep cloning example:\n";
$original = new ReflectionExample('Original', ['nested' => new stdClass()]);
$clone = ObjectCloner::deepClone($original);

echo "Original title: " . $analyzer->getPropertyValue($original, 'title') . "\n";
echo "Clone title: " . $analyzer->getPropertyValue($clone, 'title') . "\n";

// Modify clone
$analyzer->setPropertyValue($clone, 'title', 'Cloned Title');
echo "After modification:\n";
echo "Original title: " . $analyzer->getPropertyValue($original, 'title') . "\n";
echo "Clone title: " . $analyzer->getPropertyValue($clone, 'title') . "\n";
```

Reflection provides runtime introspection and manipulation of classes,  
methods, and properties. Use ReflectionClass for class analysis,  
ReflectionMethod for method information, and ReflectionProperty for  
property access. Reflection enables building frameworks, dependency  
injection containers, serializers, and debugging tools that work  
with any class structure.  

## Attribute usage

Using PHP 8+ attributes for metadata and declarative programming.  

```php
<?php

// Define custom attributes
#[Attribute(Attribute::TARGET_CLASS)]
class Entity {
    public function __construct(
        public string $tableName,
        public string $primaryKey = 'id'
    ) {}
}

#[Attribute(Attribute::TARGET_PROPERTY)]
class Column {
    public function __construct(
        public string $name,
        public string $type = 'string',
        public bool $nullable = false,
        public mixed $default = null
    ) {}
}

#[Attribute(Attribute::TARGET_METHOD)]
class Route {
    public function __construct(
        public string $path,
        public string $method = 'GET',
        public array $middleware = []
    ) {}
}

#[Attribute(Attribute::TARGET_METHOD | Attribute::IS_REPEATABLE)]
class Permission {
    public function __construct(public string $permission) {}
}

#[Attribute(Attribute::TARGET_METHOD)]
class Cache {
    public function __construct(
        public int $ttl = 3600,
        public string $key = '',
        public array $tags = []
    ) {}
}

#[Attribute(Attribute::TARGET_PROPERTY)]
class Validate {
    public function __construct(
        public array $rules = [],
        public string $message = ''
    ) {}
}

// Example entity with attributes
#[Entity(tableName: 'users', primaryKey: 'user_id')]
class User {
    #[Column(name: 'user_id', type: 'integer')]
    private int $id;
    
    #[Column(name: 'username', type: 'string')]
    #[Validate(rules: ['required', 'min:3', 'max:50'], message: 'Username must be 3-50 characters')]
    private string $username;
    
    #[Column(name: 'email', type: 'string')]
    #[Validate(rules: ['required', 'email'], message: 'Valid email is required')]
    private string $email;
    
    #[Column(name: 'created_at', type: 'datetime', default: 'CURRENT_TIMESTAMP')]
    private DateTime $createdAt;
    
    #[Column(name: 'is_active', type: 'boolean', default: true)]
    private bool $isActive = true;
    
    public function __construct(string $username, string $email) {
        $this->username = $username;
        $this->email = $email;
        $this->createdAt = new DateTime();
    }
    
    #[Route(path: '/users/{id}', method: 'GET')]
    #[Cache(ttl: 1800, tags: ['users'])]
    public function show(int $id): array {
        return ['id' => $id, 'username' => $this->username];
    }
    
    #[Route(path: '/users', method: 'POST')]
    #[Permission('user.create')]
    public function create(array $data): self {
        return new self($data['username'], $data['email']);
    }
    
    #[Route(path: '/users/{id}', method: 'PUT')]
    #[Permission('user.edit')]
    #[Permission('user.edit.own')]
    public function update(int $id, array $data): bool {
        // Update logic here
        return true;
    }
    
    #[Route(path: '/users/{id}', method: 'DELETE')]
    #[Permission('user.delete')]
    #[Cache(ttl: 0)] // Disable cache
    public function delete(int $id): bool {
        // Delete logic here
        return true;
    }
    
    public function getUsername(): string {
        return $this->username;
    }
    
    public function getEmail(): string {
        return $this->email;
    }
}

// Attribute processor classes
class EntityMapper {
    public static function getTableInfo(string $className): array {
        $reflection = new ReflectionClass($className);
        $entityAttributes = $reflection->getAttributes(Entity::class);
        
        if (empty($entityAttributes)) {
            throw new InvalidArgumentException("Class {$className} is not an entity");
        }
        
        $entity = $entityAttributes[0]->newInstance();
        
        return [
            'table_name' => $entity->tableName,
            'primary_key' => $entity->primaryKey,
            'columns' => self::getColumns($reflection)
        ];
    }
    
    private static function getColumns(ReflectionClass $reflection): array {
        $columns = [];
        
        foreach ($reflection->getProperties() as $property) {
            $columnAttributes = $property->getAttributes(Column::class);
            
            if (!empty($columnAttributes)) {
                $column = $columnAttributes[0]->newInstance();
                $columns[$property->getName()] = [
                    'name' => $column->name,
                    'type' => $column->type,
                    'nullable' => $column->nullable,
                    'default' => $column->default
                ];
            }
        }
        
        return $columns;
    }
    
    public static function generateCreateTableSQL(string $className): string {
        $info = self::getTableInfo($className);
        $sql = "CREATE TABLE {$info['table_name']} (\n";
        
        $columnDefs = [];
        foreach ($info['columns'] as $property => $column) {
            $def = "    {$column['name']} ";
            
            $def .= match($column['type']) {
                'integer' => 'INT',
                'string' => 'VARCHAR(255)',
                'datetime' => 'DATETIME',
                'boolean' => 'TINYINT(1)',
                default => 'TEXT'
            };
            
            if (!$column['nullable']) {
                $def .= ' NOT NULL';
            }
            
            if ($column['default'] !== null) {
                if ($column['default'] === 'CURRENT_TIMESTAMP') {
                    $def .= ' DEFAULT CURRENT_TIMESTAMP';
                } else {
                    $def .= ' DEFAULT ' . (is_string($column['default']) ? "'{$column['default']}'" : $column['default']);
                }
            }
            
            $columnDefs[] = $def;
        }
        
        $sql .= implode(",\n", $columnDefs);
        $sql .= ",\n    PRIMARY KEY ({$info['primary_key']})\n";
        $sql .= ");";
        
        return $sql;
    }
}

class RouteCollector {
    public static function getRoutes(string $className): array {
        $reflection = new ReflectionClass($className);
        $routes = [];
        
        foreach ($reflection->getMethods() as $method) {
            $routeAttributes = $method->getAttributes(Route::class);
            
            foreach ($routeAttributes as $routeAttribute) {
                $route = $routeAttribute->newInstance();
                
                // Get permissions
                $permissions = [];
                foreach ($method->getAttributes(Permission::class) as $permAttr) {
                    $permissions[] = $permAttr->newInstance()->permission;
                }
                
                // Get cache info
                $cacheInfo = null;
                $cacheAttributes = $method->getAttributes(Cache::class);
                if (!empty($cacheAttributes)) {
                    $cache = $cacheAttributes[0]->newInstance();
                    $cacheInfo = [
                        'ttl' => $cache->ttl,
                        'key' => $cache->key,
                        'tags' => $cache->tags
                    ];
                }
                
                $routes[] = [
                    'path' => $route->path,
                    'method' => $route->method,
                    'handler' => $className . '::' . $method->getName(),
                    'middleware' => $route->middleware,
                    'permissions' => $permissions,
                    'cache' => $cacheInfo
                ];
            }
        }
        
        return $routes;
    }
}

class Validator {
    public static function validateObject(object $object): array {
        $reflection = new ReflectionClass($object);
        $errors = [];
        
        foreach ($reflection->getProperties() as $property) {
            $validateAttributes = $property->getAttributes(Validate::class);
            
            if (!empty($validateAttributes)) {
                $validate = $validateAttributes[0]->newInstance();
                $property->setAccessible(true);
                $value = $property->getValue($object);
                
                $fieldErrors = self::validateValue($value, $validate->rules);
                if (!empty($fieldErrors)) {
                    $errors[$property->getName()] = $validate->message ?: $fieldErrors;
                }
            }
        }
        
        return $errors;
    }
    
    private static function validateValue(mixed $value, array $rules): array {
        $errors = [];
        
        foreach ($rules as $rule) {
            if ($rule === 'required' && empty($value)) {
                $errors[] = 'Field is required';
            } elseif (str_starts_with($rule, 'min:')) {
                $min = (int) substr($rule, 4);
                if (is_string($value) && strlen($value) < $min) {
                    $errors[] = "Minimum length is {$min}";
                }
            } elseif (str_starts_with($rule, 'max:')) {
                $max = (int) substr($rule, 4);
                if (is_string($value) && strlen($value) > $max) {
                    $errors[] = "Maximum length is {$max}";
                }
            } elseif ($rule === 'email' && !filter_var($value, FILTER_VALIDATE_EMAIL)) {
                $errors[] = 'Invalid email format';
            }
        }
        
        return $errors;
    }
}

// Usage examples
echo "=== Attribute Usage Examples ===\n\n";

// Entity mapping
$tableInfo = EntityMapper::getTableInfo(User::class);
echo "Table Information:\n";
print_r($tableInfo);

echo "\nGenerated SQL:\n";
echo EntityMapper::generateCreateTableSQL(User::class) . "\n\n";

// Route collection
$routes = RouteCollector::getRoutes(User::class);
echo "Routes:\n";
foreach ($routes as $route) {
    echo "- {$route['method']} {$route['path']} -> {$route['handler']}\n";
    if (!empty($route['permissions'])) {
        echo "  Permissions: " . implode(', ', $route['permissions']) . "\n";
    }
    if ($route['cache']) {
        echo "  Cache: TTL={$route['cache']['ttl']}s";
        if (!empty($route['cache']['tags'])) {
            echo ", Tags=" . implode(',', $route['cache']['tags']);
        }
        echo "\n";
    }
}

// Validation
echo "\nValidation Examples:\n";

// Valid user
$validUser = new User('johndoe', 'john@example.com');
$validationErrors = Validator::validateObject($validUser);
echo "Valid user errors: " . (empty($validationErrors) ? 'None' : json_encode($validationErrors)) . "\n";

// Create invalid user by reflection (bypassing constructor validation)
$reflection = new ReflectionClass(User::class);
$invalidUser = $reflection->newInstanceWithoutConstructor();

$usernameProperty = $reflection->getProperty('username');
$usernameProperty->setAccessible(true);
$usernameProperty->setValue($invalidUser, 'ab'); // Too short

$emailProperty = $reflection->getProperty('email');
$emailProperty->setAccessible(true);
$emailProperty->setValue($invalidUser, 'invalid-email'); // Invalid format

$validationErrors = Validator::validateObject($invalidUser);
echo "Invalid user errors:\n";
print_r($validationErrors);

// Attribute inspection
echo "\nAttribute Details:\n";
$userReflection = new ReflectionClass(User::class);

// Class attributes
foreach ($userReflection->getAttributes() as $attribute) {
    echo "Class attribute: " . $attribute->getName() . "\n";
    print_r($attribute->getArguments());
}

// Method attributes
$showMethod = $userReflection->getMethod('show');
echo "\nMethod 'show' attributes:\n";
foreach ($showMethod->getAttributes() as $attribute) {
    echo "- " . $attribute->getName() . "\n";
    print_r($attribute->getArguments());
}
```

Attributes provide metadata that can be read at runtime using reflection.  
They enable declarative programming where configuration is embedded directly  
in code. Use attributes for dependency injection, ORM mapping, validation  
rules, routing configuration, and aspect-oriented programming. They reduce  
boilerplate code and keep configuration close to the code it affects.  

## Final classes and methods

Using final keyword to prevent inheritance and method overriding.  

```php
<?php

// Final class - cannot be extended
final class SecurityToken {
    private string $token;
    private DateTime $expiresAt;
    private array $claims = [];
    
    public function __construct(string $token, DateTime $expiresAt, array $claims = []) {
        if (empty($token)) {
            throw new InvalidArgumentException("Token cannot be empty");
        }
        
        $this->token = $token;
        $this->expiresAt = $expiresAt;
        $this->claims = $claims;
    }
    
    public function getToken(): string {
        return $this->token;
    }
    
    public function isExpired(): bool {
        return new DateTime() > $this->expiresAt;
    }
    
    public function getClaims(): array {
        return $this->claims;
    }
    
    public function getClaim(string $key): mixed {
        return $this->claims[$key] ?? null;
    }
    
    public function getExpirationTime(): DateTime {
        return $this->expiresAt;
    }
    
    // This method cannot be overridden due to final class
    public function isValid(): bool {
        return !$this->isExpired() && !empty($this->token);
    }
}

// Regular class with final methods
class PaymentProcessor {
    protected float $balance = 0.0;
    protected array $transactions = [];
    
    public function __construct(float $initialBalance = 0.0) {
        $this->balance = $initialBalance;
    }
    
    // Final method - cannot be overridden by subclasses
    final public function processPayment(float $amount, string $currency = 'USD'): string {
        if ($amount <= 0) {
            throw new InvalidArgumentException("Amount must be positive");
        }
        
        $transactionId = $this->generateTransactionId();
        
        // This is the core payment processing logic that must not be modified
        $this->validatePayment($amount, $currency);
        $this->executePayment($amount, $currency);
        $this->recordTransaction($transactionId, $amount, $currency);
        
        return $transactionId;
    }
    
    // Final method for critical security operations
    final protected function generateTransactionId(): string {
        return uniqid('txn_', true) . '_' . hash('sha256', random_bytes(32));
    }
    
    // Final method to ensure transaction integrity
    final protected function recordTransaction(string $id, float $amount, string $currency): void {
        $this->transactions[] = [
            'id' => $id,
            'amount' => $amount,
            'currency' => $currency,
            'timestamp' => new DateTime(),
            'status' => 'completed'
        ];
    }
    
    // These methods can be overridden by subclasses
    protected function validatePayment(float $amount, string $currency): void {
        if ($amount > $this->balance) {
            throw new RuntimeException("Insufficient balance");
        }
    }
    
    protected function executePayment(float $amount, string $currency): void {
        $this->balance -= $amount;
        echo "Processed payment of {$amount} {$currency}\n";
    }
    
    public function getBalance(): float {
        return $this->balance;
    }
    
    public function getTransactions(): array {
        return $this->transactions;
    }
    
    public function deposit(float $amount): void {
        if ($amount > 0) {
            $this->balance += $amount;
        }
    }
}

// Subclass that extends PaymentProcessor
class CreditCardProcessor extends PaymentProcessor {
    private string $cardNumber;
    private string $cvv;
    
    public function __construct(float $initialBalance, string $cardNumber, string $cvv) {
        parent::__construct($initialBalance);
        $this->cardNumber = $this->maskCardNumber($cardNumber);
        $this->cvv = $cvv;
    }
    
    // Override validation with card-specific logic
    protected function validatePayment(float $amount, string $currency): void {
        // Call parent validation first
        parent::validatePayment($amount, $currency);
        
        // Additional card validation
        if (strlen($this->cvv) !== 3) {
            throw new RuntimeException("Invalid CVV");
        }
        
        if (!$this->validateCardNumber()) {
            throw new RuntimeException("Invalid card number");
        }
    }
    
    // Override execution with card-specific processing
    protected function executePayment(float $amount, string $currency): void {
        echo "Processing card payment: {$this->cardNumber}\n";
        echo "Contacting card network...\n";
        
        // Simulate network delay
        usleep(100000); // 0.1 seconds
        
        parent::executePayment($amount, $currency);
        echo "Card payment authorized\n";
    }
    
    private function maskCardNumber(string $cardNumber): string {
        $cleaned = preg_replace('/\D/', '', $cardNumber);
        return str_repeat('*', strlen($cleaned) - 4) . substr($cleaned, -4);
    }
    
    private function validateCardNumber(): bool {
        // Simplified validation - just check if it's not empty
        return !empty($this->cardNumber);
    }
    
    public function getCardNumber(): string {
        return $this->cardNumber;
    }
}

// Abstract base class with final methods
abstract class DatabaseConnection {
    protected string $host;
    protected string $database;
    protected bool $connected = false;
    
    public function __construct(string $host, string $database) {
        $this->host = $host;
        $this->database = $database;
    }
    
    // Final method - connection logic must be consistent
    final public function connect(): bool {
        if ($this->connected) {
            return true;
        }
        
        try {
            $this->beforeConnect();
            $this->establishConnection();
            $this->afterConnect();
            $this->connected = true;
            
            echo "Connected to {$this->host}:{$this->database}\n";
            return true;
        } catch (Exception $e) {
            echo "Connection failed: " . $e->getMessage() . "\n";
            return false;
        }
    }
    
    // Final method - disconnection must be handled consistently
    final public function disconnect(): void {
        if ($this->connected) {
            $this->closeConnection();
            $this->connected = false;
            echo "Disconnected from {$this->host}:{$this->database}\n";
        }
    }
    
    // Abstract methods that must be implemented
    abstract protected function establishConnection(): void;
    abstract protected function closeConnection(): void;
    abstract public function query(string $sql): array;
    
    // Hook methods that can be overridden
    protected function beforeConnect(): void {}
    protected function afterConnect(): void {}
    
    // Final getter for connection status
    final public function isConnected(): bool {
        return $this->connected;
    }
}

// Concrete implementation
class MySQLConnection extends DatabaseConnection {
    private ?mysqli $connection = null;
    
    protected function establishConnection(): void {
        // Simulate MySQL connection
        $this->connection = new stdClass(); // Mock connection
        if (!$this->connection) {
            throw new RuntimeException("Could not connect to MySQL");
        }
    }
    
    protected function closeConnection(): void {
        if ($this->connection) {
            // $this->connection->close(); // Would close real connection
            $this->connection = null;
        }
    }
    
    public function query(string $sql): array {
        if (!$this->connected) {
            throw new RuntimeException("Not connected to database");
        }
        
        echo "Executing MySQL query: {$sql}\n";
        return []; // Mock result
    }
    
    protected function beforeConnect(): void {
        echo "Preparing MySQL connection...\n";
    }
    
    protected function afterConnect(): void {
        echo "MySQL connection established\n";
        // Set connection options, charset, etc.
    }
}

// Usage examples
echo "=== Final Classes and Methods Examples ===\n\n";

// Final class example
$token = new SecurityToken(
    'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9',
    new DateTime('+1 hour'),
    ['user_id' => 123, 'role' => 'admin']
);

echo "Token valid: " . ($token->isValid() ? 'Yes' : 'No') . "\n";
echo "User role: " . $token->getClaim('role') . "\n";
echo "Expires at: " . $token->getExpirationTime()->format('Y-m-d H:i:s') . "\n\n";

// Try to extend final class (this would cause a fatal error if uncommented)
/*
class ExtendedSecurityToken extends SecurityToken {
    // Fatal error: Class ExtendedSecurityToken cannot extend final class SecurityToken
}
*/

// Payment processor example
$processor = new PaymentProcessor(1000.0);
$processor->deposit(500.0);

echo "Initial balance: $" . $processor->getBalance() . "\n";

$transactionId = $processor->processPayment(250.0, 'USD');
echo "Transaction ID: {$transactionId}\n";
echo "Remaining balance: $" . $processor->getBalance() . "\n\n";

// Credit card processor
$cardProcessor = new CreditCardProcessor(2000.0, '4111111111111111', '123');
echo "Card processor balance: $" . $cardProcessor->getBalance() . "\n";
echo "Card number: " . $cardProcessor->getCardNumber() . "\n";

$cardTransactionId = $cardProcessor->processPayment(100.0, 'USD');
echo "Card transaction ID: {$cardTransactionId}\n\n";

// Try to override final method (this would cause a fatal error if uncommented)
/*
class BadPaymentProcessor extends PaymentProcessor {
    // Fatal error: Cannot override final method PaymentProcessor::processPayment()
    public function processPayment(float $amount, string $currency = 'USD'): string {
        return 'hacked';
    }
}
*/

// Database connection example
$mysql = new MySQLConnection('localhost', 'myapp');

echo "Connection status: " . ($mysql->isConnected() ? 'Connected' : 'Disconnected') . "\n";
$mysql->connect();
echo "Connection status: " . ($mysql->isConnected() ? 'Connected' : 'Disconnected') . "\n";

if ($mysql->isConnected()) {
    $mysql->query("SELECT * FROM users");
}

$mysql->disconnect();
echo "Connection status: " . ($mysql->isConnected() ? 'Connected' : 'Disconnected') . "\n";
```

The `final` keyword prevents class inheritance and method overriding, ensuring  
critical functionality remains unchanged. Use final classes for value objects,  
security-sensitive classes, or when the class design is complete. Use final  
methods for core algorithms, security operations, or workflow steps that must  
remain consistent across inheritance hierarchies.  

## Object comparison

Comparing objects using different methods and implementing custom comparison logic.  

```php
<?php

class Product {
    public function __construct(
        private int $id,
        private string $name,
        private float $price,
        private string $category,
        private DateTime $createdAt
    ) {}
    
    public function getId(): int {
        return $this->id;
    }
    
    public function getName(): string {
        return $this->name;
    }
    
    public function getPrice(): float {
        return $this->price;
    }
    
    public function getCategory(): string {
        return $this->category;
    }
    
    public function getCreatedAt(): DateTime {
        return $this->createdAt;
    }
    
    // Custom equality comparison
    public function equals(Product $other): bool {
        return $this->id === $other->id &&
               $this->name === $other->name &&
               $this->price === $other->price &&
               $this->category === $other->category;
    }
    
    // Compare by specific attribute
    public function isSameProduct(Product $other): bool {
        return $this->id === $other->id;
    }
    
    public function hasSamePrice(Product $other): bool {
        return abs($this->price - $other->price) < 0.01; // Handle float precision
    }
    
    public function isInSameCategory(Product $other): bool {
        return strcasecmp($this->category, $other->category) === 0;
    }
    
    public function __toString(): string {
        return "{$this->name} ({$this->category}) - \${$this->price}";
    }
}

class Person {
    public function __construct(
        private string $firstName,
        private string $lastName,
        private int $age,
        private string $email
    ) {}
    
    public function getFirstName(): string {
        return $this->firstName;
    }
    
    public function getLastName(): string {
        return $this->lastName;
    }
    
    public function getFullName(): string {
        return $this->firstName . ' ' . $this->lastName;
    }
    
    public function getAge(): int {
        return $this->age;
    }
    
    public function getEmail(): string {
        return $this->email;
    }
    
    // Identity comparison - same person
    public function isSamePerson(Person $other): bool {
        return $this->email === $other->email; // Using email as unique identifier
    }
    
    // Name comparison
    public function hasSameName(Person $other): bool {
        return strcasecmp($this->firstName, $other->firstName) === 0 &&
               strcasecmp($this->lastName, $other->lastName) === 0;
    }
    
    // Age comparison
    public function isSameAge(Person $other): bool {
        return $this->age === $other->age;
    }
    
    public function isOlderThan(Person $other): bool {
        return $this->age > $other->age;
    }
    
    public function isYoungerThan(Person $other): bool {
        return $this->age < $other->age;
    }
    
    public function __toString(): string {
        return "{$this->getFullName()} ({$this->age} years old)";
    }
}

// Comparable interface for custom comparison
interface ComparableInterface {
    public function compareTo(object $other): int;
}

class Score implements ComparableInterface {
    public function __construct(
        private string $playerName,
        private int $points,
        private DateTime $achievedAt
    ) {}
    
    public function getPlayerName(): string {
        return $this->playerName;
    }
    
    public function getPoints(): int {
        return $this->points;
    }
    
    public function getAchievedAt(): DateTime {
        return $this->achievedAt;
    }
    
    // Implement comparison (returns -1, 0, or 1)
    public function compareTo(object $other): int {
        if (!$other instanceof Score) {
            throw new InvalidArgumentException("Can only compare with other Score objects");
        }
        
        // Primary comparison: points (descending - higher is better)
        if ($this->points !== $other->points) {
            return $other->points <=> $this->points; // Reverse for descending
        }
        
        // Secondary comparison: time achieved (ascending - earlier is better)
        return $this->achievedAt <=> $other->achievedAt;
    }
    
    public function isHigherThan(Score $other): bool {
        return $this->compareTo($other) < 0; // Less than 0 means this is "higher"
    }
    
    public function isLowerThan(Score $other): bool {
        return $this->compareTo($other) > 0;
    }
    
    public function isEqualTo(Score $other): bool {
        return $this->compareTo($other) === 0;
    }
    
    public function __toString(): string {
        return "{$this->playerName}: {$this->points} points ({$this->achievedAt->format('Y-m-d H:i:s')})";
    }
}

class ObjectComparator {
    // Compare objects using various methods
    public static function compareObjects(object $obj1, object $obj2): array {
        $results = [];
        
        // Reference comparison (===)
        $results['reference_equal'] = $obj1 === $obj2;
        
        // Loose comparison (==)
        $results['loose_equal'] = $obj1 == $obj2;
        
        // Same class
        $results['same_class'] = get_class($obj1) === get_class($obj2);
        
        // Instance of comparison
        $results['instance_compatible'] = $obj1 instanceof get_class($obj2);
        
        // Serialization comparison (deep equality)
        $results['serialization_equal'] = serialize($obj1) === serialize($obj2);
        
        // Property-by-property comparison
        if (get_class($obj1) === get_class($obj2)) {
            $results['properties_equal'] = self::compareProperties($obj1, $obj2);
        } else {
            $results['properties_equal'] = false;
        }
        
        return $results;
    }
    
    private static function compareProperties(object $obj1, object $obj2): bool {
        $reflection = new ReflectionClass($obj1);
        
        foreach ($reflection->getProperties() as $property) {
            $property->setAccessible(true);
            $value1 = $property->getValue($obj1);
            $value2 = $property->getValue($obj2);
            
            if ($value1 !== $value2) {
                return false;
            }
        }
        
        return true;
    }
    
    // Sort array of objects using custom comparison
    public static function sortObjects(array $objects, callable $compareFn): array {
        usort($objects, $compareFn);
        return $objects;
    }
    
    // Sort comparable objects
    public static function sortComparableObjects(array $objects): array {
        return self::sortObjects($objects, fn($a, $b) => $a->compareTo($b));
    }
    
    // Find unique objects
    public static function getUniqueObjects(array $objects, callable $equalityFn): array {
        $unique = [];
        
        foreach ($objects as $object) {
            $isUnique = true;
            
            foreach ($unique as $uniqueObject) {
                if ($equalityFn($object, $uniqueObject)) {
                    $isUnique = false;
                    break;
                }
            }
            
            if ($isUnique) {
                $unique[] = $object;
            }
        }
        
        return $unique;
    }
}

// Usage examples
echo "=== Object Comparison Examples ===\n\n";

// Product comparison
$product1 = new Product(1, 'Laptop', 999.99, 'Electronics', new DateTime('2024-01-15'));
$product2 = new Product(1, 'Laptop', 999.99, 'Electronics', new DateTime('2024-01-15'));
$product3 = new Product(2, 'Mouse', 29.99, 'Electronics', new DateTime('2024-01-16'));
$product4 = $product1; // Same reference

echo "Product Comparisons:\n";
echo "Product 1: {$product1}\n";
echo "Product 2: {$product2}\n";
echo "Product 3: {$product3}\n\n";

// Different comparison methods
$comparison = ObjectComparator::compareObjects($product1, $product2);
echo "Product1 vs Product2 (same data, different objects):\n";
foreach ($comparison as $method => $result) {
    echo "- {$method}: " . ($result ? 'true' : 'false') . "\n";
}

echo "\nProduct1 vs Product4 (same reference):\n";
$comparison = ObjectComparator::compareObjects($product1, $product4);
foreach ($comparison as $method => $result) {
    echo "- {$method}: " . ($result ? 'true' : 'false') . "\n";
}

// Custom equality methods
echo "\nCustom Product Comparisons:\n";
echo "Product1 equals Product2: " . ($product1->equals($product2) ? 'Yes' : 'No') . "\n";
echo "Product1 same as Product3: " . ($product1->isSameProduct($product3) ? 'Yes' : 'No') . "\n";
echo "Product1 same category as Product3: " . ($product1->isInSameCategory($product3) ? 'Yes' : 'No') . "\n";

// Person comparison
echo "\nPerson Comparisons:\n";
$person1 = new Person('John', 'Doe', 30, 'john.doe@example.com');
$person2 = new Person('John', 'Doe', 32, 'john.doe@work.com');
$person3 = new Person('Jane', 'Smith', 30, 'jane.smith@example.com');

echo "Person 1: {$person1}\n";
echo "Person 2: {$person2}\n";
echo "Person 3: {$person3}\n\n";

echo "Person1 same person as Person2: " . ($person1->isSamePerson($person2) ? 'Yes' : 'No') . "\n";
echo "Person1 same name as Person2: " . ($person1->hasSameName($person2) ? 'Yes' : 'No') . "\n";
echo "Person1 same age as Person3: " . ($person1->isSameAge($person3) ? 'Yes' : 'No') . "\n";
echo "Person2 older than Person1: " . ($person2->isOlderThan($person1) ? 'Yes' : 'No') . "\n";

// Score comparison with ComparableInterface
echo "\nScore Comparisons:\n";
$scores = [
    new Score('Alice', 1200, new DateTime('2024-01-15 10:30:00')),
    new Score('Bob', 1500, new DateTime('2024-01-15 11:00:00')),
    new Score('Charlie', 1200, new DateTime('2024-01-15 09:45:00')),
    new Score('Diana', 1800, new DateTime('2024-01-15 12:15:00')),
    new Score('Eve', 1500, new DateTime('2024-01-15 10:45:00'))
];

echo "Original scores:\n";
foreach ($scores as $score) {
    echo "- {$score}\n";
}

$sortedScores = ObjectComparator::sortComparableObjects($scores);
echo "\nSorted scores (highest first, then earliest time):\n";
foreach ($sortedScores as $score) {
    echo "- {$score}\n";
}

// Custom comparison examples
echo "\nScore comparison examples:\n";
echo "Alice higher than Bob: " . ($scores[0]->isHigherThan($scores[1]) ? 'Yes' : 'No') . "\n";
echo "Charlie higher than Alice: " . ($scores[2]->isHigherThan($scores[0]) ? 'Yes' : 'No') . "\n";
echo "Bob equal to Eve: " . ($scores[1]->isEqualTo($scores[4]) ? 'Yes' : 'No') . "\n";

// Unique objects example
echo "\nUnique Products (by category):\n";
$products = [
    new Product(1, 'Laptop', 999.99, 'Electronics', new DateTime()),
    new Product(2, 'Mouse', 29.99, 'Electronics', new DateTime()),
    new Product(3, 'Desk', 299.99, 'Furniture', new DateTime()),
    new Product(4, 'Chair', 199.99, 'Furniture', new DateTime()),
    new Product(5, 'Phone', 699.99, 'Electronics', new DateTime())
];

$uniqueByCategory = ObjectComparator::getUniqueObjects(
    $products,
    fn($a, $b) => $a->isInSameCategory($b)
);

foreach ($uniqueByCategory as $product) {
    echo "- {$product}\n";
}

// Sort products by price
echo "\nProducts sorted by price:\n";
$sortedByPrice = ObjectComparator::sortObjects(
    $products,
    fn($a, $b) => $a->getPrice() <=> $b->getPrice()
);

foreach ($sortedByPrice as $product) {
    echo "- {$product}\n";
}
```

Object comparison in PHP involves multiple approaches: reference comparison  
(===) checks if variables point to the same object, loose comparison (==)  
compares object contents, and custom methods enable domain-specific equality.  
Implement ComparableInterface for consistent sorting, use serialization for  
deep equality, and create custom comparison methods for business logic needs.  

## Advanced OOP patterns

Implementing sophisticated object-oriented patterns for complex applications.  

```php
<?php

// Command Pattern
interface CommandInterface {
    public function execute(): mixed;
    public function undo(): mixed;
    public function getDescription(): string;
}

class InvokerHistory {
    private array $history = [];
    private int $currentPosition = -1;
    
    public function execute(CommandInterface $command): mixed {
        // Remove any commands after current position (for redo functionality)
        $this->history = array_slice($this->history, 0, $this->currentPosition + 1);
        
        $result = $command->execute();
        $this->history[] = $command;
        $this->currentPosition++;
        
        return $result;
    }
    
    public function undo(): bool {
        if ($this->currentPosition >= 0) {
            $command = $this->history[$this->currentPosition];
            $command->undo();
            $this->currentPosition--;
            return true;
        }
        return false;
    }
    
    public function redo(): bool {
        if ($this->currentPosition < count($this->history) - 1) {
            $this->currentPosition++;
            $command = $this->history[$this->currentPosition];
            $command->execute();
            return true;
        }
        return false;
    }
    
    public function getHistory(): array {
        return array_map(fn($cmd) => $cmd->getDescription(), $this->history);
    }
    
    public function canUndo(): bool {
        return $this->currentPosition >= 0;
    }
    
    public function canRedo(): bool {
        return $this->currentPosition < count($this->history) - 1;
    }
}

class TextDocument {
    private string $content = '';
    
    public function insertText(string $text, int $position = null): void {
        $position = $position ?? strlen($this->content);
        $this->content = substr($this->content, 0, $position) . $text . substr($this->content, $position);
    }
    
    public function deleteText(int $start, int $length): string {
        $deleted = substr($this->content, $start, $length);
        $this->content = substr($this->content, 0, $start) . substr($this->content, $start + $length);
        return $deleted;
    }
    
    public function getContent(): string {
        return $this->content;
    }
    
    public function setContent(string $content): void {
        $this->content = $content;
    }
    
    public function getLength(): int {
        return strlen($this->content);
    }
}

class InsertTextCommand implements CommandInterface {
    private string $insertedText;
    private int $position;
    
    public function __construct(
        private TextDocument $document,
        string $text,
        ?int $position = null
    ) {
        $this->insertedText = $text;
        $this->position = $position ?? $document->getLength();
    }
    
    public function execute(): mixed {
        $this->document->insertText($this->insertedText, $this->position);
        return $this->position;
    }
    
    public function undo(): mixed {
        $this->document->deleteText($this->position, strlen($this->insertedText));
        return $this->position;
    }
    
    public function getDescription(): string {
        return "Insert '{$this->insertedText}' at position {$this->position}";
    }
}

class DeleteTextCommand implements CommandInterface {
    private string $deletedText = '';
    
    public function __construct(
        private TextDocument $document,
        private int $start,
        private int $length
    ) {}
    
    public function execute(): mixed {
        $this->deletedText = $this->document->deleteText($this->start, $this->length);
        return $this->deletedText;
    }
    
    public function undo(): mixed {
        $this->document->insertText($this->deletedText, $this->start);
        return $this->start;
    }
    
    public function getDescription(): string {
        return "Delete {$this->length} characters from position {$this->start}";
    }
}

// Strategy Pattern
interface SortStrategyInterface {
    public function sort(array $data): array;
    public function getName(): string;
}

class BubbleSortStrategy implements SortStrategyInterface {
    public function sort(array $data): array {
        $n = count($data);
        $result = $data;
        
        for ($i = 0; $i < $n - 1; $i++) {
            for ($j = 0; $j < $n - $i - 1; $j++) {
                if ($result[$j] > $result[$j + 1]) {
                    $temp = $result[$j];
                    $result[$j] = $result[$j + 1];
                    $result[$j + 1] = $temp;
                }
            }
        }
        
        return $result;
    }
    
    public function getName(): string {
        return 'Bubble Sort';
    }
}

class QuickSortStrategy implements SortStrategyInterface {
    public function sort(array $data): array {
        return $this->quickSort($data);
    }
    
    private function quickSort(array $arr): array {
        if (count($arr) < 2) {
            return $arr;
        }
        
        $pivot = $arr[0];
        $less = [];
        $greater = [];
        
        for ($i = 1; $i < count($arr); $i++) {
            if ($arr[$i] <= $pivot) {
                $less[] = $arr[$i];
            } else {
                $greater[] = $arr[$i];
            }
        }
        
        return array_merge(
            $this->quickSort($less),
            [$pivot],
            $this->quickSort($greater)
        );
    }
    
    public function getName(): string {
        return 'Quick Sort';
    }
}

class MergeSortStrategy implements SortStrategyInterface {
    public function sort(array $data): array {
        return $this->mergeSort($data);
    }
    
    private function mergeSort(array $arr): array {
        if (count($arr) <= 1) {
            return $arr;
        }
        
        $middle = intval(count($arr) / 2);
        $left = array_slice($arr, 0, $middle);
        $right = array_slice($arr, $middle);
        
        return $this->merge($this->mergeSort($left), $this->mergeSort($right));
    }
    
    private function merge(array $left, array $right): array {
        $result = [];
        $i = $j = 0;
        
        while ($i < count($left) && $j < count($right)) {
            if ($left[$i] <= $right[$j]) {
                $result[] = $left[$i++];
            } else {
                $result[] = $right[$j++];
            }
        }
        
        while ($i < count($left)) {
            $result[] = $left[$i++];
        }
        
        while ($j < count($right)) {
            $result[] = $right[$j++];
        }
        
        return $result;
    }
    
    public function getName(): string {
        return 'Merge Sort';
    }
}

class SortContext {
    private SortStrategyInterface $strategy;
    
    public function __construct(SortStrategyInterface $strategy) {
        $this->strategy = $strategy;
    }
    
    public function setStrategy(SortStrategyInterface $strategy): void {
        $this->strategy = $strategy;
    }
    
    public function sortData(array $data): array {
        $start = microtime(true);
        $result = $this->strategy->sort($data);
        $duration = microtime(true) - $start;
        
        echo "Sorted using {$this->strategy->getName()} in " . 
             number_format($duration * 1000, 2) . "ms\n";
        
        return $result;
    }
    
    public function getCurrentStrategy(): string {
        return $this->strategy->getName();
    }
}

// Template Method Pattern
abstract class DataProcessor {
    // Template method - defines the algorithm structure
    public final function processData(array $data): array {
        $this->beforeProcessing($data);
        
        $validated = $this->validateData($data);
        $transformed = $this->transformData($validated);
        $processed = $this->applyBusinessLogic($transformed);
        $result = $this->formatOutput($processed);
        
        $this->afterProcessing($result);
        
        return $result;
    }
    
    // Hook methods - can be overridden
    protected function beforeProcessing(array $data): void {}
    protected function afterProcessing(array $result): void {}
    
    // Abstract methods - must be implemented by subclasses
    abstract protected function validateData(array $data): array;
    abstract protected function transformData(array $data): array;
    abstract protected function applyBusinessLogic(array $data): array;
    abstract protected function formatOutput(array $data): array;
}

class UserDataProcessor extends DataProcessor {
    private array $errors = [];
    
    protected function beforeProcessing(array $data): void {
        echo "Starting user data processing for " . count($data) . " records\n";
        $this->errors = [];
    }
    
    protected function validateData(array $data): array {
        $valid = [];
        
        foreach ($data as $index => $user) {
            if (empty($user['email']) || !filter_var($user['email'], FILTER_VALIDATE_EMAIL)) {
                $this->errors[] = "Invalid email at index {$index}";
                continue;
            }
            
            if (empty($user['name']) || strlen($user['name']) < 2) {
                $this->errors[] = "Invalid name at index {$index}";
                continue;
            }
            
            $valid[] = $user;
        }
        
        return $valid;
    }
    
    protected function transformData(array $data): array {
        return array_map(function($user) {
            return [
                'name' => ucwords(strtolower($user['name'])),
                'email' => strtolower($user['email']),
                'age' => $user['age'] ?? 0,
                'created_at' => new DateTime()
            ];
        }, $data);
    }
    
    protected function applyBusinessLogic(array $data): array {
        return array_map(function($user) {
            $user['category'] = match(true) {
                $user['age'] < 18 => 'minor',
                $user['age'] < 65 => 'adult',
                default => 'senior'
            };
            
            $user['email_domain'] = substr($user['email'], strpos($user['email'], '@') + 1);
            
            return $user;
        }, $data);
    }
    
    protected function formatOutput(array $data): array {
        return array_map(function($user) {
            return [
                'id' => uniqid('user_'),
                'name' => $user['name'],
                'email' => $user['email'],
                'age' => $user['age'],
                'category' => $user['category'],
                'domain' => $user['email_domain'],
                'processed_at' => $user['created_at']->format('Y-m-d H:i:s')
            ];
        }, $data);
    }
    
    protected function afterProcessing(array $result): void {
        echo "Processing completed: " . count($result) . " valid records\n";
        if (!empty($this->errors)) {
            echo "Errors: " . implode(', ', $this->errors) . "\n";
        }
    }
    
    public function getErrors(): array {
        return $this->errors;
    }
}

// Usage examples
echo "=== Advanced OOP Patterns Examples ===\n\n";

// Command Pattern Example
$document = new TextDocument();
$history = new InvokerHistory();

echo "Command Pattern - Text Editor:\n";
$history->execute(new InsertTextCommand($document, "Hello there!"));
echo "After insert: '{$document->getContent()}'\n";

$history->execute(new InsertTextCommand($document, " How are you?"));
echo "After second insert: '{$document->getContent()}'\n";

$history->execute(new DeleteTextCommand($document, 5, 7)); // Remove " there!"
echo "After delete: '{$document->getContent()}'\n";

echo "History: " . json_encode($history->getHistory()) . "\n";

$history->undo();
echo "After undo: '{$document->getContent()}'\n";

$history->undo();
echo "After second undo: '{$document->getContent()}'\n";

$history->redo();
echo "After redo: '{$document->getContent()}'\n";

// Strategy Pattern Example
echo "\nStrategy Pattern - Sorting Algorithms:\n";
$data = [64, 34, 25, 12, 22, 11, 90, 5];
echo "Original data: " . json_encode($data) . "\n";

$sortContext = new SortContext(new BubbleSortStrategy());
$bubbleSorted = $sortContext->sortData($data);
echo "Result: " . json_encode($bubbleSorted) . "\n";

$sortContext->setStrategy(new QuickSortStrategy());
$quickSorted = $sortContext->sortData($data);
echo "Result: " . json_encode($quickSorted) . "\n";

$sortContext->setStrategy(new MergeSortStrategy());
$mergeSorted = $sortContext->sortData($data);
echo "Result: " . json_encode($mergeSorted) . "\n";

// Template Method Pattern Example
echo "\nTemplate Method Pattern - Data Processing:\n";
$processor = new UserDataProcessor();

$userData = [
    ['name' => 'john doe', 'email' => 'john@example.com', 'age' => 30],
    ['name' => 'jane smith', 'email' => 'jane@test.com', 'age' => 25],
    ['name' => 'bob', 'email' => 'invalid-email', 'age' => 35], // Invalid email
    ['name' => 'alice johnson', 'email' => 'alice@company.org', 'age' => 45],
    ['name' => 'x', 'email' => 'short@name.com', 'age' => 20] // Invalid name
];

$processed = $processor->processData($userData);

echo "\nProcessed Users:\n";
foreach ($processed as $user) {
    echo "- {$user['name']} ({$user['category']}) - {$user['email']} @ {$user['domain']}\n";
}

echo "\nErrors encountered: " . count($processor->getErrors()) . "\n";
```

Advanced OOP patterns solve complex design problems through proven solutions.  
Command pattern encapsulates requests for undo/redo functionality, Strategy  
pattern enables runtime algorithm selection, and Template Method defines  
algorithm structure while allowing customization. These patterns promote  
code reuse, maintainability, and extensibility in sophisticated applications.  
