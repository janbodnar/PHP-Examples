# Random Values

This chapter covers 25 comprehensive PHP random value generation examples  
designed to demonstrate the full range of randomization capabilities in  
modern PHP 8.4. These examples progress from basic random numbers to  
advanced random data generation including cryptographically secure  
randomness, weighted selection, and complex data structures. Each example  
includes detailed explanations to help you master random value generation  
in PHP applications.  

## Basic random integers

Generating random whole numbers within specified ranges.  

```php
<?php

// Generate random integers in different ranges
$dice = random_int(1, 6);
$percent = random_int(0, 100);  
$lottery = random_int(1, 1000000);

echo "Dice roll: $dice\n";
echo "Percentage: $percent%\n";
echo "Lottery number: $lottery\n";

// Multiple random values
echo "Random numbers 1-10: ";
for ($i = 0; $i < 5; $i++) {
    echo random_int(1, 10) . " ";
}
echo "\n";
```

The `random_int()` function generates cryptographically secure random  
integers within a specified range. It's the preferred method for  
generating random numbers in PHP 7.0+ due to its security properties.  

## Legacy random functions

Using older random functions for backward compatibility.  

```php
<?php

// MT (Mersenne Twister) random functions
mt_srand(42); // Seed for reproducible results
$mt_value = mt_rand(1, 100);
$mt_max = mt_getrandmax();

echo "MT Random (1-100): $mt_value\n";
echo "MT Max value: $mt_max\n";

// Standard rand() function
srand(42); // Seed for reproducible results  
$rand_value = rand(1, 50);
$rand_max = getrandmax();

echo "Rand (1-50): $rand_value\n";
echo "Rand max value: $rand_max\n";

// Reset to random seed
mt_srand();
srand();
```

While `mt_rand()` and `rand()` are faster, `random_int()` should be  
preferred for security-sensitive applications. These functions use  
pseudo-random algorithms that can be seeded for reproducible results.  

## Random floating point numbers

Generating random decimal values with various precisions.  

```php
<?php

// Basic float generation (0-1)
$basic_float = mt_rand() / mt_getrandmax();
$precise_float = random_int(0, PHP_INT_MAX) / PHP_INT_MAX;

echo "Basic float: " . round($basic_float, 4) . "\n";
echo "Precise float: " . round($precise_float, 6) . "\n";

// Random float in custom range
function randomFloat($min, $max, $decimals = 2) {
    $range = $max - $min;
    $random = mt_rand() / mt_getrandmax();
    return round($min + ($range * $random), $decimals);
}

$price = randomFloat(10.50, 99.99, 2);
$temperature = randomFloat(-20.0, 40.0, 1);
$percentage = randomFloat(0.0, 100.0, 3);

echo "Random price: $$price\n";
echo "Random temperature: {$temperature}°C\n";
echo "Random percentage: $percentage%\n";
```

PHP doesn't have a built-in random float function, so we create one  
by dividing random integers by their maximum value. This provides  
uniform distribution across the specified range.  

## Random string generation

Creating random strings with various character sets.  

```php
<?php

function generateRandomString($length, $chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789') {
    $result = '';
    $charsLength = strlen($chars);
    
    for ($i = 0; $i < $length; $i++) {
        $result .= $chars[random_int(0, $charsLength - 1)];
    }
    
    return $result;
}

// Different character sets
$alphanumeric = generateRandomString(10);
$lowercase = generateRandomString(8, 'abcdefghijklmnopqrstuvwxyz');
$numbers = generateRandomString(6, '0123456789');
$special = generateRandomString(12, 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*');

echo "Alphanumeric: $alphanumeric\n";
echo "Lowercase: $lowercase\n";  
echo "Numbers only: $numbers\n";
echo "With special chars: $special\n";
```

Random string generation selects characters from a defined set using  
`random_int()` for secure randomness. Useful for generating passwords,  
tokens, and unique identifiers in applications.  

## Random array element selection

Picking random elements from arrays and collections.  

```php
<?php

$colors = ['red', 'blue', 'green', 'yellow', 'purple', 'orange'];
$foods = ['pizza', 'burger', 'sushi', 'pasta', 'salad', 'tacos'];
$numbers = [10, 25, 33, 47, 58, 69, 71, 88, 94];

// Single random selection
$randomColor = $colors[array_rand($colors)];
$randomFood = $foods[array_rand($foods)];

echo "Random color: $randomColor\n";
echo "Random food: $randomFood\n";

// Multiple random selections
$multipleColors = array_rand($colors, 3);
echo "Multiple colors: ";
foreach ($multipleColors as $key) {
    echo $colors[$key] . " ";
}
echo "\n";

// Random selection with custom function
function selectRandom($array, $count = 1) {
    $keys = array_rand($array, min($count, count($array)));
    return $count === 1 ? [$array[$keys]] : array_map(fn($k) => $array[$k], $keys);
}

$randomNumbers = selectRandom($numbers, 4);
echo "Random numbers: " . implode(', ', $randomNumbers) . "\n";
```

The `array_rand()` function returns random keys from arrays. For single  
selections, it returns one key; for multiple selections, it returns an  
array of keys. Always use the keys to access actual values.  

## Random boolean values

Generating true/false values with optional weighting.  

```php
<?php

// Simple 50/50 boolean
$coinFlip = (bool) random_int(0, 1);
$isTrue = random_int(0, 1) === 1;

echo "Coin flip: " . ($coinFlip ? 'Heads' : 'Tails') . "\n";
echo "Is true: " . ($isTrue ? 'Yes' : 'No') . "\n";

// Weighted boolean (70% true, 30% false)  
function weightedBoolean($truePercentage = 50) {
    return random_int(1, 100) <= $truePercentage;
}

$mostlyTrue = weightedBoolean(70);
$mostlyFalse = weightedBoolean(25);
$balanced = weightedBoolean();

echo "70% true: " . ($mostlyTrue ? 'True' : 'False') . "\n";
echo "25% true: " . ($mostlyFalse ? 'True' : 'False') . "\n";
echo "50% true: " . ($balanced ? 'True' : 'False') . "\n";

// Multiple boolean tests
echo "Random decisions: ";
for ($i = 0; $i < 10; $i++) {
    echo (weightedBoolean(60) ? 'Y' : 'N');
}
echo "\n";
```

