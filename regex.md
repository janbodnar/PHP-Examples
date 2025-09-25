# Regular Expressions

This chapter covers 25 comprehensive PHP regular expression examples  
designed to demonstrate the full range of pattern matching and text  
manipulation capabilities in modern PHP 8.4. These examples progress  
from basic pattern matching to advanced regex features including named  
groups, lookarounds, and callback functions. Each example includes  
detailed explanations to help you master PHP regex development.  

## Basic pattern matching

Simple pattern matching to find text patterns in strings.  

```php
<?php

$text = "Visit our website at https://example.com today!";
$email = "contact@example.com";
$phone = "555-123-4567";

// Basic pattern matching
if (preg_match('/https?:\/\/[^\s]+/', $text, $matches)) {
    echo "Found URL: " . $matches[0] . "\n";
}

// Email pattern
if (preg_match('/\w+@\w+\.\w+/', $email)) {
    echo "Valid email format\n";
}

// Phone number pattern  
if (preg_match('/\d{3}-\d{3}-\d{4}/', $phone)) {
    echo "Valid phone format\n";
}
```

The preg_match() function returns 1 if pattern is found, 0 if not found.  
It stores the first match in the optional matches array. Basic patterns  
use literal characters and simple metacharacters like \d and \w.  

## Pattern matching with capture groups

Using parentheses to capture specific parts of matched patterns.  

```php
<?php

$date = "Today is 2024-12-15";
$phone = "Call me at (555) 123-4567";
$url = "Visit https://www.example.com/path";

// Capture date parts
if (preg_match('/(\d{4})-(\d{2})-(\d{2})/', $date, $matches)) {
    echo "Year: {$matches[1]}, Month: {$matches[2]}, Day: {$matches[3]}\n";
}

// Capture phone parts
if (preg_match('/\((\d{3})\)\s(\d{3})-(\d{4})/', $phone, $matches)) {
    echo "Area: {$matches[1]}, Exchange: {$matches[2]}, Number: {$matches[3]}\n";
}

// Capture URL components
if (preg_match('/(https?):\/\/([^\/]+)(.*)/', $url, $matches)) {
    echo "Protocol: {$matches[1]}, Domain: {$matches[2]}, Path: {$matches[3]}\n";
}
```

Capture groups use parentheses to extract specific pattern parts.  
The matches array index 0 contains the full match, while indices 1+  
contain individual captured groups in order of appearance.  

## Finding all matches

Using preg_match_all() to find every occurrence of a pattern.  

```php
<?php

$text = "Contact us at info@example.com or support@example.com for help";
$log = "2024-01-15 ERROR, 2024-01-16 INFO, 2024-01-17 WARNING";
$html = "<p>Hello</p><div>World</div><span>there</span>";

// Find all email addresses
preg_match_all('/\w+@\w+\.\w+/', $text, $emails);
echo "Found emails: " . implode(', ', $emails[0]) . "\n";

// Find all dates
preg_match_all('/\d{4}-\d{2}-\d{2}/', $log, $dates);
echo "Found dates: " . implode(', ', $dates[0]) . "\n";

// Find all HTML tags
preg_match_all('/<(\w+)>/', $html, $tags);
echo "Found tags: " . implode(', ', $tags[1]) . "\n";
```

The preg_match_all() function finds all pattern matches in a string.  
It returns the number of full pattern matches found and populates  
the matches array with all occurrences and captured groups.  

## Pattern replacement

Using preg_replace() to substitute matched patterns with new text.  

```php
<?php

$text = "Call us at 555-123-4567 or 555-987-6543";
$html = "<script>alert('bad')</script><p>Good content</p>";
$dates = "Date: 2024/12/15 and 2024/01/30";

// Clean phone numbers
$cleaned = preg_replace('/\d{3}-\d{3}-\d{4}/', '[PHONE]', $text);
echo "Cleaned: $cleaned\n";

// Remove script tags
$safe = preg_replace('/<script[^>]*>.*?<\/script>/is', '', $html);
echo "Safe HTML: $safe\n";

// Format dates
$formatted = preg_replace('/(\d{4})\/(\d{2})\/(\d{2})/', '$2-$3-$1', $dates);
echo "Formatted: $formatted\n";
```

The preg_replace() function substitutes all pattern matches with  
replacement text. Backreferences like $1, $2 refer to captured  
groups. The 's' flag makes . match newlines.  

## Character classes and quantifiers

Using character classes and quantifiers for flexible pattern matching.  

```php
<?php

$text = "Items: apple123, BANANA456, Cherry_789";
$passwords = ["abc123", "Password1", "STRONG@123", "weak"];
$codes = ["A1B2", "X9Y8Z7", "ABC", "123456"];

// Character classes
preg_match_all('/[a-zA-Z]+\d+/', $text, $alphanumeric);
echo "Alphanumeric items: " . implode(', ', $alphanumeric[0]) . "\n";

// Quantifiers
foreach ($passwords as $pwd) {
    if (preg_match('/^.{8,}$/', $pwd)) {
        echo "$pwd has 8+ characters\n";
    }
}

// Complex character classes
foreach ($codes as $code) {
    if (preg_match('/^[A-Z]\d[A-Z]\d+$/', $code)) {
        echo "$code matches pattern\n";
    }
}
```

Character classes like [a-zA-Z] match any character in the set.  
Quantifiers like {8,} specify repetition counts. The ^ and $ anchors  
match string start and end respectively.  

## Anchors and boundaries

Using anchors to specify pattern position within strings.  

