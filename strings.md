# Strings

This chapter covers 35 comprehensive PHP string examples designed to  
demonstrate the full range of string manipulation capabilities in modern  
PHP 8.4. These examples progress from basic string operations to advanced  
text processing techniques including regular expressions, encoding  
conversions, and specialized string utilities. Each example includes  
detailed explanations to help you master PHP string development.  

## String creation and literals

Different ways to create and define string values.  

```php
<?php

$singleQuoted = 'Hello there!';
$doubleQuoted = "Hello there!";
$empty = '';
$withSpaces = '  Welcome to PHP  ';
$multiline = 'Line one
Line two
Line three';

echo $singleQuoted . "\n";
echo $doubleQuoted . "\n"; 
echo strlen($empty) . "\n";        // 0
echo trim($withSpaces) . "\n";     // Welcome to PHP
echo $multiline . "\n";
```

Single quotes preserve literal text while double quotes enable variable  
interpolation and escape sequences. Both create string literals, but  
double quotes process embedded variables and special characters.  

## String concatenation

Combining multiple strings into a single string value.  

```php
<?php

$first = 'Hello';
$second = 'World';
$space = ' ';

$result1 = $first . $space . $second;
$result2 = $first . ', ' . $second . '!';

// Concatenation assignment
$message = 'PHP';
$message .= ' is';
$message .= ' awesome';

echo $result1 . "\n";      // Hello World
echo $result2 . "\n";      // Hello, World!
echo $message . "\n";      // PHP is awesome
```

The dot operator (.) concatenates strings in PHP. The .= operator  
appends to existing strings. Concatenation is left-associative and  
can chain multiple strings together efficiently.  

## String interpolation

Embedding variables and expressions within double-quoted strings.  

```php
<?php

$name = 'Alice';
$age = 30;
$items = ['apple', 'banana', 'cherry'];

echo "My name is $name\n";
echo "I am $age years old\n";
echo "Next year I'll be " . ($age + 1) . "\n";
echo "I have " . count($items) . " favorite fruits\n";

// Complex interpolation with curly braces
echo "My name is {$name} and I'm {$age} years old\n";
echo "First item: {$items[0]}\n";
```

Double-quoted strings process variable names prefixed with $. Complex  
expressions require concatenation or curly braces for clarity.  
Simple variables can be embedded directly within the string.  

## Escape sequences

Using backslash notation for special characters in strings.  

```php
<?php

$quote = "She said, \"Hello there!\"";
$path = "C:\\Users\\Alice\\Documents";
$tab = "Name\tAge\tCity";
$newlines = "Line 1\nLine 2\nLine 3";
$unicode = "Unicode heart: \u{2764}";

echo $quote . "\n";
echo $path . "\n";
echo $tab . "\n";
echo $newlines . "\n";
echo $unicode . "\n";

// Raw string without escape processing
$raw = 'Literal backslash: \n and quote: \'';
echo $raw . "\n";
```

Double quotes process escape sequences like \n, \t, \", and \\. Single  
quotes only process \' and \\. Unicode sequences use \u{codepoint}  
format for international characters and symbols.  

## Heredoc and nowdoc syntax

Multi-line string literals with flexible formatting options.  

```php
<?php

$name = 'Bob';
$age = 25;

// Heredoc (processes variables and escapes)
$heredoc = <<<EOT
Hello there, $name!
You are $age years old.
This is a multi-line string
with variable interpolation.
EOT;

// Nowdoc (literal text, no processing)
$nowdoc = <<<'END'
This is literal text.
Variables like $name are not processed.
Escape sequences like \n are preserved.
END;

echo $heredoc . "\n\n";
echo $nowdoc . "\n";
```

Heredoc syntax uses <<< followed by an identifier, processes variables  
like double quotes. Nowdoc uses quoted identifier, preserves literal  
text like single quotes. Both support multi-line formatting.  

## String length and character access

Measuring string length and accessing individual characters.  

```php
<?php

$text = 'Hello World';
$unicode = 'Héllo Wörld';
$empty = '';

echo strlen($text) . "\n";           // 11
echo mb_strlen($unicode) . "\n";     // 11 (multibyte aware)
echo strlen($empty) . "\n";          // 0

// Character access by position
echo $text[0] . "\n";                // H
echo $text[6] . "\n";                // W
echo $text[-1] . "\n";               // d (last character, PHP 7.1+)

// Safe character access
$pos = 5;
if ($pos < strlen($text)) {
    echo "Character at position $pos: " . $text[$pos] . "\n";
}
```

Use strlen() for byte length, mb_strlen() for character count with  
multi-byte strings. Square brackets access characters by zero-based  
index. Negative indices count from the end (PHP 7.1+).  

## Case conversion functions

Converting string case for consistency and formatting.  

```php
<?php

$mixed = 'Hello WORLD from PHP';

echo strtolower($mixed) . "\n";      // hello world from php
echo strtoupper($mixed) . "\n";      // HELLO WORLD FROM PHP
echo ucfirst($mixed) . "\n";         // Hello WORLD from PHP
echo lcfirst($mixed) . "\n";         // hello WORLD from PHP
echo ucwords($mixed) . "\n";         // Hello WORLD From PHP

// Title case for proper names
$name = 'john doe smith';
echo ucwords($name) . "\n";          // John Doe Smith

// Multi-byte case conversion
$unicode = 'héllo wörld';
echo mb_strtoupper($unicode) . "\n"; // HÉLLO WÖRLD
```

Case conversion functions transform string capitalization.  
Use mb_* variants for international characters. ucwords()  
capitalizes word beginnings, useful for names and titles.  

## Substring operations

Extracting portions of strings by position and length.  

```php
<?php

$text = 'The quick brown fox jumps';

echo substr($text, 0, 3) . "\n";     // The
echo substr($text, 4, 5) . "\n";     // quick
echo substr($text, 10) . "\n";       // brown fox jumps
echo substr($text, -5) . "\n";       // jumps
echo substr($text, -10, 5) . "\n";   // brown

// Multi-byte substring
$unicode = 'Thé qüick bröwn föx';
echo mb_substr($unicode, 0, 3) . "\n";  // Thé

// Safe substring with bounds checking
$start = 4;
$length = 5;
if ($start < strlen($text)) {
    echo substr($text, $start, $length) . "\n";
}
```