Boolean randomization converts random integers to true/false values.  
Weighted booleans allow controlling probability by comparing random  
numbers against threshold percentages.  

## Cryptographically secure random bytes

Generating secure random data for cryptographic purposes.  

```php
<?php

// Generate random bytes
$bytes16 = random_bytes(16);
$bytes32 = random_bytes(32);

echo "16 bytes (hex): " . bin2hex($bytes16) . "\n";
echo "32 bytes (hex): " . bin2hex($bytes32) . "\n";
echo "16 bytes (base64): " . base64_encode($bytes16) . "\n";

// Generate secure tokens
function generateSecureToken($length = 32) {
    return bin2hex(random_bytes($length));
}

function generateBase64Token($length = 32) {
    return rtrim(base64_encode(random_bytes($length)), '=');
}

$sessionToken = generateSecureToken(16);
$apiKey = generateSecureToken(24);
$base64Token = generateBase64Token(20);

echo "Session token: $sessionToken\n";
echo "API key: $apiKey\n";
echo "Base64 token: $base64Token\n";

// Secure random salt for password hashing
$salt = base64_encode(random_bytes(22));
echo "Password salt: $salt\n";
```

The `random_bytes()` function generates cryptographically secure random  
bytes suitable for security applications like tokens, salts, and keys.  
Convert to hex or base64 for text representation.  

## Random UUID generation

Creating universally unique identifiers in various formats.  

```php
<?php

function generateUUIDv4() {
    $data = random_bytes(16);
    
    // Set version to 0100 (4)
    $data[6] = chr(ord($data[6]) & 0x0f | 0x40);
    // Set bits 6-7 to 10
    $data[8] = chr(ord($data[8]) & 0x3f | 0x80);
    
    return sprintf('%08x-%04x-%04x-%04x-%012x',
        unpack('N', substr($data, 0, 4))[1],
        unpack('n', substr($data, 4, 2))[1],
        unpack('n', substr($data, 6, 2))[1],
        unpack('n', substr($data, 8, 2))[1],
        unpack('N', substr($data, 10, 4))[1] . unpack('n', substr($data, 14, 2))[1]
    );
}

function generateSimpleUUID() {
    return sprintf('%04x%04x-%04x-%04x-%04x-%04x%04x%04x',
        random_int(0, 0xffff), random_int(0, 0xffff),
        random_int(0, 0xffff), random_int(0, 0x0fff) | 0x4000,
        random_int(0, 0x3fff) | 0x8000,
        random_int(0, 0xffff), random_int(0, 0xffff), random_int(0, 0xffff)
    );
}

$uuid1 = generateUUIDv4();
$uuid2 = generateSimpleUUID();
$shortId = substr(str_replace('-', '', generateSimpleUUID()), 0, 12);

echo "UUID v4: $uuid1\n";
echo "Simple UUID: $uuid2\n";
echo "Short ID: $shortId\n";
```

UUIDs provide globally unique identifiers using random values. Version 4  
UUIDs use random data with specific bit patterns. These are useful for  
database keys, session IDs, and distributed system identifiers.  

## Random dates and times

Generating random temporal values within specified ranges.  

```php
<?php

// Random date between two dates
function randomDateBetween($startDate, $endDate) {
    $startTimestamp = strtotime($startDate);
    $endTimestamp = strtotime($endDate);
    $randomTimestamp = random_int($startTimestamp, $endTimestamp);
    return date('Y-m-d', $randomTimestamp);
}

// Random datetime
function randomDateTimeBetween($startDate, $endDate) {
    $startTimestamp = strtotime($startDate);
    $endTimestamp = strtotime($endDate);
    $randomTimestamp = random_int($startTimestamp, $endTimestamp);
    return date('Y-m-d H:i:s', $randomTimestamp);
}

$randomDate = randomDateBetween('2020-01-01', '2024-12-31');
$randomDateTime = randomDateTimeBetween('2024-01-01 00:00:00', '2024-12-31 23:59:59');
$randomBirthday = randomDateBetween('1980-01-01', '2005-12-31');

echo "Random date: $randomDate\n";
echo "Random datetime: $randomDateTime\n";
echo "Random birthday: $randomBirthday\n";

// Random time components
$randomHour = random_int(0, 23);
$randomMinute = random_int(0, 59);
$randomSecond = random_int(0, 59);
$randomTime = sprintf('%02d:%02d:%02d', $randomHour, $randomMinute, $randomSecond);

echo "Random time: $randomTime\n";

// Random day of week
$daysOfWeek = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
$randomDay = $daysOfWeek[array_rand($daysOfWeek)];
echo "Random day: $randomDay\n";
```

Random date generation converts timestamps to random integers within  
date ranges. Useful for generating test data, scheduling random events,  
and creating sample datasets with temporal distribution.  

## Random colors

Generating random colors in various formats (hex, RGB, HSL).  

```php
<?php

// Random hex color
function randomHexColor() {
    return sprintf('#%06X', random_int(0, 0xFFFFFF));
}

// Random RGB color
function randomRGBColor() {
    $r = random_int(0, 255);
    $g = random_int(0, 255);
    $b = random_int(0, 255);
    return ['r' => $r, 'g' => $g, 'b' => $b];
}

// Random HSL color
function randomHSLColor() {
    $h = random_int(0, 360);
    $s = random_int(0, 100);
    $l = random_int(0, 100);
    return ['h' => $h, 's' => $s, 'l' => $l];
}

$hexColor = randomHexColor();
$rgbColor = randomRGBColor();
$hslColor = randomHSLColor();

echo "Hex color: $hexColor\n";
echo "RGB color: rgb({$rgbColor['r']}, {$rgbColor['g']}, {$rgbColor['b']})\n";
echo "HSL color: hsl({$hslColor['h']}, {$hslColor['s']}%, {$hslColor['l']}%)\n";

// Predefined color palette
$colorPalette = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#96CEB4', '#FFEAA7', '#DDA0DD'];
$randomPaletteColor = $colorPalette[array_rand($colorPalette)];
echo "Palette color: $randomPaletteColor\n";

// Random bright colors (high saturation and lightness)
function randomBrightColor() {
    $h = random_int(0, 360);
    $s = random_int(70, 100);
    $l = random_int(50, 80);
    return "hsl($h, {$s}%, {$l}%)";
}

echo "Bright color: " . randomBrightColor() . "\n";
```

