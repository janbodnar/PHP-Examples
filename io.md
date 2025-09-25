# I/O Operations

This chapter covers 35 comprehensive PHP I/O operation examples designed to  
demonstrate the full spectrum of input/output capabilities in modern PHP 8.4.  
These examples progress from basic file operations to advanced stream handling  
and network I/O. Each example includes detailed explanations to help you  
master PHP I/O programming for real-world applications.  

## Reading entire file

Reading complete file contents into memory at once.  

```php
<?php

$filename = '/tmp/sample.txt';

// Create a sample file first
file_put_contents($filename, "Hello there!\nThis is line 2.\nThis is line 3.");

// Read entire file as string
$content = file_get_contents($filename);
echo "File contents:\n$content\n";

// Read file as array of lines
$lines = file($filename, FILE_IGNORE_NEW_LINES);
echo "Lines array:\n";
print_r($lines);

// Clean up
unlink($filename);
```

The `file_get_contents()` function reads the entire file into a string,  
while `file()` reads it into an array of lines. Use `FILE_IGNORE_NEW_LINES`  
flag to remove newline characters from each line automatically.  

## Writing to files

Creating and writing content to files with different methods.  

```php
<?php

$filename = '/tmp/output.txt';
$data = "Hello there!\nThis is new content.\n";

// Write string to file (overwrites existing content)
$bytesWritten = file_put_contents($filename, $data);
echo "Wrote $bytesWritten bytes to file\n";

// Append to file
$additionalData = "This line is appended.\n";
file_put_contents($filename, $additionalData, FILE_APPEND);

// Read and display result
echo "Final file content:\n";
echo file_get_contents($filename);

// Clean up
unlink($filename);
```

`file_put_contents()` is the simplest way to write data to files.  
It returns the number of bytes written or FALSE on failure. Use  
`FILE_APPEND` flag to add content to existing files instead of overwriting.  

## File handle operations

Working with file handles for more control over file operations.  

```php
<?php

$filename = '/tmp/handle_example.txt';

// Open file for writing
$handle = fopen($filename, 'w');
if ($handle === false) {
    die("Cannot open file for writing\n");
}

// Write multiple lines
$lines = ["First line\n", "Second line\n", "Third line\n"];
foreach ($lines as $line) {
    fwrite($handle, $line);
}

// Close the handle
fclose($handle);

// Open file for reading
$handle = fopen($filename, 'r');
if ($handle === false) {
    die("Cannot open file for reading\n");
}

echo "Reading file line by line:\n";
while (($line = fgets($handle)) !== false) {
    echo "Line: " . trim($line) . "\n";
}

fclose($handle);
unlink($filename);
```

File handles provide granular control over I/O operations. Use `fopen()`  
to get a handle, `fwrite()` and `fgets()` for writing and reading,  
and always `fclose()` to release resources properly.  

## Checking file properties

Examining file existence, permissions, and metadata.  

```php
<?php

$filename = '/tmp/properties_test.txt';
file_put_contents($filename, "Sample content");

// Basic existence and type checks
echo "File existence and type:\n";
echo "  exists: " . (file_exists($filename) ? "yes" : "no") . "\n";
echo "  is_file: " . (is_file($filename) ? "yes" : "no") . "\n";
echo "  is_dir: " . (is_dir($filename) ? "yes" : "no") . "\n";
echo "  is_readable: " . (is_readable($filename) ? "yes" : "no") . "\n";
echo "  is_writable: " . (is_writable($filename) ? "yes" : "no") . "\n";

// File size and timestamps
echo "\nFile metadata:\n";
echo "  size: " . filesize($filename) . " bytes\n";
echo "  modified: " . date('Y-m-d H:i:s', filemtime($filename)) . "\n";
echo "  accessed: " . date('Y-m-d H:i:s', fileatime($filename)) . "\n";
echo "  permissions: " . substr(sprintf('%o', fileperms($filename)), -4) . "\n";

// Path information
$pathInfo = pathinfo($filename);
echo "\nPath information:\n";
foreach ($pathInfo as $key => $value) {
    echo "  $key: $value\n";
}

unlink($filename);
```

PHP provides numerous functions to examine file properties. Check  
existence with `file_exists()`, permissions with `is_readable()` and  
`is_writable()`, and get metadata like size and timestamps.  

## Directory operations

Creating, listing, and manipulating directories.  

```php
<?php

$baseDir = '/tmp/php_io_test';
$subDir = $baseDir . '/subdirectory';

// Create directories
if (!is_dir($baseDir)) {
    mkdir($baseDir, 0755, true);
    echo "Created base directory: $baseDir\n";
}

if (!is_dir($subDir)) {
    mkdir($subDir, 0755);
    echo "Created subdirectory: $subDir\n";
}

// Create sample files
$files = ['file1.txt', 'file2.log', 'file3.php'];
foreach ($files as $file) {
    file_put_contents("$baseDir/$file", "Content of $file");
}

// List directory contents
echo "\nDirectory contents of $baseDir:\n";
$contents = scandir($baseDir);
foreach ($contents as $item) {
    if ($item !== '.' && $item !== '..') {
        $fullPath = "$baseDir/$item";
        $type = is_dir($fullPath) ? 'DIR' : 'FILE';
        echo "  $type: $item\n";
    }
}

// Remove files and directories
foreach ($files as $file) {
    unlink("$baseDir/$file");
}
rmdir($subDir);
rmdir($baseDir);
echo "\nCleaned up directories\n";
```

Use `mkdir()` to create directories with optional recursive creation.  
`scandir()` lists directory contents as an array. Always clean up  
with `unlink()` for files and `rmdir()` for directories.  

## Reading file line by line

Processing large files efficiently without loading everything into memory.  

```php
<?php

$filename = '/tmp/large_file.txt';

// Create a sample file with multiple lines
$lines = [];
for ($i = 1; $i <= 1000; $i++) {
    $lines[] = "This is line number $i with some content.";
}
file_put_contents($filename, implode("\n", $lines));

echo "Processing file line by line:\n";
$lineCount = 0;
$totalLength = 0;

// Method 1: Using file handle
$handle = fopen($filename, 'r');
if ($handle) {
    while (($line = fgets($handle)) !== false) {
        $lineCount++;
        $totalLength += strlen(trim($line));
        
        // Process first 5 lines as example
        if ($lineCount <= 5) {
            echo "Line $lineCount: " . trim($line) . "\n";
        }
    }
    fclose($handle);
}

echo "\nFile statistics:\n";
echo "  Total lines: $lineCount\n";
echo "  Average line length: " . round($totalLength / $lineCount, 2) . " chars\n";

unlink($filename);
```

Reading files line by line is memory-efficient for large files.  
Use `fgets()` with a file handle to read one line at a time,  
processing each line before reading the next.  

## Binary file operations

Working with binary data and file modes.  

```php
<?php

$binaryFile = '/tmp/binary_data.bin';

// Create binary data
$data = '';
for ($i = 0; $i < 256; $i++) {
    $data .= chr($i);
}

// Write binary data
file_put_contents($binaryFile, $data, LOCK_EX);
echo "Written " . strlen($data) . " bytes of binary data\n";

// Read binary data
$readData = file_get_contents($binaryFile);
echo "Read " . strlen($readData) . " bytes\n";

// Verify data integrity
if ($data === $readData) {
    echo "Binary data integrity verified\n";
} else {
    echo "Data corruption detected\n";
}

// Examine first 10 bytes
echo "First 10 bytes (decimal): ";
for ($i = 0; $i < 10; $i++) {
    echo ord($readData[$i]) . " ";
}
echo "\n";

// Work with binary file handle
$handle = fopen($binaryFile, 'rb');
if ($handle) {
    fseek($handle, 128);  // Seek to position 128
    $chunk = fread($handle, 10);  // Read 10 bytes
    
    echo "Bytes 128-137 (hex): ";
    for ($i = 0; $i < strlen($chunk); $i++) {
        echo sprintf('%02x ', ord($chunk[$i]));
    }
    echo "\n";
    
    fclose($handle);
}

unlink($binaryFile);
```

Binary file operations require the 'b' flag in `fopen()` mode.  
Use `fread()` and `fwrite()` for binary data, and `fseek()`  
to navigate within files. Always verify data integrity.  

## File locking

Preventing concurrent access issues with file locking.  

```php
<?php

$counterFile = '/tmp/counter.txt';
$lockFile = '/tmp/counter.lock';

function incrementCounter($file, $lockFile): int {
    // Acquire exclusive lock
    $lock = fopen($lockFile, 'c');
    if (!flock($lock, LOCK_EX)) {
        fclose($lock);
        throw new RuntimeException('Cannot acquire lock');
    }
    
    try {
        // Read current value
        $current = file_exists($file) ? (int)file_get_contents($file) : 0;
        
        // Increment and write back
        $new = $current + 1;
        file_put_contents($file, $new);
        
        return $new;
    } finally {
        // Always release lock
        flock($lock, LOCK_UN);
        fclose($lock);
    }
}

// Initialize counter
file_put_contents($counterFile, '0');

echo "Simulating concurrent counter increments:\n";
for ($i = 1; $i <= 5; $i++) {
    try {
        $value = incrementCounter($counterFile, $lockFile);
        echo "Increment $i: counter = $value\n";
    } catch (RuntimeException $e) {
        echo "Error on increment $i: " . $e->getMessage() . "\n";
    }
}

// Clean up
unlink($counterFile);
unlink($lockFile);
```

File locking prevents race conditions in concurrent environments.  
Use `flock()` with `LOCK_EX` for exclusive access and `LOCK_UN`  
to release. Always use try/finally to ensure lock release.  

## CSV file handling

Reading and writing CSV files with proper parsing.  

```php
<?php

$csvFile = '/tmp/data.csv';

// Sample data
$people = [
    ['Name', 'Age', 'City', 'Salary'],
    ['Alice Johnson', 28, 'New York', 75000],
    ['Bob Smith', 34, 'Los Angeles', 82000],
    ['Charlie Brown', 45, 'Chicago', 95000],
    ['Diana Wilson', 29, 'Houston', 68000]
];

// Write CSV file
$handle = fopen($csvFile, 'w');
foreach ($people as $row) {
    fputcsv($handle, $row);
}
fclose($handle);

echo "CSV file created with " . count($people) . " rows\n";

// Read CSV file
echo "\nReading CSV data:\n";
$handle = fopen($csvFile, 'r');
$rowNumber = 0;

while (($row = fgetcsv($handle)) !== false) {
    $rowNumber++;
    if ($rowNumber === 1) {
        echo "Headers: " . implode(' | ', $row) . "\n";
        continue;
    }
    
    echo "Row $rowNumber: ";
    echo "Name={$row[0]}, Age={$row[1]}, City={$row[2]}, Salary=\${$row[3]}\n";
}

fclose($handle);

// Process CSV with custom delimiter
$tsvFile = '/tmp/data.tsv';
$handle = fopen($tsvFile, 'w');
foreach ($people as $row) {
    fputcsv($handle, $row, "\t");  // Tab-separated
}
fclose($handle);

echo "\nReading TSV file:\n";
$handle = fopen($tsvFile, 'r');
while (($row = fgetcsv($handle, 0, "\t")) !== false) {
    echo implode(' | ', $row) . "\n";
    break; // Just show first row
}
fclose($handle);

unlink($csvFile);
unlink($tsvFile);
```

Use `fputcsv()` and `fgetcsv()` for proper CSV handling with  
automatic escaping and parsing. Custom delimiters and enclosures  
are supported for different file formats like TSV.  

## JSON file operations

Working with JSON data in files.  

```php
<?php

$jsonFile = '/tmp/config.json';

// Sample configuration data
$config = [
    'app' => [
        'name' => 'My Application',
        'version' => '1.2.3',
        'debug' => true
    ],
    'database' => [
        'host' => 'localhost',
        'port' => 3306,
        'name' => 'app_db',
        'credentials' => [
            'user' => 'app_user',
            'password' => 'secret123'
        ]
    ],
    'features' => ['logging', 'caching', 'authentication']
];

// Write JSON file
$jsonData = json_encode($config, JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES);
if (file_put_contents($jsonFile, $jsonData) === false) {
    die("Failed to write JSON file\n");
}

echo "JSON configuration saved\n";

// Read and parse JSON file
$readData = file_get_contents($jsonFile);
$parsedConfig = json_decode($readData, true);

if (json_last_error() !== JSON_ERROR_NONE) {
    die("JSON parsing error: " . json_last_error_msg() . "\n");
}

echo "Configuration loaded:\n";
echo "  App: {$parsedConfig['app']['name']} v{$parsedConfig['app']['version']}\n";
echo "  Database: {$parsedConfig['database']['host']}:{$parsedConfig['database']['port']}\n";
echo "  Features: " . implode(', ', $parsedConfig['features']) . "\n";

// Modify and save back
$parsedConfig['app']['version'] = '1.2.4';
$parsedConfig['features'][] = 'reporting';

$updatedJson = json_encode($parsedConfig, JSON_PRETTY_PRINT);
file_put_contents($jsonFile, $updatedJson);

echo "\nConfiguration updated and saved\n";

unlink($jsonFile);
```

Always use `JSON_PRETTY_PRINT` for readable output and check  
`json_last_error()` after parsing. JSON files are ideal for  
configuration data and structured information exchange.  

## Stream operations

Working with PHP streams for advanced I/O operations.  