substr() extracts string portions by start position and optional length.  
Negative positions count from string end. Use mb_substr() for multi-byte  
strings to handle international characters correctly.  

## String trimming

Removing whitespace and characters from string boundaries.  

```php
<?php

$padded = '  Hello World  ';
$messy = "\t\nHello World\r\n";
$chars = '...Hello World...';

echo "'" . trim($padded) . "'\n";           // 'Hello World'
echo "'" . ltrim($padded) . "'\n";          // 'Hello World  '
echo "'" . rtrim($padded) . "'\n";          // '  Hello World'

echo "'" . trim($messy) . "'\n";            // 'Hello World'

// Trim specific characters
echo trim($chars, '.') . "\n";             // Hello World
echo trim('++Hello World++', '+') . "\n";  // Hello World

// Trim character sets
echo trim(' \t\nHello\t\n ', " \t\n") . "\n"; // Hello
```

trim() removes whitespace from both ends, ltrim() from left, rtrim()  
from right. Optional second parameter specifies characters to remove.  
Default removes spaces, tabs, newlines, and carriage returns.  

## String replacement

Replacing occurrences of substrings with new values.  

```php
<?php

$text = 'The quick brown fox jumps over the lazy dog';

echo str_replace('fox', 'cat', $text) . "\n";
echo str_replace('the', 'a', $text) . "\n";

// Case-insensitive replacement
echo str_ireplace('THE', 'a', $text) . "\n";

// Multiple replacements
$search = ['quick', 'brown', 'fox'];
$replace = ['slow', 'red', 'turtle'];
echo str_replace($search, $replace, $text) . "\n";

// Count replacements
$count = 0;
$result = str_replace('the', 'a', $text, $count);
echo "Replaced $count occurrences: $result\n";
```

str_replace() substitutes all occurrences of search strings. Arrays  
enable multiple simultaneous replacements. str_ireplace() ignores  
case. Optional count parameter reports replacement frequency.  

## String reversal and repetition

Creating reversed and repeated string patterns.  

```php
<?php

$text = 'Hello World';
$pattern = 'ABC';

echo strrev($text) . "\n";           // dlroW olleH
echo str_repeat($pattern, 3) . "\n"; // ABCABCABC
echo str_repeat('-', 20) . "\n";     // --------------------

// Practical applications
$border = str_repeat('=', 30);
echo $border . "\n";
echo "  Welcome to PHP Strings  \n";
echo $border . "\n";

// Creating patterns
echo str_repeat('*-', 10) . "\n";    // *-*-*-*-*-*-*-*-*-*-
```

strrev() reverses character order in strings. str_repeat() duplicates  
strings a specified number of times. Useful for creating borders,  
patterns, and formatting visual elements.  

## String searching and positioning

Finding substrings and their positions within larger strings.  

```php
<?php

$text = 'The quick brown fox jumps over the lazy dog';

echo strpos($text, 'fox') . "\n";        // 16
echo strrpos($text, 'the') . "\n";       // 32 (last occurrence)
echo strpos($text, 'cat') . "\n";        // (empty - not found)

// Case-insensitive search
echo stripos($text, 'FOX') . "\n";       // 16

// Check if substring exists
if (strpos($text, 'quick') !== false) {
    echo "Found 'quick' in text\n";
}

// Modern PHP 8+ functions
echo str_contains($text, 'fox') ? 'Contains fox' : 'No fox' . "\n";
echo str_starts_with($text, 'The') ? 'Starts with The' : 'Different start' . "\n";
echo str_ends_with($text, 'dog') ? 'Ends with dog' : 'Different end' . "\n";
```

strpos() finds first occurrence position, strrpos() finds last.  
Always use strict comparison (!== false) since position 0 is falsy.  
PHP 8+ adds str_contains(), str_starts_with(), str_ends_with().  

## String splitting and joining

Breaking strings into arrays and combining arrays into strings.  

```php
<?php

$csv = 'apple,banana,cherry,date';
$sentence = 'The quick brown fox';
$path = '/home/user/documents/file.txt';

$fruits = explode(',', $csv);
$words = explode(' ', $sentence);
$dirs = explode('/', $path);

print_r($fruits);     // Array of fruits
print_r($words);      // Array of words

// Join arrays back to strings
echo implode(' | ', $fruits) . "\n";     // apple | banana | cherry | date
echo implode('-', $words) . "\n";        // The-quick-brown-fox

// Advanced splitting with limits
$limited = explode(',', $csv, 2);        // ['apple', 'banana,cherry,date']
print_r($limited);
```

explode() splits strings into arrays using delimiters. implode() joins  
array elements with separators. Optional limit parameter controls  
maximum array elements during splitting.  

## String comparison

Comparing strings for equality, ordering, and similarity.  

```php
<?php

$str1 = 'Hello';
$str2 = 'hello';
$str3 = 'Hello';

// Case-sensitive comparison
echo ($str1 === $str3) ? 'Equal' : 'Not equal' . "\n";      // Equal
echo ($str1 === $str2) ? 'Equal' : 'Not equal' . "\n";      // Not equal

// Case-insensitive comparison
echo (strcasecmp($str1, $str2) === 0) ? 'Same' : 'Different' . "\n"; // Same

// String ordering
echo strcmp('apple', 'banana') . "\n";    // Negative (apple < banana)
echo strcmp('banana', 'apple') . "\n";    // Positive (banana > apple)

// Natural order comparison
$files = ['file1.txt', 'file10.txt', 'file2.txt'];
usort($files, 'strnatcmp');
print_r($files);      // Natural order: file1.txt, file2.txt, file10.txt
```

Use === for exact equality, strcasecmp() for case-insensitive  
comparison. strcmp() returns negative, zero, or positive for ordering.  
strnatcmp() provides natural ordering for numeric strings.  

## String pattern matching

Using wildcards and patterns for flexible string matching.  