Color randomization generates values in different color spaces. Hex  
colors use 6-digit hexadecimal values, RGB uses 0-255 ranges, and HSL  
provides intuitive hue, saturation, and lightness control.  

## Random coordinates and geometry

Generating random geometric values and spatial coordinates.  

```php
<?php

// Random coordinates
function randomCoordinate($minLat, $maxLat, $minLng, $maxLng) {
    $lat = randomFloat($minLat, $maxLat, 6);
    $lng = randomFloat($minLng, $maxLng, 6);
    return ['lat' => $lat, 'lng' => $lng];
}

function randomFloat($min, $max, $decimals = 2) {
    $range = $max - $min;
    $random = mt_rand() / mt_getrandmax();
    return round($min + ($range * $random), $decimals);
}

// Random point in different regions
$nycPoint = randomCoordinate(40.4774, 40.9176, -74.2591, -73.7004);  // NYC area
$worldPoint = randomCoordinate(-90, 90, -180, 180);                   // Anywhere on Earth

echo "NYC point: {$nycPoint['lat']}, {$nycPoint['lng']}\n";
echo "World point: {$worldPoint['lat']}, {$worldPoint['lng']}\n";

// Random geometric shapes
function randomRectangle($maxWidth = 100, $maxHeight = 100) {
    return [
        'x' => random_int(0, 500),
        'y' => random_int(0, 500),
        'width' => random_int(1, $maxWidth),
        'height' => random_int(1, $maxHeight)
    ];
}

function randomCircle($maxRadius = 50) {
    return [
        'centerX' => random_int(0, 500),
        'centerY' => random_int(0, 500),
        'radius' => random_int(1, $maxRadius)
    ];
}

$rect = randomRectangle();
$circle = randomCircle();

echo "Rectangle: ({$rect['x']},{$rect['y']}) {$rect['width']}x{$rect['height']}\n";
echo "Circle: center({$circle['centerX']},{$circle['centerY']}) radius={$circle['radius']}\n";
```

Geometric randomization creates spatial data including coordinates,  
shapes, and measurements. Useful for map applications, game development,  
and spatial analysis with realistic coordinate ranges.  

## Random text generation

Creating random words, sentences, and text content.  

```php
<?php

$words = [
    'the', 'quick', 'brown', 'fox', 'jumps', 'over', 'lazy', 'dog',
    'amazing', 'fantastic', 'incredible', 'wonderful', 'excellent', 'brilliant',
    'house', 'tree', 'mountain', 'river', 'ocean', 'sky', 'cloud', 'star'
];

$adjectives = ['red', 'blue', 'fast', 'slow', 'big', 'small', 'bright', 'dark'];
$nouns = ['car', 'book', 'phone', 'computer', 'table', 'chair', 'window', 'door'];
$verbs = ['runs', 'walks', 'flies', 'swims', 'jumps', 'dances', 'sings', 'laughs'];

function randomWord($wordList) {
    return $wordList[array_rand($wordList)];
}

function randomSentence($words, $minWords = 5, $maxWords = 12) {
    $wordCount = random_int($minWords, $maxWords);
    $sentence = [];
    
    for ($i = 0; $i < $wordCount; $i++) {
        $sentence[] = randomWord($words);
    }
    
    return ucfirst(implode(' ', $sentence)) . '.';
}

function randomParagraph($words, $minSentences = 3, $maxSentences = 6) {
    $sentenceCount = random_int($minSentences, $maxSentences);
    $sentences = [];
    
    for ($i = 0; $i < $sentenceCount; $i++) {
        $sentences[] = randomSentence($words);
    }
    
    return implode(' ', $sentences);
}

echo "Random word: " . randomWord($words) . "\n";
echo "Random sentence: " . randomSentence($words) . "\n";
echo "Random paragraph: " . randomParagraph($words) . "\n\n";

// Structured random text
$randomPhrase = randomWord($adjectives) . ' ' . randomWord($nouns) . ' ' . randomWord($verbs);
echo "Random phrase: $randomPhrase\n";
```

Text randomization combines word lists to create varied content. Useful  
for generating placeholder text, test data, and lorem ipsum alternatives  
with more meaningful word combinations.  

## Weighted random selection

Implementing probability-based selection with different weights.  

```php
<?php

class WeightedRandom {
    private array $items = [];
    private array $weights = [];
    private int $totalWeight = 0;
    
    public function addItem($item, int $weight): void {
        $this->items[] = $item;
        $this->weights[] = $weight;
        $this->totalWeight += $weight;
    }
    
    public function select() {
        if (empty($this->items)) {
            return null;
        }
        
        $random = random_int(1, $this->totalWeight);
        $currentWeight = 0;
        
        for ($i = 0; $i < count($this->items); $i++) {
            $currentWeight += $this->weights[$i];
            if ($random <= $currentWeight) {
                return $this->items[$i];
            }
        }
        
        return end($this->items);
    }
}

// Example: Weighted loot system
$loot = new WeightedRandom();
$loot->addItem('Common Sword', 50);      // 50% chance
$loot->addItem('Rare Shield', 30);       // 30% chance
$loot->addItem('Epic Bow', 15);          // 15% chance
$loot->addItem('Legendary Staff', 5);    // 5% chance

echo "Loot drops:\n";
for ($i = 0; $i < 10; $i++) {
    echo "- " . $loot->select() . "\n";
}

// Simple weighted array function
function weightedSelect(array $weightedItems) {
    $totalWeight = array_sum($weightedItems);
    $random = random_int(1, $totalWeight);
    
    foreach ($weightedItems as $item => $weight) {
        $random -= $weight;
        if ($random <= 0) {
            return $item;
        }
    }
    
    return array_key_first($weightedItems);
}

$weatherChances = [
    'Sunny' => 40,
    'Cloudy' => 30,
    'Rainy' => 20,
    'Stormy' => 10
];

echo "\nWeather forecast: " . weightedSelect($weatherChances) . "\n";
```

