# Namespaces

This chapter covers 15 comprehensive PHP namespace examples designed to  
demonstrate the full capabilities of namespaces in modern PHP 8.4.  
These examples progress from basic namespace declarations to advanced  
organizational patterns and autoloading strategies. Each example includes  
detailed explanations to help you master PHP namespace development and  
create well-structured, maintainable applications.  

## Basic namespace declaration

Creating a simple namespace to organize code logically.  

```php
<?php

namespace MyApp;

class Calculator {
    public function add($a, $b) {
        return $a + $b;
    }
    
    public function multiply($a, $b) {
        return $a * $b;
    }
}

$calc = new Calculator();
echo $calc->add(5, 3) . "\n";        // 8
echo $calc->multiply(4, 7) . "\n";   // 28
```

Namespaces provide a way to group related classes, interfaces, functions  
and constants. They prevent naming conflicts and create logical code  
organization. Classes within the same namespace can be used directly  
without qualification.  

## Namespace with classes

Defining multiple related classes within a namespace.  

```php
<?php

namespace Geometry;

class Circle {
    private $radius;
    
    public function __construct($radius) {
        $this->radius = $radius;
    }
    
    public function getArea() {
        return pi() * pow($this->radius, 2);
    }
}

class Rectangle {
    private $width, $height;
    
    public function __construct($width, $height) {
        $this->width = $width;
        $this->height = $height;
    }
    
    public function getArea() {
        return $this->width * $this->height;
    }
}

$circle = new Circle(5);
$rectangle = new Rectangle(4, 6);

echo "Circle area: " . round($circle->getArea(), 2) . "\n";      // 78.54
echo "Rectangle area: " . $rectangle->getArea() . "\n";          // 24
```

Multiple classes can share the same namespace, creating a cohesive group  
of related functionality. This promotes code organization and makes it  
easier to understand the relationships between different classes.  

## Multiple classes in same namespace

Organizing related functionality under a common namespace.  

```php
<?php

namespace Banking;

class Account {
    protected $balance;
    protected $accountNumber;
    
    public function __construct($accountNumber, $initialBalance = 0) {
        $this->accountNumber = $accountNumber;
        $this->balance = $initialBalance;
    }
    
    public function getBalance() {
        return $this->balance;
    }
    
    public function deposit($amount) {
        $this->balance += $amount;
        return $this;
    }
}

class Transaction {
    private $from, $to, $amount, $timestamp;
    
    public function __construct($from, $to, $amount) {
        $this->from = $from;
        $this->to = $to;
        $this->amount = $amount;
        $this->timestamp = time();
    }
    
    public function execute() {
        // Transfer logic would go here
        return "Transferred $this->amount from $this->from to $this->to";
    }
}

$account1 = new Account("ACC001", 1000);
$account2 = new Account("ACC002", 500);

$transaction = new Transaction("ACC001", "ACC002", 200);
echo $transaction->execute() . "\n";
echo "Account 1 balance: " . $account1->getBalance() . "\n";
```

Grouping related classes in the same namespace creates logical modules.  
Classes can interact with each other naturally within the namespace,  
promoting cohesive design and clear code organization.  

## Nested namespaces

Creating hierarchical namespace structures for complex applications.  

```php
<?php

namespace App\Models\User;

class Profile {
    private $firstName, $lastName, $email;
    
    public function __construct($firstName, $lastName, $email) {
        $this->firstName = $firstName;
        $this->lastName = $lastName;
        $this->email = $email;
    }
    
    public function getFullName() {
        return $this->firstName . ' ' . $this->lastName;
    }
    
    public function getEmail() {
        return $this->email;
    }
}

namespace App\Services\Email;

class Mailer {
    public function send($to, $subject, $body) {
        // Email sending logic
        return "Email sent to $to: $subject";
    }
}

// Usage
$profile = new \App\Models\User\Profile("Alice", "Johnson", "alice@example.com");
$mailer = new \App\Services\Email\Mailer();

echo $profile->getFullName() . "\n";
echo $mailer->send($profile->getEmail(), "Welcome!", "Hello there!") . "\n";
```