```php
<?php

$filename = 'document.pdf';
$email = 'user@example.com';
$phone = '555-123-4567';

// Simple pattern matching with fnmatch
echo fnmatch('*.pdf', $filename) ? 'PDF file' : 'Not PDF' . "\n";
echo fnmatch('*.txt', $filename) ? 'Text file' : 'Not text' . "\n";

// Wildcard patterns
$patterns = ['*.pdf', '*.doc*', '*.txt'];
foreach ($patterns as $pattern) {
    if (fnmatch($pattern, $filename)) {
        echo "Matches pattern: $pattern\n";
    }
}

// Custom pattern function
function matchesPattern($text, $pattern) {
    $regex = str_replace(['*', '?'], ['.*', '.'], $pattern);
    return preg_match("/^$regex$/i", $text);
}

echo matchesPattern($email, '*@*.com') ? 'Valid email pattern' : 'Invalid' . "\n";
```

fnmatch() provides shell-style pattern matching with * and ? wildcards.  
Custom pattern functions can convert wildcards to regular expressions  
for more flexible matching capabilities.  

## Content testing functions

Testing string content for specific characteristics and properties.  

```php
<?php

$numeric = '12345';
$alpha = 'HelloWorld';
$alphanumeric = 'Hello123';
$mixed = 'Hello, World!';

// Content type testing
echo is_numeric($numeric) ? 'Numeric' : 'Not numeric' . "\n";
echo ctype_alpha($alpha) ? 'Alphabetic' : 'Not alphabetic' . "\n";
echo ctype_alnum($alphanumeric) ? 'Alphanumeric' : 'Not alphanumeric' . "\n";
echo ctype_digit($numeric) ? 'All digits' : 'Not all digits' . "\n";

// Character class testing
echo ctype_upper('HELLO') ? 'All uppercase' : 'Mixed case' . "\n";
echo ctype_lower('hello') ? 'All lowercase' : 'Mixed case' . "\n";
echo ctype_space("   \t\n") ? 'All whitespace' : 'Contains non-space' . "\n";

// Practical validation
function isValidUsername($username) {
    return ctype_alnum($username) && strlen($username) >= 3;
}

echo isValidUsername('user123') ? 'Valid username' : 'Invalid username' . "\n";
```

ctype_* functions test string character classes efficiently. is_numeric()  
tests for numeric values including floats. These functions enable robust  
input validation and content filtering.  

## String starts with and ends with

Testing string prefixes and suffixes for conditional processing.  

```php
<?php

$url = 'https://www.example.com/path';
$filename = 'document.pdf';
$message = 'Error: Invalid input provided';

// PHP 8+ native functions
echo str_starts_with($url, 'https://') ? 'Secure URL' : 'Insecure URL' . "\n";
echo str_ends_with($filename, '.pdf') ? 'PDF file' : 'Other file' . "\n";
echo str_starts_with($message, 'Error:') ? 'Error message' : 'Normal message' . "\n";

// Multiple prefix/suffix checking
$secureProtocols = ['https://', 'ftps://', 'sftp://'];
$isSecure = false;
foreach ($secureProtocols as $protocol) {
    if (str_starts_with($url, $protocol)) {
        $isSecure = true;
        break;
    }
}
echo $isSecure ? 'Secure protocol' : 'Insecure protocol' . "\n";

// File extension checking
$imageExtensions = ['.jpg', '.png', '.gif', '.webp'];
$isImage = false;
foreach ($imageExtensions as $ext) {
    if (str_ends_with(strtolower($filename), $ext)) {
        $isImage = true;
        break;
    }
}
```

str_starts_with() and str_ends_with() provide clean prefix/suffix  
testing. These functions are case-sensitive and more readable than  
substr() comparisons for boundary checking.  

## Character type checking

Analyzing individual characters and their properties.  

```php
<?php

$text = 'Hello World 123!';

for ($i = 0; $i < strlen($text); $i++) {
    $char = $text[$i];
    $types = [];
    
    if (ctype_alpha($char)) $types[] = 'letter';
    if (ctype_digit($char)) $types[] = 'digit';
    if (ctype_space($char)) $types[] = 'space';
    if (ctype_punct($char)) $types[] = 'punctuation';
    if (ctype_upper($char)) $types[] = 'uppercase';
    if (ctype_lower($char)) $types[] = 'lowercase';
    
    echo "Character '$char': " . implode(', ', $types) . "\n";
}

// Character frequency analysis
function analyzeCharacters($text) {
    $stats = [
        'letters' => 0,
        'digits' => 0,
        'spaces' => 0,
        'punctuation' => 0
    ];
    
    for ($i = 0; $i < strlen($text); $i++) {
        $char = $text[$i];
        if (ctype_alpha($char)) $stats['letters']++;
        elseif (ctype_digit($char)) $stats['digits']++;
        elseif (ctype_space($char)) $stats['spaces']++;
        elseif (ctype_punct($char)) $stats['punctuation']++;
    }
    
    return $stats;
}

print_r(analyzeCharacters($text));
```

Character type functions analyze individual string characters.  
Combine multiple checks to classify characters into categories.  
Useful for text analysis and validation routines.  

## String encoding detection

Detecting and handling different character encodings.  

```php
<?php

$utf8Text = 'Hello, Wörld! 🌍';
$latinText = mb_convert_encoding('Café', 'ISO-8859-1', 'UTF-8');

// Detect encoding
echo mb_detect_encoding($utf8Text) . "\n";      // UTF-8
echo mb_detect_encoding($latinText) . "\n";     // ISO-8859-1

// Check if string is valid UTF-8
echo mb_check_encoding($utf8Text, 'UTF-8') ? 'Valid UTF-8' : 'Invalid UTF-8' . "\n";

// List supported encodings
$encodings = mb_list_encodings();
echo "Supported encodings: " . count($encodings) . "\n";

// Encoding conversion
$converted = mb_convert_encoding($latinText, 'UTF-8', 'ISO-8859-1');
echo "Converted: $converted\n";

// Set default encoding
mb_internal_encoding('UTF-8');
echo "Internal encoding: " . mb_internal_encoding() . "\n";

// Clean invalid characters
$cleaned = mb_convert_encoding($utf8Text, 'UTF-8', 'UTF-8');
echo "Cleaned: $cleaned\n";
```

