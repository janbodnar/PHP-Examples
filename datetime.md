# DateTime

This chapter covers 25 comprehensive PHP DateTime examples demonstrating  
date and time manipulation, calculations, formatting, and advanced temporal  
operations. These examples progress from basic date creation to sophisticated  
timezone handling and business logic, using modern PHP 8.4 features. Each  
example includes clear explanations to help you master temporal data handling  
in PHP applications.

## Creating DateTime objects

The foundation of date and time work in PHP.  

```php
<?php

// Current date and time
$now = new DateTime();
echo "Current time: " . $now->format('Y-m-d H:i:s') . "\n";

// Specific date and time
$specific = new DateTime('2024-06-15 14:30:00');
echo "Specific time: " . $specific->format('Y-m-d H:i:s') . "\n";

// From timestamp
$fromTimestamp = new DateTime('@1735689600'); // Unix timestamp
echo "From timestamp: " . $fromTimestamp->format('Y-m-d H:i:s') . "\n";

// Current date at midnight
$midnight = new DateTime('today');
echo "Today at midnight: " . $midnight->format('Y-m-d H:i:s') . "\n";
```

DateTime objects are created using the `new DateTime()` constructor with  
optional time strings. The class supports various input formats including  
timestamps, relative formats like 'today', and precise ISO dates. When no  
parameter is provided, it defaults to the current date and time.  

## Basic date formatting

Display dates in different formats for various use cases.  

```php
<?php

$date = new DateTime('2024-03-15 16:45:30');

echo "Standard formats:\n";
echo "  Y-m-d: " . $date->format('Y-m-d') . "\n";
echo "  m/d/Y: " . $date->format('m/d/Y') . "\n";
echo "  d.m.Y: " . $date->format('d.m.Y') . "\n";
echo "  H:i:s: " . $date->format('H:i:s') . "\n";

echo "\nReadable formats:\n";
echo "  Full: " . $date->format('l, F j, Y') . "\n";
echo "  Short: " . $date->format('D, M j') . "\n";
echo "  With time: " . $date->format('M j, Y g:i A') . "\n";

echo "\nISO and RFC formats:\n";
echo "  ISO 8601: " . $date->format('c') . "\n";
echo "  RFC 2822: " . $date->format('r') . "\n";
echo "  Unix timestamp: " . $date->format('U') . "\n";
```

The `format()` method uses format characters to display dates in countless  
ways. Common patterns include Y-m-d for databases, l F j Y for readable  
text, and c for ISO 8601 standard. Format characters can be combined  
freely to match any display requirement.  

## Date arithmetic with DateInterval

Add and subtract time periods using structured intervals.  

```php
<?php

$date = new DateTime('2024-01-15');
echo "Original date: " . $date->format('Y-m-d') . "\n";

// Add various intervals
$date->add(new DateInterval('P1M')); // Add 1 month
echo "Plus 1 month: " . $date->format('Y-m-d') . "\n";

$date->add(new DateInterval('P2W')); // Add 2 weeks  
echo "Plus 2 weeks: " . $date->format('Y-m-d') . "\n";

$date->add(new DateInterval('P3D')); // Add 3 days
echo "Plus 3 days: " . $date->format('Y-m-d') . "\n";

// Subtract intervals
$date->sub(new DateInterval('PT5H')); // Subtract 5 hours
echo "Minus 5 hours: " . $date->format('Y-m-d H:i') . "\n";

// Complex interval
$date->add(new DateInterval('P1Y2M3DT4H5M6S')); // 1y 2m 3d 4h 5m 6s
echo "Complex addition: " . $date->format('Y-m-d H:i:s') . "\n";
```

DateInterval follows ISO 8601 duration format: P for period, T for time.  
Years (Y), months (M), days (D) come before T; hours (H), minutes (M),  
seconds (S) come after. This provides precise control over date arithmetic  
while handling month lengths and leap years automatically.  

## Timezone handling

Working with different timezones and conversions.  

```php
<?php

// Create date with specific timezone
$utc = new DateTime('2024-06-15 12:00:00', new DateTimeZone('UTC'));
echo "UTC: " . $utc->format('Y-m-d H:i:s T') . "\n";

// Convert to different timezones
$newYork = clone $utc;
$newYork->setTimezone(new DateTimeZone('America/New_York'));
echo "New York: " . $newYork->format('Y-m-d H:i:s T') . "\n";

$tokyo = clone $utc;
$tokyo->setTimezone(new DateTimeZone('Asia/Tokyo'));
echo "Tokyo: " . $tokyo->format('Y-m-d H:i:s T') . "\n";

$london = clone $utc;
$london->setTimezone(new DateTimeZone('Europe/London'));
echo "London: " . $london->format('Y-m-d H:i:s T') . "\n";

// Get all available timezones (sample)
$timezones = DateTimeZone::listIdentifiers(DateTimeZone::AMERICA);
echo "\nFirst 5 American timezones:\n";
foreach (array_slice($timezones, 0, 5) as $tz) {
    echo "  $tz\n";
}
```

Timezone handling uses DateTimeZone objects with IANA timezone identifiers.  
The `setTimezone()` method converts times while preserving the actual  
moment. This enables proper handling of global applications where users  
are in different time zones but need consistent temporal references.  

## Date comparisons

Compare dates and times for chronological operations.  

```php
<?php

$date1 = new DateTime('2024-03-15');
$date2 = new DateTime('2024-06-20');
$date3 = new DateTime('2024-03-15');

echo "Date comparisons:\n";
echo "date1: " . $date1->format('Y-m-d') . "\n";
echo "date2: " . $date2->format('Y-m-d') . "\n";
echo "date3: " . $date3->format('Y-m-d') . "\n\n";

// Basic comparisons
if ($date1 < $date2) {
    echo "date1 is before date2\n";
}

if ($date1 == $date3) {
    echo "date1 equals date3\n";
}

// Using comparison method
$result = $date1 <=> $date2; // Spaceship operator
echo "Spaceship result: $result\n"; // -1, 0, or 1

// Find earliest and latest
$dates = [$date1, $date2, $date3];
$earliest = min($dates);
$latest = max($dates);

echo "Earliest: " . $earliest->format('Y-m-d') . "\n";
echo "Latest: " . $latest->format('Y-m-d') . "\n";
```

DateTime objects support direct comparison operators (<, >, ==, <=, >=).  
The spaceship operator (<=>) returns -1, 0, or 1 for less than, equal,  
or greater than comparisons. Functions like `min()` and `max()` work  
directly with DateTime arrays for finding chronological extremes.  

## Calculating date differences

Find intervals between dates with detailed breakdowns.  

```php
<?php

$start = new DateTime('2024-01-15');
$end = new DateTime('2024-06-20');

// Calculate difference
$interval = $start->diff($end);

echo "Date difference between " . $start->format('Y-m-d') . 
     " and " . $end->format('Y-m-d') . ":\n";
echo "  Years: " . $interval->y . "\n";
echo "  Months: " . $interval->m . "\n";
echo "  Days: " . $interval->d . "\n";
echo "  Total days: " . $interval->days . "\n";
echo "  Hours: " . $interval->h . "\n";
echo "  Minutes: " . $interval->i . "\n";
echo "  Seconds: " . $interval->s . "\n";

// Check if future or past
if ($interval->invert) {
    echo "  Direction: Past (start > end)\n";
} else {
    echo "  Direction: Future (start < end)\n";
}

// Format the interval
echo "  Formatted: " . $interval->format('%y years, %m months, %d days') . "\n";
```

The `diff()` method returns a DateInterval object containing detailed  
information about the time span between dates. Properties like `days`  
give total counts while `y`, `m`, `d` show the breakdown. The `invert`  
property indicates temporal direction of the difference.  

## Working with timestamps

Convert between DateTime objects and Unix timestamps.  

```php
<?php

// Current timestamp
$now = time();
echo "Current timestamp: $now\n";

// Convert timestamp to DateTime
$dateFromTimestamp = new DateTime();
$dateFromTimestamp->setTimestamp($now);
echo "From timestamp: " . $dateFromTimestamp->format('Y-m-d H:i:s') . "\n";

// Get timestamp from DateTime
$date = new DateTime('2024-12-25 15:30:00');
$timestamp = $date->getTimestamp();
echo "Christmas 2024 timestamp: $timestamp\n";

// Microsecond precision
$microtime = microtime(true);
$preciseDt = DateTime::createFromFormat('U.u', sprintf('%.6F', $microtime));
echo "Precise time: " . $preciseDt->format('Y-m-d H:i:s.u') . "\n";

// Timestamp arithmetic
$futureTimestamp = $timestamp + (7 * 24 * 60 * 60); // Add 7 days
$futureDate = new DateTime();
$futureDate->setTimestamp($futureTimestamp);
echo "Week after Christmas: " . $futureDate->format('Y-m-d') . "\n";
```

Timestamps represent seconds since Unix epoch (1970-01-01 00:00:00 UTC).  
Use `getTimestamp()` and `setTimestamp()` for conversions. For microsecond  
precision, combine `microtime(true)` with `createFromFormat()` using the  
'U.u' format to preserve fractional seconds.  

## Parsing date strings

Convert various string formats to DateTime objects.  