```php
<?php

$emails = ["user@example.com", "invalid@", "@invalid.com", "good@test.org"];
$words = ["The cat sat on the mat", "catch the cat", "concatenate"];
$lines = "Start line\nMiddle content\nEnd line";

// String anchors
foreach ($emails as $email) {
    if (preg_match('/^[^@]+@[^@]+\.[^@]+$/', $email)) {
        echo "$email is valid format\n";
    }
}

// Word boundaries
foreach ($words as $text) {
    if (preg_match('/\bcat\b/', $text)) {
        echo "'$text' contains word 'cat'\n";
    }
}

// Line anchors
if (preg_match('/^Start/m', $lines)) {
    echo "Found line starting with 'Start'\n";
}
```

The ^ anchor matches string/line start, $ matches string/line end.  
Word boundaries \b match positions between word and non-word characters.  
The 'm' modifier enables multiline mode for ^ and $ anchors.  

## Named capture groups

Using named groups for clearer and more maintainable regex patterns.  

```php
<?php

$log = "2024-12-15 14:30:45 ERROR Database connection failed";
$email = "john.doe@example.com";
$address = "123 Main Street, New York, NY 10001";

// Named groups in log parsing
$pattern = '/(?P<date>\d{4}-\d{2}-\d{2}) (?P<time>\d{2}:\d{2}:\d{2}) (?P<level>\w+) (?P<message>.*)/';
if (preg_match($pattern, $log, $matches)) {
    echo "Date: {$matches['date']}\n";
    echo "Time: {$matches['time']}\n";
    echo "Level: {$matches['level']}\n";
    echo "Message: {$matches['message']}\n";
}

// Email parsing
if (preg_match('/(?P<user>[^@]+)@(?P<domain>.+)/', $email, $matches)) {
    echo "User: {$matches['user']}, Domain: {$matches['domain']}\n";
}
```

Named capture groups use (?P<name>pattern) syntax for clearer code.  
Access captured values using $matches['name'] instead of numeric  
indices. This makes complex patterns more readable and maintainable.  

## Lookarounds and assertions

Using lookahead and lookbehind assertions for advanced pattern matching.  

```php
<?php

$passwords = ["Password1", "password", "PASSWORD", "Pass123!"];
$text = "JavaScript is great, but Java is different";
$numbers = "100USD, 200EUR, 300GBP, 150";

// Positive lookahead
foreach ($passwords as $pwd) {
    $pattern = '/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/';
    if (preg_match($pattern, $pwd)) {
        echo "$pwd meets complexity requirements\n";
    }
}

// Negative lookahead
if (preg_match('/Java(?!Script)/', $text)) {
    echo "Found Java (not JavaScript)\n";
}

// Positive lookbehind
preg_match_all('/(?<=\d{3})[A-Z]{3}/', $numbers, $currencies);
echo "Currencies: " . implode(', ', $currencies[0]) . "\n";
```

Lookarounds assert what comes before/after without consuming characters.  
(?=pattern) is positive lookahead, (?!pattern) is negative lookahead.  
(?<=pattern) is positive lookbehind, (?<!pattern) is negative lookbehind.  

## Case-insensitive and global matching

Using regex flags for different matching behaviors.  

```php
<?php

$text = "PHP is Great! php rocks and PHP rules";
$html = "<DIV>Content</DIV><div>more</div>";
$multiline = "First line\nSecond LINE\nThird Line";

// Case-insensitive matching
preg_match_all('/php/i', $text, $matches);
echo "Found " . count($matches[0]) . " PHP mentions\n";

// Case-insensitive replacement
$result = preg_replace('/php/i', 'PHP', $text);
echo "Normalized: $result\n";

// Multiple flags
preg_match_all('/<div>/i', $html, $divs);
echo "Found " . count($divs[0]) . " div tags\n";

// Multiline matching
preg_match_all('/line$/im', $multiline, $endings);
echo "Lines ending with 'line': " . count($endings[0]) . "\n";
```

The 'i' flag enables case-insensitive matching. The 'm' flag enables  
multiline mode where ^ and $ match line boundaries. The 's' flag makes  
. match newlines. Flags can be combined like '/pattern/ims'.  

## Regex with callback functions

Using preg_replace_callback() for dynamic pattern replacement.  

```php
<?php

$text = "Prices: $10.50, $25.99, $100.00";
$dates = "Events on 2024-01-15, 2024-03-20, and 2024-12-31";
$template = "Hello {name}, welcome to {site}!";

// Transform prices
$expensive = preg_replace_callback('/\$(\d+\.\d{2})/', function($matches) {
    $price = floatval($matches[1]);
    return $price > 20 ? "EXPENSIVE: \${$matches[1]}" : "\${$matches[1]}";
}, $text);
echo "$expensive\n";

// Format dates
$formatted = preg_replace_callback('/(\d{4})-(\d{2})-(\d{2})/', function($m) {
    return date('F j, Y', mktime(0, 0, 0, $m[2], $m[3], $m[1]));
}, $dates);
echo "$formatted\n";

// Simple templating
$data = ['name' => 'Alice', 'site' => 'Example.com'];
$result = preg_replace_callback('/\{(\w+)\}/', function($m) use ($data) {
    return $data[$m[1]] ?? $m[0];
}, $template);
echo "$result\n";
```

The preg_replace_callback() function calls a callback for each match.  
The callback receives matches array and returns replacement text.  
This enables dynamic, context-sensitive replacements.  

## Pattern splitting and tokenization

Using preg_split() to break strings into arrays based on patterns.  