Weighted random selection allows items to have different probabilities  
of being chosen. The total weight determines the probability space, and  
each item's weight represents its likelihood of selection.  

## Random sampling without replacement

Selecting multiple random items ensuring no duplicates.  

```php
<?php

function randomSample(array $items, int $count): array {
    if ($count >= count($items)) {
        return $items;
    }
    
    $shuffled = $items;
    shuffle($shuffled);
    return array_slice($shuffled, 0, $count);
}

function randomSampleWithKeys(array $items, int $count): array {
    $keys = array_keys($items);
    $selectedKeys = randomSample($keys, $count);
    
    $result = [];
    foreach ($selectedKeys as $key) {
        $result[$key] = $items[$key];
    }
    
    return $result;
}

// Reservoir sampling algorithm for large datasets
function reservoirSample(array $items, int $count): array {
    $reservoir = [];
    
    foreach ($items as $i => $item) {
        if ($i < $count) {
            $reservoir[] = $item;
        } else {
            $j = random_int(0, $i);
            if ($j < $count) {
                $reservoir[$j] = $item;
            }
        }
    }
    
    return $reservoir;
}

$deck = ['A♠', 'K♠', 'Q♠', 'J♠', '10♠', '9♠', '8♠', '7♠', '6♠', '5♠'];
$students = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank', 'Grace'];
$products = [
    'laptop' => 999.99,
    'mouse' => 29.99,
    'keyboard' => 79.99,
    'monitor' => 299.99,
    'speaker' => 149.99
];

$hand = randomSample($deck, 5);
$selectedStudents = randomSample($students, 3);
$featuredProducts = randomSampleWithKeys($products, 2);

echo "Random hand: " . implode(', ', $hand) . "\n";
echo "Selected students: " . implode(', ', $selectedStudents) . "\n";
echo "Featured products:\n";
foreach ($featuredProducts as $name => $price) {
    echo "- $name: $$price\n";
}

// Large dataset example
$largeDataset = range(1, 1000);
$sample = reservoirSample($largeDataset, 10);
echo "Sample from 1-1000: " . implode(', ', $sample) . "\n";
```

Random sampling without replacement ensures unique selections from a  
dataset. Shuffle-based sampling works for small sets, while reservoir  
sampling handles large datasets efficiently with constant memory usage.  

## Seeded random generation

Creating reproducible random sequences using seeds.  

```php
<?php

class SeededRandom {
    private int $seed;
    
    public function __construct(int $seed) {
        $this->seed = $seed;
        mt_srand($seed);
    }
    
    public function integer(int $min, int $max): int {
        return mt_rand($min, $max);
    }
    
    public function float(): float {
        return mt_rand() / mt_getrandmax();
    }
    
    public function boolean(): bool {
        return $this->integer(0, 1) === 1;
    }
    
    public function choice(array $items) {
        return $items[array_rand($items)];
    }
    
    public function reseed(int $newSeed): void {
        $this->seed = $newSeed;
        mt_srand($newSeed);
    }
    
    public function getSeed(): int {
        return $this->seed;
    }
}

// Deterministic random sequence
$rng1 = new SeededRandom(12345);
$rng2 = new SeededRandom(12345);

echo "Seeded sequence 1:\n";
for ($i = 0; $i < 5; $i++) {
    echo "- " . $rng1->integer(1, 100) . "\n";
}

echo "\nSeeded sequence 2 (same seed):\n";
for ($i = 0; $i < 5; $i++) {
    echo "- " . $rng2->integer(1, 100) . "\n";
}

// Different seed produces different sequence
$rng3 = new SeededRandom(54321);
echo "\nDifferent seed sequence:\n";
for ($i = 0; $i < 5; $i++) {
    echo "- " . $rng3->integer(1, 100) . "\n";
}

// Practical example: Reproducible test data
function generateTestUser(int $id): array {
    $rng = new SeededRandom($id);
    $names = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve'];
    
    return [
        'id' => $id,
        'name' => $rng->choice($names),
        'age' => $rng->integer(18, 65),
        'active' => $rng->boolean(),
        'score' => round($rng->float() * 100, 2)
    ];
}

echo "\nReproducible test users:\n";
for ($i = 1; $i <= 3; $i++) {
    $user = generateTestUser($i);
    echo "User $i: {$user['name']}, {$user['age']} years, score: {$user['score']}\n";
}
```

Seeded random generation produces reproducible sequences useful for  
testing, debugging, and simulation. The same seed always generates  
identical sequences, enabling consistent test environments.  

## Random file and directory names

Generating random filesystem-safe names and paths.  

```php
<?php

function randomFilename($extension = 'txt', $length = 8): string {
    $chars = 'abcdefghijklmnopqrstuvwxyz0123456789';
    $name = '';
    
    for ($i = 0; $i < $length; $i++) {
        $name .= $chars[random_int(0, strlen($chars) - 1)];
    }
    
    return $name . '.' . $extension;
}

function randomDirectoryName($length = 10): string {
    $chars = 'abcdefghijklmnopqrstuvwxyz0123456789_-';
    $name = '';
    
    for ($i = 0; $i < $length; $i++) {
        $name .= $chars[random_int(0, strlen($chars) - 1)];
    }
    
    return $name;
}

function randomPath($depth = 3, $dirNameLength = 6, $filename = null): string {
    $parts = [];
    
    for ($i = 0; $i < $depth; $i++) {
        $parts[] = randomDirectoryName($dirNameLength);
    }
    
    if ($filename) {
        $parts[] = $filename;
    }
    
    return implode('/', $parts);
}

function temporaryFilename($prefix = 'tmp', $extension = 'tmp'): string {
    $timestamp = time();
    $random = random_int(1000, 9999);
    return "{$prefix}_{$timestamp}_{$random}.{$extension}";
}

echo "Random filenames:\n";
echo "- " . randomFilename('txt') . "\n";
echo "- " . randomFilename('jpg', 12) . "\n";
echo "- " . randomFilename('pdf', 6) . "\n";

echo "\nRandom directory names:\n";
echo "- " . randomDirectoryName() . "\n";
echo "- " . randomDirectoryName(15) . "\n";

echo "\nRandom paths:\n";
echo "- " . randomPath(2, 8, 'document.pdf') . "\n";
echo "- " . randomPath(4, 6) . "\n";

echo "\nTemporary filenames:\n";
echo "- " . temporaryFilename() . "\n";
echo "- " . temporaryFilename('cache', 'dat') . "\n";
echo "- " . temporaryFilename('upload', 'jpg') . "\n";

// Safe filename generation (removes unsafe characters)
function safeRandomFilename($baseName, $extension = 'txt'): string {
    $safeName = preg_replace('/[^a-zA-Z0-9_-]/', '_', $baseName);
    $random = random_int(100, 999);
    return $safeName . '_' . $random . '.' . $extension;
}

echo "\nSafe filenames:\n";
echo "- " . safeRandomFilename('My Document!', 'pdf') . "\n";
echo "- " . safeRandomFilename('user@email.com', 'txt') . "\n";
```