```php
<?php

// Standard parsing
$dates = [
    '2024-03-15',
    '2024/03/15',  
    'March 15, 2024',
    '15-Mar-2024',
    '2024-03-15T14:30:00Z',
    'tomorrow',
    '+1 week',
    'last Monday'
];

echo "Parsing various date formats:\n";
foreach ($dates as $dateString) {
    try {
        $parsed = new DateTime($dateString);
        echo "  '$dateString' -> " . $parsed->format('Y-m-d H:i:s') . "\n";
    } catch (Exception $e) {
        echo "  '$dateString' -> ERROR: " . $e->getMessage() . "\n";
    }
}

// Custom format parsing
$customDate = '15/03/2024 2:30 PM';
$custom = DateTime::createFromFormat('d/m/Y g:i A', $customDate);
if ($custom) {
    echo "\nCustom format '$customDate' -> " . $custom->format('Y-m-d H:i:s') . "\n";
}

// Handling parse errors
$lastErrors = DateTime::getLastErrors();
if ($lastErrors && $lastErrors['error_count'] > 0) {
    echo "Parse errors: " . implode(', ', $lastErrors['errors']) . "\n";
}
```

DateTime constructor accepts many string formats automatically. For custom  
formats, use `createFromFormat()` with format strings. Relative formats  
like 'tomorrow' and '+1 week' are supported. Always handle parsing errors  
as malformed strings throw exceptions.  

## Age calculation

Calculate precise ages with various metrics.  

```php
<?php

function calculateAge(string $birthdate): array {
    $birth = new DateTime($birthdate);
    $now = new DateTime();
    $interval = $birth->diff($now);
    
    // Calculate next birthday
    $nextBirthday = clone $birth;
    $nextBirthday->setDate($now->format('Y'), $birth->format('m'), $birth->format('d'));
    
    if ($nextBirthday <= $now) {
        $nextBirthday->add(new DateInterval('P1Y'));
    }
    
    $daysToNext = $now->diff($nextBirthday)->days;
    
    return [
        'years' => $interval->y,
        'months' => $interval->m,
        'days' => $interval->d,
        'total_days' => $interval->days,
        'total_hours' => $interval->days * 24,
        'next_birthday' => $nextBirthday->format('Y-m-d'),
        'days_to_next_birthday' => $daysToNext
    ];
}

$birthdates = ['1990-06-15', '2000-12-31', '1985-02-29'];

foreach ($birthdates as $birthdate) {
    echo "Age calculation for $birthdate:\n";
    $age = calculateAge($birthdate);
    
    foreach ($age as $key => $value) {
        $label = ucfirst(str_replace('_', ' ', $key));
        echo "  $label: $value\n";
    }
    echo "\n";
}
```

Age calculation uses date differences to compute years, months, and days  
lived. The function handles leap years correctly and calculates next  
birthday by adjusting the birth year. This approach provides comprehensive  
age metrics for applications requiring detailed temporal information.  

## Business day calculations

Work with business days, excluding weekends and holidays.  

```php
<?php

function isBusinessDay(DateTime $date, array $holidays = []): bool {
    $dayOfWeek = (int)$date->format('N'); // 1=Monday, 7=Sunday
    
    // Weekend check
    if ($dayOfWeek > 5) {
        return false;
    }
    
    // Holiday check
    $dateString = $date->format('Y-m-d');
    return !in_array($dateString, $holidays);
}

function addBusinessDays(DateTime $startDate, int $daysToAdd, array $holidays = []): DateTime {
    $currentDate = clone $startDate;
    $addedDays = 0;
    
    while ($addedDays < $daysToAdd) {
        $currentDate->add(new DateInterval('P1D'));
        
        if (isBusinessDay($currentDate, $holidays)) {
            $addedDays++;
        }
    }
    
    return $currentDate;
}

function countBusinessDays(DateTime $start, DateTime $end, array $holidays = []): int {
    $current = clone $start;
    $count = 0;
    
    while ($current <= $end) {
        if (isBusinessDay($current, $holidays)) {
            $count++;
        }
        $current->add(new DateInterval('P1D'));
    }
    
    return $count;
}

// Example usage
$holidays = ['2024-07-04', '2024-12-25', '2024-01-01'];
$start = new DateTime('2024-06-01');
$end = new DateTime('2024-06-30');

echo "Business day calculations:\n";
echo "Period: " . $start->format('Y-m-d') . " to " . $end->format('Y-m-d') . "\n";
echo "Business days in period: " . countBusinessDays($start, $end, $holidays) . "\n";

$deliveryDate = addBusinessDays($start, 5, $holidays);
echo "5 business days after start: " . $deliveryDate->format('Y-m-d') . "\n";
```

Business day calculations exclude weekends (Saturday/Sunday) and specified  
holidays. The `format('N')` method returns ISO day numbers where Monday  
is 1 and Sunday is 7. These functions are essential for calculating  
delivery dates, processing times, and financial settlements.  

## DateTimeImmutable for safety

Use immutable DateTime objects to prevent accidental modifications.  

```php
<?php

// Mutable DateTime can cause issues
$mutableDate = new DateTime('2024-03-15');
$originalDate = $mutableDate;

$mutableDate->add(new DateInterval('P1M'));

echo "Mutable DateTime issue:\n";
echo "Original after modification: " . $originalDate->format('Y-m-d') . "\n";
echo "Modified: " . $mutableDate->format('Y-m-d') . "\n\n";

// DateTimeImmutable is safer
$immutableDate = new DateTimeImmutable('2024-03-15');
$originalImmutable = $immutableDate;

$modifiedImmutable = $immutableDate->add(new DateInterval('P1M'));

echo "DateTimeImmutable safety:\n";
echo "Original stays unchanged: " . $originalImmutable->format('Y-m-d') . "\n";
echo "New modified object: " . $modifiedImmutable->format('Y-m-d') . "\n\n";

// Chain operations safely
$chainedResult = (new DateTimeImmutable('2024-01-01'))
    ->add(new DateInterval('P3M'))
    ->add(new DateInterval('P2W'))
    ->setTime(14, 30, 0);

echo "Chained operations result: " . $chainedResult->format('Y-m-d H:i:s') . "\n";
```

DateTimeImmutable prevents accidental modifications by returning new  
instances instead of modifying the original. This eliminates subtle bugs  
where shared date objects are unexpectedly changed. Method chaining works  
naturally since each operation returns a new immutable object.  

## Date validation

Validate dates and handle invalid date scenarios.  

```php
<?php

function validateDate(string $date, string $format = 'Y-m-d'): array {
    $datetime = DateTime::createFromFormat($format, $date);
    $errors = DateTime::getLastErrors() ?: ['error_count' => 0, 'warning_count' => 0, 'errors' => [], 'warnings' => []];
    
    $result = [
        'valid' => false,
        'date' => null,
        'errors' => [],
        'warnings' => []
    ];
    
    if ($datetime && $errors['error_count'] === 0) {
        // Check if the date matches exactly (prevents things like Feb 30)
        if ($datetime->format($format) === $date) {
            $result['valid'] = true;
            $result['date'] = $datetime;
        } else {
            $result['errors'][] = 'Date was auto-corrected, original invalid';
        }
    }
    
    if ($errors['error_count'] > 0) {
        $result['errors'] = array_merge($result['errors'], $errors['errors']);
    }
    
    if ($errors['warning_count'] > 0) {
        $result['warnings'] = $errors['warnings'];
    }
    
    return $result;
}

$testDates = [
    '2024-03-15',    // Valid
    '2024-02-30',    // Invalid (Feb 30th)
    '2024-13-01',    // Invalid (month 13)
    '2024-02-29',    // Valid (leap year)
    '2023-02-29',    // Invalid (not leap year)
    '15/03/2024',    // Valid with custom format
    'invalid-date'   // Completely invalid
];

foreach ($testDates as $testDate) {
    $result = validateDate($testDate);
    echo "Date: '$testDate'\n";
    echo "  Valid: " . ($result['valid'] ? 'Yes' : 'No') . "\n";
    
    if ($result['valid']) {
        echo "  Parsed: " . $result['date']->format('Y-m-d') . "\n";
    }
    
    if (!empty($result['errors'])) {
        echo "  Errors: " . implode(', ', $result['errors']) . "\n";
    }
    echo "\n";
}
```

Date validation prevents invalid dates from being processed. The  
`createFromFormat()` method with `getLastErrors()` provides detailed error  
information. Checking if the formatted date matches the input prevents  
auto-correction of impossible dates like February 30th.  

## Working with weeks

Handle week-based date calculations and ISO week numbers.  

```php
<?php

function getWeekInfo(DateTime $date): array {
    return [
        'iso_week' => $date->format('W'),
        'iso_year' => $date->format('o'), // ISO year can differ from calendar year
        'week_day_iso' => $date->format('N'), // 1=Monday, 7=Sunday
        'week_day_numeric' => $date->format('w'), // 0=Sunday, 6=Saturday
        'week_day_name' => $date->format('l'),
        'week_start' => $date->modify('monday this week')->format('Y-m-d'),
        'week_end' => $date->modify('sunday this week')->format('Y-m-d')
    ];
}

function getDateFromWeek(int $year, int $week, int $dayOfWeek = 1): DateTime {
    $date = new DateTime();
    $date->setISODate($year, $week, $dayOfWeek);
    return $date;
}

function getWeeksInMonth(int $year, int $month): array {
    $firstDay = new DateTime("$year-$month-01");
    $lastDay = new DateTime($firstDay->format('Y-m-t')); // Last day of month
    
    $weeks = [];
    $current = clone $firstDay;
    
    while ($current <= $lastDay) {
        $weekInfo = getWeekInfo(clone $current);
        $weekKey = $weekInfo['iso_year'] . '-W' . $weekInfo['iso_week'];
        
        if (!isset($weeks[$weekKey])) {
            $weeks[$weekKey] = [
                'week_number' => $weekInfo['iso_week'],
                'year' => $weekInfo['iso_year'],
                'start_date' => $weekInfo['week_start'],
                'end_date' => $weekInfo['week_end']
            ];
        }
        
        $current->add(new DateInterval('P7D'));
    }
    
    return array_values($weeks);
}

// Example usage
$testDate = new DateTime('2024-03-15');
echo "Week information for " . $testDate->format('Y-m-d') . ":\n";
$weekInfo = getWeekInfo(clone $testDate);
foreach ($weekInfo as $key => $value) {
    $label = ucfirst(str_replace('_', ' ', $key));
    echo "  $label: $value\n";
}

echo "\nFirst day of ISO week 25, 2024: ";
echo getDateFromWeek(2024, 25, 1)->format('Y-m-d') . "\n";

echo "\nWeeks in March 2024:\n";
$weeks = getWeeksInMonth(2024, 3);
foreach ($weeks as $week) {
    echo "  Week {$week['week_number']}: {$week['start_date']} to {$week['end_date']}\n";
}
```