Multibyte functions handle various character encodings properly.  
mb_detect_encoding() identifies text encoding, mb_convert_encoding()  
converts between encodings for international text processing.  

## Regular expressions

Pattern matching with powerful regex capabilities.  

```php
<?php

$text = 'Contact us at info@example.com or call (555) 123-4567';

// Basic pattern matching
if (preg_match('/\b\w+@\w+\.\w+\b/', $text, $matches)) {
    echo "Email found: " . $matches[0] . "\n";
}

// Extract all matches
preg_match_all('/\b\d{3}[-.]?\d{3}[-.]?\d{4}\b/', $text, $phones);
echo "Phone numbers: " . implode(', ', $phones[0]) . "\n";

// Pattern replacement
$cleaned = preg_replace('/[^\w\s]/', '', $text);
echo "Cleaned text: $cleaned\n";

// Named capture groups
$pattern = '/(?P<name>\w+)@(?P<domain>\w+\.\w+)/';
if (preg_match($pattern, $text, $matches)) {
    echo "Email parts - Name: {$matches['name']}, Domain: {$matches['domain']}\n";
}

// Validation function
function validateEmail($email) {
    return preg_match('/^[^\s@]+@[^\s@]+\.[^\s@]+$/', $email);
}

echo validateEmail('test@example.com') ? 'Valid email' : 'Invalid email' . "\n";
```

Regular expressions provide powerful pattern matching capabilities.  
preg_match() finds single matches, preg_match_all() finds all matches.  
Named groups and validation patterns enable complex text processing.  

## String parsing and extraction

Extracting structured data from formatted strings.  

```php
<?php

$logEntry = '2024-01-15 14:30:22 [ERROR] Database connection failed: timeout';
$csvLine = 'John,Doe,30,Engineer,New York';
$url = 'https://api.example.com/users/123?format=json&limit=10';

// Parse log entry
if (preg_match('/(\d{4}-\d{2}-\d{2}) (\d{2}:\d{2}:\d{2}) \[(\w+)\] (.+)/', $logEntry, $matches)) {
    $logData = [
        'date' => $matches[1],
        'time' => $matches[2],
        'level' => $matches[3],
        'message' => $matches[4]
    ];
    print_r($logData);
}

// Parse CSV data
$csvData = str_getcsv($csvLine);
$person = [
    'first_name' => $csvData[0],
    'last_name' => $csvData[1],
    'age' => (int)$csvData[2],
    'job' => $csvData[3],
    'city' => $csvData[4]
];
print_r($person);

// Parse URL components
$urlParts = parse_url($url);
parse_str($urlParts['query'], $queryParams);
echo "Host: {$urlParts['host']}\n";
echo "Path: {$urlParts['path']}\n";
print_r($queryParams);
```

PHP provides specialized parsing functions for common formats.  
str_getcsv() handles CSV data, parse_url() breaks down URLs.  
Regular expressions extract custom structured data patterns.  

## String formatting with sprintf

Creating formatted strings with placeholders and specifications.  

```php
<?php

$name = 'Alice';
$age = 30;
$balance = 1234.56;
$items = 42;

// Basic formatting
echo sprintf("Hello, %s! You are %d years old.\n", $name, $age);

// Number formatting
echo sprintf("Balance: $%.2f\n", $balance);                    // $1234.56
echo sprintf("Items: %04d\n", $items);                        // Items: 0042
echo sprintf("Percentage: %d%%\n", 85);                       // Percentage: 85%

// Alignment and padding
echo sprintf("|%-10s|%10s|\n", 'Left', 'Right');              // Left-aligned, right-aligned
echo sprintf("|%010d|\n", 123);                               // Zero-padded: |0000000123|

// Multiple format specifiers
$template = "User: %-10s Age: %3d Balance: $%8.2f Status: %s\n";
echo sprintf($template, 'John', 25, 1500.75, 'Active');
echo sprintf($template, 'Jane', 32, 2750.00, 'Premium');

// Date formatting
echo sprintf("Today is %s, %02d/%02d/%04d\n", 'Monday', 1, 15, 2024);
```

sprintf() creates formatted strings using format specifiers. %s for  
strings, %d for integers, %f for floats. Width, alignment, and  
padding options provide precise output formatting.  

## String padding and alignment

Adding padding characters for consistent string lengths and alignment.  

```php
<?php

$items = ['Apple', 'Banana', 'Cherry', 'Date'];
$prices = [1.50, 2.25, 3.75, 0.95];

// Left padding with spaces
foreach ($items as $item) {
    echo str_pad($item, 10, ' ', STR_PAD_LEFT) . " - Product\n";
}

echo "\n";

// Right padding
foreach ($items as $item) {
    echo str_pad($item, 10, ' ', STR_PAD_RIGHT) . " - Available\n";
}

echo "\n";

// Center padding
foreach ($items as $item) {
    echo str_pad($item, 12, ' ', STR_PAD_BOTH) . "\n";
}

// Custom padding characters
echo str_pad('TITLE', 20, '=', STR_PAD_BOTH) . "\n";          // =======TITLE========
echo str_pad('123', 8, '0', STR_PAD_LEFT) . "\n";            // 00000123

// Table formatting
echo "Product     Price\n";
echo str_repeat('-', 15) . "\n";
for ($i = 0; $i < count($items); $i++) {
    echo str_pad($items[$i], 10) . ' $' . number_format($prices[$i], 2) . "\n";
}
```

str_pad() adds padding characters to reach specified lengths.  
Supports left, right, and center alignment. Useful for creating  
formatted tables and aligned output displays.  

## String encoding conversions

Converting between different character encodings for compatibility.  