Random filename generation creates unique, filesystem-safe names using  
alphanumeric characters. Useful for temporary files, uploads, and  
preventing filename conflicts in file operations.  

## Random email generation

Creating random email addresses for testing and development.  

```php
<?php

function randomEmail(): string {
    $firstNames = ['john', 'jane', 'mike', 'sarah', 'david', 'emma', 'chris', 'anna'];
    $lastNames = ['smith', 'johnson', 'brown', 'davis', 'miller', 'wilson', 'moore', 'taylor'];
    $domains = ['gmail.com', 'yahoo.com', 'hotmail.com', 'outlook.com', 'example.com'];
    
    $firstName = $firstNames[array_rand($firstNames)];
    $lastName = $lastNames[array_rand($lastNames)];
    $domain = $domains[array_rand($domains)];
    
    $variations = [
        $firstName . $lastName,
        $firstName . '.' . $lastName,
        $firstName . '_' . $lastName,
        $firstName . random_int(10, 999),
        $firstName[0] . $lastName,
        $firstName . $lastName[0] . random_int(10, 99)
    ];
    
    $username = $variations[array_rand($variations)];
    return strtolower($username) . '@' . $domain;
}

function randomCorporateEmail($company = 'example'): string {
    $firstNames = ['alex', 'jordan', 'casey', 'taylor', 'morgan', 'riley', 'avery', 'sage'];
    $lastNames = ['adams', 'baker', 'clark', 'davis', 'evans', 'foster', 'garcia', 'harris'];
    
    $firstName = $firstNames[array_rand($firstNames)];
    $lastName = $lastNames[array_rand($lastNames)];
    
    $formats = [
        $firstName . '.' . $lastName,
        $firstName[0] . $lastName,
        $firstName . '_' . $lastName,
        $lastName . '.' . $firstName[0]
    ];
    
    $username = $formats[array_rand($formats)];
    return strtolower($username) . '@' . $company . '.com';
}

function randomTestEmail($testDomain = 'test.local'): string {
    $adjectives = ['happy', 'clever', 'bright', 'swift', 'kind', 'bold', 'calm', 'wise'];
    $animals = ['cat', 'dog', 'fox', 'owl', 'bee', 'ant', 'elk', 'bat'];
    
    $adjective = $adjectives[array_rand($adjectives)];
    $animal = $animals[array_rand($animals)];
    $number = random_int(1, 999);
    
    return $adjective . $animal . $number . '@' . $testDomain;
}

echo "Random emails:\n";
for ($i = 0; $i < 5; $i++) {
    echo "- " . randomEmail() . "\n";
}

echo "\nCorporate emails:\n";
for ($i = 0; $i < 3; $i++) {
    echo "- " . randomCorporateEmail('techcorp') . "\n";
}

echo "\nTest emails:\n";
for ($i = 0; $i < 3; $i++) {
    echo "- " . randomTestEmail() . "\n";
}

// Email with specific pattern
function patternEmail($pattern = '{first}.{last}{number}@{domain}'): string {
    $replacements = [
        '{first}' => ['alex', 'sam', 'jordan', 'casey'][array_rand(['alex', 'sam', 'jordan', 'casey'])],
        '{last}' => ['smith', 'jones', 'davis', 'brown'][array_rand(['smith', 'jones', 'davis', 'brown'])],
        '{number}' => random_int(10, 99),
        '{domain}' => ['example.com', 'test.org'][array_rand(['example.com', 'test.org'])]
    ];
    
    return str_replace(array_keys($replacements), $replacements, $pattern);
}

echo "\nPattern-based emails:\n";
echo "- " . patternEmail() . "\n";
echo "- " . patternEmail('{first}{number}@demo.com') . "\n";
```

Random email generation combines name parts, numbers, and domains to  
create realistic email addresses. Useful for testing user registration,  
email validation, and creating sample datasets.  

## Random phone numbers

Generating random phone numbers in various formats.  