ISO week calculations use Monday as the first day of the week and assign  
weeks to years based on which year contains the Thursday. The `setISODate()`  
method creates dates from year, week, and day numbers. Week-based logic  
is essential for business reporting and calendar applications.  

## Month and year operations

Navigate and manipulate months and years effectively.  

```php
<?php

function getMonthInfo(DateTime $date): array {
    $firstDay = new DateTime($date->format('Y-m-01'));
    $lastDay = new DateTime($date->format('Y-m-t'));
    
    return [
        'month_name' => $date->format('F'),
        'month_short' => $date->format('M'),
        'month_number' => $date->format('n'),
        'year' => $date->format('Y'),
        'days_in_month' => $date->format('t'),
        'first_day' => $firstDay->format('Y-m-d'),
        'last_day' => $lastDay->format('Y-m-d'),
        'first_weekday' => $firstDay->format('l'),
        'last_weekday' => $lastDay->format('l')
    ];
}

function addMonths(DateTime $date, int $months): DateTime {
    $result = clone $date;
    
    if ($months > 0) {
        for ($i = 0; $i < $months; $i++) {
            $result->add(new DateInterval('P1M'));
        }
    } else {
        for ($i = 0; $i > $months; $i--) {
            $result->sub(new DateInterval('P1M'));
        }
    }
    
    return $result;
}

function getQuarter(DateTime $date): array {
    $month = (int)$date->format('n');
    $quarter = ceil($month / 3);
    $year = (int)$date->format('Y');
    
    $quarterStart = new DateTime("$year-" . ((($quarter - 1) * 3) + 1) . "-01");
    $quarterEnd = clone $quarterStart;
    $quarterEnd->add(new DateInterval('P3M'))->sub(new DateInterval('P1D'));
    
    return [
        'quarter' => $quarter,
        'year' => $year,
        'start_date' => $quarterStart->format('Y-m-d'),
        'end_date' => $quarterEnd->format('Y-m-d'),
        'name' => "Q$quarter $year"
    ];
}

// Example usage
$date = new DateTime('2024-03-15');

echo "Month information for " . $date->format('Y-m-d') . ":\n";
$monthInfo = getMonthInfo($date);
foreach ($monthInfo as $key => $value) {
    $label = ucfirst(str_replace('_', ' ', $key));
    echo "  $label: $value\n";
}

echo "\nMonth arithmetic:\n";
$futureMonth = addMonths($date, 8);
echo "8 months later: " . $futureMonth->format('Y-m-d') . "\n";

$pastMonth = addMonths($date, -5);
echo "5 months earlier: " . $pastMonth->format('Y-m-d') . "\n";

echo "\nQuarter information:\n";
$quarterInfo = getQuarter($date);
foreach ($quarterInfo as $key => $value) {
    $label = ucfirst(str_replace('_', ' ', $key));
    echo "  $label: $value\n";
}
```

Month operations require careful handling of day-of-month values when the  
target month has fewer days. The `format('t')` gives days in month, while  
`format('Y-m-t')` creates the last day. Quarter calculations divide months  
by three and compute the appropriate date ranges.  

## Date periods and iteration

Generate sequences of dates using DatePeriod.  

```php
<?php

// Basic date period - every day
$start = new DateTime('2024-03-01');
$end = new DateTime('2024-03-10');
$interval = new DateInterval('P1D'); // 1 day

$period = new DatePeriod($start, $interval, $end);

echo "Daily period from " . $start->format('Y-m-d') . " to " . $end->format('Y-m-d') . ":\n";
foreach ($period as $date) {
    echo "  " . $date->format('Y-m-d l') . "\n";
}

// Weekly period with recurrence count
$weeklyPeriod = new DatePeriod(
    new DateTime('2024-01-01'),
    new DateInterval('P1W'),
    4 // 4 recurrences
);

echo "\nWeekly period (4 weeks):\n";
foreach ($weeklyPeriod as $date) {
    echo "  Week starting: " . $date->format('Y-m-d') . "\n";
}

// Business days only
function generateBusinessDays(DateTime $start, DateTime $end): Generator {
    $current = clone $start;
    
    while ($current <= $end) {
        $dayOfWeek = (int)$current->format('N');
        if ($dayOfWeek <= 5) { // Monday to Friday
            yield clone $current;
        }
        $current->add(new DateInterval('P1D'));
    }
}

echo "\nBusiness days in first week of March 2024:\n";
$businessStart = new DateTime('2024-03-01');
$businessEnd = new DateTime('2024-03-07');

foreach (generateBusinessDays($businessStart, $businessEnd) as $businessDay) {
    echo "  " . $businessDay->format('Y-m-d l') . "\n";
}

// Monthly recurring dates
$monthlyStart = new DateTime('2024-01-15');
$monthlyPeriod = new DatePeriod(
    $monthlyStart,
    new DateInterval('P1M'),
    new DateTime('2024-06-01')
);

echo "\nMonthly recurrence (15th of each month):\n";
foreach ($monthlyPeriod as $date) {
    echo "  " . $date->format('Y-m-d (F Y)') . "\n";
}
```

DatePeriod creates iteratable sequences of dates based on start date,  
interval, and either end date or recurrence count. Custom generators  
provide more flexibility for complex patterns like business days.  
This approach is memory-efficient for large date ranges.  

## Performance considerations

Optimize DateTime operations for better performance.  

```php
<?php

// Benchmark different approaches
function benchmarkOperation(callable $operation, string $description, int $iterations = 10000): void {
    $startTime = microtime(true);
    $startMemory = memory_get_usage();
    
    for ($i = 0; $i < $iterations; $i++) {
        $operation();
    }
    
    $endTime = microtime(true);
    $endMemory = memory_get_usage();
    
    $duration = ($endTime - $startTime) * 1000; // Convert to milliseconds
    $memoryUsed = $endMemory - $startMemory;
    
    echo "$description:\n";
    echo "  Time: " . number_format($duration, 2) . " ms\n";
    echo "  Memory: " . number_format($memoryUsed) . " bytes\n\n";
}

echo "Performance comparison for $iterations operations:\n\n";
$iterations = 10000;

// DateTime vs DateTimeImmutable creation
benchmarkOperation(function() {
    new DateTime('2024-03-15');
}, 'DateTime creation', $iterations);

benchmarkOperation(function() {
    new DateTimeImmutable('2024-03-15');
}, 'DateTimeImmutable creation', $iterations);

// String formatting vs timestamp
$date = new DateTime('2024-03-15');
benchmarkOperation(function() use ($date) {
    $date->format('Y-m-d');
}, 'DateTime formatting', $iterations);

benchmarkOperation(function() use ($date) {
    $date->getTimestamp();
}, 'Timestamp extraction', $iterations);

// Clone vs new instance
benchmarkOperation(function() use ($date) {
    clone $date;
}, 'DateTime cloning', $iterations);

benchmarkOperation(function() {
    new DateTime('2024-03-15');
}, 'New DateTime instance', $iterations);

// Caching expensive operations
class OptimizedDateCalculator {
    private static array $cache = [];
    
    public static function getCachedAge(string $birthdate): array {
        if (!isset(self::$cache[$birthdate])) {
            $birth = new DateTime($birthdate);
            $now = new DateTime();
            $interval = $birth->diff($now);
            
            self::$cache[$birthdate] = [
                'years' => $interval->y,
                'days' => $interval->days
            ];
        }
        
        return self::$cache[$birthdate];
    }
}

// Test caching performance
benchmarkOperation(function() {
    OptimizedDateCalculator::getCachedAge('1990-06-15');
}, 'Cached age calculation', $iterations);

echo "Tips for better DateTime performance:\n";
echo "1. Reuse DateTime objects when possible\n";
echo "2. Use timestamps for simple comparisons\n";
echo "3. Cache expensive calculations\n";
echo "4. Consider DateTimeImmutable for safer code\n";
echo "5. Use generators for large date sequences\n";
```

Performance optimization focuses on minimizing object creation and  
expensive operations. Caching calculations, reusing objects, and choosing  
appropriate methods based on use case can significantly improve performance.  
Profile your specific usage patterns to identify bottlenecks.  

## Advanced date manipulation

Complex date operations and edge case handling.  