Nested namespaces create hierarchical organization using backslashes as  
separators. This mirrors directory structures and helps organize large  
applications into logical modules and sub-modules.  

## Using namespaced classes with fully qualified names

Accessing classes from different namespaces using complete paths.  

```php
<?php

namespace Database;

class Connection {
    private $host, $database;
    
    public function __construct($host, $database) {
        $this->host = $host;
        $this->database = $database;
    }
    
    public function connect() {
        return "Connected to $this->database at $this->host";
    }
}

namespace Logging;

class Logger {
    public function log($message) {
        $timestamp = date('Y-m-d H:i:s');
        return "[$timestamp] $message";
    }
}

namespace App;

// Using fully qualified names
$db = new \Database\Connection("localhost", "myapp");
$logger = new \Logging\Logger();

echo $db->connect() . "\n";
echo $logger->log("Application started") . "\n";

// Can also reference global namespace
$dateTime = new \DateTime();
echo "Current time: " . $dateTime->format('Y-m-d H:i:s') . "\n";
```

Fully qualified names start with a backslash and specify the complete  
namespace path. This approach provides explicit clarity about which  
class is being used, preventing ambiguity in complex applications.  

## Importing classes with use statements

Simplifying class references by importing namespaces.  

```php
<?php

namespace Utils\String;

class Formatter {
    public static function capitalize($text) {
        return ucfirst(strtolower($text));
    }
    
    public static function slug($text) {
        return strtolower(preg_replace('/[^A-Za-z0-9-]+/', '-', $text));
    }
}

namespace Utils\Array;

class Helper {
    public static function pluck($array, $key) {
        return array_column($array, $key);
    }
    
    public static function groupBy($array, $key) {
        $groups = [];
        foreach ($array as $item) {
            $groups[$item[$key]][] = $item;
        }
        return $groups;
    }
}

namespace App;

// Import specific classes
use Utils\String\Formatter;
use Utils\Array\Helper;

// Now use without full qualification
echo Formatter::capitalize("HELLO THERE") . "\n";      // Hello there
echo Formatter::slug("My Blog Post Title") . "\n";     // my-blog-post-title

$data = [
    ['name' => 'Alice', 'department' => 'IT'],
    ['name' => 'Bob', 'department' => 'HR'],
    ['name' => 'Charlie', 'department' => 'IT']
];

$names = Helper::pluck($data, 'name');
print_r($names);    // ['Alice', 'Bob', 'Charlie']
```

The `use` statement imports classes into the current namespace, allowing  
shorter references. This improves code readability while maintaining  
clear namespace organization and avoiding naming conflicts.  

## Aliasing classes with use statements

Creating alternative names for imported classes to avoid conflicts.  

```php
<?php

namespace External\Library;

class Logger {
    public function log($message) {
        return "External: $message";
    }
}

namespace Internal\Utils;

class Logger {
    public function log($message) {
        return "Internal: $message";
    }
}

namespace App;

// Import with aliases to avoid naming conflicts
use External\Library\Logger as ExternalLogger;
use Internal\Utils\Logger as InternalLogger;

// Alternative aliasing syntax
use DateTime as Date;

$extLogger = new ExternalLogger();
$intLogger = new InternalLogger();
$now = new Date();

echo $extLogger->log("System event") . "\n";
echo $intLogger->log("Application event") . "\n";
echo "Current date: " . $now->format('Y-m-d') . "\n";

// Can also alias for shorter names
use External\Library\Logger as Log;
$shortLogger = new Log();
echo $shortLogger->log("Short alias used") . "\n";
```

Aliases allow importing classes with conflicting names or creating  
shorter, more convenient names. The `as` keyword creates an alternative  
name that can be used within the current namespace scope.  

## Multiple imports in single use statement

Efficiently importing multiple classes from the same namespace.  