```php
<?php

$csv = 'apple,banana,"cherry, red",date';
$text = "word1    word2\tword3\nword4";
$path = "/home/user/documents/file.txt";
$sentence = "Hello world! How are you? Fine, thanks.";

// Complex CSV splitting
$parts = preg_split('/,(?=(?:[^"]*"[^"]*")*[^"]*$)/', $csv);
echo "CSV parts: " . implode(' | ', $parts) . "\n";

// Split on whitespace
$words = preg_split('/\s+/', trim($text));
echo "Words: " . implode(', ', $words) . "\n";

// Split path
$pathParts = preg_split('/[\/\\\\]/', $path, -1, PREG_SPLIT_NO_EMPTY);
echo "Path parts: " . implode(' > ', $pathParts) . "\n";

// Split keeping delimiters
$tokens = preg_split('/([.!?])/', $sentence, -1, PREG_SPLIT_DELIM_CAPTURE | PREG_SPLIT_NO_EMPTY);
print_r($tokens);
```

The preg_split() function divides strings using pattern delimiters.  
PREG_SPLIT_NO_EMPTY removes empty elements from results.  
PREG_SPLIT_DELIM_CAPTURE includes delimiter matches in results.  

## Email validation patterns

Comprehensive email address validation using regex patterns.  

```php
<?php

$emails = [
    "user@example.com",
    "test.email@domain.co.uk", 
    "invalid.email",
    "user+tag@example.org",
    "user@sub.domain.com",
    "@invalid.com",
    "user@.com"
];

// Basic email pattern
$basicPattern = '/^[^\s@]+@[^\s@]+\.[^\s@]+$/';

// More comprehensive pattern
$advancedPattern = '/^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/';

// RFC-compliant pattern (simplified)
$rfcPattern = '/^[a-zA-Z0-9.!#$%&\'*+\/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/';

foreach ($emails as $email) {
    $basic = preg_match($basicPattern, $email);
    $advanced = preg_match($advancedPattern, $email);
    $rfc = preg_match($rfcPattern, $email);
    
    echo "$email: Basic($basic) Advanced($advanced) RFC($rfc)\n";
}
```

Email validation ranges from simple to complex patterns. Basic patterns  
check for @ symbol and dots. Advanced patterns validate character sets  
and structure. RFC-compliant patterns follow email specifications.  

## URL and URI validation

Validating and parsing URLs with regular expressions.  

```php
<?php

$urls = [
    "https://www.example.com",
    "http://example.com/path?param=value",
    "https://sub.domain.com:8080/path",
    "ftp://files.example.com/file.zip",
    "invalid-url",
    "www.example.com",
    "https://example.com/path#section"
];

// Basic URL pattern
$urlPattern = '/^https?:\/\/[^\s\/$.?#].[^\s]*$/i';

// Detailed URL parsing
$detailedPattern = '/^(?P<scheme>https?):\/\/(?P<host>[^\/\s:]+)(?::(?P<port>\d+))?(?P<path>\/[^\s?#]*)?(?:\?(?P<query>[^#\s]*))?(?:#(?P<fragment>[^\s]*))?$/i';

foreach ($urls as $url) {
    if (preg_match($urlPattern, $url)) {
        echo "$url is valid URL\n";
        
        if (preg_match($detailedPattern, $url, $matches)) {
            echo "  Scheme: {$matches['scheme']}\n";
            echo "  Host: {$matches['host']}\n";
            if (!empty($matches['port'])) echo "  Port: {$matches['port']}\n";
            if (!empty($matches['path'])) echo "  Path: {$matches['path']}\n";
            if (!empty($matches['query'])) echo "  Query: {$matches['query']}\n";
            if (!empty($matches['fragment'])) echo "  Fragment: {$matches['fragment']}\n";
        }
    }
}
```

URL patterns validate scheme, host, port, path, query and fragment.  
Named groups enable easy extraction of URL components for further  
processing. Consider using parse_url() for production URL parsing.  

## Phone number patterns

Validating various phone number formats with regex patterns.  

```php
<?php

$phones = [
    "555-123-4567",
    "(555) 123-4567", 
    "555.123.4567",
    "5551234567",
    "+1-555-123-4567",
    "1-555-123-4567 ext 123",
    "invalid-phone",
    "+44 20 7946 0958"
];

// US phone patterns
$patterns = [
    'dash' => '/^\d{3}-\d{3}-\d{4}$/',
    'parentheses' => '/^\(\d{3}\) \d{3}-\d{4}$/',
    'dots' => '/^\d{3}\.\d{3}\.\d{4}$/',
    'digits' => '/^\d{10}$/',
    'international' => '/^\+1-\d{3}-\d{3}-\d{4}$/',
    'with_extension' => '/^\d{3}-\d{3}-\d{4} ext \d+$/'
];

// Flexible phone parser
$flexiblePattern = '/^(?:\+?1[-.\s]?)?\(?([0-9]{3})\)?[-.\s]?([0-9]{3})[-.\s]?([0-9]{4})(?:\s?(?:ext|extension)\.?\s?(\d+))?$/';

foreach ($phones as $phone) {
    echo "Phone: $phone\n";
    
    foreach ($patterns as $name => $pattern) {
        if (preg_match($pattern, $phone)) {
            echo "  Matches $name format\n";
        }
    }
    
    if (preg_match($flexiblePattern, $phone, $matches)) {
        echo "  Parsed: ({$matches[1]}) {$matches[2]}-{$matches[3]}";
        if (!empty($matches[4])) echo " ext {$matches[4]}";
        echo "\n";
    }
    echo "\n";
}
```

Phone number validation requires handling multiple formats and  
international variations. Flexible patterns parse components while  
strict patterns validate specific formats.  

## Date and time patterns

Matching and parsing various date and time formats.  