```php
<?php

class AdvancedDateOperations {
    
    /**
     * Find the nth occurrence of a weekday in a month
     */
    public static function getNthWeekdayOfMonth(int $year, int $month, int $weekday, int $occurrence): ?DateTime {
        $date = new DateTime("$year-$month-01");
        $firstWeekday = (int)$date->format('N');
        
        // Calculate first occurrence
        $daysToAdd = ($weekday - $firstWeekday + 7) % 7;
        $date->add(new DateInterval("P{$daysToAdd}D"));
        
        // Add weeks for nth occurrence
        if ($occurrence > 1) {
            $weeksToAdd = $occurrence - 1;
            $date->add(new DateInterval("P{$weeksToAdd}W"));
        }
        
        // Check if still in the same month
        return (int)$date->format('n') === $month ? $date : null;
    }
    
    /**
     * Calculate working days between dates with custom schedules
     */
    public static function calculateWorkingHours(DateTime $start, DateTime $end, array $workingHours = ['09:00', '17:00']): float {
        $totalHours = 0;
        $current = clone $start;
        
        while ($current <= $end) {
            $dayOfWeek = (int)$current->format('N');
            
            // Skip weekends
            if ($dayOfWeek <= 5) {
                $dayStart = clone $current;
                $dayStart->setTime(...explode(':', $workingHours[0]));
                
                $dayEnd = clone $current;
                $dayEnd->setTime(...explode(':', $workingHours[1]));
                
                // Calculate overlap with working hours
                $periodStart = max($current, $dayStart);
                $periodEnd = min($end, $dayEnd);
                
                if ($periodStart < $periodEnd) {
                    $diff = $periodStart->diff($periodEnd);
                    $hours = $diff->h + ($diff->i / 60) + ($diff->s / 3600);
                    $totalHours += $hours;
                }
            }
            
            $current->add(new DateInterval('P1D'))->setTime(0, 0, 0);
        }
        
        return $totalHours;
    }
    
    /**
     * Find Easter date for a given year
     */
    public static function getEaster(int $year): DateTime {
        $timestamp = easter_date($year);
        $easter = new DateTime();
        $easter->setTimestamp($timestamp);
        return $easter;
    }
    
    /**
     * Handle leap seconds (conceptual - PHP doesn't handle actual leap seconds)
     */
    public static function isLeapYear(int $year): bool {
        return $year % 4 === 0 && ($year % 100 !== 0 || $year % 400 === 0);
    }
    
    /**
     * Calculate moon phases (simplified calculation)
     */
    public static function getMoonPhase(DateTime $date): array {
        // Simple approximation - not astronomically accurate
        $timestamp = $date->getTimestamp();
        $daysSinceNewMoon = ($timestamp - mktime(0, 0, 0, 1, 6, 2000)) / (24 * 60 * 60);
        $phase = fmod($daysSinceNewMoon, 29.53058867); // Lunar month length
        
        $phaseNames = [
            'New Moon' => [0, 1],
            'Waxing Crescent' => [1, 7.38],
            'First Quarter' => [7.38, 8.38],
            'Waxing Gibbous' => [8.38, 14.76],
            'Full Moon' => [14.76, 15.76],
            'Waning Gibbous' => [15.76, 22.14],
            'Last Quarter' => [22.14, 23.14],
            'Waning Crescent' => [23.14, 29.53]
        ];
        
        foreach ($phaseNames as $name => $range) {
            if ($phase >= $range[0] && $phase < $range[1]) {
                return ['name' => $name, 'illumination' => abs(cos(($phase / 29.53) * 2 * M_PI))];
            }
        }
        
        return ['name' => 'New Moon', 'illumination' => 0];
    }
}

// Example usage
echo "Advanced date operations:\n\n";

// Find third Monday of March 2024
$thirdMonday = AdvancedDateOperations::getNthWeekdayOfMonth(2024, 3, 1, 3);
echo "Third Monday of March 2024: " . ($thirdMonday ? $thirdMonday->format('Y-m-d') : 'None') . "\n";

// Calculate working hours
$workStart = new DateTime('2024-03-15 08:30:00');
$workEnd = new DateTime('2024-03-17 15:45:00');
$workingHours = AdvancedDateOperations::calculateWorkingHours($workStart, $workEnd);
echo "Working hours between periods: " . number_format($workingHours, 2) . " hours\n";

// Easter calculation
$easter2024 = AdvancedDateOperations::getEaster(2024);
echo "Easter 2024: " . $easter2024->format('Y-m-d l') . "\n";

// Leap year check
$testYears = [2024, 2023, 2000, 1900];
foreach ($testYears as $year) {
    $isLeap = AdvancedDateOperations::isLeapYear($year) ? 'Yes' : 'No';
    echo "$year is leap year: $isLeap\n";
}

// Moon phase (simplified)
$moonPhase = AdvancedDateOperations::getMoonPhase(new DateTime('2024-03-15'));
echo "Moon phase on 2024-03-15: {$moonPhase['name']} (" . 
     number_format($moonPhase['illumination'] * 100, 1) . "% illuminated)\n";
```

Advanced operations handle complex calculations like recurring events,  
astronomical phenomena, and business-specific date logic. These examples  
demonstrate techniques for specialized requirements while maintaining  
readable, maintainable code structure.  

## Date serialization and storage

Handle DateTime objects in databases and APIs.  

```php
<?php

class DateTimeSerializer {
    
    public static function toArray(DateTime $date): array {
        return [
            'timestamp' => $date->getTimestamp(),
            'timezone' => $date->getTimezone()->getName(),
            'iso8601' => $date->format('c'),
            'formatted' => $date->format('Y-m-d H:i:s')
        ];
    }
    
    public static function fromArray(array $data): DateTime {
        if (isset($data['timestamp']) && isset($data['timezone'])) {
            $date = new DateTime();
            $date->setTimestamp($data['timestamp']);
            $date->setTimezone(new DateTimeZone($data['timezone']));
            return $date;
        }
        
        if (isset($data['iso8601'])) {
            return new DateTime($data['iso8601']);
        }
        
        throw new InvalidArgumentException('Invalid date array format');
    }
    
    public static function toJson(DateTime $date): string {
        return json_encode(self::toArray($date));
    }
    
    public static function fromJson(string $json): DateTime {
        $data = json_decode($json, true);
        if (json_last_error() !== JSON_ERROR_NONE) {
            throw new InvalidArgumentException('Invalid JSON format');
        }
        
        return self::fromArray($data);
    }
    
    /**
     * Database-friendly formats
     */
    public static function forDatabase(DateTime $date, string $format = 'mysql'): string {
        switch ($format) {
            case 'mysql':
                return $date->format('Y-m-d H:i:s');
            case 'postgresql':
                return $date->format('Y-m-d H:i:s.uO'); // With microseconds and timezone
            case 'sqlite':
                return $date->format('Y-m-d H:i:s');
            case 'iso8601':
                return $date->format('c');
            default:
                throw new InvalidArgumentException("Unknown database format: $format");
        }
    }
    
    public static function fromDatabase(string $dateString, string $format = 'mysql', ?string $timezone = null): DateTime {
        $formats = [
            'mysql' => 'Y-m-d H:i:s',
            'postgresql' => 'Y-m-d H:i:s.uO',
            'sqlite' => 'Y-m-d H:i:s',
            'iso8601' => 'c'
        ];
        
        if (!isset($formats[$format])) {
            throw new InvalidArgumentException("Unknown database format: $format");
        }
        
        $date = DateTime::createFromFormat($formats[$format], $dateString);
        
        if (!$date) {
            throw new InvalidArgumentException("Could not parse date: $dateString");
        }
        
        if ($timezone && $format !== 'postgresql') {
            $date->setTimezone(new DateTimeZone($timezone));
        }
        
        return $date;
    }
}

// Example usage
$originalDate = new DateTime('2024-03-15 14:30:00', new DateTimeZone('America/New_York'));

echo "Original date: " . $originalDate->format('Y-m-d H:i:s T') . "\n\n";

// Array serialization
$dateArray = DateTimeSerializer::toArray($originalDate);
echo "Array format:\n";
print_r($dateArray);

$restoredFromArray = DateTimeSerializer::fromArray($dateArray);
echo "Restored from array: " . $restoredFromArray->format('Y-m-d H:i:s T') . "\n\n";

// JSON serialization
$dateJson = DateTimeSerializer::toJson($originalDate);
echo "JSON format: $dateJson\n";

$restoredFromJson = DateTimeSerializer::fromJson($dateJson);
echo "Restored from JSON: " . $restoredFromJson->format('Y-m-d H:i:s T') . "\n\n";

// Database formats
echo "Database formats:\n";
foreach (['mysql', 'postgresql', 'sqlite', 'iso8601'] as $format) {
    $dbFormat = DateTimeSerializer::forDatabase($originalDate, $format);
    echo "  $format: $dbFormat\n";
    
    try {
        $restored = DateTimeSerializer::fromDatabase($dbFormat, $format, 'America/New_York');
        echo "    Restored: " . $restored->format('Y-m-d H:i:s T') . "\n";
    } catch (Exception $e) {
        echo "    Error: " . $e->getMessage() . "\n";
    }
}
```

Serialization handles conversion between DateTime objects and storage  
formats. Different databases have specific requirements for date storage.  
JSON serialization is useful for APIs, while preserving timezone  
information prevents data corruption across different environments.  

## Testing and mocking time

Handle time-dependent code in unit tests.  