```php
<?php

namespace Graphics\Shapes;

class Circle {
    private $radius;
    
    public function __construct($radius) {
        $this->radius = $radius;
    }
    
    public function area() {
        return pi() * $this->radius * $this->radius;
    }
}

class Square {
    private $side;
    
    public function __construct($side) {
        $this->side = $side;
    }
    
    public function area() {
        return $this->side * $this->side;
    }
}

class Triangle {
    private $base, $height;
    
    public function __construct($base, $height) {
        $this->base = $base;
        $this->height = $height;
    }
    
    public function area() {
        return 0.5 * $this->base * $this->height;
    }
}

namespace App;

// Import multiple classes from same namespace
use Graphics\Shapes\{Circle, Square, Triangle};

// Alternative: individual imports with aliases
// use Graphics\Shapes\{
//     Circle as C,
//     Square as S,
//     Triangle as T
// };

$circle = new Circle(5);
$square = new Square(4);
$triangle = new Triangle(6, 8);

echo "Circle area: " . round($circle->area(), 2) . "\n";
echo "Square area: " . $square->area() . "\n";
echo "Triangle area: " . $triangle->area() . "\n";
```

Group imports using curly braces reduce repetitive use statements when  
importing multiple classes from the same namespace. This keeps imports  
organized and easier to read in files with many dependencies.  

## Function and constant namespaces

Organizing functions and constants within namespaces.  

```php
<?php

namespace Math\Calculations;

function factorial($n) {
    if ($n <= 1) return 1;
    return $n * factorial($n - 1);
}

function fibonacci($n) {
    if ($n <= 1) return $n;
    return fibonacci($n - 1) + fibonacci($n - 2);
}

const PI_EXTENDED = 3.14159265359;
const GOLDEN_RATIO = 1.61803398875;

namespace String\Utilities;

function reverse($str) {
    return strrev($str);
}

function isPalindrome($str) {
    $clean = strtolower(preg_replace('/[^A-Za-z0-9]/', '', $str));
    return $clean === strrev($clean);
}

const DEFAULT_ENCODING = 'UTF-8';

namespace App;

// Import functions and constants
use function Math\Calculations\factorial;
use function Math\Calculations\fibonacci;
use function String\Utilities\{reverse, isPalindrome};

use const Math\Calculations\PI_EXTENDED;
use const String\Utilities\DEFAULT_ENCODING;

echo "Factorial of 5: " . factorial(5) . "\n";         // 120
echo "Fibonacci of 7: " . fibonacci(7) . "\n";          // 13
echo "Reversed 'hello': " . reverse("hello") . "\n";    // olleh
echo "'racecar' is palindrome: " . (isPalindrome("racecar") ? 'Yes' : 'No') . "\n";

echo "Extended PI: " . PI_EXTENDED . "\n";
echo "Default encoding: " . DEFAULT_ENCODING . "\n";
```

Functions and constants can be namespaced just like classes. Import them  
using `use function` and `use const` statements. This provides better  
organization for utility functions and prevents global namespace pollution.  

## Namespace resolution operator

Using the scope resolution operator for namespace navigation.  

```php
<?php

namespace App\Core;

class Config {
    public static $settings = ['debug' => true, 'version' => '1.0'];
    
    public static function get($key) {
        return self::$settings[$key] ?? null;
    }
    
    public static function getAll() {
        return self::$settings;
    }
}

class Application {
    public function getVersion() {
        // Access Config from same namespace
        return Config::get('version');
    }
    
    public function isDebug() {
        // Using parent (same namespace)
        return Config::get('debug');
    }
    
    public function getCurrentTime() {
        // Access global namespace
        return (new \DateTime())->format('H:i:s');
    }
}

namespace Database;

class Connection {
    public function getAppVersion() {
        // Access class from different namespace
        return \App\Core\Config::get('version');
    }
    
    public function getAppConfig() {
        // Full qualification required
        return \App\Core\Config::getAll();
    }
}

namespace App;

$app = new Core\Application();
$db = new \Database\Connection();

echo "App version: " . $app->getVersion() . "\n";
echo "Debug mode: " . ($app->isDebug() ? 'ON' : 'OFF') . "\n";
echo "Current time: " . $app->getCurrentTime() . "\n";
echo "Version from DB: " . $db->getAppVersion() . "\n";
```