```php
<?php

$dates = [
    "2024-12-15",
    "12/15/2024",
    "15-12-2024",
    "Dec 15, 2024",
    "15 December 2024",
    "2024-12-15 14:30:45",
    "12/15/24 2:30 PM",
    "invalid-date"
];

$patterns = [
    'iso_date' => '/^\d{4}-\d{2}-\d{2}$/',
    'us_date' => '/^\d{1,2}\/\d{1,2}\/\d{4}$/',
    'uk_date' => '/^\d{1,2}-\d{1,2}-\d{4}$/',
    'iso_datetime' => '/^\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}$/',
    'us_datetime' => '/^\d{1,2}\/\d{1,2}\/\d{2} \d{1,2}:\d{2} (AM|PM)$/i'
];

// Comprehensive date parser
$dateParser = '/^(?P<day>\d{1,2})[-\/\s](?P<month>\d{1,2}|[A-Za-z]+)[-\/\s](?P<year>\d{2,4})(?:\s+(?P<time>\d{1,2}:\d{2}(?::\d{2})?)(?:\s*(?P<ampm>AM|PM))?)?$/i';

foreach ($dates as $date) {
    echo "Date: $date\n";
    
    foreach ($patterns as $name => $pattern) {
        if (preg_match($pattern, $date)) {
            echo "  Matches $name format\n";
        }
    }
    
    if (preg_match($dateParser, $date, $matches)) {
        echo "  Parsed components:\n";
        echo "    Day: {$matches['day']}\n";
        echo "    Month: {$matches['month']}\n";
        echo "    Year: {$matches['year']}\n";
        if (!empty($matches['time'])) echo "    Time: {$matches['time']}\n";
        if (!empty($matches['ampm'])) echo "    AM/PM: {$matches['ampm']}\n";
    }
    echo "\n";
}
```

Date patterns handle various formats including ISO, US, and European  
styles. Time patterns accommodate 12/24 hour formats with optional  
AM/PM indicators. Consider using DateTime for production date parsing.  

## HTML tag matching and parsing

Using regex to match and extract HTML tags and content.  

```php
<?php

$html = '
<html>
<head><title>Test Page</title></head>
<body>
    <div class="container" id="main">
        <h1>Welcome</h1>
        <p style="color: red;">Hello there!</p>
        <img src="image.jpg" alt="Test image" />
        <a href="https://example.com">Link</a>
    </div>
    <!-- Comment -->
</body>
</html>';

// Match all tags
preg_match_all('/<(\w+)[^>]*>/', $html, $tags);
echo "Found tags: " . implode(', ', array_unique($tags[1])) . "\n";

// Extract tag attributes
preg_match_all('/<(\w+)([^>]*)>/', $html, $matches);
for ($i = 0; $i < count($matches[0]); $i++) {
    $tag = $matches[1][$i];
    $attrs = $matches[2][$i];
    if (trim($attrs)) {
        echo "Tag: $tag, Attributes: " . trim($attrs) . "\n";
    }
}

// Extract specific content
preg_match('/<title>(.*?)<\/title>/', $html, $title);
if ($title) echo "Title: {$title[1]}\n";

preg_match_all('/<a[^>]*href=["\']([^"\']+)["\'][^>]*>(.*?)<\/a>/i', $html, $links);
for ($i = 0; $i < count($links[0]); $i++) {
    echo "Link: {$links[2][$i]} -> {$links[1][$i]}\n";
}

// Remove HTML tags
$clean = preg_replace('/<[^>]*>/', '', $html);
echo "Clean text: " . trim(preg_replace('/\s+/', ' ', $clean)) . "\n";
```

HTML parsing with regex has limitations but works for simple cases.  
Non-greedy quantifiers (.*?) prevent matching across multiple tags.  
Use proper HTML parsers like DOMDocument for complex HTML processing.  

## Credit card and validation patterns

Validating credit card numbers and other financial data.  

```php
<?php

$creditCards = [
    "4532-1234-5678-9012",  // Visa
    "5555-1234-5678-9012",  // MasterCard
    "3782-8224-6310-005",   // American Express
    "6011-1234-5678-9012",  // Discover
    "invalid-card-number"
];

$patterns = [
    'visa' => '/^4\d{3}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}$/',
    'mastercard' => '/^5[1-5]\d{2}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}$/',
    'amex' => '/^3[47]\d{2}[\s-]?\d{6}[\s-]?\d{5}$/',
    'discover' => '/^6011[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}$/',
    'generic' => '/^\d{4}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}$/'
];

// Luhn algorithm validation function
function validateLuhn($number) {
    $cleanNumber = preg_replace('/\D/', '', $number);
    $sum = 0;
    $alternate = false;
    
    for ($i = strlen($cleanNumber) - 1; $i >= 0; $i--) {
        $digit = intval($cleanNumber[$i]);
        
        if ($alternate) {
            $digit *= 2;
            if ($digit > 9) $digit -= 9;
        }
        
        $sum += $digit;
        $alternate = !$alternate;
    }
    
    return $sum % 10 === 0;
}

foreach ($creditCards as $card) {
    echo "Card: $card\n";
    
    foreach ($patterns as $type => $pattern) {
        if (preg_match($pattern, $card)) {
            echo "  Format: $type\n";
            if (validateLuhn($card)) {
                echo "  Luhn validation: PASS\n";
            } else {
                echo "  Luhn validation: FAIL\n";
            }
            break;
        }
    }
    echo "\n";
}
```

Credit card patterns match major card types by prefix and length.  
Format validation ensures proper structure but doesn't guarantee  
validity. Luhn algorithm provides mathematical validation.  

## Log file parsing and analysis

Parsing structured log files with complex regex patterns.  