```php
<?php

class TimeProvider {
    private static ?DateTime $fixedTime = null;
    
    public static function now(): DateTime {
        return self::$fixedTime ? clone self::$fixedTime : new DateTime();
    }
    
    public static function nowImmutable(): DateTimeImmutable {
        $now = self::now();
        return new DateTimeImmutable($now->format('Y-m-d H:i:s'), $now->getTimezone());
    }
    
    public static function setFixedTime(?DateTime $time): void {
        self::$fixedTime = $time ? clone $time : null;
    }
    
    public static function freeze(string $time = 'now'): void {
        self::$fixedTime = new DateTime($time);
    }
    
    public static function unfreeze(): void {
        self::$fixedTime = null;
    }
    
    public static function travel(string $time): void {
        self::$fixedTime = new DateTime($time);
    }
}

class TimeDependentService {
    
    public function getAgeInYears(string $birthdate): int {
        $birth = new DateTime($birthdate);
        $now = TimeProvider::now();
        return $birth->diff($now)->y;
    }
    
    public function isBusinessHour(): bool {
        $now = TimeProvider::now();
        $hour = (int)$now->format('H');
        $dayOfWeek = (int)$now->format('N');
        
        return $dayOfWeek <= 5 && $hour >= 9 && $hour < 17;
    }
    
    public function getNextBusinessDay(): DateTime {
        $date = TimeProvider::now();
        
        do {
            $date->add(new DateInterval('P1D'));
            $dayOfWeek = (int)$date->format('N');
        } while ($dayOfWeek > 5);
        
        return $date;
    }
    
    public function scheduleReminder(int $daysFromNow): DateTime {
        $reminder = TimeProvider::now();
        $reminder->add(new DateInterval("P{$daysFromNow}D"));
        return $reminder;
    }
}

// Testing scenarios
echo "Time-dependent testing examples:\n\n";

$service = new TimeDependentService();

// Test 1: Normal operation
echo "1. Normal operation:\n";
$age = $service->getAgeInYears('1990-06-15');
echo "   Age: $age years\n";
echo "   Business hour: " . ($service->isBusinessHour() ? 'Yes' : 'No') . "\n\n";

// Test 2: Freeze time for consistent testing
echo "2. Frozen time testing:\n";
TimeProvider::freeze('2024-03-15 14:30:00');
echo "   Frozen at: " . TimeProvider::now()->format('Y-m-d H:i:s') . "\n";
echo "   Age: " . $service->getAgeInYears('1990-06-15') . " years\n";
echo "   Business hour: " . ($service->isBusinessHour() ? 'Yes' : 'No') . "\n";
echo "   Next business day: " . $service->getNextBusinessDay()->format('Y-m-d') . "\n\n";

// Test 3: Time travel for edge cases
echo "3. Time travel testing:\n";
$testTimes = [
    '2024-12-25 10:00:00', // Christmas
    '2024-03-16 08:00:00', // Before business hours
    '2024-03-16 18:00:00', // After business hours
    '2024-03-17 12:00:00'  // Sunday
];

foreach ($testTimes as $testTime) {
    TimeProvider::travel($testTime);
    $current = TimeProvider::now();
    echo "   Time: " . $current->format('Y-m-d H:i:s l') . "\n";
    echo "   Business hour: " . ($service->isBusinessHour() ? 'Yes' : 'No') . "\n";
    echo "   Next business day: " . $service->getNextBusinessDay()->format('Y-m-d') . "\n";
    echo "\n";
}

// Test 4: Reminder scheduling
echo "4. Reminder scheduling:\n";
TimeProvider::travel('2024-03-15 12:00:00');
$reminder = $service->scheduleReminder(7);
echo "   Scheduled from: " . TimeProvider::now()->format('Y-m-d H:i:s') . "\n";
echo "   Reminder date: " . $reminder->format('Y-m-d H:i:s') . "\n\n";

// Reset time
TimeProvider::unfreeze();
echo "5. Back to real time: " . TimeProvider::now()->format('Y-m-d H:i:s') . "\n";

echo "\nTesting tips:\n";
echo "- Use TimeProvider for all time-dependent operations\n";
echo "- Freeze time for predictable test results\n";
echo "- Test edge cases with time travel\n";
echo "- Always reset time after tests\n";
echo "- Mock external time sources consistently\n";
```

Time mocking enables deterministic testing of time-dependent functionality.  
By centralizing time access through a provider, tests can control temporal  
flow to verify edge cases, business logic, and time-sensitive operations  
without waiting for actual time to pass.  

## Date localization and formatting

Handle dates in different languages and cultural formats.  

```php
<?php

class DateLocalizer {
    private array $locales = [
        'en' => [
            'months' => ['January', 'February', 'March', 'April', 'May', 'June',
                        'July', 'August', 'September', 'October', 'November', 'December'],
            'days' => ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'],
            'format' => 'M j, Y'
        ],
        'es' => [
            'months' => ['enero', 'febrero', 'marzo', 'abril', 'mayo', 'junio',
                        'julio', 'agosto', 'septiembre', 'octubre', 'noviembre', 'diciembre'],
            'days' => ['domingo', 'lunes', 'martes', 'miércoles', 'jueves', 'viernes', 'sábado'],
            'format' => 'j \\d\\e M \\d\\e Y'
        ],
        'fr' => [
            'months' => ['janvier', 'février', 'mars', 'avril', 'mai', 'juin',
                        'juillet', 'août', 'septembre', 'octobre', 'novembre', 'décembre'],
            'days' => ['dimanche', 'lundi', 'mardi', 'mercredi', 'jeudi', 'vendredi', 'samedi'],
            'format' => 'j M Y'
        ]
    ];
    
    public function formatDate(DateTime $date, string $locale = 'en'): string {
        if (!isset($this->locales[$locale])) {
            throw new InvalidArgumentException("Unsupported locale: $locale");
        }
        
        $config = $this->locales[$locale];
        $monthNum = (int)$date->format('n') - 1;
        $dayNum = (int)$date->format('w');
        
        $formatted = $date->format($config['format']);
        
        // Replace English month names
        $formatted = str_replace($date->format('M'), $config['months'][$monthNum], $formatted);
        $formatted = str_replace($date->format('F'), $config['months'][$monthNum], $formatted);
        
        // Replace English day names
        $formatted = str_replace($date->format('D'), substr($config['days'][$dayNum], 0, 3), $formatted);
        $formatted = str_replace($date->format('l'), $config['days'][$dayNum], $formatted);
        
        return $formatted;
    }
    
    public function getRelativeTime(DateTime $date, string $locale = 'en'): string {
        $now = new DateTime();
        $diff = $now->diff($date);
        
        $translations = [
            'en' => ['ago' => 'ago', 'in' => 'in', 'now' => 'now'],
            'es' => ['ago' => 'hace', 'in' => 'en', 'now' => 'ahora'],
            'fr' => ['ago' => 'il y a', 'in' => 'dans', 'now' => 'maintenant']
        ];
        
        $t = $translations[$locale] ?? $translations['en'];
        
        if ($diff->days == 0 && $diff->h == 0 && $diff->i < 5) {
            return $t['now'];
        }
        
        $isPast = $date < $now;
        
        if ($diff->y > 0) {
            $unit = $diff->y . ($locale === 'en' ? ' year' . ($diff->y > 1 ? 's' : '') : ' años');
        } elseif ($diff->m > 0) {
            $unit = $diff->m . ($locale === 'en' ? ' month' . ($diff->m > 1 ? 's' : '') : ' meses');
        } elseif ($diff->d > 0) {
            $unit = $diff->d . ($locale === 'en' ? ' day' . ($diff->d > 1 ? 's' : '') : ' días');
        } else {
            $unit = $diff->h . ($locale === 'en' ? ' hour' . ($diff->h > 1 ? 's' : '') : ' horas');
        }
        
        return $isPast ? "$unit {$t['ago']}" : "{$t['in']} $unit";
    }
}

// Example usage
$localizer = new DateLocalizer();
$date = new DateTime('2024-03-15');

echo "Localized date formatting:\n";
foreach (['en', 'es', 'fr'] as $locale) {
    echo "  $locale: " . $localizer->formatDate($date, $locale) . "\n";
}

echo "\nRelative time examples:\n";
$relativeDates = [
    new DateTime('-2 hours'),
    new DateTime('-3 days'),
    new DateTime('+1 week')
];

foreach ($relativeDates as $relDate) {
    echo "  English: " . $localizer->getRelativeTime($relDate, 'en') . "\n";
    echo "  Spanish: " . $localizer->getRelativeTime($relDate, 'es') . "\n";
    echo "  French: " . $localizer->getRelativeTime($relDate, 'fr') . "\n\n";
}
```

Date localization adapts temporal information for different cultures and  
languages. This includes translated month/day names, appropriate date  
formats, and culturally relevant relative time expressions. Proper  
localization improves user experience in international applications.  

## Recurring events and schedules

Generate and manage recurring date patterns.  