The scope resolution operator (::) accesses static methods, properties,  
and constants. Within namespaces, use relative names for same-namespace  
access or fully qualified names for cross-namespace references.  

## Global namespace and fallback

Understanding global namespace access and automatic fallback behavior.  

```php
<?php

// Global namespace function
function globalFunction($message) {
    return "Global: $message";
}

// Global constant
define('GLOBAL_CONSTANT', 'I am global');

namespace App\Utils;

// Namespaced function with same name
function globalFunction($message) {
    return "Namespaced: $message";
}

// Demonstrate namespace resolution
function testResolution() {
    // Calls namespaced version first
    echo globalFunction("test") . "\n";
    
    // Explicitly call global version
    echo \globalFunction("test") . "\n";
    
    // Built-in functions automatically resolve to global
    echo strlen("hello") . "\n";  // No \ needed
    
    // Constants fall back to global if not found in namespace
    echo GLOBAL_CONSTANT . "\n";
    
    // Classes fall back to global namespace
    $date = new DateTime();  // Resolves to \DateTime
    echo $date->format('Y-m-d') . "\n";
    
    // Explicitly global class access
    $stdClass = new \stdClass();
    $stdClass->property = "Global class";
    echo $stdClass->property . "\n";
}

testResolution();

// Testing from different namespace
namespace Other;

function anotherTest() {
    // This will fall back to global since not defined in Other namespace
    echo globalFunction("fallback test") . "\n";
    
    // Access specific namespace
    echo \App\Utils\globalFunction("specific namespace") . "\n";
}

anotherTest();
```

PHP automatically falls back to the global namespace for undefined  
functions, constants, and classes. Built-in PHP functions and classes  
are always in the global namespace but don't require the leading  
backslash for access.  

## Autoloading with namespaces

Implementing PSR-4 autoloading standards with namespace mapping.  

```php
<?php

// Autoloader implementation (typically in composer or custom autoloader)
spl_autoload_register(function ($className) {
    // PSR-4: Convert namespace to file path
    $prefix = 'App\\';
    $baseDir = __DIR__ . '/src/';
    
    // Check if class uses the namespace prefix
    $len = strlen($prefix);
    if (strncmp($prefix, $className, $len) !== 0) {
        return; // No, move to next registered autoloader
    }
    
    // Get relative class name
    $relativeClass = substr($className, $len);
    
    // Replace namespace separators with directory separators
    $file = $baseDir . str_replace('\\', '/', $relativeClass) . '.php';
    
    // If file exists, require it
    if (file_exists($file)) {
        require $file;
    }
});

// Example directory structure:
// src/
//   Models/
//     User.php        (App\Models\User)
//     Post.php        (App\Models\Post)
//   Services/
//     EmailService.php (App\Services\EmailService)
//   Controllers/
//     UserController.php (App\Controllers\UserController)

// This would be in src/Models/User.php
namespace App\Models;

class User {
    private $id, $name, $email;
    
    public function __construct($id, $name, $email) {
        $this->id = $id;
        $this->name = $name;
        $this->email = $email;
    }
    
    public function getName() {
        return $this->name;
    }
}

// This would be in src/Services/EmailService.php
namespace App\Services;

use App\Models\User;

class EmailService {
    public function sendWelcome(User $user) {
        return "Welcome email sent to " . $user->getName();
    }
}

// Usage - classes are automatically loaded
$user = new App\Models\User(1, "Alice", "alice@example.com");
$emailService = new App\Services\EmailService();

echo $emailService->sendWelcome($user) . "\n";
```

Autoloading automatically loads classes when they're first used. PSR-4  
standard maps namespace prefixes to directory paths, enabling organized  
file structures that mirror namespace hierarchies for maintainable code.  

## Namespace and file structure organization

Organizing files and directories to match namespace structure.  