```php
<?php

// Create memory stream
$memoryStream = fopen('php://memory', 'w+');
fwrite($memoryStream, "Hello there!\nThis is in memory.\n");
rewind($memoryStream);

echo "Memory stream contents:\n";
while (!feof($memoryStream)) {
    echo fgets($memoryStream);
}
fclose($memoryStream);

// Temporary file stream
$tempStream = fopen('php://temp/maxmemory:1024', 'w+');
for ($i = 1; $i <= 100; $i++) {
    fwrite($tempStream, "Line $i: " . str_repeat('x', 50) . "\n");
}

echo "\nTemporary stream size: " . ftell($tempStream) . " bytes\n";
rewind($tempStream);

// Read first 3 lines
for ($i = 0; $i < 3; $i++) {
    echo fgets($tempStream);
}

fclose($tempStream);

// Filter stream example
$data = "HELLO WORLD\nTHIS IS UPPERCASE\nCONVERT TO LOWERCASE\n";
$upperFile = '/tmp/uppercase.txt';
file_put_contents($upperFile, $data);

// Apply lowercase filter
$context = stream_context_create([
    'string.rot13' => [],  // Built-in filter example
]);

echo "\nFiltered stream (using context):\n";
$filteredContent = file_get_contents($upperFile);
echo strtolower($filteredContent);

unlink($upperFile);
```

PHP streams provide flexible I/O abstractions. Memory streams  
keep data in RAM, temp streams spill to disk when needed.  
Stream contexts and filters enable data transformation.  

## Standard input/output

Working with console input and output streams.  

```php
<?php

// Standard output
echo "This goes to STDOUT\n";

// Standard error
fwrite(STDERR, "This goes to STDERR\n");

// Standard input (simulate with a file for demonstration)
$inputFile = '/tmp/input.txt';
file_put_contents($inputFile, "Alice\n25\nDeveloper\n");

echo "Reading simulated user input:\n";
$input = fopen($inputFile, 'r');

echo "Enter your name: ";
$name = trim(fgets($input));

echo "Enter your age: ";
$age = trim(fgets($input));

echo "Enter your job: ";
$job = trim(fgets($input));

fclose($input);

echo "\nUser information:\n";
echo "  Name: $name\n";
echo "  Age: $age\n";
echo "  Job: $job\n";

// Output to different streams
$outputs = [
    'stdout' => STDOUT,
    'stderr' => STDERR,
];

foreach ($outputs as $name => $stream) {
    fwrite($stream, "Message to $name\n");
}

unlink($inputFile);
```

Use `STDIN`, `STDOUT`, and `STDERR` constants for standard streams.  
`fgets(STDIN)` reads user input, while `fwrite(STDERR, ...)` sends  
error messages to the error stream separate from normal output.  

## File copying and moving

Copying, moving, and renaming files safely.  

```php
<?php

$sourceDir = '/tmp/source';
$destDir = '/tmp/destination';

// Create directories
mkdir($sourceDir, 0755, true);
mkdir($destDir, 0755, true);

$originalFile = "$sourceDir/original.txt";
$copyFile = "$destDir/copy.txt";
$movedFile = "$destDir/moved.txt";

// Create original file
$content = "Original file content\nLine 2\nLine 3\n";
file_put_contents($originalFile, $content);

echo "Created original file: $originalFile\n";

// Copy file
if (copy($originalFile, $copyFile)) {
    echo "File copied successfully to: $copyFile\n";
} else {
    echo "Failed to copy file\n";
}

// Verify copy
if (file_exists($copyFile) && file_get_contents($copyFile) === $content) {
    echo "Copy verified - content matches\n";
}

// Move/rename file
if (rename($copyFile, $movedFile)) {
    echo "File moved/renamed to: $movedFile\n";
} else {
    echo "Failed to move/rename file\n";
}

// Check file states
echo "\nFile existence check:\n";
echo "  Original exists: " . (file_exists($originalFile) ? "yes" : "no") . "\n";
echo "  Copy exists: " . (file_exists($copyFile) ? "yes" : "no") . "\n";
echo "  Moved exists: " . (file_exists($movedFile) ? "yes" : "no") . "\n";

// File permissions and timestamps
echo "\nFile information:\n";
foreach ([$originalFile, $movedFile] as $file) {
    if (file_exists($file)) {
        $name = basename($file);
        echo "  $name: " . filesize($file) . " bytes, ";
        echo "modified: " . date('Y-m-d H:i:s', filemtime($file)) . "\n";
    }
}

// Clean up
unlink($originalFile);
unlink($movedFile);
rmdir($sourceDir);
rmdir($destDir);
```

Use `copy()` to duplicate files and `rename()` to move or rename them.  
Always verify operations and handle failures appropriately.  
File permissions and timestamps are preserved during operations.  

## Directory traversal

Recursively traversing directory trees.  

```php
<?php

$testDir = '/tmp/traverse_test';

// Create test directory structure
$dirs = [
    $testDir,
    "$testDir/subdir1",
    "$testDir/subdir2",
    "$testDir/subdir1/nested"
];

foreach ($dirs as $dir) {
    mkdir($dir, 0755, true);
}

// Create test files
$files = [
    "$testDir/root.txt" => "Root file",
    "$testDir/subdir1/file1.txt" => "File in subdir1",
    "$testDir/subdir1/file2.log" => "Log file in subdir1",
    "$testDir/subdir2/file3.php" => "<?php echo 'PHP file'; ?>",
    "$testDir/subdir1/nested/deep.txt" => "Deep nested file"
];

foreach ($files as $path => $content) {
    file_put_contents($path, $content);
}

// Method 1: Recursive directory iteration
echo "Recursive directory traversal:\n";
$iterator = new RecursiveIteratorIterator(
    new RecursiveDirectoryIterator($testDir, RecursiveDirectoryIterator::SKIP_DOTS)
);

foreach ($iterator as $file) {
    $relativePath = str_replace($testDir . '/', '', $file->getPathname());
    $type = $file->isDir() ? 'DIR' : 'FILE';
    $size = $file->isFile() ? $file->getSize() . ' bytes' : '';
    echo "  $type: $relativePath $size\n";
}

// Method 2: Manual recursive function
function listDirectory($dir, $prefix = '') {
    $items = scandir($dir);
    foreach ($items as $item) {
        if ($item === '.' || $item === '..') continue;
        
        $fullPath = "$dir/$item";
        echo $prefix . ($prefix ? '├── ' : '') . $item;
        
        if (is_dir($fullPath)) {
            echo " [DIR]\n";
            listDirectory($fullPath, $prefix . '│   ');
        } else {
            echo " (" . filesize($fullPath) . " bytes)\n";
        }
    }
}

echo "\nTree-style listing:\n";
listDirectory($testDir);

// Clean up
function removeDirectory($dir) {
    $iterator = new RecursiveIteratorIterator(
        new RecursiveDirectoryIterator($dir, RecursiveDirectoryIterator::SKIP_DOTS),
        RecursiveIteratorIterator::CHILD_FIRST
    );

    foreach ($iterator as $file) {
        if ($file->isDir()) {
            rmdir($file->getPathname());
        } else {
            unlink($file->getPathname());
        }
    }
    rmdir($dir);
}

removeDirectory($testDir);
echo "\nCleaned up test directory\n";
```

Use `RecursiveDirectoryIterator` with `RecursiveIteratorIterator`  
for efficient directory traversal. The `CHILD_FIRST` flag is useful  
for cleanup operations where subdirectories must be empty first.  

## File permissions and ownership

Managing file permissions and ownership information.  

```php
<?php

$testFile = '/tmp/permissions_test.txt';
file_put_contents($testFile, "Permission test file");

echo "File permissions and ownership:\n";

// Get current permissions
$perms = fileperms($testFile);
$octalPerms = sprintf('%o', $perms);
$readablePerms = substr($octalPerms, -3);

echo "  Current permissions: $readablePerms (octal)\n";

// Detailed permission breakdown
$owner = ($perms & 0x0100) ? 'r' : '-';
$owner .= ($perms & 0x0080) ? 'w' : '-';
$owner .= ($perms & 0x0040) ? 'x' : '-';

$group = ($perms & 0x0020) ? 'r' : '-';
$group .= ($perms & 0x0010) ? 'w' : '-';
$group .= ($perms & 0x0008) ? 'x' : '-';

$other = ($perms & 0x0004) ? 'r' : '-';
$other .= ($perms & 0x0002) ? 'w' : '-';
$other .= ($perms & 0x0001) ? 'x' : '-';

echo "  Symbolic: $owner$group$other\n";

// Test individual permissions
$checks = [
    'readable' => is_readable($testFile),
    'writable' => is_writable($testFile),
    'executable' => is_executable($testFile)
];

foreach ($checks as $check => $result) {
    echo "  Is $check: " . ($result ? "yes" : "no") . "\n";
}

// Change permissions
if (chmod($testFile, 0644)) {
    echo "\nChanged permissions to 644\n";
    $newPerms = sprintf('%o', fileperms($testFile) & 0777);
    echo "  New permissions: " . substr($newPerms, -3) . "\n";
}

// File ownership information (may not be available on all systems)
$owner = function_exists('posix_getpwuid') ? posix_getpwuid(fileowner($testFile)) : null;
$group = function_exists('posix_getgrgid') ? posix_getgrgid(filegroup($testFile)) : null;

if ($owner) {
    echo "  Owner: {$owner['name']} (UID: " . fileowner($testFile) . ")\n";
}
if ($group) {
    echo "  Group: {$group['name']} (GID: " . filegroup($testFile) . ")\n";
}

unlink($testFile);
```

Use `fileperms()` to get permission bits and `chmod()` to change them.  
Permission checking functions like `is_readable()` and `is_writable()`  
provide convenient access control verification.  

## Working with compressed files

Reading and writing compressed data.  

```php
<?php

$originalFile = '/tmp/original.txt';
$gzipFile = '/tmp/compressed.gz';
$zipFile = '/tmp/archive.zip';

// Create sample data
$data = str_repeat("This is line " . rand(1, 1000) . " with sample data.\n", 1000);
file_put_contents($originalFile, $data);

$originalSize = filesize($originalFile);
echo "Original file size: $originalSize bytes\n";

// GZIP compression
$compressed = gzencode($data, 9);  // Maximum compression
file_put_contents($gzipFile, $compressed);

$compressedSize = filesize($gzipFile);
$ratio = round((1 - $compressedSize / $originalSize) * 100, 2);
echo "GZIP compressed size: $compressedSize bytes ({$ratio}% reduction)\n";

// GZIP decompression
$decompressed = gzdecode(file_get_contents($gzipFile));
if ($decompressed === $data) {
    echo "GZIP decompression successful\n";
}

// ZIP archive (requires zip extension)
if (extension_loaded('zip')) {
    $zip = new ZipArchive();
    if ($zip->open($zipFile, ZipArchive::CREATE) === TRUE) {
        $zip->addFromString('sample.txt', $data);
        $zip->addFromString('info.txt', "Archive created: " . date('Y-m-d H:i:s'));
        $zip->close();
        
        $zipSize = filesize($zipFile);
        echo "ZIP archive size: $zipSize bytes\n";
        
        // Read from ZIP
        $zip = new ZipArchive();
        if ($zip->open($zipFile) === TRUE) {
            echo "ZIP contents:\n";
            for ($i = 0; $i < $zip->numFiles; $i++) {
                $filename = $zip->getNameIndex($i);
                echo "  File: $filename\n";
            }
            $zip->close();
        }
    }
} else {
    echo "ZIP extension not available\n";
}

// Clean up
unlink($originalFile);
unlink($gzipFile);
if (file_exists($zipFile)) {
    unlink($zipFile);
}
```

Use `gzencode()` and `gzdecode()` for GZIP compression.  
The `ZipArchive` class provides complete ZIP file manipulation  
capabilities for creating and reading compressed archives.  

## Network I/O operations

Reading data from URLs and network resources.  

```php
<?php

// HTTP context for custom headers and options
$context = stream_context_create([
    'http' => [
        'method' => 'GET',
        'header' => [
            'User-Agent: PHP HTTP Client',
            'Accept: application/json, text/plain'
        ],
        'timeout' => 30
    ]
]);

// Simulate network request (using a data URL since external access may be limited)
$jsonData = json_encode([
    'message' => 'Hello there!',
    'timestamp' => date('c'),
    'data' => range(1, 10)
]);

$dataUrl = 'data://application/json;base64,' . base64_encode($jsonData);

echo "Fetching data from simulated URL:\n";
$response = file_get_contents($dataUrl, false, $context);

if ($response !== false) {
    echo "Response received: " . strlen($response) . " bytes\n";
    
    $decoded = json_decode($response, true);
    if ($decoded) {
        echo "Message: {$decoded['message']}\n";
        echo "Timestamp: {$decoded['timestamp']}\n";
        echo "Data array: " . implode(', ', $decoded['data']) . "\n";
    }
} else {
    echo "Failed to fetch data\n";
}

// cURL example (if extension is available)
if (extension_loaded('curl')) {
    echo "\nUsing cURL for HTTP requests:\n";
    
    $ch = curl_init();
    curl_setopt_array($ch, [
        CURLOPT_URL => $dataUrl,
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_TIMEOUT => 30,
        CURLOPT_USERAGENT => 'PHP cURL Client',
        CURLOPT_FOLLOWLOCATION => true
    ]);
    
    $curlResponse = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    $error = curl_error($ch);
    curl_close($ch);
    
    if ($curlResponse !== false && empty($error)) {
        echo "cURL request successful (HTTP $httpCode)\n";
        echo "Response size: " . strlen($curlResponse) . " bytes\n";
    } else {
        echo "cURL error: $error\n";
    }
} else {
    echo "\ncURL extension not available\n";
}

// Socket example for low-level network I/O
echo "\nSocket operations:\n";
$socket = socket_create(AF_INET, SOCK_STREAM, SOL_TCP);
if ($socket !== false) {
    echo "Socket created successfully\n";
    socket_close($socket);
} else {
    echo "Socket creation failed\n";
}
```