```php
<?php

class RecurrencePattern {
    private DateTime $startDate;
    private ?DateTime $endDate;
    private string $frequency;
    private int $interval;
    private array $options;
    
    public function __construct(DateTime $startDate, string $frequency, int $interval = 1, array $options = []) {
        $this->startDate = clone $startDate;
        $this->frequency = $frequency;
        $this->interval = $interval;
        $this->options = $options;
        $this->endDate = isset($options['until']) ? new DateTime($options['until']) : null;
    }
    
    public function getOccurrences(int $limit = 10): array {
        $occurrences = [];
        $current = clone $this->startDate;
        $count = 0;
        
        while ($count < $limit && (!$this->endDate || $current <= $this->endDate)) {
            if ($this->matchesPattern($current)) {
                $occurrences[] = clone $current;
                $count++;
            }
            
            $current = $this->getNextCandidate($current);
        }
        
        return $occurrences;
    }
    
    private function getNextCandidate(DateTime $current): DateTime {
        $next = clone $current;
        
        switch ($this->frequency) {
            case 'daily':
                $next->add(new DateInterval("P{$this->interval}D"));
                break;
            case 'weekly':
                $next->add(new DateInterval("P{$this->interval}W"));
                break;
            case 'monthly':
                $next->add(new DateInterval("P{$this->interval}M"));
                break;
            case 'yearly':
                $next->add(new DateInterval("P{$this->interval}Y"));
                break;
            default:
                $next->add(new DateInterval('P1D'));
        }
        
        return $next;
    }
    
    private function matchesPattern(DateTime $date): bool {
        // Check day of week filter
        if (isset($this->options['weekdays'])) {
            $dayOfWeek = (int)$date->format('N');
            if (!in_array($dayOfWeek, $this->options['weekdays'])) {
                return false;
            }
        }
        
        // Check month filter
        if (isset($this->options['months'])) {
            $month = (int)$date->format('n');
            if (!in_array($month, $this->options['months'])) {
                return false;
            }
        }
        
        // Check day of month filter
        if (isset($this->options['monthdays'])) {
            $day = (int)$date->format('j');
            if (!in_array($day, $this->options['monthdays'])) {
                return false;
            }
        }
        
        return true;
    }
}

// Example usage
echo "Recurring event examples:\n\n";

// Weekly meeting every Tuesday and Thursday
$weeklyMeeting = new RecurrencePattern(
    new DateTime('2024-03-05'), // Tuesday
    'weekly',
    1,
    ['weekdays' => [2, 4]] // Tuesday and Thursday
);

echo "Weekly meetings (Tue/Thu):\n";
foreach ($weeklyMeeting->getOccurrences(6) as $occurrence) {
    echo "  " . $occurrence->format('Y-m-d l') . "\n";
}

echo "\nMonthly report (15th of each month):\n";
$monthlyReport = new RecurrencePattern(
    new DateTime('2024-01-15'),
    'monthly',
    1,
    ['monthdays' => [15]]
);

foreach ($monthlyReport->getOccurrences(6) as $occurrence) {
    echo "  " . $occurrence->format('Y-m-d (M Y)') . "\n";
}

echo "\nQuarterly review (last Friday of Mar/Jun/Sep/Dec):\n";
$quarterlyReview = new RecurrencePattern(
    new DateTime('2024-03-29'),
    'monthly',
    3,
    ['weekdays' => [5], 'months' => [3, 6, 9, 12]]
);

foreach ($quarterlyReview->getOccurrences(4) as $occurrence) {
    echo "  " . $occurrence->format('Y-m-d l (M Y)') . "\n";
}
```

Recurring patterns enable systematic event scheduling with complex rules.  
The pattern system handles frequency, intervals, and filters for weekdays,  
months, and specific dates. This approach scales from simple repetitions  
to sophisticated scheduling requirements.  

## Date range operations

Work with continuous date ranges and overlaps.  

```php
<?php

class DateRange {
    private DateTime $start;
    private DateTime $end;
    
    public function __construct(DateTime $start, DateTime $end) {
        if ($start > $end) {
            throw new InvalidArgumentException('Start date must be before end date');
        }
        $this->start = clone $start;
        $this->end = clone $end;
    }
    
    public function getStart(): DateTime {
        return clone $this->start;
    }
    
    public function getEnd(): DateTime {
        return clone $this->end;
    }
    
    public function getDuration(): DateInterval {
        return $this->start->diff($this->end);
    }
    
    public function contains(DateTime $date): bool {
        return $date >= $this->start && $date <= $this->end;
    }
    
    public function overlaps(DateRange $other): bool {
        return $this->start <= $other->end && $this->end >= $other->start;
    }
    
    public function getOverlap(DateRange $other): ?DateRange {
        if (!$this->overlaps($other)) {
            return null;
        }
        
        $overlapStart = max($this->start, $other->start);
        $overlapEnd = min($this->end, $other->end);
        
        return new DateRange($overlapStart, $overlapEnd);
    }
    
    public function merge(DateRange $other): ?DateRange {
        if (!$this->overlaps($other) && !$this->isAdjacent($other)) {
            return null;
        }
        
        $mergedStart = min($this->start, $other->start);
        $mergedEnd = max($this->end, $other->end);
        
        return new DateRange($mergedStart, $mergedEnd);
    }
    
    public function isAdjacent(DateRange $other): bool {
        $oneDayInterval = new DateInterval('P1D');
        
        $thisEndPlusOne = clone $this->end;
        $thisEndPlusOne->add($oneDayInterval);
        
        $otherEndPlusOne = clone $other->end;
        $otherEndPlusOne->add($oneDayInterval);
        
        return $thisEndPlusOne == $other->start || $otherEndPlusOne == $this->start;
    }
    
    public function split(DateTime $splitDate): array {
        if (!$this->contains($splitDate) || $splitDate == $this->start || $splitDate == $this->end) {
            return [$this];
        }
        
        $dayBefore = clone $splitDate;
        $dayBefore->sub(new DateInterval('P1D'));
        
        return [
            new DateRange($this->start, $dayBefore),
            new DateRange($splitDate, $this->end)
        ];
    }
    
    public function __toString(): string {
        return $this->start->format('Y-m-d') . ' to ' . $this->end->format('Y-m-d');
    }
}

// Example usage
echo "Date range operations:\n\n";

$range1 = new DateRange(new DateTime('2024-03-01'), new DateTime('2024-03-15'));
$range2 = new DateRange(new DateTime('2024-03-10'), new DateTime('2024-03-25'));
$range3 = new DateRange(new DateTime('2024-03-16'), new DateTime('2024-03-31'));

echo "Range 1: $range1\n";
echo "Range 2: $range2\n";
echo "Range 3: $range3\n\n";

// Test overlaps
echo "Overlap tests:\n";
echo "  Range 1 overlaps Range 2: " . ($range1->overlaps($range2) ? 'Yes' : 'No') . "\n";
echo "  Range 1 overlaps Range 3: " . ($range1->overlaps($range3) ? 'Yes' : 'No') . "\n";
echo "  Range 2 overlaps Range 3: " . ($range2->overlaps($range3) ? 'Yes' : 'No') . "\n";

// Get overlap period
$overlap = $range1->getOverlap($range2);
if ($overlap) {
    echo "  Overlap between Range 1 and 2: $overlap\n";
}

// Test adjacency and merging
echo "  Range 1 adjacent to Range 3: " . ($range1->isAdjacent($range3) ? 'Yes' : 'No') . "\n";

$merged = $range1->merge($range2);
if ($merged) {
    echo "  Merged Range 1 and 2: $merged\n";
}

// Test contains
$testDate = new DateTime('2024-03-12');
echo "  Range 1 contains {$testDate->format('Y-m-d')}: " . 
     ($range1->contains($testDate) ? 'Yes' : 'No') . "\n";

// Split range
$splitDate = new DateTime('2024-03-08');
$splitRanges = $range1->split($splitDate);
echo "  Split Range 1 at {$splitDate->format('Y-m-d')}:\n";
foreach ($splitRanges as $i => $splitRange) {
    echo "    Part " . ($i + 1) . ": $splitRange\n";
}
```

Date ranges represent continuous time periods with operations for overlap  
detection, merging, splitting, and containment checks. These operations  
are essential for scheduling systems, booking applications, and temporal  
data analysis where period relationships matter.  

## Date caching and optimization

Implement efficient date operations with caching.  