```php
<?php

// File: src/Blog/Models/Post.php
namespace Blog\Models;

class Post {
    private $title, $content, $author, $published;
    
    public function __construct($title, $content, $author) {
        $this->title = $title;
        $this->content = $content;
        $this->author = $author;
        $this->published = false;
    }
    
    public function publish() {
        $this->published = true;
        return $this;
    }
    
    public function getTitle() {
        return $this->title;
    }
    
    public function isPublished() {
        return $this->published;
    }
}

// File: src/Blog/Services/PostManager.php
namespace Blog\Services;

use Blog\Models\Post;

class PostManager {
    private $posts = [];
    
    public function addPost(Post $post) {
        $this->posts[] = $post;
        return $this;
    }
    
    public function publishAll() {
        foreach ($this->posts as $post) {
            $post->publish();
        }
        return count($this->posts);
    }
    
    public function getPublishedCount() {
        return count(array_filter($this->posts, fn($post) => $post->isPublished()));
    }
}

// File: src/Blog/Controllers/BlogController.php
namespace Blog\Controllers;

use Blog\Models\Post;
use Blog\Services\PostManager;

class BlogController {
    private $postManager;
    
    public function __construct(PostManager $postManager) {
        $this->postManager = $postManager;
    }
    
    public function createPost($title, $content, $author) {
        $post = new Post($title, $content, $author);
        $this->postManager->addPost($post);
        return $post;
    }
    
    public function publishAllPosts() {
        return $this->postManager->publishAll();
    }
}

// Usage example (this would be in a separate file)
$postManager = new Blog\Services\PostManager();
$controller = new Blog\Controllers\BlogController($postManager);

$post1 = $controller->createPost("Getting Started", "Hello there!", "Alice");
$post2 = $controller->createPost("Advanced Topics", "Complex stuff...", "Bob");

echo "Created posts, publishing all...\n";
$publishedCount = $controller->publishAllPosts();
echo "Published $publishedCount posts\n";
```

Organize files to mirror namespace structure with one class per file.  
This convention makes code predictable and easier to navigate, following  
PSR-4 standards that most PHP frameworks and tools expect.  

## Namespaces with interfaces and traits

Using namespaces with interfaces and traits for better code organization.  

```php
<?php

namespace Contracts;

interface Renderable {
    public function render(): string;
}

interface Cacheable {
    public function getCacheKey(): string;
    public function getCacheDuration(): int;
}

namespace Traits;

trait Timestampable {
    private $createdAt;
    private $updatedAt;
    
    public function initializeTimestamps() {
        $this->createdAt = new \DateTime();
        $this->updatedAt = new \DateTime();
    }
    
    public function updateTimestamp() {
        $this->updatedAt = new \DateTime();
    }
    
    public function getCreatedAt() {
        return $this->createdAt;
    }
    
    public function getUpdatedAt() {
        return $this->updatedAt;
    }
}

trait Identifiable {
    private $id;
    
    public function setId($id) {
        $this->id = $id;
        return $this;
    }
    
    public function getId() {
        return $this->id;
    }
}

namespace Models;

use Contracts\{Renderable, Cacheable};
use Traits\{Timestampable, Identifiable};

class Article implements Renderable, Cacheable {
    use Timestampable, Identifiable;
    
    private $title, $content;
    
    public function __construct($title, $content) {
        $this->title = $title;
        $this->content = $content;
        $this->initializeTimestamps();
    }
    
    public function render(): string {
        return "<article><h1>{$this->title}</h1><p>{$this->content}</p></article>";
    }
    
    public function getCacheKey(): string {
        return "article_" . $this->getId();
    }
    
    public function getCacheDuration(): int {
        return 3600; // 1 hour
    }
    
    public function getTitle() {
        return $this->title;
    }
}

// Usage
$article = new Models\Article("PHP Namespaces", "Namespaces help organize code...");
$article->setId(123);

echo $article->render() . "\n";
echo "Cache key: " . $article->getCacheKey() . "\n";
echo "Created: " . $article->getCreatedAt()->format('Y-m-d H:i:s') . "\n";
echo "ID: " . $article->getId() . "\n";
```