Use `file_get_contents()` with stream contexts for simple HTTP requests.  
cURL provides more control over HTTP operations. Sockets enable  
low-level network programming for custom protocols.  

## Error handling in I/O

Proper error handling for file operations.  

```php
<?php

function safeFileOperation($filename, $operation) {
    $errors = [];
    
    try {
        switch ($operation) {
            case 'read':
                if (!file_exists($filename)) {
                    throw new RuntimeException("File does not exist: $filename");
                }
                
                if (!is_readable($filename)) {
                    throw new RuntimeException("File is not readable: $filename");
                }
                
                $content = file_get_contents($filename);
                if ($content === false) {
                    throw new RuntimeException("Failed to read file: $filename");
                }
                
                return ['success' => true, 'data' => $content];
                
            case 'write':
                $testData = "Test data for error handling\n";
                
                $dirname = dirname($filename);
                if (!is_dir($dirname)) {
                    if (!mkdir($dirname, 0755, true)) {
                        throw new RuntimeException("Cannot create directory: $dirname");
                    }
                }
                
                if (!is_writable($dirname)) {
                    throw new RuntimeException("Directory not writable: $dirname");
                }
                
                $result = file_put_contents($filename, $testData);
                if ($result === false) {
                    throw new RuntimeException("Failed to write file: $filename");
                }
                
                return ['success' => true, 'bytes' => $result];
                
            default:
                throw new InvalidArgumentException("Unknown operation: $operation");
        }
    } catch (Exception $e) {
        return [
            'success' => false,
            'error' => $e->getMessage(),
            'type' => get_class($e)
        ];
    }
}

// Test error handling
$testFiles = [
    '/tmp/test_valid.txt',
    '/nonexistent/path/file.txt',  // Should fail
    '/tmp/error_test.txt'
];

echo "Testing file error handling:\n";

foreach ($testFiles as $file) {
    echo "\nTesting file: $file\n";
    
    // Test write operation
    $writeResult = safeFileOperation($file, 'write');
    if ($writeResult['success']) {
        echo "  Write: SUCCESS ({$writeResult['bytes']} bytes)\n";
        
        // Test read operation
        $readResult = safeFileOperation($file, 'read');
        if ($readResult['success']) {
            echo "  Read: SUCCESS (" . strlen($readResult['data']) . " bytes)\n";
            unlink($file);  // Clean up successful writes
        } else {
            echo "  Read: FAILED - {$readResult['error']}\n";
        }
    } else {
        echo "  Write: FAILED - {$writeResult['error']} ({$writeResult['type']})\n";
    }
}

// Demonstrate different error types
echo "\nTesting different error scenarios:\n";

$errorTests = [
    'invalid_operation' => function() {
        return safeFileOperation('/tmp/test.txt', 'invalid');
    },
    'permission_denied' => function() {
        // Simulate permission denied (may not work on all systems)
        $restrictedFile = '/root/restricted.txt';
        return safeFileOperation($restrictedFile, 'write');
    }
];

foreach ($errorTests as $testName => $testFunction) {
    $result = $testFunction();
    echo "  $testName: ";
    if ($result['success']) {
        echo "UNEXPECTED SUCCESS\n";
    } else {
        echo "FAILED as expected - {$result['error']}\n";
    }
}
```

Always check file existence, permissions, and operation results.  
Use try/catch blocks to handle exceptions gracefully and provide  
meaningful error messages for debugging and user feedback.  

## Working with temporary files

Creating and managing temporary files safely.  

```php
<?php

echo "Temporary file operations:\n";

// Method 1: Using tempnam()
$tempFile1 = tempnam(sys_get_temp_dir(), 'php_test_');
echo "Created temp file: $tempFile1\n";

file_put_contents($tempFile1, "Temporary file content\nLine 2\n");
echo "Written to temp file: " . filesize($tempFile1) . " bytes\n";

// Method 2: Using temporary file handle
$tempHandle = tmpfile();
fwrite($tempHandle, "This is in a temporary file handle\n");
rewind($tempHandle);

$tempContent = fread($tempHandle, 1024);
echo "Temp handle content: " . trim($tempContent) . "\n";

// Method 3: Custom temporary directory
$customTempDir = sys_get_temp_dir() . '/php_custom_temp';
if (!is_dir($customTempDir)) {
    mkdir($customTempDir, 0755, true);
}

$tempFile2 = $customTempDir . '/custom_' . uniqid() . '.tmp';
file_put_contents($tempFile2, "Custom temporary file\n");
echo "Custom temp file: " . basename($tempFile2) . "\n";

// Temporary file with automatic cleanup
function withTempFile($callback) {
    $tempFile = tempnam(sys_get_temp_dir(), 'callback_');
    try {
        return $callback($tempFile);
    } finally {
        if (file_exists($tempFile)) {
            unlink($tempFile);
        }
    }
}

$result = withTempFile(function($file) {
    file_put_contents($file, "Callback temporary file\n");
    return file_get_contents($file);
});

echo "Callback result: " . trim($result) . "\n";

// Get system temporary directory info
echo "\nSystem temporary directory information:\n";
echo "  Path: " . sys_get_temp_dir() . "\n";
echo "  Writable: " . (is_writable(sys_get_temp_dir()) ? "yes" : "no") . "\n";
echo "  Free space: " . formatBytes(disk_free_space(sys_get_temp_dir())) . "\n";

function formatBytes($size, $precision = 2) {
    $units = ['B', 'KB', 'MB', 'GB', 'TB'];
    for ($i = 0; $size > 1024 && $i < count($units) - 1; $i++) {
        $size /= 1024;
    }
    return round($size, $precision) . ' ' . $units[$i];
}

// Clean up
unlink($tempFile1);
fclose($tempHandle);  // This automatically deletes the temp file
unlink($tempFile2);
rmdir($customTempDir);

echo "\nCleaned up temporary files\n";
```

Use `tempnam()` for named temporary files and `tmpfile()` for  
anonymous ones. Temporary files are automatically created in the  
system temp directory and should be cleaned up explicitly.  

## File monitoring and watching

Monitoring files for changes (simplified implementation).  

```php
<?php

$monitorFile = '/tmp/monitored.txt';
$watchDuration = 5; // seconds

// Create initial file
file_put_contents($monitorFile, "Initial content\n");
$initialMtime = filemtime($monitorFile);
$initialSize = filesize($monitorFile);

echo "Starting file monitoring for $watchDuration seconds...\n";
echo "Monitored file: $monitorFile\n";
echo "Initial state: size=$initialSize bytes, modified=" . date('H:i:s', $initialMtime) . "\n";

// Simulate file changes in background (normally this would be external)
$changeScript = '/tmp/file_changer.php';
file_put_contents($changeScript, '<?php
$file = $argv[1] ?? "/tmp/monitored.txt";
sleep(2);
file_put_contents($file, "Updated content at " . date("H:i:s") . "\n", FILE_APPEND);
sleep(2);
file_put_contents($file, "Another update at " . date("H:i:s") . "\n", FILE_APPEND);
');

// Start background process
$process = proc_open("php $changeScript $monitorFile", [], $pipes);

// Monitor loop
$startTime = time();
$lastMtime = $initialMtime;
$lastSize = $initialSize;

while (time() - $startTime < $watchDuration) {
    clearstatcache(); // Clear file stat cache
    
    if (!file_exists($monitorFile)) {
        echo "WARNING: File was deleted!\n";
        break;
    }
    
    $currentMtime = filemtime($monitorFile);
    $currentSize = filesize($monitorFile);
    
    if ($currentMtime !== $lastMtime || $currentSize !== $lastSize) {
        echo "CHANGE DETECTED at " . date('H:i:s') . ":\n";
        echo "  Size: $lastSize -> $currentSize bytes\n";
        echo "  Modified: " . date('H:i:s', $lastMtime) . " -> " . date('H:i:s', $currentMtime) . "\n";
        
        // Show new content
        $content = file_get_contents($monitorFile);
        $lines = explode("\n", trim($content));
        echo "  Last line: " . end($lines) . "\n";
        
        $lastMtime = $currentMtime;
        $lastSize = $currentSize;
    }
    
    sleep(1);
}

// Wait for background process
proc_close($process);

echo "\nFinal file content:\n";
echo file_get_contents($monitorFile);

// Clean up
unlink($monitorFile);
unlink($changeScript);
```

File monitoring requires periodic checking of file metadata.  
Use `clearstatcache()` to ensure fresh file information.  
Production systems should use inotify or similar OS-level monitoring.  

## Advanced file operations

Complex file manipulation techniques.  

```php
<?php

$workDir = '/tmp/advanced_file_ops';
mkdir($workDir, 0755, true);

// File splitting
function splitFile($filename, $chunkSize) {
    $chunks = [];
    $handle = fopen($filename, 'rb');
    $chunkIndex = 0;
    
    while (!feof($handle)) {
        $chunkFile = dirname($filename) . '/' . basename($filename, '.txt') . "_chunk_{$chunkIndex}.txt";
        $chunkHandle = fopen($chunkFile, 'wb');
        
        $written = stream_copy_to_stream($handle, $chunkHandle, $chunkSize);
        fclose($chunkHandle);
        
        if ($written > 0) {
            $chunks[] = $chunkFile;
            $chunkIndex++;
        } else {
            unlink($chunkFile);
            break;
        }
    }
    
    fclose($handle);
    return $chunks;
}

// File merging
function mergeFiles($chunkFiles, $outputFile) {
    $output = fopen($outputFile, 'wb');
    
    foreach ($chunkFiles as $chunkFile) {
        $chunk = fopen($chunkFile, 'rb');
        stream_copy_to_stream($chunk, $output);
        fclose($chunk);
    }
    
    fclose($output);
}

// Create test file
$originalFile = "$workDir/large_file.txt";
$content = '';
for ($i = 1; $i <= 1000; $i++) {
    $content .= "This is line $i with some sample content for testing.\n";
}
file_put_contents($originalFile, $content);

$originalSize = filesize($originalFile);
echo "Created test file: $originalSize bytes\n";

// Split file into chunks
$chunks = splitFile($originalFile, 10000); // 10KB chunks
echo "Split into " . count($chunks) . " chunks:\n";
foreach ($chunks as $chunk) {
    echo "  " . basename($chunk) . ": " . filesize($chunk) . " bytes\n";
}

// Merge chunks back
$mergedFile = "$workDir/merged_file.txt";
mergeFiles($chunks, $mergedFile);

$mergedSize = filesize($mergedFile);
echo "\nMerged file size: $mergedSize bytes\n";

// Verify integrity
if ($originalSize === $mergedSize) {
    $originalHash = md5_file($originalFile);
    $mergedHash = md5_file($mergedFile);
    
    if ($originalHash === $mergedHash) {
        echo "File integrity verified (MD5: $originalHash)\n";
    } else {
        echo "File corruption detected!\n";
    }
} else {
    echo "Size mismatch detected!\n";
}

// File rotation example
function rotateLogFile($logFile, $maxSize = 1000, $maxFiles = 3) {
    if (!file_exists($logFile) || filesize($logFile) < $maxSize) {
        return false;
    }
    
    // Rotate existing backups
    for ($i = $maxFiles - 1; $i > 0; $i--) {
        $oldBackup = "$logFile." . ($i - 1);
        $newBackup = "$logFile.$i";
        
        if (file_exists($oldBackup)) {
            rename($oldBackup, $newBackup);
        }
    }
    
    // Move current log to .0
    rename($logFile, "$logFile.0");
    
    return true;
}

// Test log rotation
$logFile = "$workDir/app.log";
for ($i = 1; $i <= 5; $i++) {
    file_put_contents($logFile, "Log entry $i: " . date('Y-m-d H:i:s') . "\n", FILE_APPEND);
    
    if (rotateLogFile($logFile, 50)) {
        echo "Log rotated at entry $i\n";
    }
}

echo "\nFinal log files:\n";
$logFiles = glob("$workDir/app.log*");
foreach ($logFiles as $log) {
    echo "  " . basename($log) . ": " . filesize($log) . " bytes\n";
}

// Clean up
function removeDirectory($dir) {
    $files = glob("$dir/*");
    foreach ($files as $file) {
        unlink($file);
    }
    rmdir($dir);
}

removeDirectory($workDir);
echo "\nCleaned up working directory\n";
```

Advanced operations like file splitting, merging, and rotation  
are useful for log management and large file processing.  
Always verify data integrity using checksums like MD5.  

## Performance optimization

Optimizing I/O operations for better performance.  