```php
<?php

function randomPhoneUS(): string {
    $areaCode = random_int(200, 999);
    $exchange = random_int(200, 999);
    $number = random_int(1000, 9999);
    
    return "($areaCode) $exchange-$number";
}

function randomPhoneInternational(): string {
    $countryCodes = ['+1', '+44', '+33', '+49', '+81', '+86', '+61', '+39'];
    $countryCode = $countryCodes[array_rand($countryCodes)];
    
    $length = random_int(8, 12);
    $number = '';
    
    for ($i = 0; $i < $length; $i++) {
        $number .= random_int(0, 9);
    }
    
    return $countryCode . ' ' . $number;
}

function randomPhoneMobile(): string {
    $prefixes = ['555', '123', '456', '789', '321', '654', '987'];
    $prefix = $prefixes[array_rand($prefixes)];
    $middle = random_int(100, 999);
    $last = random_int(1000, 9999);
    
    return "$prefix-$middle-$last";
}

function formatPhoneNumber($digits, $format = 'US'): string {
    switch ($format) {
        case 'US':
            return '(' . substr($digits, 0, 3) . ') ' . 
                   substr($digits, 3, 3) . '-' . 
                   substr($digits, 6, 4);
        case 'EU':
            return '+' . substr($digits, 0, 2) . ' ' . 
                   substr($digits, 2, 3) . ' ' . 
                   substr($digits, 5, 3) . ' ' . 
                   substr($digits, 8);
        case 'DOTS':
            return substr($digits, 0, 3) . '.' . 
                   substr($digits, 3, 3) . '.' . 
                   substr($digits, 6, 4);
        default:
            return $digits;
    }
}

function randomFormattedPhone($format = 'US'): string {
    $digitsNeeded = match($format) {
        'US' => 10,
        'EU' => 12,
        'DOTS' => 10,
        default => 10
    };
    
    $digits = '';
    for ($i = 0; $i < $digitsNeeded; $i++) {
        $digits .= random_int($i === 0 ? 1 : 0, 9);
    }
    
    return formatPhoneNumber($digits, $format);
}

echo "US format phones:\n";
for ($i = 0; $i < 3; $i++) {
    echo "- " . randomPhoneUS() . "\n";
}

echo "\nInternational phones:\n";
for ($i = 0; $i < 3; $i++) {
    echo "- " . randomPhoneInternational() . "\n";
}

echo "\nMobile phones:\n";
for ($i = 0; $i < 3; $i++) {
    echo "- " . randomPhoneMobile() . "\n";
}

echo "\nFormatted phones:\n";
echo "- US: " . randomFormattedPhone('US') . "\n";
echo "- EU: " . randomFormattedPhone('EU') . "\n";
echo "- Dots: " . randomFormattedPhone('DOTS') . "\n";

// Specific area codes
function randomPhoneByArea($areaCode): string {
    $exchange = random_int(200, 999);
    $number = random_int(1000, 9999);
    return "($areaCode) $exchange-$number";
}

echo "\nArea-specific phones:\n";
echo "- NYC: " . randomPhoneByArea(212) . "\n";
echo "- LA: " . randomPhoneByArea(310) . "\n";
```

Phone number generation creates realistic numbers in various formats.  
Different regions have specific patterns and area codes. Useful for  
testing forms, contact databases, and user registration systems.  

## Random mathematical sequences

Generating random mathematical patterns and sequences.  

```php
<?php

function randomArithmeticSequence($length = 10): array {
    $start = random_int(1, 50);
    $difference = random_int(1, 10);
    $sequence = [];
    
    for ($i = 0; $i < $length; $i++) {
        $sequence[] = $start + ($i * $difference);
    }
    
    return $sequence;
}

function randomGeometricSequence($length = 8): array {
    $start = random_int(1, 10);
    $ratio = random_int(2, 4);
    $sequence = [];
    $current = $start;
    
    for ($i = 0; $i < $length; $i++) {
        $sequence[] = $current;
        $current *= $ratio;
        if ($current > 1000000) break; // Prevent overflow
    }
    
    return $sequence;
}

function randomFibonacciLike($length = 10): array {
    $a = random_int(1, 10);
    $b = random_int(1, 10);
    $sequence = [$a, $b];
    
    for ($i = 2; $i < $length; $i++) {
        $next = $sequence[$i-1] + $sequence[$i-2];
        $sequence[] = $next;
    }
    
    return $sequence;
}

function randomPolynomialValues($degree = 2, $points = 10): array {
    $coefficients = [];
    for ($i = 0; $i <= $degree; $i++) {
        $coefficients[] = random_int(-5, 5);
    }
    
    $values = [];
    for ($x = 1; $x <= $points; $x++) {
        $y = 0;
        for ($i = 0; $i <= $degree; $i++) {
            $y += $coefficients[$i] * pow($x, $i);
        }
        $values[] = ['x' => $x, 'y' => $y];
    }
    
    return $values;
}

function randomPrimeList($count = 10): array {
    $primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97];
    return randomSample($primes, min($count, count($primes)));
}

function randomSample($array, $count) {
    shuffle($array);
    return array_slice($array, 0, $count);
}

$arithmetic = randomArithmeticSequence(8);
$geometric = randomGeometricSequence(6);
$fibLike = randomFibonacciLike(8);
$polynomial = randomPolynomialValues(2, 5);
$primes = randomPrimeList(7);

echo "Arithmetic sequence: " . implode(', ', $arithmetic) . "\n";
echo "Geometric sequence: " . implode(', ', $geometric) . "\n";
echo "Fibonacci-like: " . implode(', ', $fibLike) . "\n";

echo "Polynomial values:\n";
foreach ($polynomial as $point) {
    echo "  f({$point['x']}) = {$point['y']}\n";
}

echo "Random primes: " . implode(', ', $primes) . "\n";

// Statistical distributions
function randomNormalDistribution($mean = 0, $stdDev = 1, $count = 10): array {
    $values = [];
    
    for ($i = 0; $i < $count; $i++) {
        // Box-Muller transformation
        $u1 = mt_rand() / mt_getrandmax();
        $u2 = mt_rand() / mt_getrandmax();
        
        $z0 = sqrt(-2 * log($u1)) * cos(2 * pi() * $u2);
        $value = $z0 * $stdDev + $mean;
        $values[] = round($value, 2);
    }
    
    return $values;
}

$normalDist = randomNormalDistribution(100, 15, 8);
echo "Normal distribution (μ=100, σ=15): " . implode(', ', $normalDist) . "\n";
```

Mathematical sequence generation creates random number patterns following  
arithmetic, geometric, or other mathematical rules. Useful for testing  
algorithms, generating sample data, and educational examples.  

## Random enum and choice values

Selecting random values from predefined sets and enumerations.  