```php
<?php

$utf8Text = 'Café, naïve, résumé';
$unicodeText = 'Hello 🌍 World 🚀';

// Convert to different encodings
$latin1 = mb_convert_encoding($utf8Text, 'ISO-8859-1', 'UTF-8');
$ascii = mb_convert_encoding($utf8Text, 'ASCII//IGNORE', 'UTF-8');

echo "Original UTF-8: $utf8Text\n";
echo "Latin-1: $latin1\n";
echo "ASCII (ignored): $ascii\n";

// Handle encoding errors
$converted = mb_convert_encoding($unicodeText, 'ASCII//TRANSLIT', 'UTF-8');
echo "Transliterated: $converted\n";

// Encoding validation
echo mb_check_encoding($utf8Text, 'UTF-8') ? 'Valid UTF-8' : 'Invalid UTF-8' . "\n";

// Convert encoding and detect
$mystery = mb_convert_encoding('Hello', 'ISO-8859-1', 'UTF-8');
$detected = mb_detect_encoding($mystery, ['UTF-8', 'ISO-8859-1', 'ASCII']);
echo "Detected encoding: $detected\n";

// Bulk conversion
$texts = ['café', 'naïve', 'résumé'];
$converted = array_map(function($text) {
    return mb_convert_encoding($text, 'ASCII//TRANSLIT', 'UTF-8');
}, $texts);
print_r($converted);
```

Multibyte functions handle encoding conversions safely. Use //IGNORE  
to skip invalid characters, //TRANSLIT to approximate characters.  
Essential for international text processing and data migration.  

## HTML entity handling

Converting between plain text and HTML entities for web safety.  

```php
<?php

$userInput = '<script>alert("XSS")</script>';
$htmlContent = '&lt;div&gt;Hello &amp; goodbye&lt;/div&gt;';
$specialChars = 'Price: $100 & up, "premium" quality';

// Encode HTML entities
echo htmlspecialchars($userInput) . "\n";
echo htmlentities($specialChars) . "\n";

// Decode HTML entities
echo html_entity_decode($htmlContent) . "\n";

// Encoding with options
$encoded = htmlspecialchars($specialChars, ENT_QUOTES | ENT_HTML5, 'UTF-8');
echo "Encoded: $encoded\n";

// Raw URL encoding for URLs
$urlParam = 'hello world & more';
echo urlencode($urlParam) . "\n";           // hello+world+%26+more
echo rawurlencode($urlParam) . "\n";        // hello%20world%20%26%20more

// Decode URL encoding
$urlEncoded = 'hello%20world%26more';
echo urldecode($urlEncoded) . "\n";         // hello world&more

// Safe HTML content creation
function safeHtml($text, $allowTags = false) {
    if ($allowTags) {
        return strip_tags($text, '<b><i><em><strong>');
    }
    return htmlspecialchars($text, ENT_QUOTES, 'UTF-8');
}

echo safeHtml($userInput) . "\n";
```

HTML entity functions prevent XSS attacks and ensure safe web output.  
htmlspecialchars() handles basic entities, htmlentities() handles all.  
URL encoding functions prepare strings for web transmission.  

## URL encoding and decoding

Preparing strings for safe transmission in URLs and web forms.  

```php
<?php

$searchQuery = 'hello world & more';
$filename = 'my document (final).pdf';
$email = 'user+tag@example.com';

// URL encoding for query parameters
echo urlencode($searchQuery) . "\n";        // hello+world+%26+more
echo rawurlencode($searchQuery) . "\n";     // hello%20world%20%26%20more

// URL decoding
$encoded = 'hello%20world%26more';
echo urldecode($encoded) . "\n";            // hello world&more
echo rawurldecode($encoded) . "\n";         // hello world&more

// Build query string
$params = [
    'q' => 'php strings',
    'sort' => 'date',
    'limit' => 20
];
echo http_build_query($params) . "\n";      // q=php+strings&sort=date&limit=20

// Parse query string
$query = 'name=John+Doe&age=30&city=New+York';
parse_str($query, $parsed);
print_r($parsed);

// Safe filename for URLs
function urlSafeFilename($filename) {
    $safe = preg_replace('/[^a-zA-Z0-9._-]/', '_', $filename);
    $safe = preg_replace('/_+/', '_', $safe);
    return trim($safe, '_');
}

echo urlSafeFilename($filename) . "\n";     // my_document_final.pdf
```

URL encoding ensures safe transmission of special characters in web  
requests. Use urlencode() for form data, rawurlencode() for path  
components. http_build_query() creates query strings from arrays.  

## Base64 encoding and decoding

Encoding binary data and strings for safe text transmission.  

```php
<?php

$plainText = 'Hello, World!';
$binaryData = "Binary data: \x00\x01\x02\x03";
$imageData = 'Imagine this is image data...';

// Basic Base64 encoding
$encoded = base64_encode($plainText);
echo "Encoded: $encoded\n";

// Base64 decoding
$decoded = base64_decode($encoded);
echo "Decoded: $decoded\n";

// URL-safe Base64 (RFC 4648)
$urlSafe = rtrim(strtr($encoded, '+/', '-_'), '=');
echo "URL-safe: $urlSafe\n";

// Decode URL-safe Base64
$padded = str_pad(strtr($urlSafe, '-_', '+/'), strlen($urlSafe) % 4, '=', STR_PAD_RIGHT);
$urlDecoded = base64_decode($padded);
echo "URL decoded: $urlDecoded\n";

// File encoding simulation
function encodeFile($content, $mimeType = 'text/plain') {
    $encoded = base64_encode($content);
    return "data:$mimeType;base64,$encoded";
}

$dataUri = encodeFile($imageData, 'image/png');
echo "Data URI: " . substr($dataUri, 0, 50) . "...\n";

// Chunked encoding for large data
$largeText = str_repeat('Hello World! ', 100);
$encoded = base64_encode($largeText);
$chunked = chunk_split($encoded, 64, "\n");
echo "Chunked encoding (first 200 chars):\n" . substr($chunked, 0, 200) . "...\n";
```

Base64 encoding converts binary data to ASCII text for safe  
transmission. Essential for email attachments, data URIs, and  
API communications. chunk_split() formats long encoded strings.  

## String hashing

Creating hash values for data integrity and security purposes.  