Namespaces work seamlessly with interfaces and traits, allowing clean  
separation of contracts, behaviors, and implementations. This promotes  
SOLID principles and creates highly maintainable, testable code.  

## Advanced namespace patterns and best practices

Implementing advanced namespace patterns for large applications.  

```php
<?php

// Domain-driven design namespace structure
namespace Ecommerce\Domain\Product;

abstract class AbstractProduct {
    protected $id, $name, $price;
    
    public function __construct($id, $name, $price) {
        $this->id = $id;
        $this->name = $name;
        $this->price = $price;
    }
    
    abstract public function calculateTax(): float;
    
    public function getPrice() {
        return $this->price;
    }
    
    public function getName() {
        return $this->name;
    }
}

class DigitalProduct extends AbstractProduct {
    public function calculateTax(): float {
        return $this->price * 0.10; // 10% digital tax
    }
}

class PhysicalProduct extends AbstractProduct {
    private $weight, $dimensions;
    
    public function __construct($id, $name, $price, $weight, $dimensions) {
        parent::__construct($id, $name, $price);
        $this->weight = $weight;
        $this->dimensions = $dimensions;
    }
    
    public function calculateTax(): float {
        return $this->price * 0.08; // 8% physical goods tax
    }
    
    public function calculateShipping(): float {
        return $this->weight * 0.5; // Simple shipping calculation
    }
}

namespace Ecommerce\Application\Services;

use Ecommerce\Domain\Product\{AbstractProduct, PhysicalProduct};

class OrderCalculator {
    public function calculateTotal(array $products): array {
        $subtotal = 0;
        $tax = 0;
        $shipping = 0;
        
        foreach ($products as $product) {
            if (!$product instanceof AbstractProduct) {
                throw new \InvalidArgumentException("Invalid product type");
            }
            
            $subtotal += $product->getPrice();
            $tax += $product->calculateTax();
            
            if ($product instanceof PhysicalProduct) {
                $shipping += $product->calculateShipping();
            }
        }
        
        return [
            'subtotal' => $subtotal,
            'tax' => round($tax, 2),
            'shipping' => round($shipping, 2),
            'total' => round($subtotal + $tax + $shipping, 2)
        ];
    }
}

namespace Ecommerce\Infrastructure\Persistence;

class ProductRepository {
    private $products = [];
    
    public function save(\Ecommerce\Domain\Product\AbstractProduct $product) {
        $this->products[$product->getName()] = $product;
        return $product;
    }
    
    public function findByName(string $name): ?\Ecommerce\Domain\Product\AbstractProduct {
        return $this->products[$name] ?? null;
    }
    
    public function getAll(): array {
        return array_values($this->products);
    }
}

// Application layer coordination
namespace App;

use Ecommerce\Domain\Product\{DigitalProduct, PhysicalProduct};
use Ecommerce\Application\Services\OrderCalculator;
use Ecommerce\Infrastructure\Persistence\ProductRepository;

// Create products
$ebook = new DigitalProduct(1, "PHP Guide", 29.99);
$laptop = new PhysicalProduct(2, "Laptop", 799.99, 2.5, "30x20x3cm");

// Store products
$repository = new ProductRepository();
$repository->save($ebook);
$repository->save($laptop);

// Calculate order total
$calculator = new OrderCalculator();
$orderTotal = $calculator->calculateTotal([$ebook, $laptop]);

echo "Order Summary:\n";
foreach ($orderTotal as $key => $value) {
    echo ucfirst($key) . ": $" . $value . "\n";
}

echo "\nProducts in repository:\n";
foreach ($repository->getAll() as $product) {
    echo "- " . $product->getName() . " ($" . $product->getPrice() . ")\n";
}
```

Advanced namespace patterns follow domain-driven design principles,  
separating concerns into domain, application, and infrastructure layers.  
This creates maintainable, testable applications with clear boundaries  
and dependencies that flow in the correct direction.  