```php
<?php

$testDir = '/tmp/io_performance';
mkdir($testDir, 0755, true);

// Benchmark file operations
function benchmark($description, $callback) {
    $start = microtime(true);
    $result = $callback();
    $end = microtime(true);
    
    $duration = ($end - $start) * 1000; // Convert to milliseconds
    echo sprintf("%-40s: %8.2f ms\n", $description, $duration);
    
    return $result;
}

// Create test data
$smallData = str_repeat("Small data line\n", 100);
$largeData = str_repeat("Large data line with more content here\n", 10000);

echo "I/O Performance Benchmarks:\n";
echo str_repeat("-", 60) . "\n";

// File writing benchmarks
benchmark("Small file - file_put_contents", function() use ($testDir, $smallData) {
    return file_put_contents("$testDir/small_fpc.txt", $smallData);
});

benchmark("Small file - fwrite", function() use ($testDir, $smallData) {
    $handle = fopen("$testDir/small_fwrite.txt", 'w');
    $result = fwrite($handle, $smallData);
    fclose($handle);
    return $result;
});

benchmark("Large file - file_put_contents", function() use ($testDir, $largeData) {
    return file_put_contents("$testDir/large_fpc.txt", $largeData);
});

benchmark("Large file - fwrite", function() use ($testDir, $largeData) {
    $handle = fopen("$testDir/large_fwrite.txt", 'w');
    $result = fwrite($handle, $largeData);
    fclose($handle);
    return $result;
});

// Buffered vs unbuffered writing
benchmark("Large file - buffered chunks", function() use ($testDir, $largeData) {
    $handle = fopen("$testDir/large_buffered.txt", 'w');
    $chunks = str_split($largeData, 8192); // 8KB chunks
    foreach ($chunks as $chunk) {
        fwrite($handle, $chunk);
    }
    fclose($handle);
    return strlen($largeData);
});

// Reading benchmarks
benchmark("Large file - file_get_contents", function() use ($testDir) {
    return strlen(file_get_contents("$testDir/large_fpc.txt"));
});

benchmark("Large file - fread chunks", function() use ($testDir) {
    $handle = fopen("$testDir/large_fpc.txt", 'r');
    $totalSize = 0;
    while (!feof($handle)) {
        $chunk = fread($handle, 8192);
        $totalSize += strlen($chunk);
    }
    fclose($handle);
    return $totalSize;
});

benchmark("Large file - line by line", function() use ($testDir) {
    $handle = fopen("$testDir/large_fpc.txt", 'r');
    $lineCount = 0;
    while (($line = fgets($handle)) !== false) {
        $lineCount++;
    }
    fclose($handle);
    return $lineCount;
});

// Memory usage comparison
echo "\nMemory Usage Analysis:\n";
echo str_repeat("-", 60) . "\n";

function getMemoryUsage() {
    return [
        'current' => memory_get_usage(true),
        'peak' => memory_get_peak_usage(true)
    ];
}

function formatMemory($bytes) {
    return round($bytes / 1024 / 1024, 2) . ' MB';
}

// Memory efficient file processing
$memBefore = getMemoryUsage();

// Process file line by line (memory efficient)
$lineCount = 0;
$handle = fopen("$testDir/large_fpc.txt", 'r');
while (($line = fgets($handle)) !== false) {
    $lineCount++; // Process line without storing
}
fclose($handle);

$memAfter = getMemoryUsage();

echo "Line-by-line processing ($lineCount lines):\n";
echo "  Memory before: " . formatMemory($memBefore['current']) . "\n";
echo "  Memory after:  " . formatMemory($memAfter['current']) . "\n";
echo "  Peak memory:   " . formatMemory($memAfter['peak']) . "\n";

// Memory intensive processing
$memBefore = getMemoryUsage();

// Load entire file into memory
$allContent = file_get_contents("$testDir/large_fpc.txt");
$lines = explode("\n", $allContent);
unset($allContent); // Free memory

$memAfter = getMemoryUsage();

echo "\nLoad-all processing (" . count($lines) . " lines):\n";
echo "  Memory before: " . formatMemory($memBefore['current']) . "\n";
echo "  Memory after:  " . formatMemory($memAfter['current']) . "\n";
echo "  Peak memory:   " . formatMemory($memAfter['peak']) . "\n";

// File system performance tips
echo "\nPerformance Tips:\n";
echo "  1. Use fread() with 8KB+ chunks for large files\n";
echo "  2. Process files line-by-line for memory efficiency\n";
echo "  3. Use file_get_contents() for small files only\n";
echo "  4. Consider stream_copy_to_stream() for file copying\n";
echo "  5. Use appropriate buffer sizes for your use case\n";

// Clean up
array_map('unlink', glob("$testDir/*"));
rmdir($testDir);
```

Performance optimization focuses on choosing appropriate methods  
for file size and available memory. Line-by-line processing is  
memory-efficient for large files, while `file_get_contents()` is fast for small ones.  

## File uploading simulation

Handling file upload operations safely.  

```php
<?php

$uploadDir = '/tmp/uploads';
$maxFileSize = 1024 * 1024; // 1MB

// Create upload directory
if (!is_dir($uploadDir)) {
    mkdir($uploadDir, 0755, true);
}

function simulateUpload($filename, $content, $uploadDir, $maxFileSize) {
    $errors = [];
    
    // Validate file size
    if (strlen($content) > $maxFileSize) {
        $errors[] = "File too large: " . strlen($content) . " bytes (max: $maxFileSize)";
    }
    
    // Validate file extension
    $allowedExtensions = ['txt', 'jpg', 'png', 'pdf', 'doc'];
    $extension = strtolower(pathinfo($filename, PATHINFO_EXTENSION));
    
    if (!in_array($extension, $allowedExtensions)) {
        $errors[] = "Invalid file extension: $extension";
    }
    
    // Sanitize filename
    $safeFilename = preg_replace('/[^a-zA-Z0-9._-]/', '', $filename);
    
    if (empty($errors)) {
        $targetPath = $uploadDir . '/' . $safeFilename;
        
        // Avoid overwrites
        $counter = 1;
        while (file_exists($targetPath)) {
            $name = pathinfo($safeFilename, PATHINFO_FILENAME);
            $ext = pathinfo($safeFilename, PATHINFO_EXTENSION);
            $targetPath = $uploadDir . '/' . $name . "_$counter." . $ext;
            $counter++;
        }
        
        file_put_contents($targetPath, $content);
        
        return [
            'success' => true,
            'path' => $targetPath,
            'size' => strlen($content)
        ];
    }
    
    return [
        'success' => false,
        'errors' => $errors
    ];
}

// Simulate file uploads
$uploads = [
    ['file.txt', 'Sample text content'],
    ['document.pdf', str_repeat('PDF content', 1000)],
    ['script.php', '<?php echo "Not allowed"; ?>'],  // Should fail
    ['image.jpg', str_repeat('X', 2 * 1024 * 1024)]   // Too large
];

echo "Simulating file uploads:\n";
foreach ($uploads as [$filename, $content]) {
    $result = simulateUpload($filename, $content, $uploadDir, $maxFileSize);
    
    if ($result['success']) {
        echo "✓ $filename uploaded successfully ({$result['size']} bytes)\n";
        echo "  Saved as: " . basename($result['path']) . "\n";
    } else {
        echo "✗ $filename upload failed:\n";
        foreach ($result['errors'] as $error) {
            echo "  - $error\n";
        }
    }
}

// Clean up
array_map('unlink', glob("$uploadDir/*"));
rmdir($uploadDir);
```

File upload handling requires validation of size, type, and filename  
sanitization. Always store uploads outside the web root and validate  
file contents, not just extensions, for security.  

## XML file processing

Reading and writing XML files with proper parsing.  

```php
<?php

$xmlFile = '/tmp/books.xml';

// Create XML content
$xmlContent = '<?xml version="1.0" encoding="UTF-8"?>
<library>
    <book id="1">
        <title>PHP Essentials</title>
        <author>Jane Developer</author>
        <year>2024</year>
        <price currency="USD">29.99</price>
    </book>
    <book id="2">
        <title>Advanced Programming</title>
        <author>John Coder</author>
        <year>2023</year>
        <price currency="EUR">34.50</price>
    </book>
</library>';

file_put_contents($xmlFile, $xmlContent);

// Parse XML using SimpleXML
$xml = simplexml_load_file($xmlFile);
if ($xml === false) {
    die("Failed to parse XML file\n");
}

echo "Library contains " . count($xml->book) . " books:\n";

foreach ($xml->book as $book) {
    $id = (string)$book['id'];
    $title = (string)$book->title;
    $author = (string)$book->author;
    $year = (int)$book->year;
    $price = (float)$book->price;
    $currency = (string)$book->price['currency'];
    
    echo "Book #$id: '$title' by $author ($year) - $price $currency\n";
}

// Modify XML
$newBook = $xml->addChild('book');
$newBook->addAttribute('id', '3');
$newBook->addChild('title', 'Web Development Guide');
$newBook->addChild('author', 'Sarah Wilson');
$newBook->addChild('year', '2024');
$priceNode = $newBook->addChild('price', '24.99');
$priceNode->addAttribute('currency', 'USD');

// Save modified XML
$dom = new DOMDocument('1.0', 'UTF-8');
$dom->formatOutput = true;
$dom->loadXML($xml->asXML());
$dom->save($xmlFile);

echo "\nAdded new book, updated XML file\n";

// Re-read to verify
$updatedXml = simplexml_load_file($xmlFile);
echo "Updated library now contains " . count($updatedXml->book) . " books\n";

unlink($xmlFile);
```

Use `simplexml_load_file()` for basic XML parsing and `DOMDocument`  
for advanced manipulation. Always validate XML structure and handle  
parsing errors appropriately for robust applications.  

## Database file operations

Working with SQLite database files.  

```php
<?php

$dbFile = '/tmp/example.db';

try {
    // Create SQLite database
    $pdo = new PDO("sqlite:$dbFile");
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    
    // Create table
    $createTable = "
        CREATE TABLE users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            email TEXT UNIQUE NOT NULL,
            created_at DATETIME DEFAULT CURRENT_TIMESTAMP
        )
    ";
    $pdo->exec($createTable);
    
    // Insert sample data
    $insertQuery = "INSERT INTO users (name, email) VALUES (?, ?)";
    $stmt = $pdo->prepare($insertQuery);
    
    $users = [
        ['Alice Johnson', 'alice@example.com'],
        ['Bob Smith', 'bob@example.com'],
        ['Charlie Brown', 'charlie@example.com']
    ];
    
    foreach ($users as $user) {
        $stmt->execute($user);
    }
    
    echo "Created database with " . count($users) . " users\n";
    echo "Database file size: " . filesize($dbFile) . " bytes\n";
    
    // Query data
    $selectQuery = "SELECT * FROM users ORDER BY created_at";
    $stmt = $pdo->query($selectQuery);
    
    echo "\nUsers in database:\n";
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        echo "ID: {$row['id']}, Name: {$row['name']}, ";
        echo "Email: {$row['email']}, Created: {$row['created_at']}\n";
    }
    
    // Database file operations
    $backupFile = '/tmp/example_backup.db';
    copy($dbFile, $backupFile);
    echo "\nDatabase backed up to: $backupFile\n";
    
    // Verify backup
    $backupPdo = new PDO("sqlite:$backupFile");
    $count = $backupPdo->query("SELECT COUNT(*) FROM users")->fetchColumn();
    echo "Backup contains $count users\n";
    
    // Clean up
    unlink($dbFile);
    unlink($backupFile);
    
} catch (PDOException $e) {
    echo "Database error: " . $e->getMessage() . "\n";
}
```

SQLite databases are single files, making them easy to backup,  
move, and manage. Use proper error handling and transactions  
for data integrity in production applications.  

## Configuration file management

Managing application configuration files in different formats.  

```php
<?php

$configDir = '/tmp/config';
mkdir($configDir, 0755, true);

// INI configuration
$iniFile = "$configDir/app.ini";
$iniConfig = "
[database]
host = localhost
port = 3306
name = myapp
user = dbuser

[cache]
enabled = true
ttl = 3600
driver = redis

[features]
debug = false
logging = true
";

file_put_contents($iniFile, trim($iniConfig));

// Read INI file
$config = parse_ini_file($iniFile, true);
echo "INI Configuration:\n";
foreach ($config as $section => $settings) {
    echo "[$section]\n";
    foreach ($settings as $key => $value) {
        echo "  $key = $value\n";
    }
}

// YAML-style configuration (simplified)
$yamlFile = "$configDir/app.yaml";
$yamlConfig = "
database:
  host: localhost
  port: 3306
  name: myapp
  credentials:
    user: dbuser
    password: secret

cache:
  enabled: true
  ttl: 3600
  driver: redis

features:
  - debug
  - logging
  - caching
";

file_put_contents($yamlFile, trim($yamlConfig));

// Simple YAML parser (for demonstration)
function parseSimpleYaml($content) {
    $lines = explode("\n", $content);
    $result = [];
    $currentSection = null;
    
    foreach ($lines as $line) {
        $line = trim($line);
        if (empty($line) || $line[0] === '#') continue;
        
        if (preg_match('/^(\w+):$/', $line, $matches)) {
            $currentSection = $matches[1];
            $result[$currentSection] = [];
        } elseif (preg_match('/^  (\w+): (.+)$/', $line, $matches)) {
            $result[$currentSection][$matches[1]] = $matches[2];
        } elseif (preg_match('/^  - (\w+)$/', $line, $matches)) {
            $result[$currentSection][] = $matches[1];
        }
    }
    
    return $result;
}

$yamlData = parseSimpleYaml(file_get_contents($yamlFile));
echo "\nYAML Configuration:\n";
print_r($yamlData);

// Environment file
$envFile = "$configDir/.env";
$envConfig = "
APP_NAME=My Application
APP_ENV=production
APP_DEBUG=false

DB_HOST=localhost
DB_PORT=3306
DB_NAME=myapp

CACHE_ENABLED=true
CACHE_TTL=3600
";

file_put_contents($envFile, trim($envConfig));

// Parse environment file
function parseEnvFile($filename) {
    $vars = [];
    $lines = file($filename, FILE_IGNORE_NEW_LINES | FILE_SKIP_EMPTY_LINES);
    
    foreach ($lines as $line) {
        if (strpos($line, '=') !== false && $line[0] !== '#') {
            [$key, $value] = explode('=', $line, 2);
            $vars[trim($key)] = trim($value);
        }
    }
    
    return $vars;
}

$envVars = parseEnvFile($envFile);
echo "\nEnvironment Configuration:\n";
foreach ($envVars as $key => $value) {
    echo "$key=$value\n";
}

// Clean up
unlink($iniFile);
unlink($yamlFile);
unlink($envFile);
rmdir($configDir);
```