```php
<?php

$password = 'secretPassword123';
$data = 'Important data to hash';
$file_content = 'File content for checksum';

// Password hashing (secure)
$hashedPassword = password_hash($password, PASSWORD_DEFAULT);
echo "Password hash: $hashedPassword\n";

// Verify password
$isValid = password_verify($password, $hashedPassword);
echo "Password valid: " . ($isValid ? 'Yes' : 'No') . "\n";

// MD5 hash (not secure for passwords)
echo "MD5: " . md5($data) . "\n";

// SHA hashes
echo "SHA1: " . sha1($data) . "\n";
echo "SHA256: " . hash('sha256', $data) . "\n";
echo "SHA512: " . hash('sha512', $data) . "\n";

// HMAC (Hash-based Message Authentication Code)
$secret = 'secret_key';
$hmac = hash_hmac('sha256', $data, $secret);
echo "HMAC-SHA256: $hmac\n";

// File integrity checking
function generateChecksum($content) {
    return [
        'md5' => md5($content),
        'sha1' => sha1($content),
        'sha256' => hash('sha256', $content)
    ];
}

$checksums = generateChecksum($file_content);
print_r($checksums);
```

Use password_hash() and password_verify() for secure password handling.  
SHA algorithms provide data integrity checking. HMAC adds authentication  
with secret keys. Never use MD5/SHA1 for password storage.  

## String similarity comparison

Measuring similarity between strings for fuzzy matching.  

```php
<?php

$str1 = 'Hello World';
$str2 = 'Hello Word';
$str3 = 'Helo World';
$str4 = 'Goodbye World';

// Levenshtein distance (character differences)
echo "Levenshtein distance:\n";
echo "'$str1' vs '$str2': " . levenshtein($str1, $str2) . "\n";  // 1
echo "'$str1' vs '$str3': " . levenshtein($str1, $str3) . "\n";  // 1
echo "'$str1' vs '$str4': " . levenshtein($str1, $str4) . "\n";  // 7

// Similar text percentage
similar_text($str1, $str2, $percent);
echo "Similar text percentage: " . round($percent, 2) . "%\n";

// Soundex (phonetic similarity)
echo "Soundex codes:\n";
echo "Smith: " . soundex('Smith') . "\n";
echo "Smyth: " . soundex('Smyth') . "\n";
echo "Schmidt: " . soundex('Schmidt') . "\n";

// Metaphone (improved phonetic algorithm)
echo "Metaphone codes:\n";
echo "Smith: " . metaphone('Smith') . "\n";
echo "Smyth: " . metaphone('Smyth') . "\n";

// Fuzzy matching function
function fuzzyMatch($needle, $haystack, $threshold = 80) {
    $matches = [];
    foreach ($haystack as $item) {
        similar_text($needle, $item, $percent);
        if ($percent >= $threshold) {
            $matches[] = ['text' => $item, 'similarity' => $percent];
        }
    }
    return $matches;
}

$names = ['John Smith', 'Jon Smyth', 'Jane Smith', 'Bob Johnson'];
$matches = fuzzyMatch('John Smith', $names, 70);
print_r($matches);
```

String similarity functions enable fuzzy matching and spell checking.  
Levenshtein calculates edit distance, similar_text measures percentage  
similarity. Soundex and Metaphone provide phonetic matching.  

## Multi-byte string functions

Handling international characters and Unicode text properly.  

```php
<?php

$english = 'Hello World';
$japanese = 'こんにちは世界';
$arabic = 'مرحبا بالعالم';
$emoji = 'Hello �� World 🌍';

// Multi-byte string length
echo "English length: " . mb_strlen($english) . "\n";          // 11
echo "Japanese length: " . mb_strlen($japanese) . "\n";        // 7
echo "Arabic length: " . mb_strlen($arabic) . "\n";           // 12
echo "Emoji length: " . mb_strlen($emoji) . "\n";             // 15

// Multi-byte substring
echo "Japanese substr: " . mb_substr($japanese, 0, 3) . "\n";
echo "Arabic substr: " . mb_substr($arabic, 0, 5) . "\n";

// Multi-byte case conversion
echo "Japanese upper: " . mb_strtoupper($japanese) . "\n";
echo "Arabic lower: " . mb_strtolower($arabic) . "\n";

// Multi-byte string position
$text = 'Café naïve résumé';
echo "Position of 'naïve': " . mb_strpos($text, 'naïve') . "\n";

// Multi-byte word wrapping
$longText = 'これは日本語の長いテキストです。複数の行に分割する必要があります。';
echo "Wrapped text:\n" . mb_wordwrap($longText, 10, "\n", true) . "\n";

// Character encoding
echo "Text encoding: " . mb_detect_encoding($japanese) . "\n";

// Split multi-byte string into characters
$chars = mb_str_split($emoji, 1);
foreach ($chars as $i => $char) {
    echo "Char $i: '$char'\n";
}
```

Multi-byte functions handle Unicode text correctly across different  
languages and character sets. Essential for international applications  
processing non-ASCII text and emoji characters.  

## String tokenization

Breaking strings into meaningful tokens for parsing and analysis.  

```php
<?php

$sentence = 'The quick brown fox jumps over the lazy dog.';
$code = 'function hello($name) { return "Hello, $name!"; }';
$csv = 'name,age,city|John,30,NYC|Jane,25,LA';

// Simple word tokenization
$words = preg_split('/\s+/', trim($sentence));
echo "Words: " . implode(' | ', $words) . "\n";

// Tokenize by multiple delimiters
$tokens = preg_split('/[\s,;.!?]+/', $sentence, -1, PREG_SPLIT_NO_EMPTY);
echo "Tokens: " . implode(' | ', $tokens) . "\n";

// CSV tokenization with multiple separators
$lines = explode('|', $csv);
$data = [];
foreach ($lines as $line) {
    if (trim($line)) {
        $data[] = str_getcsv($line);
    }
}
print_r($data);

// Advanced tokenization function
function tokenize($text, $patterns = []) {
    $defaultPatterns = [
        'word' => '/\b[a-zA-Z]+\b/',
        'number' => '/\b\d+\.?\d*\b/',
        'punctuation' => '/[.,;!?]/',
        'whitespace' => '/\s+/'
    ];
    
    $patterns = array_merge($defaultPatterns, $patterns);
    $tokens = [];
    $position = 0;
    
    while ($position < strlen($text)) {
        $matched = false;
        foreach ($patterns as $type => $pattern) {
            if (preg_match($pattern, $text, $matches, PREG_OFFSET_CAPTURE, $position)) {
                if ($matches[0][1] === $position) {
                    $tokens[] = [
                        'type' => $type,
                        'value' => $matches[0][0],
                        'position' => $position
                    ];
                    $position += strlen($matches[0][0]);
                    $matched = true;
                    break;
                }
            }
        }
        if (!$matched) $position++;
    }
    
    return $tokens;
}

$tokens = tokenize($sentence);
foreach ($tokens as $token) {
    if ($token['type'] !== 'whitespace') {
        echo "{$token['type']}: '{$token['value']}'\n";
    }
}
```