```php
<?php

$logContent = '
192.168.1.100 - - [15/Dec/2024:14:30:45 +0000] "GET /index.php HTTP/1.1" 200 1234
10.0.0.5 - admin [15/Dec/2024:14:31:22 +0000] "POST /login.php HTTP/1.1" 401 567
192.168.1.200 - - [15/Dec/2024:14:32:10 +0000] "GET /images/logo.png HTTP/1.1" 404 89
[2024-12-15 14:33:15] ERROR: Database connection failed in /var/www/app.php:125
[2024-12-15 14:33:20] INFO: User login successful for user_id: 12345
[2024-12-15 14:33:25] WARNING: High memory usage detected: 85%
';

// Apache access log pattern
$accessPattern = '/^(\S+) \S+ (\S+) \[([^\]]+)\] "(\S+) (\S+) ([^"]+)" (\d+) (\d+)$/m';

// Application log pattern  
$appPattern = '/^\[([^\]]+)\] (\w+): (.+)$/m';

echo "=== Apache Access Logs ===\n";
if (preg_match_all($accessPattern, $logContent, $matches, PREG_SET_ORDER)) {
    foreach ($matches as $match) {
        echo "IP: {$match[1]}\n";
        echo "User: {$match[2]}\n";
        echo "Time: {$match[3]}\n";
        echo "Method: {$match[4]}\n";
        echo "URL: {$match[5]}\n";
        echo "Status: {$match[7]}\n";
        echo "Size: {$match[8]}\n";
        echo "---\n";
    }
}

echo "\n=== Application Logs ===\n";
if (preg_match_all($appPattern, $logContent, $matches, PREG_SET_ORDER)) {
    foreach ($matches as $match) {
        echo "Time: {$match[1]}\n";
        echo "Level: {$match[2]}\n";
        echo "Message: {$match[3]}\n";
        echo "---\n";
    }
}

// Extract specific patterns
preg_match_all('/(\d+\.\d+\.\d+\.\d+)/', $logContent, $ips);
echo "\nUnique IPs: " . implode(', ', array_unique($ips[0])) . "\n";

preg_match_all('/HTTP\/[\d.]+"\s+(\d{3})/', $logContent, $codes);
echo "Status codes: " . implode(', ', $codes[1]) . "\n";
```

Log parsing requires patterns that handle structured data formats.  
PREG_SET_ORDER returns matches as separate arrays for each full match.  
Complex logs may need multiple patterns for different entry types.  

## Data extraction from text

Extracting structured data from unstructured text using regex.  

```php
<?php

$invoice = '
Invoice #INV-2024-001234
Date: December 15, 2024
Customer: Acme Corporation
Address: 123 Business Ave, Suite 456, New York, NY 10001
Phone: (555) 123-4567
Email: billing@acme.com

Item                Qty    Price    Total
Widget A            10     $25.50   $255.00
Widget B            5      $45.99   $229.95
Service Fee         1      $50.00   $50.00

Subtotal:           $534.95
Tax (8.25%):        $44.13
Total:              $579.08
';

// Extract invoice header
$patterns = [
    'invoice_number' => '/Invoice #([A-Z0-9-]+)/',
    'date' => '/Date: ([^\\n]+)/',
    'customer' => '/Customer: ([^\\n]+)/',
    'phone' => '/Phone: (\([0-9]{3}\) [0-9]{3}-[0-9]{4})/',
    'email' => '/Email: ([^\\s]+@[^\\s]+)/'
];

echo "=== Invoice Details ===\n";
foreach ($patterns as $field => $pattern) {
    if (preg_match($pattern, $invoice, $matches)) {
        echo ucfirst(str_replace('_', ' ', $field)) . ": {$matches[1]}\n";
    }
}

// Extract line items
$itemPattern = '/^([A-Za-z\s]+)\s+(\d+)\s+\$([0-9.]+)\s+\$([0-9.]+)$/m';
if (preg_match_all($itemPattern, $invoice, $items, PREG_SET_ORDER)) {
    echo "\n=== Line Items ===\n";
    foreach ($items as $item) {
        echo "Item: {$item[1]}\n";
        echo "Qty: {$item[2]}\n";
        echo "Price: \${$item[3]}\n";
        echo "Total: \${$item[4]}\n";
        echo "---\n";
    }
}

// Extract totals
$totalPattern = '/(Subtotal|Tax \([^)]+\)|Total):\s+\$([0-9.]+)/';
if (preg_match_all($totalPattern, $invoice, $totals, PREG_SET_ORDER)) {
    echo "\n=== Totals ===\n";
    foreach ($totals as $total) {
        echo "{$total[1]}: \${$total[2]}\n";
    }
}
```

Data extraction patterns target specific structures within text.  
Multiline mode enables line-by-line matching. Named patterns  
organize complex extraction logic for maintainable code.  

## Password strength validation

Comprehensive password validation using multiple regex patterns.  