```php
<?php

class CachedDateCalculator {
    private array $cache = [];
    private array $stats = ['hits' => 0, 'misses' => 0];
    
    public function getCachedDiff(DateTime $start, DateTime $end): DateInterval {
        $key = $start->format('Y-m-d H:i:s') . '|' . $end->format('Y-m-d H:i:s');
        
        if (isset($this->cache['diffs'][$key])) {
            $this->stats['hits']++;
            return $this->cache['diffs'][$key];
        }
        
        $this->stats['misses']++;
        $diff = $start->diff($end);
        $this->cache['diffs'][$key] = $diff;
        
        return $diff;
    }
    
    public function getCachedBusinessDays(DateTime $start, DateTime $end): int {
        $key = $start->format('Y-m-d') . '|' . $end->format('Y-m-d');
        
        if (isset($this->cache['business_days'][$key])) {
            $this->stats['hits']++;
            return $this->cache['business_days'][$key];
        }
        
        $this->stats['misses']++;
        $count = $this->calculateBusinessDays($start, $end);
        $this->cache['business_days'][$key] = $count;
        
        return $count;
    }
    
    private function calculateBusinessDays(DateTime $start, DateTime $end): int {
        $current = clone $start;
        $count = 0;
        
        while ($current <= $end) {
            $dayOfWeek = (int)$current->format('N');
            if ($dayOfWeek <= 5) {
                $count++;
            }
            $current->add(new DateInterval('P1D'));
        }
        
        return $count;
    }
    
    public function getWeekdaysInMonth(int $year, int $month, int $weekday): array {
        $key = "$year|$month|$weekday";
        
        if (isset($this->cache['weekdays_in_month'][$key])) {
            $this->stats['hits']++;
            return $this->cache['weekdays_in_month'][$key];
        }
        
        $this->stats['misses']++;
        
        $firstDay = new DateTime("$year-$month-01");
        $lastDay = new DateTime($firstDay->format('Y-m-t'));
        
        $weekdays = [];
        $current = clone $firstDay;
        
        while ($current <= $lastDay) {
            if ((int)$current->format('N') === $weekday) {
                $weekdays[] = clone $current;
            }
            $current->add(new DateInterval('P1D'));
        }
        
        $this->cache['weekdays_in_month'][$key] = $weekdays;
        return $weekdays;
    }
    
    public function precomputeBusinessDays(int $year): void {
        for ($month = 1; $month <= 12; $month++) {
            $start = new DateTime("$year-$month-01");
            $end = new DateTime($start->format('Y-m-t'));
            $this->getCachedBusinessDays($start, $end);
        }
    }
    
    public function getStats(): array {
        $total = $this->stats['hits'] + $this->stats['misses'];
        $hitRate = $total > 0 ? ($this->stats['hits'] / $total) * 100 : 0;
        
        return [
            'hits' => $this->stats['hits'],
            'misses' => $this->stats['misses'],
            'hit_rate' => number_format($hitRate, 1) . '%',
            'cache_size' => count($this->cache, COUNT_RECURSIVE)
        ];
    }
    
    public function clearCache(): void {
        $this->cache = [];
        $this->stats = ['hits' => 0, 'misses' => 0];
    }
}

// Example usage with performance measurement
$calculator = new CachedDateCalculator();

echo "Cached date calculations:\n\n";

// Precompute business days for 2024
$startTime = microtime(true);
$calculator->precomputeBusinessDays(2024);
$precomputeTime = (microtime(true) - $startTime) * 1000;

echo "Precomputed business days for 2024 in " . number_format($precomputeTime, 2) . " ms\n\n";

// Test repeated calculations
$testPairs = [
    [new DateTime('2024-01-01'), new DateTime('2024-01-31')],
    [new DateTime('2024-02-01'), new DateTime('2024-02-29')],
    [new DateTime('2024-03-01'), new DateTime('2024-03-31')],
    [new DateTime('2024-01-01'), new DateTime('2024-01-31')], // Repeat
    [new DateTime('2024-02-01'), new DateTime('2024-02-29')], // Repeat
];

echo "Business days calculations (with caching):\n";
foreach ($testPairs as $i => [$start, $end]) {
    $startTime = microtime(true);
    $businessDays = $calculator->getCachedBusinessDays($start, $end);
    $calcTime = (microtime(true) - $startTime) * 1000;
    
    echo "  " . $start->format('M Y') . ": $businessDays business days " .
         "(" . number_format($calcTime, 3) . " ms)\n";
}

echo "\n";

// Get all Mondays in March 2024
$mondays = $calculator->getWeekdaysInMonth(2024, 3, 1);
echo "Mondays in March 2024:\n";
foreach ($mondays as $monday) {
    echo "  " . $monday->format('Y-m-d l') . "\n";
}

echo "\nCache statistics:\n";
$stats = $calculator->getStats();
foreach ($stats as $key => $value) {
    $label = ucfirst(str_replace('_', ' ', $key));
    echo "  $label: $value\n";
}
```

Caching frequently computed date operations significantly improves  
performance in applications with repetitive date calculations. Strategic  
precomputation and cache key design enable sub-millisecond response times  
for complex temporal queries while maintaining memory efficiency.  

## Error handling and edge cases

Handle exceptional scenarios in date operations gracefully.  

```php
<?php

class RobustDateHandler {
    private array $errors = [];
    
    public function safeCreateDate(string $dateString, ?string $timezone = null): ?DateTime {
        try {
            $tz = $timezone ? new DateTimeZone($timezone) : null;
            return new DateTime($dateString, $tz);
        } catch (Exception $e) {
            $this->errors[] = "Failed to create date from '$dateString': " . $e->getMessage();
            return null;
        }
    }
    
    public function safeFormatDate(?DateTime $date, string $format = 'Y-m-d'): string {
        if ($date === null) {
            return 'Invalid Date';
        }
        
        try {
            return $date->format($format);
        } catch (Exception $e) {
            $this->errors[] = "Failed to format date: " . $e->getMessage();
            return 'Format Error';
        }
    }
    
    public function validateDateRange(DateTime $start, DateTime $end): array {
        $issues = [];
        
        if ($start > $end) {
            $issues[] = 'Start date is after end date';
        }
        
        // Check for unrealistic ranges
        $diff = $start->diff($end);
        if ($diff->y > 100) {
            $issues[] = 'Date range exceeds 100 years - possible error';
        }
        
        // Check for same date
        if ($start->format('Y-m-d') === $end->format('Y-m-d')) {
            $issues[] = 'Start and end dates are the same';
        }
        
        // Check for weekend-only ranges
        $current = clone $start;
        $hasWeekday = false;
        $dayCount = 0;
        
        while ($current <= $end && $dayCount < 100) { // Limit check to prevent infinite loops
            $dayOfWeek = (int)$current->format('N');
            if ($dayOfWeek <= 5) {
                $hasWeekday = true;
                break;
            }
            $current->add(new DateInterval('P1D'));
            $dayCount++;
        }
        
        if (!$hasWeekday && $dayCount < 100) {
            $issues[] = 'Date range contains no weekdays';
        }
        
        return [
            'valid' => empty($issues),
            'issues' => $issues
        ];
    }
    
    public function handleTimezoneConversion(DateTime $date, string $targetTimezone): ?DateTime {
        try {
            $targetTz = new DateTimeZone($targetTimezone);
            $converted = clone $date;
            $converted->setTimezone($targetTz);
            return $converted;
        } catch (Exception $e) {
            $this->errors[] = "Timezone conversion failed to '$targetTimezone': " . $e->getMessage();
            return null;
        }
    }
    
    public function safeAddInterval(DateTime $date, string $intervalSpec): ?DateTime {
        try {
            $interval = new DateInterval($intervalSpec);
            $result = clone $date;
            $result->add($interval);
            
            // Check for overflow to year 10000+ (common DateTime limit)
            if ((int)$result->format('Y') > 9999) {
                $this->errors[] = "Date calculation resulted in year beyond 9999";
                return null;
            }
            
            return $result;
        } catch (Exception $e) {
            $this->errors[] = "Failed to add interval '$intervalSpec': " . $e->getMessage();
            return null;
        }
    }
    
    public function getLeapYearSafety(DateTime $date): array {
        $year = (int)$date->format('Y');
        $month = (int)$date->format('n');
        $day = (int)$date->format('j');
        
        $warnings = [];
        
        // Check for Feb 29 in non-leap years
        if ($month === 2 && $day === 29 && !$this->isLeapYear($year)) {
            $warnings[] = "February 29 in non-leap year $year";
        }
        
        // Check for Feb 29 date arithmetic that might cause issues
        if ($month === 2 && $day === 29) {
            $nextYear = $year + 1;
            if (!$this->isLeapYear($nextYear)) {
                $warnings[] = "Adding 1 year would result in invalid Feb 29";
            }
        }
        
        return [
            'is_leap_year' => $this->isLeapYear($year),
            'warnings' => $warnings
        ];
    }
    
    private function isLeapYear(int $year): bool {
        return $year % 4 === 0 && ($year % 100 !== 0 || $year % 400 === 0);
    }
    
    public function getErrors(): array {
        return $this->errors;
    }
    
    public function clearErrors(): void {
        $this->errors = [];
    }
}

// Example usage
$handler = new RobustDateHandler();

echo "Robust date handling examples:\n\n";

// Test safe date creation
$testDates = [
    '2024-03-15',           // Valid
    '2024-02-30',           // Invalid day
    '2024-13-01',           // Invalid month
    'not-a-date',           // Invalid format
    '2024-02-29'            // Valid leap year date
];

echo "Safe date creation:\n";
foreach ($testDates as $dateStr) {
    $date = $handler->safeCreateDate($dateStr);
    $formatted = $handler->safeFormatDate($date);
    echo "  '$dateStr' -> $formatted\n";
}

echo "\nErrors encountered:\n";
foreach ($handler->getErrors() as $error) {
    echo "  - $error\n";
}

$handler->clearErrors();

// Test date range validation
echo "\nDate range validation:\n";
$ranges = [
    [new DateTime('2024-03-01'), new DateTime('2024-03-31')], // Good
    [new DateTime('2024-03-31'), new DateTime('2024-03-01')], // Reversed
    [new DateTime('1900-01-01'), new DateTime('2024-01-01')], // Very long
    [new DateTime('2024-03-16'), new DateTime('2024-03-17')]  // Weekend only
];

foreach ($ranges as $i => [$start, $end]) {
    $validation = $handler->validateDateRange($start, $end);
    echo "  Range " . ($i + 1) . ": " . 
         ($validation['valid'] ? 'Valid' : 'Invalid') . "\n";
    
    if (!empty($validation['issues'])) {
        foreach ($validation['issues'] as $issue) {
            echo "    - $issue\n";
        }
    }
}

// Test leap year handling
echo "\nLeap year safety checks:\n";
$leapDates = [
    new DateTime('2024-02-29'), // Leap year
    new DateTime('2023-02-28'), // Non-leap year
    new DateTime('2000-02-29')  // Century leap year
];

foreach ($leapDates as $date) {
    $safety = $handler->getLeapYearSafety($date);
    echo "  " . $date->format('Y-m-d') . 
         " (Leap: " . ($safety['is_leap_year'] ? 'Yes' : 'No') . ")";
    
    if (!empty($safety['warnings'])) {
        echo " - Warnings: " . implode(', ', $safety['warnings']);
    }
    echo "\n";
}

// Test timezone conversion safety
echo "\nTimezone conversion safety:\n";
$date = new DateTime('2024-06-15 12:00:00');
$timezones = ['America/New_York', 'Invalid/Timezone', 'Europe/London'];

foreach ($timezones as $tz) {
    $converted = $handler->handleTimezoneConversion($date, $tz);
    if ($converted) {
        echo "  $tz: " . $converted->format('Y-m-d H:i:s T') . "\n";
    } else {
        echo "  $tz: Conversion failed\n";
    }
}

echo "\nFinal error summary:\n";
$finalErrors = $handler->getErrors();
if (empty($finalErrors)) {
    echo "No errors during safe operations\n";
} else {
    foreach ($finalErrors as $error) {
        echo "  - $error\n";
    }
}
```