String tokenization breaks text into meaningful units for parsing.  
Regular expressions identify token patterns. Custom tokenizers handle  
complex parsing requirements for languages and data formats.  

## String template processing

Processing template strings with variable substitution and logic.  

```php
<?php

$template = 'Hello {name}, you have {count} messages in your {folder} folder.';
$advancedTemplate = 'Welcome {user.name}! Your balance is ${user.balance|number_format}.';

// Simple template substitution
function processTemplate($template, $variables) {
    $result = $template;
    foreach ($variables as $key => $value) {
        $result = str_replace('{' . $key . '}', $value, $result);
    }
    return $result;
}

$vars = [
    'name' => 'Alice',
    'count' => 5,
    'folder' => 'inbox'
];

echo processTemplate($template, $vars) . "\n";

// Advanced template with nested values and filters
function advancedTemplate($template, $data) {
    return preg_replace_callback('/\{([^}]+)\}/', function($matches) use ($data) {
        $expression = $matches[1];
        
        // Handle filters (value|filter)
        if (strpos($expression, '|') !== false) {
            list($path, $filter) = explode('|', $expression, 2);
            $value = getNestedValue($data, trim($path));
            return applyFilter($value, trim($filter));
        }
        
        return getNestedValue($data, $expression);
    }, $template);
}

function getNestedValue($data, $path) {
    $keys = explode('.', $path);
    $value = $data;
    foreach ($keys as $key) {
        $value = $value[$key] ?? '';
    }
    return $value;
}

function applyFilter($value, $filter) {
    switch ($filter) {
        case 'number_format':
            return number_format((float)$value, 2);
        case 'upper':
            return strtoupper($value);
        case 'lower':
            return strtolower($value);
        default:
            return $value;
    }
}

$userData = [
    'user' => [
        'name' => 'John',
        'balance' => 1234.56
    ]
];

echo advancedTemplate($advancedTemplate, $userData) . "\n";

// Template with conditionals
$conditionalTemplate = 'Hello {name}{if has_messages}, you have {message_count} new messages{endif}!';

function conditionalTemplate($template, $data) {
    // Simple if/endif processing
    $result = preg_replace_callback('/\{if\s+(\w+)\}(.*?)\{endif\}/s', function($matches) use ($data) {
        $condition = $matches[1];
        $content = $matches[2];
        return !empty($data[$condition]) ? $content : '';
    }, $template);
    
    // Process regular variables
    return processTemplate($result, $data);
}

$msgData = [
    'name' => 'Bob',
    'has_messages' => true,
    'message_count' => 3
];

echo conditionalTemplate($conditionalTemplate, $msgData) . "\n";
```

Template processing enables dynamic content generation with variable  
substitution, filters, and conditional logic. Useful for emails,  
reports, and code generation systems.  

## String transliteration

Converting text between different scripts and character systems.  

```php
<?php

$texts = [
    'Café naïve résumé',           // French accents
    'Москва',                      // Russian Cyrillic
    'العربية',                     // Arabic
    'こんにちは',                   // Japanese Hiragana
    '北京',                        // Chinese
];

// Basic transliteration using iconv
foreach ($texts as $text) {
    $transliterated = iconv('UTF-8', 'ASCII//TRANSLIT', $text);
    echo "$text -> $transliterated\n";
}

echo "\n";

// Custom transliteration mappings
$transliterationMap = [
    'à' => 'a', 'á' => 'a', 'â' => 'a', 'ã' => 'a', 'ä' => 'a', 'å' => 'a',
    'è' => 'e', 'é' => 'e', 'ê' => 'e', 'ë' => 'e',
    'ì' => 'i', 'í' => 'i', 'î' => 'i', 'ï' => 'i',
    'ò' => 'o', 'ó' => 'o', 'ô' => 'o', 'õ' => 'o', 'ö' => 'o',
    'ù' => 'u', 'ú' => 'u', 'û' => 'u', 'ü' => 'u',
    'ñ' => 'n', 'ç' => 'c',
    'ß' => 'ss'
];

function customTransliterate($text, $map) {
    return strtr($text, $map);
}

$accentedText = 'Café, naïve, résumé, piñata';
echo "Custom transliteration: " . customTransliterate($accentedText, $transliterationMap) . "\n";

// URL slug generation
function generateSlug($text) {
    // Transliterate to ASCII
    $slug = iconv('UTF-8', 'ASCII//TRANSLIT', $text);
    
    // Convert to lowercase
    $slug = strtolower($slug);
    
    // Replace non-alphanumeric with hyphens
    $slug = preg_replace('/[^a-z0-9]+/', '-', $slug);
    
    // Remove leading/trailing hyphens
    $slug = trim($slug, '-');
    
    return $slug;
}

$titles = [
    'Café Reviews & Ratings',
    'José's Piñata Store',
    'München Weather Report'
];

foreach ($titles as $title) {
    echo "'$title' -> '" . generateSlug($title) . "'\n";
}

// Romanization examples
$romanizations = [
    'Москва' => 'Moscow',      // Russian
    '東京' => 'Tokyo',          // Japanese
    '北京' => 'Beijing',        // Chinese
    'Αθήνα' => 'Athens'        // Greek
];

echo "\nRomanization examples:\n";
foreach ($romanizations as $original => $romanized) {
    echo "$original -> $romanized\n";
}
```