```php
<?php

$passwords = [
    "password123",
    "Password123",
    "P@ssw0rd123",
    "ComplexP@ss123!",
    "weak",
    "ALLUPPERCASE123",
    "alllowercase123",
    "NoNumbers!@#",
    "P@ss123"
];

// Individual strength checks
$checks = [
    'min_length' => ['/^.{8,}$/', 'At least 8 characters'],
    'has_lower' => ['/[a-z]/', 'Contains lowercase letter'],
    'has_upper' => ['/[A-Z]/', 'Contains uppercase letter'], 
    'has_digit' => ['/\d/', 'Contains digit'],
    'has_special' => ['/[!@#$%^&*(),.?":{}|<>]/', 'Contains special character'],
    'no_common' => ['/^(?!password|123456|qwerty)/i', 'Not common password']
];

// Comprehensive strength pattern
$strongPattern = '/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#$%^&*(),.?":{}|<>]).{8,}$/';

function calculateStrength($password, $checks) {
    $score = 0;
    $feedback = [];
    
    foreach ($checks as $name => $check) {
        if (preg_match($check[0], $password)) {
            $score++;
        } else {
            $feedback[] = $check[1];
        }
    }
    
    return ['score' => $score, 'feedback' => $feedback];
}

foreach ($passwords as $password) {
    echo "Password: '$password'\n";
    
    $strength = calculateStrength($password, $checks);
    $percentage = round(($strength['score'] / count($checks)) * 100);
    
    echo "Strength: {$strength['score']}/" . count($checks) . " ($percentage%)\n";
    
    if (preg_match($strongPattern, $password)) {
        echo "Overall: STRONG ✓\n";
    } else {
        echo "Overall: WEAK ✗\n";
        if (!empty($strength['feedback'])) {
            echo "Missing:\n";
            foreach ($strength['feedback'] as $item) {
                echo "  - $item\n";
            }
        }
    }
    echo "\n";
}
```

Password validation combines multiple regex patterns for comprehensive  
strength checking. Individual patterns test specific requirements while  
lookaheads enable complex combined validation in single patterns.  

## XML and markup parsing

Parsing XML and markup languages with regular expressions.  

```php
<?php

$xml = '
<?xml version="1.0" encoding="UTF-8"?>
<bookstore>
    <book id="001" category="fiction">
        <title>The Great Novel</title>
        <author>Jane Smith</author>
        <price currency="USD">19.99</price>
        <publish_date>2024-01-15</publish_date>
    </book>
    <book id="002" category="non-fiction">
        <title>Learning PHP</title>
        <author>John Doe</author>
        <price currency="USD">29.99</price>
        <publish_date>2024-02-20</publish_date>
    </book>
</bookstore>';

// Extract all tag names
preg_match_all('/<(\w+)(?:[^>]*)>/', $xml, $tags);
echo "Tags found: " . implode(', ', array_unique($tags[1])) . "\n\n";

// Parse book elements
$bookPattern = '/<book([^>]*)>(.*?)<\/book>/s';
if (preg_match_all($bookPattern, $xml, $books, PREG_SET_ORDER)) {
    foreach ($books as $index => $book) {
        echo "=== Book " . ($index + 1) . " ===\n";
        
        // Extract attributes
        preg_match_all('/(\w+)=["\']([^"\']*)["\']/', $book[1], $attrs);
        for ($i = 0; $i < count($attrs[0]); $i++) {
            echo "Attribute {$attrs[1][$i]}: {$attrs[2][$i]}\n";
        }
        
        // Extract child elements
        $elements = ['title', 'author', 'price', 'publish_date'];
        foreach ($elements as $element) {
            $pattern = "/<{$element}[^>]*>([^<]*)<\/{$element}>/";
            if (preg_match($pattern, $book[2], $matches)) {
                echo ucfirst($element) . ": {$matches[1]}\n";
            }
        }
        
        // Extract price with currency
        if (preg_match('/<price currency="([^"]*)">(.*?)<\/price>/', $book[2], $price)) {
            echo "Price with currency: {$price[2]} {$price[1]}\n";
        }
        
        echo "\n";
    }
}

// Simple XML cleanup
$cleaned = preg_replace('/<!--.*?-->/s', '', $xml); // Remove comments
$cleaned = preg_replace('/\s+/', ' ', $cleaned);     // Normalize whitespace
echo "Cleaned XML length: " . strlen($cleaned) . " characters\n";
```

XML parsing with regex handles simple structures but has limitations  
with nested elements and complex markup. The 's' flag makes . match  
newlines for multiline content. Use proper XML parsers for production.  

## Performance optimization techniques

Optimizing regex patterns for better performance and efficiency.  

```php
<?php

$text = str_repeat("The quick brown fox jumps over the lazy dog. ", 1000);
$email_list = str_repeat("user1@example.com, user2@test.org, invalid-email, user3@domain.net, ", 250);

// Performance comparison
function benchmarkPattern($pattern, $text, $function, $iterations = 1000) {
    $start = microtime(true);
    
    for ($i = 0; $i < $iterations; $i++) {
        $function($pattern, $text);
    }
    
    $end = microtime(true);
    return ($end - $start) * 1000; // Convert to milliseconds
}

// Efficient vs inefficient patterns
$patterns = [
    'inefficient_email' => '/.*@.*\..*/',
    'efficient_email' => '/[^\s@]+@[^\s@]+\.[^\s@]+/',
    'inefficient_word' => '/.*(fox).*/',
    'efficient_word' => '/\bfox\b/',
    'greedy_quantifier' => '/".+"/',
    'non_greedy_quantifier' => '/".+?"/',
    'compiled_pattern' => null // Will be compiled
];

// Compile pattern for reuse
$compiled = '/[^\s@]+@[^\s@]+\.[^\s@]+/';

echo "=== Performance Benchmarks ===\n";

// Email matching benchmark
echo "Email pattern performance (1000 iterations):\n";
$inefficient_time = benchmarkPattern($patterns['inefficient_email'], $email_list, 'preg_match_all');
$efficient_time = benchmarkPattern($patterns['efficient_email'], $email_list, 'preg_match_all');

echo "Inefficient pattern: {$inefficient_time:.2f}ms\n";
echo "Efficient pattern: {$efficient_time:.2f}ms\n";
echo "Improvement: " . number_format(($inefficient_time - $efficient_time) / $inefficient_time * 100, 1) . "%\n\n";

// Word boundary benchmark
echo "Word matching performance (1000 iterations):\n";
$greedy_time = benchmarkPattern($patterns['inefficient_word'], $text, 'preg_match');
$boundary_time = benchmarkPattern($patterns['efficient_word'], $text, 'preg_match');

echo "Greedy pattern: {$greedy_time:.2f}ms\n";
echo "Word boundary: {$boundary_time:.2f}ms\n";
echo "Improvement: " . number_format(($greedy_time - $boundary_time) / $greedy_time * 100, 1) . "%\n\n";

// Optimization tips
echo "=== Optimization Tips ===\n";
echo "1. Use specific character classes instead of '.*'\n";
echo "2. Anchor patterns when possible (^, $, \\b)\n";
echo "3. Use non-greedy quantifiers when appropriate\n";
echo "4. Avoid excessive backtracking with nested quantifiers\n";
echo "5. Compile patterns for reuse in loops\n";
echo "6. Use word boundaries (\\b) instead of greedy matching\n";
echo "7. Test pattern performance with realistic data\n";
```