Support multiple configuration formats (INI, JSON, YAML, ENV) to  
accommodate different deployment scenarios. Always validate  
configuration values and provide sensible defaults.  

## Image file operations

Basic image file handling and metadata extraction.  

```php
<?php

// Create a simple test image (1x1 pixel PNG)
$pngData = base64_decode('iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mP8/5+hHgAHggJ/PchI7wAAAABJRU5ErkJggg==');
$imageFile = '/tmp/test.png';
file_put_contents($imageFile, $pngData);

echo "Image file operations:\n";

// Basic image information
$imageSize = getimagesize($imageFile);
if ($imageSize) {
    echo "Image dimensions: {$imageSize[0]}x{$imageSize[1]} pixels\n";
    echo "Image type: " . image_type_to_mime_type($imageSize[2]) . "\n";
    echo "File size: " . filesize($imageFile) . " bytes\n";
}

// Image file validation
function validateImageFile($filename) {
    $validTypes = [IMAGETYPE_JPEG, IMAGETYPE_PNG, IMAGETYPE_GIF];
    $imageInfo = getimagesize($filename);
    
    if (!$imageInfo) {
        return ['valid' => false, 'error' => 'Not a valid image file'];
    }
    
    if (!in_array($imageInfo[2], $validTypes)) {
        return ['valid' => false, 'error' => 'Unsupported image type'];
    }
    
    return [
        'valid' => true,
        'width' => $imageInfo[0],
        'height' => $imageInfo[1],
        'type' => image_type_to_mime_type($imageInfo[2])
    ];
}

$validation = validateImageFile($imageFile);
if ($validation['valid']) {
    echo "Image validation: PASSED\n";
    echo "Type: {$validation['type']}\n";
} else {
    echo "Image validation: FAILED - {$validation['error']}\n";
}

// Create thumbnail-sized image data
function createThumbnail($sourceFile, $maxWidth, $maxHeight) {
    $imageInfo = getimagesize($sourceFile);
    if (!$imageInfo) return false;
    
    $originalWidth = $imageInfo[0];
    $originalHeight = $imageInfo[1];
    
    // Calculate thumbnail dimensions
    $ratio = min($maxWidth / $originalWidth, $maxHeight / $originalHeight);
    $thumbWidth = (int)($originalWidth * $ratio);
    $thumbHeight = (int)($originalHeight * $ratio);
    
    return [
        'original' => ['width' => $originalWidth, 'height' => $originalHeight],
        'thumbnail' => ['width' => $thumbWidth, 'height' => $thumbHeight],
        'ratio' => $ratio
    ];
}

$thumbnailInfo = createThumbnail($imageFile, 150, 150);
if ($thumbnailInfo) {
    echo "\nThumbnail calculation:\n";
    echo "Original: {$thumbnailInfo['original']['width']}x{$thumbnailInfo['original']['height']}\n";
    echo "Thumbnail: {$thumbnailInfo['thumbnail']['width']}x{$thumbnailInfo['thumbnail']['height']}\n";
    echo "Scale ratio: " . round($thumbnailInfo['ratio'], 3) . "\n";
}

// Image file operations
$copyFile = '/tmp/test_copy.png';
copy($imageFile, $copyFile);

echo "\nImage file copied to: " . basename($copyFile) . "\n";
echo "Copy size: " . filesize($copyFile) . " bytes\n";

// Verify image integrity
$originalHash = md5_file($imageFile);
$copyHash = md5_file($copyFile);
echo "Integrity check: " . ($originalHash === $copyHash ? "PASSED" : "FAILED") . "\n";

// Clean up
unlink($imageFile);
unlink($copyFile);
```

Image file operations require format validation and dimension checking.  
Use `getimagesize()` for metadata extraction and always validate  
image files before processing to prevent security issues.  

## Log file processing

Advanced log file parsing and analysis.  

```php
<?php

$logFile = '/tmp/application.log';

// Create sample log entries
$logEntries = [
    '2024-01-15 10:30:15 [INFO] Application started',
    '2024-01-15 10:30:16 [INFO] Database connected',
    '2024-01-15 10:31:22 [WARNING] Slow query detected: SELECT * FROM users',
    '2024-01-15 10:32:01 [ERROR] Failed to connect to cache server',
    '2024-01-15 10:32:05 [INFO] Cache server reconnected',
    '2024-01-15 10:35:42 [INFO] User login: admin@example.com',
    '2024-01-15 10:36:13 [WARNING] Multiple login attempts from IP 192.168.1.100',
    '2024-01-15 10:37:28 [ERROR] Database connection lost',
    '2024-01-15 10:37:30 [INFO] Database reconnected',
    '2024-01-15 10:40:15 [INFO] Application shutdown'
];

file_put_contents($logFile, implode("\n", $logEntries) . "\n");

// Log parser function
function parseLogFile($filename) {
    $stats = [
        'total_lines' => 0,
        'levels' => ['INFO' => 0, 'WARNING' => 0, 'ERROR' => 0],
        'time_range' => ['start' => null, 'end' => null],
        'entries' => []
    ];
    
    $handle = fopen($filename, 'r');
    
    while (($line = fgets($handle)) !== false) {
        $line = trim($line);
        if (empty($line)) continue;
        
        $stats['total_lines']++;
        
        // Parse log line format: YYYY-MM-DD HH:MM:SS [LEVEL] MESSAGE
        if (preg_match('/^(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) \[(\w+)\] (.+)$/', $line, $matches)) {
            $timestamp = $matches[1];
            $level = $matches[2];
            $message = $matches[3];
            
            // Update statistics
            if (isset($stats['levels'][$level])) {
                $stats['levels'][$level]++;
            }
            
            // Track time range
            if ($stats['time_range']['start'] === null || $timestamp < $stats['time_range']['start']) {
                $stats['time_range']['start'] = $timestamp;
            }
            if ($stats['time_range']['end'] === null || $timestamp > $stats['time_range']['end']) {
                $stats['time_range']['end'] = $timestamp;
            }
            
            $stats['entries'][] = [
                'timestamp' => $timestamp,
                'level' => $level,
                'message' => $message
            ];
        }
    }
    
    fclose($handle);
    return $stats;
}

// Filter logs by level
function filterLogsByLevel($entries, $level) {
    return array_filter($entries, fn($entry) => $entry['level'] === $level);
}

// Parse log file
$logStats = parseLogFile($logFile);

echo "Log file analysis:\n";
echo "Total lines: {$logStats['total_lines']}\n";
echo "Time range: {$logStats['time_range']['start']} to {$logStats['time_range']['end']}\n";

echo "\nLevel distribution:\n";
foreach ($logStats['levels'] as $level => $count) {
    $percentage = round(($count / $logStats['total_lines']) * 100, 1);
    echo "  $level: $count entries ({$percentage}%)\n";
}

// Show error entries
$errors = filterLogsByLevel($logStats['entries'], 'ERROR');
if (!empty($errors)) {
    echo "\nError entries:\n";
    foreach ($errors as $error) {
        echo "  {$error['timestamp']}: {$error['message']}\n";
    }
}

// Log rotation simulation
function rotateLog($logFile, $maxLines = 5) {
    $lines = file($logFile, FILE_IGNORE_NEW_LINES);
    
    if (count($lines) > $maxLines) {
        $rotatedFile = $logFile . '.1';
        
        // Move old log
        if (file_exists($rotatedFile)) {
            unlink($rotatedFile);
        }
        rename($logFile, $rotatedFile);
        
        // Keep recent entries in main log
        $recentLines = array_slice($lines, -$maxLines);
        file_put_contents($logFile, implode("\n", $recentLines) . "\n");
        
        return true;
    }
    
    return false;
}

// Add more entries to trigger rotation
for ($i = 1; $i <= 10; $i++) {
    $newEntry = date('Y-m-d H:i:s') . " [INFO] Additional entry $i\n";
    file_put_contents($logFile, $newEntry, FILE_APPEND);
}

if (rotateLog($logFile, 8)) {
    echo "\nLog rotated due to size\n";
    echo "Current log lines: " . count(file($logFile, FILE_IGNORE_NEW_LINES)) . "\n";
}

// Clean up
unlink($logFile);
if (file_exists($logFile . '.1')) {
    unlink($logFile . '.1');
}
```

Log file processing involves parsing structured text, extracting  
statistics, and managing file rotation. Use regular expressions  
for pattern matching and implement rotation for size management.  

## Batch file processing

Processing multiple files in batches efficiently.  

```php
<?php

$batchDir = '/tmp/batch_processing';
$processedDir = '/tmp/processed';
$errorDir = '/tmp/errors';

// Create directories
foreach ([$batchDir, $processedDir, $errorDir] as $dir) {
    if (!is_dir($dir)) {
        mkdir($dir, 0755, true);
    }
}

// Create sample files for batch processing
$sampleFiles = [];
for ($i = 1; $i <= 20; $i++) {
    $filename = "file_$i.txt";
    $content = "Content for file $i\nLine 2 of file $i\n";
    
    // Introduce some "corrupt" files
    if ($i % 7 === 0) {
        $content = ""; // Empty file to simulate error
    }
    
    $filepath = "$batchDir/$filename";
    file_put_contents($filepath, $content);
    $sampleFiles[] = $filepath;
}

echo "Created " . count($sampleFiles) . " files for batch processing\n";

// Batch processor class
class BatchFileProcessor {
    private $sourceDir;
    private $successDir;
    private $errorDir;
    private $batchSize;
    
    public function __construct($sourceDir, $successDir, $errorDir, $batchSize = 5) {
        $this->sourceDir = $sourceDir;
        $this->successDir = $successDir;
        $this->errorDir = $errorDir;
        $this->batchSize = $batchSize;
    }
    
    public function process() {
        $files = glob($this->sourceDir . '/*.txt');
        $batches = array_chunk($files, $this->batchSize);
        
        $stats = [
            'total_files' => count($files),
            'batches' => count($batches),
            'processed' => 0,
            'errors' => 0
        ];
        
        echo "Processing {$stats['total_files']} files in {$stats['batches']} batches\n";
        
        foreach ($batches as $batchNum => $batch) {
            echo "Processing batch " . ($batchNum + 1) . " (" . count($batch) . " files)...\n";
            
            foreach ($batch as $file) {
                if ($this->processFile($file)) {
                    $stats['processed']++;
                } else {
                    $stats['errors']++;
                }
            }
        }
        
        return $stats;
    }
    
    private function processFile($filePath) {
        $filename = basename($filePath);
        
        try {
            // Simulate file processing
            $content = file_get_contents($filePath);
            
            // Validation
            if (empty(trim($content))) {
                throw new RuntimeException("Empty or invalid file");
            }
            
            // Process content (example: add timestamp)
            $processedContent = "Processed on: " . date('Y-m-d H:i:s') . "\n" . $content;
            $processedContent .= "Processing completed\n";
            
            // Move to success directory
            $targetPath = $this->successDir . '/' . $filename;
            file_put_contents($targetPath, $processedContent);
            unlink($filePath);
            
            echo "  ✓ $filename processed successfully\n";
            return true;
            
        } catch (Exception $e) {
            // Move to error directory
            $errorPath = $this->errorDir . '/' . $filename;
            if (file_exists($filePath)) {
                rename($filePath, $errorPath);
            }
            
            // Log error
            $errorLog = $this->errorDir . '/errors.log';
            $logEntry = date('Y-m-d H:i:s') . " ERROR processing $filename: " . $e->getMessage() . "\n";
            file_put_contents($errorLog, $logEntry, FILE_APPEND);
            
            echo "  ✗ $filename failed: " . $e->getMessage() . "\n";
            return false;
        }
    }
}

// Process files in batches
$processor = new BatchFileProcessor($batchDir, $processedDir, $errorDir, 3);
$results = $processor->process();

echo "\nBatch processing completed:\n";
echo "  Total files: {$results['total_files']}\n";
echo "  Successfully processed: {$results['processed']}\n";
echo "  Errors: {$results['errors']}\n";

// Verify results
$processedFiles = glob("$processedDir/*.txt");
$errorFiles = glob("$errorDir/*.txt");

echo "\nVerification:\n";
echo "  Processed directory: " . count($processedFiles) . " files\n";
echo "  Error directory: " . count($errorFiles) . " files\n";

if (file_exists("$errorDir/errors.log")) {
    echo "  Error log exists with " . count(file("$errorDir/errors.log")) . " entries\n";
}

// Clean up
function cleanDirectory($dir) {
    $files = glob("$dir/*");
    foreach ($files as $file) {
        unlink($file);
    }
    rmdir($dir);
}

cleanDirectory($processedDir);
cleanDirectory($errorDir);
cleanDirectory($batchDir);

echo "\nCleaned up all directories\n";
```

Batch processing enables efficient handling of large file sets by  
processing them in manageable chunks. Implement proper error handling  
and organize results into success and error directories.  

## File backup and restoration

Creating and restoring file backups with compression.  