```php
<?php

enum Status: string {
    case ACTIVE = 'active';
    case INACTIVE = 'inactive';
    case PENDING = 'pending';
    case SUSPENDED = 'suspended';
}

enum Priority: int {
    case LOW = 1;
    case MEDIUM = 2;
    case HIGH = 3;
    case CRITICAL = 4;
}

function randomEnum($enumClass) {
    $cases = $enumClass::cases();
    return $cases[array_rand($cases)];
}

function randomEnumValue($enumClass) {
    return randomEnum($enumClass)->value;
}

// Traditional choice arrays
$colors = ['red', 'blue', 'green', 'yellow', 'purple'];
$sizes = ['XS', 'S', 'M', 'L', 'XL', 'XXL'];
$categories = ['electronics', 'clothing', 'books', 'sports', 'home'];

function randomChoice(array $choices) {
    return $choices[array_rand($choices)];
}

function randomChoices(array $choices, int $count): array {
    $selected = [];
    for ($i = 0; $i < $count; $i++) {
        $selected[] = randomChoice($choices);
    }
    return $selected;
}

// Multiple choice with weights
$productTypes = [
    'laptop' => 20,
    'phone' => 35,
    'tablet' => 15,
    'watch' => 25,
    'headphones' => 5
];

function weightedChoice(array $weighted): string {
    $total = array_sum($weighted);
    $random = random_int(1, $total);
    
    foreach ($weighted as $item => $weight) {
        $random -= $weight;
        if ($random <= 0) {
            return $item;
        }
    }
    
    return array_key_first($weighted);
}

echo "Random enum values:\n";
echo "- Status: " . randomEnum(Status::class)->value . "\n";
echo "- Priority: " . randomEnum(Priority::class)->value . "\n";

echo "\nRandom choices:\n";
echo "- Color: " . randomChoice($colors) . "\n";
echo "- Size: " . randomChoice($sizes) . "\n";
echo "- Category: " . randomChoice($categories) . "\n";

echo "\nMultiple random choices:\n";
$multiColors = randomChoices($colors, 3);
echo "- 3 colors: " . implode(', ', $multiColors) . "\n";

echo "\nWeighted selection:\n";
for ($i = 0; $i < 5; $i++) {
    echo "- Product: " . weightedChoice($productTypes) . "\n";
}

// Configuration options
$configurations = [
    'theme' => ['light', 'dark', 'auto'],
    'language' => ['en', 'fr', 'de', 'es', 'it'],
    'timezone' => ['UTC', 'EST', 'PST', 'CET', 'JST'],
    'currency' => ['USD', 'EUR', 'GBP', 'JPY', 'CAD']
];

function randomConfiguration(array $config): array {
    $result = [];
    foreach ($config as $key => $options) {
        $result[$key] = randomChoice($options);
    }
    return $result;
}

$randomConfig = randomConfiguration($configurations);
echo "\nRandom configuration:\n";
foreach ($randomConfig as $key => $value) {
    echo "- $key: $value\n";
}
```

Enum and choice randomization selects values from predefined sets.  
Modern PHP enums provide type-safe options, while traditional arrays  
offer flexibility. Weighted selection allows probability control.  

## Random object and class instances

Creating random instances of objects and classes with random properties.  

```php
<?php

class RandomUser {
    public function __construct(
        public string $name,
        public int $age,
        public string $email,
        public bool $active,
        public array $preferences
    ) {}
    
    public static function random(): self {
        $names = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank'];
        $domains = ['gmail.com', 'yahoo.com', 'outlook.com'];
        $preferences = ['dark_mode', 'notifications', 'newsletter', 'analytics'];
        
        $name = $names[array_rand($names)];
        $age = random_int(18, 65);
        $email = strtolower($name) . random_int(10, 999) . '@' . $domains[array_rand($domains)];
        $active = (bool) random_int(0, 1);
        
        // Random preferences (0-4 items)
        $prefCount = random_int(0, count($preferences));
        $selectedPrefs = array_slice($preferences, 0, $prefCount);
        shuffle($selectedPrefs);
        
        return new self($name, $age, $email, $active, array_slice($selectedPrefs, 0, $prefCount));
    }
}

class RandomProduct {
    public function __construct(
        public string $name,
        public float $price,
        public string $category,
        public int $stock,
        public array $tags
    ) {}
    
    public static function random(): self {
        $adjectives = ['Super', 'Mega', 'Ultra', 'Pro', 'Premium', 'Deluxe'];
        $nouns = ['Widget', 'Gadget', 'Tool', 'Device', 'Kit', 'Set'];
        $categories = ['electronics', 'home', 'sports', 'books', 'clothing'];
        $tags = ['popular', 'new', 'sale', 'recommended', 'bestseller'];
        
        $name = $adjectives[array_rand($adjectives)] . ' ' . $nouns[array_rand($nouns)];
        $price = round(random_int(999, 99999) / 100, 2);
        $category = $categories[array_rand($categories)];
        $stock = random_int(0, 100);
        
        // Random tags (1-3 items)
        $tagCount = random_int(1, 3);
        $selectedTags = array_slice($tags, 0, $tagCount);
        shuffle($selectedTags);
        
        return new self($name, $price, $category, $stock, array_slice($selectedTags, 0, $tagCount));
    }
}

class RandomDataGenerator {
    public static function users(int $count): array {
        $users = [];
        for ($i = 0; $i < $count; $i++) {
            $users[] = RandomUser::random();
        }
        return $users;
    }
    
    public static function products(int $count): array {
        $products = [];
        for ($i = 0; $i < $count; $i++) {
            $products[] = RandomProduct::random();
        }
        return $products;
    }
    
    public static function mixed(array $types, int $totalCount): array {
        $results = [];
        
        for ($i = 0; $i < $totalCount; $i++) {
            $type = $types[array_rand($types)];
            $results[] = $type::random();
        }
        
        return $results;
    }
}

// Generate random instances
$user = RandomUser::random();
$product = RandomProduct::random();

echo "Random User:\n";
echo "  Name: {$user->name}\n";
echo "  Age: {$user->age}\n";
echo "  Email: {$user->email}\n";
echo "  Active: " . ($user->active ? 'Yes' : 'No') . "\n";
echo "  Preferences: " . implode(', ', $user->preferences) . "\n";

echo "\nRandom Product:\n";
echo "  Name: {$product->name}\n";
echo "  Price: $" . number_format($product->price, 2) . "\n";
echo "  Category: {$product->category}\n";
echo "  Stock: {$product->stock}\n";
echo "  Tags: " . implode(', ', $product->tags) . "\n";

// Generate collections
$users = RandomDataGenerator::users(3);
$products = RandomDataGenerator::products(2);

echo "\nRandom Users Collection:\n";
foreach ($users as $i => $user) {
    echo "  " . ($i + 1) . ". {$user->name} ({$user->age}) - {$user->email}\n";
}

echo "\nRandom Products Collection:\n";
foreach ($products as $i => $product) {
    echo "  " . ($i + 1) . ". {$product->name} - $" . number_format($product->price, 2) . "\n";
}
```