Pattern optimization focuses on reducing backtracking and improving  
specificity. Anchors and word boundaries prevent unnecessary matching.  
Non-greedy quantifiers stop at first match rather than longest match.  

## Unicode and international text

Handling Unicode characters and international text with regex.  

```php
<?php

// Enable UTF-8 mode
$texts = [
    "Café français",
    "日本語テキスト", 
    "Ελληνικά",
    "العربية",
    "한국어",
    "Русский текст",
    "中文字符",
    "Emoji: 🌟⭐✨"
];

// Unicode-aware patterns (use 'u' flag)
$patterns = [
    'letters' => '/\p{L}+/u',           // Any letter
    'word_chars' => '/\w+/u',           // Word characters
    'whitespace' => '/\s+/u',           // Whitespace
    'punctuation' => '/\p{P}+/u',       // Punctuation
    'numbers' => '/\p{N}+/u',           // Numbers
    'symbols' => '/\p{S}+/u',           // Symbols
    'emoji' => '/[\x{1F600}-\x{1F64F}]|[\x{1F300}-\x{1F5FF}]|[\x{1F680}-\x{1F6FF}]|[\x{1F1E0}-\x{1F1FF}]|[\x{2600}-\x{26FF}]|[\x{2700}-\x{27BF}]/u'
];

foreach ($texts as $text) {
    echo "Text: $text\n";
    echo "Length: " . mb_strlen($text) . " characters\n";
    
    foreach ($patterns as $name => $pattern) {
        if (preg_match_all($pattern, $text, $matches)) {
            echo "  $name: " . implode(', ', array_unique($matches[0])) . "\n";
        }
    }
    echo "\n";
}

// Language-specific patterns
$email_intl = "用户@example.中国";
$url_intl = "https://例え.テスト";

// International domain names (basic check)
$idn_pattern = '/^[^\s@]+@[^\s@]+\.[\p{L}\p{N}.-]+$/u';
if (preg_match($idn_pattern, $email_intl)) {
    echo "International email format valid: $email_intl\n";
}

// Unicode normalization example
$accented = "café";
$normalized = preg_replace('/[àáâãäå]/u', 'a', $accented);
$normalized = preg_replace('/[èéêë]/u', 'e', $normalized);
echo "Normalized: $accented -> $normalized\n";

// Case conversion with Unicode
$mixed = "Mixed CASE Ελληνικά 中文";
echo "Lowercase: " . mb_strtolower($mixed, 'UTF-8') . "\n";
echo "Uppercase: " . mb_strtoupper($mixed, 'UTF-8') . "\n";
```

Unicode regex requires the 'u' flag for proper UTF-8 handling.  
Unicode property classes like \p{L} match character categories.  
Use mb_ functions for proper multibyte string operations.  

## File path and directory validation

Validating and parsing file paths, extensions, and directory structures.  

```php
<?php

$paths = [
    "/home/user/documents/file.txt",
    "C:\\Users\\Username\\Desktop\\image.png", 
    "../relative/path/script.php",
    "~/config/settings.json",
    "file.pdf",
    "folder/",
    "/root",
    "invalid<>path.txt"
];

$patterns = [
    'unix_absolute' => '/^\/(?:[^\/\0]+\/)*[^\/\0]*$/',
    'windows_absolute' => '/^[A-Za-z]:[\\\\\/](?:[^\\\\\/\0<>:"|?*]+[\\\\\/])*[^\\\\\/\0<>:"|?*]*$/',
    'relative' => '/^(?!\/|[A-Za-z]:)[^<>:"|?*\0]+$/',
    'filename_only' => '/^[^\/\\\\<>:"|?*\0]+\.[a-zA-Z0-9]+$/',
    'directory' => '/^.*\/$/',
    'hidden_file' => '/\/\.[^\/]*$/',
    'extension' => '/\.([a-zA-Z0-9]+)$/'
];

foreach ($paths as $path) {
    echo "Path: $path\n";
    
    foreach ($patterns as $type => $pattern) {
        if (preg_match($pattern, $path)) {
            echo "  Type: $type\n";
        }
    }
    
    // Extract components
    if (preg_match('/^(.+)\/([^\/]+)$/', $path, $matches)) {
        echo "  Directory: {$matches[1]}\n";
        echo "  Filename: {$matches[2]}\n";
    }
    
    if (preg_match('/\.([a-zA-Z0-9]+)$/', $path, $ext)) {
        echo "  Extension: {$ext[1]}\n";
    }
    
    // Security check
    if (preg_match('/\.\.|[<>:"|?*\0]/', $path)) {
        echo "  Security: UNSAFE\n";
    } else {
        echo "  Security: SAFE\n";
    }
    
    echo "\n";
}

// File type validation
$allowedTypes = [
    'image' => '/\.(jpg|jpeg|png|gif|webp)$/i',
    'document' => '/\.(pdf|doc|docx|txt|rtf)$/i',
    'archive' => '/\.(zip|rar|tar|gz|7z)$/i',
    'script' => '/\.(php|js|py|sh)$/i'
];

$testFiles = ["image.jpg", "document.pdf", "script.php", "unknown.xyz"];

echo "=== File Type Classification ===\n";
foreach ($testFiles as $file) {
    echo "$file: ";
    $classified = false;
    foreach ($allowedTypes as $type => $pattern) {
        if (preg_match($pattern, $file)) {
            echo "$type file\n";
            $classified = true;
            break;
        }
    }
    if (!$classified) echo "unknown type\n";
}
```