```php
<?php

$sourceDir = '/tmp/backup_source';
$backupDir = '/tmp/backups';
$restoreDir = '/tmp/restored';

// Create directories
foreach ([$sourceDir, $backupDir, $restoreDir] as $dir) {
    if (!is_dir($dir)) {
        mkdir($dir, 0755, true);
    }
}

// Create source files
$sourceFiles = [
    'config.json' => json_encode(['app' => 'test', 'version' => '1.0']),
    'data.csv' => "Name,Age\nAlice,25\nBob,30",
    'readme.txt' => "This is a readme file\nWith multiple lines",
    'script.php' => '<?php echo "Hello there!"; ?>'
];

foreach ($sourceFiles as $filename => $content) {
    file_put_contents("$sourceDir/$filename", $content);
}

echo "Created " . count($sourceFiles) . " source files\n";

// Backup system class
class FileBackup {
    private $sourceDir;
    private $backupDir;
    
    public function __construct($sourceDir, $backupDir) {
        $this->sourceDir = $sourceDir;
        $this->backupDir = $backupDir;
    }
    
    public function createBackup($compressionLevel = 9) {
        $timestamp = date('Y-m-d_H-i-s');
        $backupFile = $this->backupDir . "/backup_$timestamp.gz";
        
        // Create archive
        $archiveData = $this->createArchive();
        
        // Compress
        $compressed = gzencode($archiveData, $compressionLevel);
        file_put_contents($backupFile, $compressed);
        
        $originalSize = strlen($archiveData);
        $compressedSize = filesize($backupFile);
        $ratio = round((1 - $compressedSize / $originalSize) * 100, 2);
        
        return [
            'backup_file' => $backupFile,
            'original_size' => $originalSize,
            'compressed_size' => $compressedSize,
            'compression_ratio' => $ratio
        ];
    }
    
    private function createArchive() {
        $files = glob($this->sourceDir . '/*');
        $archive = '';
        
        foreach ($files as $file) {
            if (is_file($file)) {
                $relativePath = basename($file);
                $content = file_get_contents($file);
                $size = strlen($content);
                
                // Simple archive format: filename|size|content
                $archive .= $relativePath . '|' . $size . '|' . $content . "\n---FILE_SEPARATOR---\n";
            }
        }
        
        return $archive;
    }
    
    public function restore($backupFile, $restoreDir) {
        if (!file_exists($backupFile)) {
            throw new RuntimeException("Backup file not found: $backupFile");
        }
        
        // Decompress
        $compressed = file_get_contents($backupFile);
        $archiveData = gzdecode($compressed);
        
        if ($archiveData === false) {
            throw new RuntimeException("Failed to decompress backup file");
        }
        
        // Extract files
        $fileEntries = explode("\n---FILE_SEPARATOR---\n", $archiveData);
        $restoredCount = 0;
        
        foreach ($fileEntries as $entry) {
            $entry = trim($entry);
            if (empty($entry)) continue;
            
            $parts = explode('|', $entry, 3);
            if (count($parts) === 3) {
                $filename = $parts[0];
                $size = (int)$parts[1];
                $content = $parts[2];
                
                // Verify size
                if (strlen($content) === $size) {
                    $restorePath = $restoreDir . '/' . $filename;
                    file_put_contents($restorePath, $content);
                    $restoredCount++;
                }
            }
        }
        
        return $restoredCount;
    }
    
    public function listBackups() {
        $backups = glob($this->backupDir . '/backup_*.gz');
        $backupInfo = [];
        
        foreach ($backups as $backup) {
            $backupInfo[] = [
                'file' => basename($backup),
                'path' => $backup,
                'size' => filesize($backup),
                'created' => date('Y-m-d H:i:s', filemtime($backup))
            ];
        }
        
        return $backupInfo;
    }
}

// Create backup
$backup = new FileBackup($sourceDir, $backupDir);
$backupResult = $backup->createBackup();

echo "\nBackup created:\n";
echo "  File: " . basename($backupResult['backup_file']) . "\n";
echo "  Original size: {$backupResult['original_size']} bytes\n";
echo "  Compressed size: {$backupResult['compressed_size']} bytes\n";
echo "  Compression ratio: {$backupResult['compression_ratio']}%\n";

// List all backups
$allBackups = $backup->listBackups();
echo "\nAvailable backups:\n";
foreach ($allBackups as $info) {
    echo "  {$info['file']}: {$info['size']} bytes, created {$info['created']}\n";
}

// Restore from backup
$restoredCount = $backup->restore($backupResult['backup_file'], $restoreDir);
echo "\nRestored $restoredCount files to: $restoreDir\n";

// Verify restoration
$restoredFiles = glob("$restoreDir/*");
echo "Verification:\n";

foreach ($restoredFiles as $restoredFile) {
    $filename = basename($restoredFile);
    $originalFile = "$sourceDir/$filename";
    
    if (file_exists($originalFile)) {
        $originalHash = md5_file($originalFile);
        $restoredHash = md5_file($restoredFile);
        $status = ($originalHash === $restoredHash) ? "✓ OK" : "✗ CORRUPTED";
        echo "  $filename: $status\n";
    }
}

// Clean up
function removeDir($dir) {
    array_map('unlink', glob("$dir/*"));
    rmdir($dir);
}

removeDir($sourceDir);
removeDir($backupDir);
removeDir($restoreDir);

echo "\nBackup and restore test completed successfully\n";
```

Backup systems should include compression, integrity checking, and  
metadata storage. Always verify restored files match originals  
using checksums and test restoration procedures regularly.  

## File synchronization

Synchronizing files between directories with change detection.  

```php
<?php

$sourceDir = '/tmp/sync_source';
$targetDir = '/tmp/sync_target';

// Create directories
foreach ([$sourceDir, $targetDir] as $dir) {
    if (!is_dir($dir)) {
        mkdir($dir, 0755, true);
    }
}

// File synchronization class
class FileSync {
    private $sourceDir;
    private $targetDir;
    
    public function __construct($sourceDir, $targetDir) {
        $this->sourceDir = $sourceDir;
        $this->targetDir = $targetDir;
    }
    
    public function sync($dryRun = false) {
        $operations = $this->analyzeChanges();
        
        if ($dryRun) {
            return $this->reportOperations($operations);
        }
        
        return $this->executeOperations($operations);
    }
    
    private function analyzeChanges() {
        $sourceFiles = $this->scanDirectory($this->sourceDir);
        $targetFiles = $this->scanDirectory($this->targetDir);
        
        $operations = [
            'copy' => [],
            'update' => [],
            'delete' => []
        ];
        
        // Find files to copy or update
        foreach ($sourceFiles as $relativePath => $sourceInfo) {
            $targetPath = $this->targetDir . '/' . $relativePath;
            
            if (!isset($targetFiles[$relativePath])) {
                // New file
                $operations['copy'][] = [
                    'source' => $sourceInfo['path'],
                    'target' => $targetPath,
                    'file' => $relativePath
                ];
            } elseif ($sourceInfo['modified'] > $targetFiles[$relativePath]['modified']) {
                // Updated file
                $operations['update'][] = [
                    'source' => $sourceInfo['path'],
                    'target' => $targetPath,
                    'file' => $relativePath
                ];
            }
        }
        
        // Find files to delete
        foreach ($targetFiles as $relativePath => $targetInfo) {
            if (!isset($sourceFiles[$relativePath])) {
                $operations['delete'][] = [
                    'target' => $targetInfo['path'],
                    'file' => $relativePath
                ];
            }
        }
        
        return $operations;
    }
    
    private function scanDirectory($dir) {
        $files = [];
        $iterator = new RecursiveIteratorIterator(
            new RecursiveDirectoryIterator($dir, RecursiveDirectoryIterator::SKIP_DOTS)
        );
        
        foreach ($iterator as $file) {
            if ($file->isFile()) {
                $relativePath = str_replace($dir . '/', '', $file->getPathname());
                $files[$relativePath] = [
                    'path' => $file->getPathname(),
                    'size' => $file->getSize(),
                    'modified' => $file->getMTime()
                ];
            }
        }
        
        return $files;
    }
    
    private function executeOperations($operations) {
        $results = [
            'copied' => 0,
            'updated' => 0,
            'deleted' => 0,
            'errors' => []
        ];
        
        // Copy new files
        foreach ($operations['copy'] as $op) {
            try {
                $this->ensureDirectory(dirname($op['target']));
                copy($op['source'], $op['target']);
                $results['copied']++;
            } catch (Exception $e) {
                $results['errors'][] = "Copy failed for {$op['file']}: " . $e->getMessage();
            }
        }
        
        // Update existing files
        foreach ($operations['update'] as $op) {
            try {
                copy($op['source'], $op['target']);
                $results['updated']++;
            } catch (Exception $e) {
                $results['errors'][] = "Update failed for {$op['file']}: " . $e->getMessage();
            }
        }
        
        // Delete removed files
        foreach ($operations['delete'] as $op) {
            try {
                unlink($op['target']);
                $results['deleted']++;
            } catch (Exception $e) {
                $results['errors'][] = "Delete failed for {$op['file']}: " . $e->getMessage();
            }
        }
        
        return $results;
    }
    
    private function reportOperations($operations) {
        $report = [
            'copy_count' => count($operations['copy']),
            'update_count' => count($operations['update']),
            'delete_count' => count($operations['delete']),
            'operations' => $operations
        ];
        
        return $report;
    }
    
    private function ensureDirectory($dir) {
        if (!is_dir($dir)) {
            mkdir($dir, 0755, true);
        }
    }
}

// Create initial source files
$initialFiles = [
    'file1.txt' => 'Content of file 1',
    'file2.txt' => 'Content of file 2',
    'subdir/file3.txt' => 'Content of file 3 in subdirectory'
];

foreach ($initialFiles as $path => $content) {
    $fullPath = "$sourceDir/$path";
    $dir = dirname($fullPath);
    if (!is_dir($dir)) {
        mkdir($dir, 0755, true);
    }
    file_put_contents($fullPath, $content);
}

// Create some target files (simulate existing sync state)
file_put_contents("$targetDir/file1.txt", 'Old content of file 1');
file_put_contents("$targetDir/old_file.txt", 'This file should be deleted');

echo "Initial sync state created\n";

// Perform dry run
$sync = new FileSync($sourceDir, $targetDir);
$dryRunReport = $sync->sync(true);

echo "\nDry run analysis:\n";
echo "  Files to copy: {$dryRunReport['copy_count']}\n";
echo "  Files to update: {$dryRunReport['update_count']}\n";
echo "  Files to delete: {$dryRunReport['delete_count']}\n";

// Perform actual sync
$syncResults = $sync->sync(false);

echo "\nSync completed:\n";
echo "  Copied: {$syncResults['copied']} files\n";
echo "  Updated: {$syncResults['updated']} files\n";
echo "  Deleted: {$syncResults['deleted']} files\n";

if (!empty($syncResults['errors'])) {
    echo "  Errors:\n";
    foreach ($syncResults['errors'] as $error) {
        echo "    - $error\n";
    }
}

// Verify synchronization
echo "\nVerification:\n";
$sourceFiles = glob("$sourceDir/*");
$targetFiles = glob("$targetDir/*");

foreach ($sourceFiles as $sourceFile) {
    $filename = basename($sourceFile);
    $targetFile = "$targetDir/$filename";
    
    if (file_exists($targetFile) && is_file($sourceFile)) {
        $match = (md5_file($sourceFile) === md5_file($targetFile)) ? "✓" : "✗";
        echo "  $filename: $match\n";
    }
}

// Clean up
function cleanDir($dir) {
    $iterator = new RecursiveIteratorIterator(
        new RecursiveDirectoryIterator($dir, RecursiveDirectoryIterator::SKIP_DOTS),
        RecursiveIteratorIterator::CHILD_FIRST
    );
    
    foreach ($iterator as $file) {
        if ($file->isDir()) {
            rmdir($file->getPathname());
        } else {
            unlink($file->getPathname());
        }
    }
    rmdir($dir);
}

cleanDir($sourceDir);
cleanDir($targetDir);

echo "\nFile synchronization test completed\n";
```

File synchronization compares directories and applies changes efficiently.  
Use modification times for change detection and implement dry-run  
functionality to preview operations before execution.  

## Archive file operations

Creating and extracting various archive formats.  