Random object generation creates instances with randomized properties.  
Static factory methods provide clean APIs for generating test data,  
while generator classes can create collections of related objects.  

## Random performance and load testing

Generating random values for performance testing and load simulation.  

```php
<?php

class LoadTestGenerator {
    public static function randomDelay($minMs = 100, $maxMs = 1000): int {
        return random_int($minMs, $maxMs) * 1000; // Convert to microseconds
    }
    
    public static function randomRequestSize(): int {
        // Simulate different request sizes in bytes
        $sizes = [
            512,    // Small request
            2048,   // Medium request  
            8192,   // Large request
            32768   // Very large request
        ];
        
        $weights = [40, 35, 20, 5]; // Percentage distribution
        $random = random_int(1, 100);
        $cumulative = 0;
        
        foreach ($weights as $i => $weight) {
            $cumulative += $weight;
            if ($random <= $cumulative) {
                return $sizes[$i];
            }
        }
        
        return $sizes[0];
    }
    
    public static function randomUserAgent(): string {
        $userAgents = [
            'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
            'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36',
            'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36',
            'Mozilla/5.0 (iPhone; CPU iPhone OS 15_0 like Mac OS X)',
            'Mozilla/5.0 (Android 11; Mobile; rv:68.0) Gecko/68.0'
        ];
        
        return $userAgents[array_rand($userAgents)];
    }
    
    public static function randomLoadProfile(): array {
        return [
            'concurrent_users' => random_int(10, 1000),
            'requests_per_second' => random_int(50, 5000),
            'duration_seconds' => random_int(60, 3600),
            'ramp_up_seconds' => random_int(10, 300),
            'think_time_ms' => random_int(500, 5000)
        ];
    }
    
    public static function randomResponseTime(): array {
        // Simulate realistic response time distribution
        $baseTime = random_int(50, 200); // Base response time
        
        // Add random spikes (5% chance of slow response)
        if (random_int(1, 100) <= 5) {
            $baseTime += random_int(2000, 10000);
        }
        
        return [
            'response_time_ms' => $baseTime,
            'dns_lookup_ms' => random_int(5, 50),
            'connection_ms' => random_int(10, 100),
            'ssl_handshake_ms' => random_int(50, 200),
            'time_to_first_byte_ms' => random_int(100, 500)
        ];
    }
}

class BenchmarkDataGenerator {
    public static function randomDataset(int $size): array {
        $data = [];
        
        for ($i = 0; $i < $size; $i++) {
            $data[] = [
                'id' => $i + 1,
                'value' => random_int(1, 1000000),
                'category' => chr(65 + random_int(0, 25)), // A-Z
                'timestamp' => time() - random_int(0, 86400 * 30) // Last 30 days
            ];
        }
        
        return $data;
    }
    
    public static function randomOperationMix(): array {
        $operations = ['read', 'write', 'update', 'delete'];
        $weights = [60, 20, 15, 5]; // Typical CRUD distribution
        
        $mix = [];
        foreach ($operations as $i => $op) {
            $mix[$op] = $weights[$i];
        }
        
        return $mix;
    }
    
    public static function randomMemoryUsage(): array {
        $baseUsage = random_int(100, 500); // MB
        $peakUsage = $baseUsage + random_int(50, 200);
        
        return [
            'initial_mb' => $baseUsage,
            'peak_mb' => $peakUsage,
            'final_mb' => $baseUsage + random_int(-20, 50),
            'gc_collections' => random_int(5, 50)
        ];
    }
}

// Generate load test parameters
$loadProfile = LoadTestGenerator::randomLoadProfile();
$responseTime = LoadTestGenerator::randomResponseTime();
$requestSize = LoadTestGenerator::randomRequestSize();

echo "Load Test Configuration:\n";
foreach ($loadProfile as $key => $value) {
    echo "  " . ucwords(str_replace('_', ' ', $key)) . ": $value\n";
}

echo "\nSample Response Time:\n";
foreach ($responseTime as $key => $value) {
    echo "  " . ucwords(str_replace('_', ' ', $key)) . ": $value\n";
}

echo "\nRequest Details:\n";
echo "  Size: " . number_format($requestSize) . " bytes\n";
echo "  User Agent: " . LoadTestGenerator::randomUserAgent() . "\n";
echo "  Delay: " . (LoadTestGenerator::randomDelay() / 1000) . " ms\n";

// Generate benchmark data
$dataset = BenchmarkDataGenerator::randomDataset(5);
$operationMix = BenchmarkDataGenerator::randomOperationMix();
$memoryUsage = BenchmarkDataGenerator::randomMemoryUsage();

echo "\nBenchmark Dataset Sample (5 records):\n";
foreach ($dataset as $record) {
    echo "  ID {$record['id']}: Value={$record['value']}, Category={$record['category']}\n";
}

echo "\nOperation Mix:\n";
foreach ($operationMix as $op => $percentage) {
    echo "  " . ucfirst($op) . ": $percentage%\n";
}

echo "\nMemory Usage Profile:\n";
foreach ($memoryUsage as $key => $value) {
    echo "  " . ucwords(str_replace('_', ' ', $key)) . ": $value\n";
}

// Simulate load test execution
function simulateLoadTest(int $duration = 10): void {
    echo "\nSimulating load test for $duration seconds:\n";
    
    $start = time();
    while (time() - $start < $duration) {
        $responseTime = random_int(50, 500);
        $statusCode = random_int(1, 100) <= 95 ? 200 : [400, 404, 500][array_rand([400, 404, 500])];
        
        echo "  Response: {$responseTime}ms - HTTP $statusCode\n";
        usleep(1000000); // 1 second delay
    }
}

simulateLoadTest(3);
```

Performance testing randomization generates realistic load patterns,  
response times, and system metrics. This data helps simulate various  
conditions for stress testing and capacity planning scenarios.  