File path validation ensures proper format and security. Pattern  
matching identifies path types, extracts components, and validates  
file extensions. Security patterns detect directory traversal attempts.  

## Configuration file parsing

Parsing various configuration file formats using regex patterns.  

```php
<?php

$iniConfig = '
; Database configuration
[database]
host = localhost
port = 3306
username = admin
password = "secret123"
database_name = myapp

; Application settings  
[app]
debug = true
timezone = "America/New_York"
max_upload_size = 10MB
';

$envConfig = '
# Environment variables
DATABASE_HOST=localhost
DATABASE_PORT=3306
API_KEY="abc123xyz"
DEBUG=true
CACHE_TTL=3600
# Comment line
EMPTY_VALUE=
';

$yamlConfig = '
database:
  host: localhost
  port: 3306
  credentials:
    username: admin
    password: secret123
app:
  name: "My Application"  
  version: 1.0.0
  features:
    - authentication
    - caching
    - logging
';

// INI file parsing
echo "=== INI Configuration ===\n";
$iniSections = [];
$currentSection = 'default';

// Parse sections
if (preg_match_all('/^\[([^\]]+)\]/m', $iniConfig, $sections)) {
    foreach ($sections[1] as $section) {
        $iniSections[$section] = [];
    }
}

// Parse key-value pairs
$kvPattern = '/^([^;#\s][^=]*?)\s*=\s*(.*)$/m';
if (preg_match_all($kvPattern, $iniConfig, $matches, PREG_SET_ORDER)) {
    $currentSection = 'default';
    
    foreach (explode("\n", $iniConfig) as $line) {
        if (preg_match('/^\[([^\]]+)\]/', $line, $sectionMatch)) {
            $currentSection = $sectionMatch[1];
        } elseif (preg_match($kvPattern, $line, $kvMatch)) {
            $key = trim($kvMatch[1]);
            $value = trim($kvMatch[2], '"');
            $iniSections[$currentSection][$key] = $value;
        }
    }
}

foreach ($iniSections as $section => $values) {
    echo "[$section]\n";
    foreach ($values as $key => $value) {
        echo "  $key = $value\n";
    }
}

// ENV file parsing
echo "\n=== Environment Configuration ===\n";
$envVars = [];
$envPattern = '/^([A-Z_][A-Z0-9_]*)\s*=\s*(.*)$/m';

if (preg_match_all($envPattern, $envConfig, $matches, PREG_SET_ORDER)) {
    foreach ($matches as $match) {
        $key = $match[1];
        $value = trim($match[2], '"');
        $envVars[$key] = $value;
        echo "$key = $value\n";
    }
}

// Basic YAML parsing (simple cases only)
echo "\n=== YAML Configuration (Basic) ===\n";
$yamlData = [];

// Parse simple key-value pairs
$simplePattern = '/^([a-z_]+):\s*(.+)$/m';
if (preg_match_all($simplePattern, $yamlConfig, $matches, PREG_SET_ORDER)) {
    foreach ($matches as $match) {
        $key = $match[1];
        $value = trim($match[2], '"');
        echo "$key: $value\n";
    }
}

// Parse nested structures (one level)
$nestedPattern = '/^([a-z_]+):\s*\n((?:\s{2}[a-z_]+:.*\n?)+)/m';
if (preg_match_all($nestedPattern, $yamlConfig, $matches, PREG_SET_ORDER)) {
    foreach ($matches as $match) {
        echo "{$match[1]}:\n";
        if (preg_match_all('/^\s{2}([a-z_]+):\s*(.+)$/m', $match[2], $subMatches, PREG_SET_ORDER)) {
            foreach ($subMatches as $sub) {
                echo "  {$sub[1]}: {$sub[2]}\n";
            }
        }
    }
}

// Validate configuration values
echo "\n=== Configuration Validation ===\n";
$validations = [
    'port' => '/^[1-9]\d{3,4}$/',
    'boolean' => '/^(true|false)$/i',
    'size' => '/^\d+(MB|GB|KB)$/i',
    'email' => '/^[^\s@]+@[^\s@]+\.[^\s@]+$/',
    'url' => '/^https?:\/\/[^\s]+$/'
];

$testValues = [
    'port' => '3306',
    'boolean' => 'true',
    'size' => '10MB',
    'email' => 'admin@example.com',
    'url' => 'https://api.example.com'
];

foreach ($testValues as $type => $value) {
    $valid = preg_match($validations[$type], $value);
    echo "$type '$value': " . ($valid ? "VALID" : "INVALID") . "\n";
}
```

Configuration parsing requires patterns for different file formats.  
INI files use sections and key-value pairs, ENV files use simple  
assignments, YAML uses indentation. Validation patterns ensure  
proper value formats for different configuration types.  