```php
<?php

$workDir = '/tmp/archive_test';
$sourceDir = "$workDir/source";
$extractDir = "$workDir/extracted";

// Create directories
foreach ([$workDir, $sourceDir, $extractDir] as $dir) {
    if (!is_dir($dir)) {
        mkdir($dir, 0755, true);
    }
}

// Create sample files
$sampleFiles = [
    'document.txt' => 'Sample document content with multiple lines' . str_repeat("\nLine content", 50),
    'config.json' => json_encode(['setting1' => 'value1', 'setting2' => 'value2'], JSON_PRETTY_PRINT),
    'data.csv' => "Name,Age,City\nAlice,25,New York\nBob,30,Los Angeles\nCharlie,35,Chicago",
    'script.php' => '<?php echo "Archive test script"; ?>'
];

foreach ($sampleFiles as $filename => $content) {
    file_put_contents("$sourceDir/$filename", $content);
}

echo "Created " . count($sampleFiles) . " files for archiving\n";

// Archive operations class
class ArchiveManager {
    public function createTarGz($sourceDir, $archivePath) {
        if (!class_exists('PharData')) {
            throw new RuntimeException('PharData class not available');
        }
        
        $phar = new PharData($archivePath . '.tar');
        $phar->buildFromDirectory($sourceDir);
        $phar->compress(Phar::GZ);
        
        // Remove uncompressed tar file
        unlink($archivePath . '.tar');
        
        return $archivePath . '.tar.gz';
    }
    
    public function extractTarGz($archivePath, $extractDir) {
        if (!class_exists('PharData')) {
            throw new RuntimeException('PharData class not available');
        }
        
        $phar = new PharData($archivePath);
        $phar->extractTo($extractDir);
        
        return true;
    }
    
    public function createZip($sourceDir, $archivePath) {
        if (!extension_loaded('zip')) {
            throw new RuntimeException('ZIP extension not available');
        }
        
        $zip = new ZipArchive();
        if ($zip->open($archivePath, ZipArchive::CREATE | ZipArchive::OVERWRITE) !== TRUE) {
            throw new RuntimeException('Cannot create ZIP file');
        }
        
        $files = new RecursiveIteratorIterator(
            new RecursiveDirectoryIterator($sourceDir, RecursiveDirectoryIterator::SKIP_DOTS)
        );
        
        foreach ($files as $file) {
            $relativePath = str_replace($sourceDir . '/', '', $file->getPathname());
            
            if ($file->isDir()) {
                $zip->addEmptyDir($relativePath);
            } else {
                $zip->addFile($file->getPathname(), $relativePath);
            }
        }
        
        $zip->close();
        return $archivePath;
    }
    
    public function extractZip($archivePath, $extractDir) {
        if (!extension_loaded('zip')) {
            throw new RuntimeException('ZIP extension not available');
        }
        
        $zip = new ZipArchive();
        if ($zip->open($archivePath) !== TRUE) {
            throw new RuntimeException('Cannot open ZIP file');
        }
        
        $zip->extractTo($extractDir);
        $zip->close();
        
        return true;
    }
    
    public function getArchiveInfo($archivePath) {
        $info = [
            'path' => $archivePath,
            'size' => filesize($archivePath),
            'type' => $this->detectArchiveType($archivePath),
            'created' => date('Y-m-d H:i:s', filemtime($archivePath))
        ];
        
        // Get file count for ZIP archives
        if ($info['type'] === 'zip' && extension_loaded('zip')) {
            $zip = new ZipArchive();
            if ($zip->open($archivePath) === TRUE) {
                $info['file_count'] = $zip->numFiles;
                $zip->close();
            }
        }
        
        return $info;
    }
    
    private function detectArchiveType($path) {
        $extension = strtolower(pathinfo($path, PATHINFO_EXTENSION));
        
        switch ($extension) {
            case 'zip':
                return 'zip';
            case 'gz':
                return pathinfo(pathinfo($path, PATHINFO_FILENAME), PATHINFO_EXTENSION) === 'tar' ? 'tar.gz' : 'gz';
            case 'tar':
                return 'tar';
            default:
                return 'unknown';
        }
    }
}

// Calculate total source size
$totalSize = 0;
$fileCount = 0;
foreach (glob("$sourceDir/*") as $file) {
    if (is_file($file)) {
        $totalSize += filesize($file);
        $fileCount++;
    }
}

echo "Source directory: $fileCount files, " . round($totalSize / 1024, 2) . " KB\n";

$archiveManager = new ArchiveManager();

// Create ZIP archive
echo "\nCreating ZIP archive...\n";
try {
    $zipFile = "$workDir/archive.zip";
    $archiveManager->createZip($sourceDir, $zipFile);
    
    $zipInfo = $archiveManager->getArchiveInfo($zipFile);
    echo "ZIP created: {$zipInfo['size']} bytes";
    if (isset($zipInfo['file_count'])) {
        echo ", {$zipInfo['file_count']} files";
    }
    echo "\n";
    
    $compressionRatio = round((1 - $zipInfo['size'] / $totalSize) * 100, 2);
    echo "Compression ratio: {$compressionRatio}%\n";
    
} catch (Exception $e) {
    echo "ZIP creation failed: " . $e->getMessage() . "\n";
}

// Create TAR.GZ archive
echo "\nCreating TAR.GZ archive...\n";
try {
    $tarGzFile = "$workDir/archive.tar.gz";
    $actualPath = $archiveManager->createTarGz($sourceDir, "$workDir/archive");
    
    $tarGzInfo = $archiveManager->getArchiveInfo($actualPath);
    echo "TAR.GZ created: {$tarGzInfo['size']} bytes\n";
    
    $compressionRatio = round((1 - $tarGzInfo['size'] / $totalSize) * 100, 2);
    echo "Compression ratio: {$compressionRatio}%\n";
    
} catch (Exception $e) {
    echo "TAR.GZ creation failed: " . $e->getMessage() . "\n";
}

// Extract and verify
if (isset($zipFile) && file_exists($zipFile)) {
    echo "\nExtracting ZIP archive...\n";
    $zipExtractDir = "$extractDir/from_zip";
    mkdir($zipExtractDir, 0755, true);
    
    try {
        $archiveManager->extractZip($zipFile, $zipExtractDir);
        $extractedFiles = glob("$zipExtractDir/*");
        echo "Extracted " . count($extractedFiles) . " files from ZIP\n";
        
        // Verify one file
        $testFile = "$zipExtractDir/document.txt";
        if (file_exists($testFile)) {
            $originalFile = "$sourceDir/document.txt";
            $match = (md5_file($testFile) === md5_file($originalFile)) ? "✓" : "✗";
            echo "File integrity check: $match\n";
        }
        
    } catch (Exception $e) {
        echo "ZIP extraction failed: " . $e->getMessage() . "\n";
    }
}

// Clean up
function removeDirectory($dir) {
    if (!is_dir($dir)) return;
    
    $files = new RecursiveIteratorIterator(
        new RecursiveDirectoryIterator($dir, RecursiveDirectoryIterator::SKIP_DOTS),
        RecursiveIteratorIterator::CHILD_FIRST
    );
    
    foreach ($files as $file) {
        if ($file->isDir()) {
            rmdir($file->getPathname());
        } else {
            unlink($file->getPathname());
        }
    }
    rmdir($dir);
}

removeDirectory($workDir);
echo "\nArchive operations completed successfully\n";
```

Archive operations support multiple formats like ZIP and TAR.GZ with  
compression. Always verify extracted files match originals and  
provide compression statistics for user feedback.  

## Real-time file processing

Processing files as they are created or modified.  

```php
<?php

$watchDir = '/tmp/realtime_watch';
$processedDir = '/tmp/realtime_processed';

// Create directories
foreach ([$watchDir, $processedDir] as $dir) {
    if (!is_dir($dir)) {
        mkdir($dir, 0755, true);
    }
}

// Real-time file processor
class RealTimeProcessor {
    private $watchDir;
    private $processedDir;
    private $knownFiles;
    private $running;
    
    public function __construct($watchDir, $processedDir) {
        $this->watchDir = $watchDir;
        $this->processedDir = $processedDir;
        $this->knownFiles = [];
        $this->running = false;
    }
    
    public function start($duration = 10) {
        $this->running = true;
        $startTime = time();
        
        echo "Starting real-time file processing for $duration seconds...\n";
        echo "Watch directory: $this->watchDir\n";
        
        // Initial scan
        $this->scanDirectory();
        
        while ($this->running && (time() - $startTime) < $duration) {
            $this->processChanges();
            usleep(500000); // Sleep 0.5 seconds
        }
        
        echo "Real-time processing stopped\n";
    }
    
    public function stop() {
        $this->running = false;
    }
    
    private function scanDirectory() {
        $currentFiles = [];
        $files = glob($this->watchDir . '/*');
        
        foreach ($files as $file) {
            if (is_file($file)) {
                $currentFiles[$file] = [
                    'size' => filesize($file),
                    'mtime' => filemtime($file)
                ];
            }
        }
        
        return $currentFiles;
    }
    
    private function processChanges() {
        $currentFiles = $this->scanDirectory();
        
        // Find new or modified files
        foreach ($currentFiles as $filePath => $fileInfo) {
            $isNew = !isset($this->knownFiles[$filePath]);
            $isModified = isset($this->knownFiles[$filePath]) && 
                         ($this->knownFiles[$filePath]['mtime'] !== $fileInfo['mtime'] ||
                          $this->knownFiles[$filePath]['size'] !== $fileInfo['size']);
            
            if ($isNew || $isModified) {
                $this->processFile($filePath, $isNew ? 'new' : 'modified');
            }
        }
        
        // Find deleted files
        foreach ($this->knownFiles as $filePath => $fileInfo) {
            if (!isset($currentFiles[$filePath])) {
                $this->handleDeletedFile($filePath);
            }
        }
        
        $this->knownFiles = $currentFiles;
    }
    
    private function processFile($filePath, $changeType) {
        $filename = basename($filePath);
        $timestamp = date('Y-m-d H:i:s');
        
        echo "[$timestamp] Processing $changeType file: $filename\n";
        
        try {
            // Wait for file to be completely written
            $this->waitForFileStability($filePath);
            
            $content = file_get_contents($filePath);
            
            // Process based on file type
            $extension = strtolower(pathinfo($filename, PATHINFO_EXTENSION));
            $processedContent = $this->processContent($content, $extension);
            
            // Save processed file
            $processedPath = $this->processedDir . '/' . $filename . '.processed';
            file_put_contents($processedPath, $processedContent);
            
            echo "  ✓ Processed and saved to: " . basename($processedPath) . "\n";
            
        } catch (Exception $e) {
            echo "  ✗ Error processing $filename: " . $e->getMessage() . "\n";
        }
    }
    
    private function waitForFileStability($filePath, $maxWait = 5) {
        $lastSize = 0;
        $stableCount = 0;
        $maxStableCount = 3;
        
        while ($stableCount < $maxStableCount && $maxWait-- > 0) {
            clearstatcache();
            $currentSize = file_exists($filePath) ? filesize($filePath) : 0;
            
            if ($currentSize === $lastSize) {
                $stableCount++;
            } else {
                $stableCount = 0;
                $lastSize = $currentSize;
            }
            
            sleep(1);
        }
    }
    
    private function processContent($content, $extension) {
        $header = "Processed on: " . date('Y-m-d H:i:s') . "\n";
        $header .= "File type: $extension\n";
        $header .= "Content length: " . strlen($content) . " bytes\n";
        $header .= str_repeat("-", 40) . "\n";
        
        switch ($extension) {
            case 'txt':
                return $header . "Line count: " . substr_count($content, "\n") . "\n" . $content;
                
            case 'json':
                $decoded = json_decode($content, true);
                if ($decoded !== null) {
                    return $header . "JSON validation: VALID\n" . 
                           json_encode($decoded, JSON_PRETTY_PRINT);
                }
                return $header . "JSON validation: INVALID\n" . $content;
                
            case 'csv':
                $lines = explode("\n", trim($content));
                return $header . "CSV rows: " . count($lines) . "\n" . $content;
                
            default:
                return $header . $content;
        }
    }
    
    private function handleDeletedFile($filePath) {
        $filename = basename($filePath);
        echo "[" . date('Y-m-d H:i:s') . "] File deleted: $filename\n";
        
        // Clean up processed file if it exists
        $processedPath = $this->processedDir . '/' . $filename . '.processed';
        if (file_exists($processedPath)) {
            unlink($processedPath);
            echo "  ✓ Cleaned up processed file\n";
        }
    }
}

// Create processor
$processor = new RealTimeProcessor($watchDir, $processedDir);

// Start file simulator in background
$simulatorScript = '/tmp/file_simulator.php';
file_put_contents($simulatorScript, '<?php
$dir = $argv[1];
$files = [
    "data1.txt" => "Sample text file\nWith multiple lines",
    "config.json" => \'{"setting1": "value1", "setting2": "value2"}\',
    "data.csv" => "Name,Age\nAlice,25\nBob,30"
];

foreach ($files as $name => $content) {
    sleep(2);
    file_put_contents("$dir/$name", $content);
    echo "Created: $name\n";
}

// Modify a file
sleep(3);
file_put_contents("$dir/data1.txt", "Modified text content\nWith additional lines\nAdded later");
echo "Modified: data1.txt\n";

// Delete a file
sleep(2);
unlink("$dir/config.json");
echo "Deleted: config.json\n";
');

// Start background simulator
$descriptors = [
    0 => ['pipe', 'r'],
    1 => ['pipe', 'w'],
    2 => ['pipe', 'w']
];

$process = proc_open("php $simulatorScript $watchDir", $descriptors, $pipes);

// Start real-time processor
$processor->start(12);

// Wait for background process
if (is_resource($process)) {
    proc_close($process);
}

// Show results
echo "\nProcessing completed. Results:\n";
$processedFiles = glob("$processedDir/*");
echo "Processed files: " . count($processedFiles) . "\n";

foreach ($processedFiles as $file) {
    echo "  " . basename($file) . " (" . filesize($file) . " bytes)\n";
}

// Clean up
unlink($simulatorScript);
array_map('unlink', glob("$watchDir/*"));
array_map('unlink', glob("$processedDir/*"));
rmdir($watchDir);
rmdir($processedDir);

echo "\nReal-time processing demonstration completed\n";
```

Real-time file processing monitors directories for changes and processes  
files immediately. Implement file stability checks to ensure files are  
completely written before processing to avoid corruption.

## File comparison and diffing

Comparing files and detecting differences between versions.  