Error handling in date operations prevents application crashes and provides  
meaningful feedback when edge cases occur. Validation checks catch common  
issues like invalid ranges, timezone problems, and leap year complications  
before they cause runtime errors.  

## Calendar and holiday calculations

Generate calendar data and handle regional holidays.  

```php
<?php

class CalendarGenerator {
    private array $holidays = [];
    private array $customHolidays = [];
    
    public function __construct() {
        // Initialize with some common holidays
        $this->holidays = [
            'fixed' => [
                '01-01' => 'New Year\'s Day',
                '07-04' => 'Independence Day (US)',
                '12-25' => 'Christmas Day'
            ],
            'calculated' => [
                'easter' => 'Easter Sunday',
                'mothers_day' => 'Mother\'s Day (US)',
                'memorial_day' => 'Memorial Day (US)',
                'labor_day' => 'Labor Day (US)',
                'thanksgiving' => 'Thanksgiving (US)'
            ]
        ];
    }
    
    public function generateMonth(int $year, int $month): array {
        $firstDay = new DateTime("$year-$month-01");
        $lastDay = new DateTime($firstDay->format('Y-m-t'));
        
        // Get first Monday of calendar (might be previous month)
        $calendarStart = clone $firstDay;
        $startDayOfWeek = (int)$firstDay->format('N');
        if ($startDayOfWeek > 1) {
            $calendarStart->sub(new DateInterval('P' . ($startDayOfWeek - 1) . 'D'));
        }
        
        // Get last Sunday of calendar (might be next month)
        $calendarEnd = clone $lastDay;
        $endDayOfWeek = (int)$lastDay->format('N');
        if ($endDayOfWeek < 7) {
            $calendarEnd->add(new DateInterval('P' . (7 - $endDayOfWeek) . 'D'));
        }
        
        $weeks = [];
        $current = clone $calendarStart;
        
        while ($current <= $calendarEnd) {
            $week = [];
            
            for ($i = 0; $i < 7; $i++) {
                $isCurrentMonth = (int)$current->format('n') === $month;
                $isToday = $current->format('Y-m-d') === (new DateTime())->format('Y-m-d');
                $isHoliday = $this->isHoliday($current);
                $isWeekend = (int)$current->format('N') > 5;
                
                $week[] = [
                    'date' => clone $current,
                    'day' => (int)$current->format('j'),
                    'is_current_month' => $isCurrentMonth,
                    'is_today' => $isToday,
                    'is_holiday' => $isHoliday,
                    'holiday_name' => $isHoliday ? $this->getHolidayName($current) : null,
                    'is_weekend' => $isWeekend,
                    'css_classes' => $this->getCssClasses($isCurrentMonth, $isToday, $isHoliday, $isWeekend)
                ];
                
                $current->add(new DateInterval('P1D'));
            }
            
            $weeks[] = $week;
        }
        
        return [
            'year' => $year,
            'month' => $month,
            'month_name' => $firstDay->format('F'),
            'first_day' => $firstDay,
            'last_day' => $lastDay,
            'weeks' => $weeks
        ];
    }
    
    private function isHoliday(DateTime $date): bool {
        $monthDay = $date->format('m-d');
        $year = (int)$date->format('Y');
        
        // Check fixed holidays
        if (isset($this->holidays['fixed'][$monthDay])) {
            return true;
        }
        
        // Check calculated holidays
        $calculatedHolidays = $this->getCalculatedHolidays($year);
        $dateString = $date->format('Y-m-d');
        
        return isset($calculatedHolidays[$dateString]);
    }
    
    private function getHolidayName(DateTime $date): ?string {
        $monthDay = $date->format('m-d');
        $year = (int)$date->format('Y');
        
        // Check fixed holidays
        if (isset($this->holidays['fixed'][$monthDay])) {
            return $this->holidays['fixed'][$monthDay];
        }
        
        // Check calculated holidays
        $calculatedHolidays = $this->getCalculatedHolidays($year);
        $dateString = $date->format('Y-m-d');
        
        return $calculatedHolidays[$dateString] ?? null;
    }
    
    private function getCalculatedHolidays(int $year): array {
        $holidays = [];
        
        // Easter (Western)
        if (function_exists('easter_date')) {
            $easter = new DateTime();
            $easter->setTimestamp(easter_date($year));
            $holidays[$easter->format('Y-m-d')] = 'Easter Sunday';
        }
        
        // Mother's Day (US) - Second Sunday in May
        $mothersDay = new DateTime("$year-05-01");
        $mothersDay->modify('second sunday');
        $holidays[$mothersDay->format('Y-m-d')] = 'Mother\'s Day';
        
        // Memorial Day (US) - Last Monday in May
        $memorialDay = new DateTime("$year-05-31");
        $memorialDay->modify('last monday');
        $holidays[$memorialDay->format('Y-m-d')] = 'Memorial Day';
        
        // Labor Day (US) - First Monday in September
        $laborDay = new DateTime("$year-09-01");
        $laborDay->modify('first monday');
        $holidays[$laborDay->format('Y-m-d')] = 'Labor Day';
        
        // Thanksgiving (US) - Fourth Thursday in November
        $thanksgiving = new DateTime("$year-11-01");
        $thanksgiving->modify('fourth thursday');
        $holidays[$thanksgiving->format('Y-m-d')] = 'Thanksgiving';
        
        return array_merge($holidays, $this->customHolidays);
    }
    
    private function getCssClasses(bool $isCurrentMonth, bool $isToday, bool $isHoliday, bool $isWeekend): array {
        $classes = [];
        
        if (!$isCurrentMonth) $classes[] = 'other-month';
        if ($isToday) $classes[] = 'today';
        if ($isHoliday) $classes[] = 'holiday';
        if ($isWeekend) $classes[] = 'weekend';
        
        return $classes;
    }
    
    public function addCustomHoliday(string $date, string $name): void {
        $this->customHolidays[$date] = $name;
    }
    
    public function renderCalendarHtml(array $calendar): string {
        $html = "<div class='calendar'>\n";
        $html .= "<h2>{$calendar['month_name']} {$calendar['year']}</h2>\n";
        $html .= "<table class='calendar-table'>\n";
        
        // Header
        $html .= "<thead><tr>\n";
        $dayNames = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'];
        foreach ($dayNames as $dayName) {
            $html .= "<th>$dayName</th>\n";
        }
        $html .= "</tr></thead>\n";
        
        // Body
        $html .= "<tbody>\n";
        foreach ($calendar['weeks'] as $week) {
            $html .= "<tr>\n";
            foreach ($week as $day) {
                $classes = implode(' ', $day['css_classes']);
                $title = $day['holiday_name'] ? "title=\"{$day['holiday_name']}\"" : '';
                $html .= "<td class='$classes' $title>{$day['day']}</td>\n";
            }
            $html .= "</tr>\n";
        }
        $html .= "</tbody>\n";
        $html .= "</table>\n";
        $html .= "</div>\n";
        
        return $html;
    }
}

// Example usage
$generator = new CalendarGenerator();

echo "Calendar generation examples:\n\n";

// Add custom holiday
$generator->addCustomHoliday('2024-03-15', 'Custom Company Day');

// Generate March 2024 calendar
$calendar = $generator->generateMonth(2024, 3);

echo "March 2024 Calendar Overview:\n";
echo "Month: {$calendar['month_name']} {$calendar['year']}\n";
echo "First day: " . $calendar['first_day']->format('Y-m-d l') . "\n";
echo "Last day: " . $calendar['last_day']->format('Y-m-d l') . "\n";
echo "Number of weeks: " . count($calendar['weeks']) . "\n\n";

echo "Sample week data (first week):\n";
foreach ($calendar['weeks'][0] as $day) {
    $status = [];
    if (!$day['is_current_month']) $status[] = 'other month';
    if ($day['is_today']) $status[] = 'today';
    if ($day['is_holiday']) $status[] = 'holiday: ' . $day['holiday_name'];
    if ($day['is_weekend']) $status[] = 'weekend';
    
    $statusText = empty($status) ? 'regular day' : implode(', ', $status);
    echo "  " . $day['date']->format('Y-m-d') . " ({$day['day']}): $statusText\n";
}

echo "\nHolidays in March 2024:\n";
foreach ($calendar['weeks'] as $week) {
    foreach ($week as $day) {
        if ($day['is_holiday'] && $day['is_current_month']) {
            echo "  " . $day['date']->format('Y-m-d l') . ": {$day['holiday_name']}\n";
        }
    }
}

// Generate simple text calendar
echo "\nSimple text calendar for March 2024:\n";
echo "Mo Tu We Th Fr Sa Su\n";
foreach ($calendar['weeks'] as $week) {
    foreach ($week as $day) {
        $dayNum = str_pad((string)$day['day'], 2, ' ', STR_PAD_LEFT);
        if (!$day['is_current_month']) {
            echo "\033[90m$dayNum\033[0m "; // Gray for other months
        } elseif ($day['is_today']) {
            echo "\033[93m$dayNum\033[0m "; // Yellow for today
        } elseif ($day['is_holiday']) {
            echo "\033[91m$dayNum\033[0m "; // Red for holidays
        } else {
            echo "$dayNum ";
        }
    }
    echo "\n";
}
```

Calendar generation creates structured representations of months with  
comprehensive metadata for each day. Holiday calculations handle both  
fixed dates and complex rules like "fourth Thursday in November".  
This system supports various display formats and cultural variations.    