Transliteration converts text between scripts while preserving  
pronunciation. iconv handles basic cases, custom mappings provide  
precise control. Essential for URL slugs and search functionality.  

## Password hashing with strings

Secure password hashing and verification for authentication systems.  

```php
<?php

$passwords = ['password123', 'SecureP@ss!', 'user_password'];

// Modern password hashing (PHP 5.5+)
foreach ($passwords as $password) {
    $hash = password_hash($password, PASSWORD_DEFAULT);
    echo "Password: $password\n";
    echo "Hash: $hash\n";
    
    // Verify password
    $isValid = password_verify($password, $hash);
    echo "Verification: " . ($isValid ? 'Valid' : 'Invalid') . "\n\n";
}

// Password hashing with options
$options = [
    'cost' => 12,  // Higher cost = more secure but slower
];
$strongHash = password_hash('mySecurePassword', PASSWORD_BCRYPT, $options);
echo "Strong hash: $strongHash\n";

// Check if hash needs rehashing (when upgrading security)
if (password_needs_rehash($strongHash, PASSWORD_DEFAULT)) {
    echo "Hash needs upgrade\n";
} else {
    echo "Hash is up to date\n";
}

// Password strength validation
function validatePasswordStrength($password) {
    $errors = [];
    
    if (strlen($password) < 8) {
        $errors[] = 'Password must be at least 8 characters';
    }
    
    if (!preg_match('/[A-Z]/', $password)) {
        $errors[] = 'Password must contain uppercase letter';
    }
    
    if (!preg_match('/[a-z]/', $password)) {
        $errors[] = 'Password must contain lowercase letter';
    }
    
    if (!preg_match('/\d/', $password)) {
        $errors[] = 'Password must contain number';
    }
    
    if (!preg_match('/[^A-Za-z0-9]/', $password)) {
        $errors[] = 'Password must contain special character';
    }
    
    return $errors;
}

$testPasswords = ['weak', 'StrongP@ss123', 'NoNumber!'];
foreach ($testPasswords as $pwd) {
    $errors = validatePasswordStrength($pwd);
    echo "Password '$pwd': " . (empty($errors) ? 'Strong' : implode(', ', $errors)) . "\n";
}

// Secure password generation
function generateSecurePassword($length = 12) {
    $chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*';
    $password = '';
    $charsLength = strlen($chars);
    
    for ($i = 0; $i < $length; $i++) {
        $password .= $chars[random_int(0, $charsLength - 1)];
    }
    
    return $password;
}

echo "\nGenerated passwords:\n";
for ($i = 0; $i < 3; $i++) {
    $generated = generateSecurePassword(16);
    echo "$generated\n";
}
```

Always use password_hash() and password_verify() for secure password  
handling. Never store plain text passwords. Implement strength  
validation and consider password generation for better security.  

## String compression

Compressing and decompressing string data for storage efficiency.  

```php
<?php

$longText = str_repeat('Hello World! This is a test of string compression. ', 100);
$jsonData = json_encode(array_fill(0, 50, ['name' => 'John', 'age' => 30, 'city' => 'New York']));

echo "Original text length: " . strlen($longText) . " bytes\n";
echo "Original JSON length: " . strlen($jsonData) . " bytes\n\n";

// Gzip compression
$gzCompressed = gzcompress($longText);
$gzRatio = round((1 - strlen($gzCompressed) / strlen($longText)) * 100, 2);
echo "Gzip compressed: " . strlen($gzCompressed) . " bytes ({$gzRatio}% reduction)\n";

// Gzip decompression
$gzDecompressed = gzuncompress($gzCompressed);
echo "Gzip match: " . ($longText === $gzDecompressed ? 'Yes' : 'No') . "\n\n";

// Deflate compression
$deflateCompressed = gzdeflate($longText);
$deflateRatio = round((1 - strlen($deflateCompressed) / strlen($longText)) * 100, 2);
echo "Deflate compressed: " . strlen($deflateCompressed) . " bytes ({$deflateRatio}% reduction)\n";

$deflateDecompressed = gzinflate($deflateCompressed);
echo "Deflate match: " . ($longText === $deflateDecompressed ? 'Yes' : 'No') . "\n\n";

// Bzip2 compression (if available)
if (function_exists('bzcompress')) {
    $bzCompressed = bzcompress($longText);
    $bzRatio = round((1 - strlen($bzCompressed) / strlen($longText)) * 100, 2);
    echo "Bzip2 compressed: " . strlen($bzCompressed) . " bytes ({$bzRatio}% reduction)\n";
    
    $bzDecompressed = bzdecompress($bzCompressed);
    echo "Bzip2 match: " . ($longText === $bzDecompressed ? 'Yes' : 'No') . "\n\n";
}

// Compression utility function
function compressString($data, $method = 'gzip') {
    switch ($method) {
        case 'gzip':
            return gzcompress($data);
        case 'deflate':
            return gzdeflate($data);
        case 'bzip2':
            return function_exists('bzcompress') ? bzcompress($data) : false;
        default:
            return false;
    }
}

function decompressString($data, $method = 'gzip') {
    switch ($method) {
        case 'gzip':
            return gzuncompress($data);
        case 'deflate':
            return gzinflate($data);
        case 'bzip2':
            return function_exists('bzdecompress') ? bzdecompress($data) : false;
        default:
            return false;
    }
}

// Test different compression methods
$methods = ['gzip', 'deflate'];
foreach ($methods as $method) {
    $compressed = compressString($jsonData, $method);
    if ($compressed !== false) {
        $ratio = round((1 - strlen($compressed) / strlen($jsonData)) * 100, 2);
        echo "$method JSON compression: " . strlen($compressed) . " bytes ({$ratio}% reduction)\n";
        
        $decompressed = decompressString($compressed, $method);
        echo "$method JSON match: " . ($jsonData === $decompressed ? 'Yes' : 'No') . "\n";
    }
}
```

String compression reduces storage space and transmission time for  
large text data. Gzip and Deflate offer good compression ratios.  
Always verify decompressed data matches original content.  