```php
<?php

$file1Path = '/tmp/version1.txt';
$file2Path = '/tmp/version2.txt';

// Create sample files for comparison
$version1 = "Line 1: Original content\nLine 2: Shared content\nLine 3: Different in v1\nLine 4: Common line\nLine 5: Unique to v1";
$version2 = "Line 1: Modified content\nLine 2: Shared content\nLine 3: Different in v2\nLine 4: Common line\nLine 6: Unique to v2";

file_put_contents($file1Path, $version1);
file_put_contents($file2Path, $version2);

// File comparison class
class FileComparer {
    public function compareFiles($file1, $file2) {
        if (!file_exists($file1) || !file_exists($file2)) {
            throw new RuntimeException("One or both files do not exist");
        }
        
        $content1 = file_get_contents($file1);
        $content2 = file_get_contents($file2);
        
        return [
            'identical' => $content1 === $content2,
            'size_diff' => strlen($content2) - strlen($content1),
            'hash1' => md5($content1),
            'hash2' => md5($content2),
            'binary_identical' => $this->binaryCompare($file1, $file2)
        ];
    }
    
    public function lineByLineDiff($file1, $file2) {
        $lines1 = file($file1, FILE_IGNORE_NEW_LINES);
        $lines2 = file($file2, FILE_IGNORE_NEW_LINES);
        
        $diff = [];
        $maxLines = max(count($lines1), count($lines2));
        
        for ($i = 0; $i < $maxLines; $i++) {
            $line1 = $lines1[$i] ?? null;
            $line2 = $lines2[$i] ?? null;
            
            if ($line1 !== $line2) {
                $diff[] = [
                    'line' => $i + 1,
                    'file1' => $line1,
                    'file2' => $line2,
                    'type' => $this->getDifferenceType($line1, $line2)
                ];
            }
        }
        
        return $diff;
    }
    
    public function generateUnifiedDiff($file1, $file2, $contextLines = 3) {
        $lines1 = file($file1, FILE_IGNORE_NEW_LINES);
        $lines2 = file($file2, FILE_IGNORE_NEW_LINES);
        
        $diff = $this->computeDiff($lines1, $lines2);
        return $this->formatUnifiedDiff($diff, basename($file1), basename($file2), $contextLines);
    }
    
    private function binaryCompare($file1, $file2) {
        $handle1 = fopen($file1, 'rb');
        $handle2 = fopen($file2, 'rb');
        
        $identical = true;
        while (!feof($handle1) && !feof($handle2)) {
            $chunk1 = fread($handle1, 8192);
            $chunk2 = fread($handle2, 8192);
            
            if ($chunk1 !== $chunk2) {
                $identical = false;
                break;
            }
        }
        
        // Check if one file is longer
        if ($identical && (feof($handle1) !== feof($handle2))) {
            $identical = false;
        }
        
        fclose($handle1);
        fclose($handle2);
        
        return $identical;
    }
    
    private function getDifferenceType($line1, $line2) {
        if ($line1 === null) return 'added';
        if ($line2 === null) return 'removed';
        return 'modified';
    }
    
    private function computeDiff($lines1, $lines2) {
        // Simple diff implementation
        $diff = [];
        $i = 0; $j = 0;
        
        while ($i < count($lines1) || $j < count($lines2)) {
            if ($i >= count($lines1)) {
                $diff[] = ['type' => 'add', 'line' => $lines2[$j], 'pos1' => $i, 'pos2' => $j];
                $j++;
            } elseif ($j >= count($lines2)) {
                $diff[] = ['type' => 'remove', 'line' => $lines1[$i], 'pos1' => $i, 'pos2' => $j];
                $i++;
            } elseif ($lines1[$i] === $lines2[$j]) {
                $diff[] = ['type' => 'equal', 'line' => $lines1[$i], 'pos1' => $i, 'pos2' => $j];
                $i++; $j++;
            } else {
                $diff[] = ['type' => 'change', 'old' => $lines1[$i], 'new' => $lines2[$j], 'pos1' => $i, 'pos2' => $j];
                $i++; $j++;
            }
        }
        
        return $diff;
    }
    
    private function formatUnifiedDiff($diff, $file1Name, $file2Name, $context) {
        $result = "--- $file1Name\n+++ $file2Name\n";
        
        foreach ($diff as $change) {
            switch ($change['type']) {
                case 'remove':
                    $result .= "-{$change['line']}\n";
                    break;
                case 'add':
                    $result .= "+{$change['line']}\n";
                    break;
                case 'change':
                    $result .= "-{$change['old']}\n";
                    $result .= "+{$change['new']}\n";
                    break;
                case 'equal':
                    $result .= " {$change['line']}\n";
                    break;
            }
        }
        
        return $result;
    }
}

// Compare files
$comparer = new FileComparer();

echo "File comparison results:\n";
$comparison = $comparer->compareFiles($file1Path, $file2Path);

echo "Files identical: " . ($comparison['identical'] ? "Yes" : "No") . "\n";
echo "Size difference: {$comparison['size_diff']} bytes\n";
echo "File 1 MD5: {$comparison['hash1']}\n";
echo "File 2 MD5: {$comparison['hash2']}\n";
echo "Binary identical: " . ($comparison['binary_identical'] ? "Yes" : "No") . "\n";

// Line-by-line differences
echo "\nLine-by-line differences:\n";
$lineDiff = $comparer->lineByLineDiff($file1Path, $file2Path);

foreach ($lineDiff as $diff) {
    echo "Line {$diff['line']} ({$diff['type']}):\n";
    if ($diff['file1'] !== null) {
        echo "  File 1: {$diff['file1']}\n";
    }
    if ($diff['file2'] !== null) {
        echo "  File 2: {$diff['file2']}\n";
    }
}

// Unified diff format
echo "\nUnified diff format:\n";
$unifiedDiff = $comparer->generateUnifiedDiff($file1Path, $file2Path);
echo $unifiedDiff;

// Compare different file types
echo "\nBinary file comparison:\n";
$binaryFile1 = '/tmp/binary1.dat';
$binaryFile2 = '/tmp/binary2.dat';

file_put_contents($binaryFile1, pack('C*', 1, 2, 3, 4, 5));
file_put_contents($binaryFile2, pack('C*', 1, 2, 3, 4, 6));  // Last byte different

$binaryComparison = $comparer->compareFiles($binaryFile1, $binaryFile2);
echo "Binary files identical: " . ($binaryComparison['identical'] ? "Yes" : "No") . "\n";

// Clean up
unlink($file1Path);
unlink($file2Path);
unlink($binaryFile1);
unlink($binaryFile2);
```

File comparison enables version control and change tracking. Use  
MD5 hashes for quick equality checks and line-by-line comparison  
for detailed difference analysis in text files.  

## File encryption and decryption

Securing files with encryption and decryption operations.  

```php
<?php

$plainFile = '/tmp/secret.txt';
$encryptedFile = '/tmp/secret.enc';
$decryptedFile = '/tmp/secret_decrypted.txt';

// Create sample sensitive data
$sensitiveData = "Confidential Information\n";
$sensitiveData .= "User: admin\n";
$sensitiveData .= "Password: super_secret_123\n";
$sensitiveData .= "API Key: sk-1234567890abcdef\n";
$sensitiveData .= "This file contains sensitive information that must be protected.";

file_put_contents($plainFile, $sensitiveData);

// File encryption class
class FileEncryption {
    private $cipher;
    private $keyLength;
    
    public function __construct($cipher = 'AES-256-CBC') {
        $this->cipher = $cipher;
        $this->keyLength = 32; // 256 bits
        
        if (!in_array($cipher, openssl_get_cipher_methods())) {
            throw new RuntimeException("Cipher $cipher is not supported");
        }
    }
    
    public function generateKey() {
        return random_bytes($this->keyLength);
    }
    
    public function encryptFile($sourcePath, $targetPath, $key) {
        if (!file_exists($sourcePath)) {
            throw new RuntimeException("Source file does not exist: $sourcePath");
        }
        
        $data = file_get_contents($sourcePath);
        $ivLength = openssl_cipher_iv_length($this->cipher);
        $iv = random_bytes($ivLength);
        
        $encrypted = openssl_encrypt($data, $this->cipher, $key, OPENSSL_RAW_DATA, $iv);
        
        if ($encrypted === false) {
            throw new RuntimeException("Encryption failed");
        }
        
        // Store IV + encrypted data
        $encryptedData = $iv . $encrypted;
        
        if (file_put_contents($targetPath, $encryptedData) === false) {
            throw new RuntimeException("Failed to write encrypted file");
        }
        
        return [
            'original_size' => strlen($data),
            'encrypted_size' => strlen($encryptedData),
            'iv_length' => $ivLength
        ];
    }
    
    public function decryptFile($sourcePath, $targetPath, $key) {
        if (!file_exists($sourcePath)) {
            throw new RuntimeException("Encrypted file does not exist: $sourcePath");
        }
        
        $encryptedData = file_get_contents($sourcePath);
        $ivLength = openssl_cipher_iv_length($this->cipher);
        
        if (strlen($encryptedData) < $ivLength) {
            throw new RuntimeException("Invalid encrypted file format");
        }
        
        $iv = substr($encryptedData, 0, $ivLength);
        $encrypted = substr($encryptedData, $ivLength);
        
        $decrypted = openssl_decrypt($encrypted, $this->cipher, $key, OPENSSL_RAW_DATA, $iv);
        
        if ($decrypted === false) {
            throw new RuntimeException("Decryption failed - wrong key or corrupted data");
        }
        
        if (file_put_contents($targetPath, $decrypted) === false) {
            throw new RuntimeException("Failed to write decrypted file");
        }
        
        return [
            'encrypted_size' => strlen($encryptedData),
            'decrypted_size' => strlen($decrypted)
        ];
    }
    
    public function encryptFileWithPassword($sourcePath, $targetPath, $password) {
        // Derive key from password using PBKDF2
        $salt = random_bytes(16);
        $key = hash_pbkdf2('sha256', $password, $salt, 10000, $this->keyLength, true);
        
        $result = $this->encryptFile($sourcePath, $targetPath . '.tmp', $key);
        
        // Prepend salt to encrypted file
        $encryptedData = file_get_contents($targetPath . '.tmp');
        $finalData = $salt . $encryptedData;
        file_put_contents($targetPath, $finalData);
        unlink($targetPath . '.tmp');
        
        $result['salt_length'] = 16;
        return $result;
    }
    
    public function decryptFileWithPassword($sourcePath, $targetPath, $password) {
        $fileData = file_get_contents($sourcePath);
        
        $salt = substr($fileData, 0, 16);
        $encryptedData = substr($fileData, 16);
        
        // Derive the same key
        $key = hash_pbkdf2('sha256', $password, $salt, 10000, $this->keyLength, true);
        
        // Write encrypted data without salt to temp file
        file_put_contents($sourcePath . '.tmp', $encryptedData);
        
        try {
            $result = $this->decryptFile($sourcePath . '.tmp', $targetPath, $key);
            unlink($sourcePath . '.tmp');
            return $result;
        } catch (Exception $e) {
            if (file_exists($sourcePath . '.tmp')) {
                unlink($sourcePath . '.tmp');
            }
            throw $e;
        }
    }
}

// Demonstrate file encryption
echo "File encryption demonstration:\n";
echo "Original file size: " . filesize($plainFile) . " bytes\n";

$encryption = new FileEncryption();

// Method 1: Encryption with random key
echo "\nMethod 1: Encryption with random key\n";
$key = $encryption->generateKey();
echo "Generated key (hex): " . bin2hex($key) . "\n";

try {
    $encResult = $encryption->encryptFile($plainFile, $encryptedFile, $key);
    echo "Encryption successful:\n";
    echo "  Original size: {$encResult['original_size']} bytes\n";
    echo "  Encrypted size: {$encResult['encrypted_size']} bytes\n";
    echo "  IV length: {$encResult['iv_length']} bytes\n";
    
    // Decrypt back
    $decResult = $encryption->decryptFile($encryptedFile, $decryptedFile, $key);
    echo "Decryption successful:\n";
    echo "  Decrypted size: {$decResult['decrypted_size']} bytes\n";
    
    // Verify integrity
    $originalHash = md5_file($plainFile);
    $decryptedHash = md5_file($decryptedFile);
    echo "Data integrity: " . ($originalHash === $decryptedHash ? "✓ VERIFIED" : "✗ CORRUPTED") . "\n";
    
} catch (Exception $e) {
    echo "Error: " . $e->getMessage() . "\n";
}

// Method 2: Password-based encryption
echo "\nMethod 2: Password-based encryption\n";
$password = "my_super_secret_password_123";
$passwordEncFile = '/tmp/secret_pwd.enc';
$passwordDecFile = '/tmp/secret_pwd_dec.txt';

try {
    $pwdEncResult = $encryption->encryptFileWithPassword($plainFile, $passwordEncFile, $password);
    echo "Password encryption successful:\n";
    echo "  Encrypted size: {$pwdEncResult['encrypted_size']} bytes\n";
    echo "  Salt length: {$pwdEncResult['salt_length']} bytes\n";
    
    // Test wrong password
    echo "Testing wrong password...\n";
    try {
        $encryption->decryptFileWithPassword($passwordEncFile, '/tmp/wrong.txt', "wrong_password");
        echo "ERROR: Decryption should have failed!\n";
    } catch (Exception $e) {
        echo "✓ Correctly rejected wrong password\n";
    }
    
    // Test correct password
    $pwdDecResult = $encryption->decryptFileWithPassword($passwordEncFile, $passwordDecFile, $password);
    echo "Password decryption successful:\n";
    echo "  Decrypted size: {$pwdDecResult['decrypted_size']} bytes\n";
    
    // Verify integrity
    $originalHash = md5_file($plainFile);
    $decryptedHash = md5_file($passwordDecFile);
    echo "Data integrity: " . ($originalHash === $decryptedHash ? "✓ VERIFIED" : "✗ CORRUPTED") . "\n";
    
} catch (Exception $e) {
    echo "Error: " . $e->getMessage() . "\n";
}

// Security considerations
echo "\nSecurity considerations:\n";
echo "• Use strong passwords (min 12 characters with mixed case, numbers, symbols)\n";
echo "• Store encryption keys securely (separate from encrypted data)\n";
echo "• Use proper key derivation (PBKDF2, bcrypt, or Argon2)\n";
echo "• Always verify data integrity after decryption\n";
echo "• Consider using authenticated encryption (AES-GCM)\n";

// Clean up
$cleanupFiles = [$plainFile, $encryptedFile, $decryptedFile, $passwordEncFile, $passwordDecFile];
foreach ($cleanupFiles as $file) {
    if (file_exists($file)) {
        // Overwrite sensitive data before deletion
        $size = filesize($file);
        file_put_contents($file, str_repeat("\0", $size));
        unlink($file);
    }
}

echo "\nFile encryption demonstration completed\n";
```

File encryption protects sensitive data using industry-standard  
algorithms. Use strong passwords, proper key derivation (PBKDF2),  
and always verify decrypted data integrity with checksums.