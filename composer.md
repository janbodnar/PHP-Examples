# Composer

This chapter covers 25 comprehensive Composer examples demonstrating  
dependency management, package creation, autoloading, and advanced  
configuration in PHP projects. These examples progress from basic  
package management to sophisticated Composer usage with modern PHP 8.4  
features. Each example includes clear explanations to help you master  
PHP's de facto dependency manager.  

## Initializing a new project

Creating a new Composer project with interactive setup.  

```php
<?php

// Create a new project directory and initialize composer.json
// Run: composer init

// Example generated composer.json
$composerJson = [
    "name" => "vendor/project-name",
    "description" => "A sample PHP project using Composer",
    "type" => "project",
    "require" => [
        "php" => ">=8.4"
    ],
    "require-dev" => [],
    "autoload" => [
        "psr-4" => [
            "App\\" => "src/"
        ]
    ],
    "authors" => [
        [
            "name" => "John Doe",
            "email" => "john@example.com"
        ]
    ],
    "minimum-stability" => "stable"
];

echo "Generated composer.json structure:\n";
echo json_encode($composerJson, JSON_PRETTY_PRINT) . "\n";
```

Composer init creates an interactive setup process for new projects.  
The generated composer.json defines project metadata, dependencies,  
autoloading rules, and author information. PSR-4 autoloading maps  
namespaces to directory structures for automatic class loading.  

## Installing dependencies

Adding and managing project dependencies with version constraints.  

```php
<?php

// Example composer.json with various dependencies
$dependencies = [
    "require" => [
        "php" => ">=8.4",
        "monolog/monolog" => "^3.0",
        "guzzlehttp/guzzle" => "^7.8",
        "symfony/console" => "~6.4",
        "twig/twig" => "3.*"
    ],
    "require-dev" => [
        "phpunit/phpunit" => "^10.0",
        "friendsofphp/php-cs-fixer" => "^3.40",
        "phpstan/phpstan" => "^1.10"
    ]
];

// Commands to install dependencies:
// composer require monolog/monolog
// composer require --dev phpunit/phpunit
// composer install (installs all dependencies)
// composer update (updates to latest compatible versions)

echo "Dependency structure:\n";
echo json_encode($dependencies, JSON_PRETTY_PRINT) . "\n";

// Example usage after installation
if (file_exists('vendor/autoload.php')) {
    require_once 'vendor/autoload.php';
    
    $logger = new \Monolog\Logger('app');
    echo "Monolog logger created successfully\n";
}
```

Version constraints control which package versions Composer installs.  
Use `^` for compatible updates, `~` for patch-level changes, and `*`  
for wildcard matching. Development dependencies are installed only  
in development environments, not production deployments.  

## PSR-4 autoloading

Setting up modern class autoloading with namespace mapping.  

```php
<?php

// composer.json autoload configuration
$autoloadConfig = [
    "autoload" => [
        "psr-4" => [
            "App\\" => "src/",
            "App\\Controllers\\" => "src/Controllers/",
            "App\\Models\\" => "src/Models/",
            "Utilities\\" => "lib/utilities/"
        ]
    ],
    "autoload-dev" => [
        "psr-4" => [
            "Tests\\" => "tests/"
        ]
    ]
];

echo "Autoload configuration:\n";
echo json_encode($autoloadConfig, JSON_PRETTY_PRINT) . "\n";

// Example class structure following PSR-4
// File: src/Controllers/UserController.php
class_alias('stdClass', 'App\\Controllers\\UserController', false);

// File: src/Models/User.php  
class_alias('stdClass', 'App\\Models\\User', false);

// After running: composer dump-autoload
if (file_exists('vendor/autoload.php')) {
    require_once 'vendor/autoload.php';
    
    // Classes are automatically loaded when referenced
    echo "PSR-4 autoloading configured for namespace mapping\n";
}
```

PSR-4 autoloading maps fully qualified class names to file paths  
automatically. The namespace prefix corresponds to the base directory,  
making class organization intuitive. Run `composer dump-autoload`  
after modifying autoload configuration to regenerate the class map.  

## Classmap autoloading

Using classmap for legacy code and non-PSR-4 compliant structures.  

```php
<?php

// composer.json with classmap autoloading
$classmapConfig = [
    "autoload" => [
        "classmap" => [
            "legacy/",
            "src/Helpers",
            "lib/third-party/OldLibrary.php"
        ],
        "files" => [
            "bootstrap/helpers.php",
            "config/constants.php"
        ]
    ]
];

echo "Classmap autoload configuration:\n";
echo json_encode($classmapConfig, JSON_PRETTY_PRINT) . "\n";

// Create example helper functions file
$helpersFile = <<<'PHP'
<?php

function app_helper($message) {
    return "Helper: " . $message;
}

function debug_info($data) {
    return json_encode($data, JSON_PRETTY_PRINT);
}

if (!defined('APP_VERSION')) {
    define('APP_VERSION', '1.0.0');
}
PHP;

echo "\nExample helpers.php content:\n";
echo $helpersFile . "\n";

// After composer dump-autoload, these are available globally
echo "\nAfter autoloading, helper functions and constants are available.\n";
```

Classmap autoloading scans directories and files to build a class map  
for legacy code that doesn't follow PSR-4. Files autoloading includes  
PHP files on every request, useful for helper functions and global  
constants that need immediate availability.  

## Version constraints and updates

Managing dependency versions with semantic versioning rules.  

```php
<?php

$versionExamples = [
    "exact_version" => "1.4.2",
    "caret_constraint" => "^1.4.2",  // >=1.4.2 <2.0.0
    "tilde_constraint" => "~1.4.2",  // >=1.4.2 <1.5.0
    "wildcard" => "1.4.*",           // >=1.4.0 <1.5.0
    "range" => ">=1.0 <2.0",         // Between 1.0 and 2.0
    "multiple" => "^1.4 || ^2.0",    // Either 1.4+ or 2.0+
    "stability" => "1.0.0-alpha",    // Pre-release version
    "dev_branch" => "dev-main"       // Development branch
];

echo "Version constraint examples:\n";
foreach ($versionExamples as $type => $constraint) {
    echo sprintf("%-20s: %s\n", $type, $constraint);
}

// Example composer.json with mixed constraints
$packageConstraints = [
    "require" => [
        "php" => ">=8.4",
        "symfony/console" => "^6.4",
        "monolog/monolog" => "~3.0",
        "guzzlehttp/guzzle" => "7.*",
        "doctrine/orm" => ">=2.10 <3.0"
    ]
];

echo "\nPractical constraint usage:\n";
echo json_encode($packageConstraints, JSON_PRETTY_PRINT) . "\n";

// Update strategies
$updateCommands = [
    "composer update" => "Update all packages to latest compatible",
    "composer update vendor/package" => "Update specific package only",
    "composer require vendor/package:^2.0" => "Install/update with constraint",
    "composer show -o" => "Show outdated packages"
];

echo "\nUpdate commands:\n";
foreach ($updateCommands as $command => $description) {
    echo "$command - $description\n";
}
```

Version constraints use semantic versioning to control package updates.  
Caret (^) allows compatible updates, tilde (~) permits patch updates,  
and exact versions lock to specific releases. Choose constraints that  
balance stability with security updates and new features.  

## Development dependencies

Separating development tools from production requirements.  

```php
<?php

$developmentConfig = [
    "require" => [
        "php" => ">=8.4",
        "symfony/console" => "^6.4",
        "monolog/monolog" => "^3.0"
    ],
    "require-dev" => [
        "phpunit/phpunit" => "^10.0",
        "friendsofphp/php-cs-fixer" => "^3.40",
        "phpstan/phpstan" => "^1.10",
        "vimeo/psalm" => "^5.15",
        "phpmd/phpmd" => "^2.13",
        "squizlabs/php_codesniffer" => "^3.7"
    ],
    "scripts" => [
        "test" => "phpunit",
        "cs-fix" => "php-cs-fixer fix",
        "analyse" => "phpstan analyse",
        "psalm" => "psalm --show-info=true",
        "quality" => [
            "@cs-fix",
            "@analyse", 
            "@psalm",
            "@test"
        ]
    ]
];

echo "Development vs Production dependencies:\n";
echo json_encode($developmentConfig, JSON_PRETTY_PRINT) . "\n";

// Installation commands for different environments
$installCommands = [
    "Development" => "composer install",
    "Production" => "composer install --no-dev --optimize-autoloader",
    "CI/CD" => "composer install --no-dev --no-scripts --no-interaction"
];

echo "\nEnvironment-specific installation:\n";
foreach ($installCommands as $env => $command) {
    echo "$env: $command\n";
}

// Example CI script integration
$ciScript = <<<'BASH'
#!/bin/bash
composer install --no-dev --optimize-autoloader
composer run-script quality
BASH;

echo "\nExample CI integration:\n$ciScript\n";
```

Development dependencies include testing frameworks, code quality tools,  
and development utilities that aren't needed in production. Use  
`--no-dev` flag for production installations to reduce package size  
and avoid security risks from development-only packages.  

## Scripts and hooks

Automating tasks with Composer scripts and lifecycle hooks.  

```php
<?php

$scriptsConfig = [
    "scripts" => [
        "pre-install-cmd" => [
            "echo 'Preparing installation...'"
        ],
        "post-install-cmd" => [
            "php artisan clear:cache",
            "@optimize"
        ],
        "pre-update-cmd" => "echo 'Starting update process'",
        "post-update-cmd" => [
            "@optimize",
            "php artisan migrate --force"
        ],
        "optimize" => [
            "composer dump-autoload --optimize",
            "php artisan config:cache",
            "php artisan route:cache"
        ],
        "test" => "vendor/bin/phpunit",
        "test-coverage" => "vendor/bin/phpunit --coverage-html coverage",
        "cs-fix" => "vendor/bin/php-cs-fixer fix --config=.php-cs-fixer.php",
        "analyse" => [
            "vendor/bin/phpstan analyse --memory-limit=1G",
            "vendor/bin/psalm --show-info=false"
        ],
        "security" => "composer audit",
        "fresh-install" => [
            "rm -rf vendor",
            "rm composer.lock", 
            "composer install"
        ]
    ],
    "scripts-descriptions" => [
        "optimize" => "Optimize application for production",
        "test-coverage" => "Run tests with coverage report",
        "fresh-install" => "Clean install of all dependencies"
    ]
];

echo "Composer scripts configuration:\n";
echo json_encode($scriptsConfig, JSON_PRETTY_PRINT) . "\n";

// Running scripts
$scriptCommands = [
    "composer run-script test" => "Run the test script",
    "composer run-script analyse" => "Run static analysis",
    "composer optimize" => "Run optimize script (shorthand)",
    "composer run-script --list" => "List all available scripts"
];

echo "\nScript execution commands:\n";
foreach ($scriptCommands as $command => $description) {
    echo "$command - $description\n";
}
```

Composer scripts automate common development tasks and can be triggered  
by lifecycle events. Use hooks like `post-install-cmd` for automatic  
optimization and `pre-update-cmd` for backup procedures. Scripts  
support both string commands and arrays of multiple commands.  

## Platform requirements

Specifying PHP version and extension requirements.  

```php
<?php

$platformConfig = [
    "require" => [
        "php" => ">=8.4",
        "ext-mbstring" => "*",
        "ext-pdo" => "*",
        "ext-pdo_mysql" => "*",
        "ext-json" => "*",
        "ext-curl" => "*",
        "ext-openssl" => "*",
        "ext-zip" => "*",
        "ext-gd" => "*"
    ],
    "config" => [
        "platform" => [
            "php" => "8.4.0"
        ]
    ]
];

echo "Platform requirements configuration:\n";
echo json_encode($platformConfig, JSON_PRETTY_PRINT) . "\n";

// Check current platform capabilities
$platformInfo = [
    "PHP Version" => PHP_VERSION,
    "Extensions" => get_loaded_extensions(),
    "Memory Limit" => ini_get('memory_limit'),
    "Max Execution Time" => ini_get('max_execution_time')
];

echo "\nCurrent platform information:\n";
echo "PHP Version: " . $platformInfo["PHP Version"] . "\n";
echo "Loaded Extensions: " . count($platformInfo["Extensions"]) . "\n";
echo "Memory Limit: " . $platformInfo["Memory Limit"] . "\n";

// Example platform check script
function checkPlatformRequirements($requirements) {
    $missing = [];
    
    foreach ($requirements as $req => $version) {
        if (strpos($req, 'ext-') === 0) {
            $extension = substr($req, 4);
            if (!extension_loaded($extension)) {
                $missing[] = $extension;
            }
        }
    }
    
    return $missing;
}

$missing = checkPlatformRequirements($platformConfig['require']);
if (empty($missing)) {
    echo "\nAll platform requirements are satisfied.\n";
} else {
    echo "\nMissing extensions: " . implode(', ', $missing) . "\n";
}
```

Platform requirements ensure your package works only on systems with  
necessary PHP versions and extensions. Use `ext-*` notation for  
extensions and specific PHP version constraints. Platform configuration  
can override detected versions for testing compatibility.  

## Working with repositories

Configuring custom package sources and repositories.  

```php
<?php

$repositoryConfig = [
    "repositories" => [
        [
            "type" => "vcs",
            "url" => "https://github.com/vendor/private-package"
        ],
        [
            "type" => "path",
            "url" => "../local-packages/*"
        ],
        [
            "type" => "composer",
            "url" => "https://packages.example.com"
        ],
        [
            "type" => "package",
            "package" => [
                "name" => "custom/legacy-lib",
                "version" => "1.0.0",
                "source" => [
                    "url" => "https://github.com/legacy/lib",
                    "type" => "git",
                    "reference" => "main"
                ],
                "autoload" => [
                    "classmap" => ["src/"]
                ]
            ]
        ]
    ],
    "minimum-stability" => "dev",
    "prefer-stable" => true
];

echo "Repository configuration:\n";
echo json_encode($repositoryConfig, JSON_PRETTY_PRINT) . "\n";

// Authentication for private repositories
$authConfig = [
    "config" => [
        "github-oauth" => [
            "github.com" => "your-github-token"
        ],
        "gitlab-token" => [
            "gitlab.com" => "your-gitlab-token"
        ],
        "http-basic" => [
            "packages.example.com" => [
                "username" => "user",
                "password" => "pass"
            ]
        ]
    ]
];

echo "\nAuthentication configuration (store in composer.json or auth.json):\n";
echo json_encode($authConfig, JSON_PRETTY_PRINT) . "\n";

// Repository priority and disable packagist
$advancedRepoConfig = [
    "repositories" => [
        [
            "type" => "composer",
            "url" => "https://private-repo.com",
            "priority" => 100
        ],
        [
            "packagist.org" => false  // Disable default Packagist
        ]
    ]
];

echo "\nAdvanced repository configuration:\n";
echo json_encode($advancedRepoConfig, JSON_PRETTY_PRINT) . "\n";
```

Repositories define where Composer searches for packages beyond the  
default Packagist. Use VCS repositories for Git/SVN, path repositories  
for local development, and package repositories for custom definitions.  
Authentication enables access to private repositories securely.  

## Creating installable packages

Packaging your code for distribution via Composer.  

```php
<?php

// Example package composer.json
$packageConfig = [
    "name" => "mycompany/awesome-library",
    "description" => "An awesome PHP library for amazing things",
    "type" => "library",
    "keywords" => ["php", "library", "awesome", "utilities"],
    "homepage" => "https://github.com/mycompany/awesome-library",
    "license" => "MIT",
    "authors" => [
        [
            "name" => "Jane Developer",
            "email" => "jane@mycompany.com",
            "homepage" => "https://github.com/janedeveloper",
            "role" => "Developer"
        ]
    ],
    "require" => [
        "php" => ">=8.4"
    ],
    "require-dev" => [
        "phpunit/phpunit" => "^10.0"
    ],
    "autoload" => [
        "psr-4" => [
            "MyCompany\\AwesomeLibrary\\" => "src/"
        ]
    ],
    "autoload-dev" => [
        "psr-4" => [
            "MyCompany\\AwesomeLibrary\\Tests\\" => "tests/"
        ]
    ],
    "extra" => [
        "branch-alias" => [
            "dev-main" => "1.x-dev"
        ]
    ],
    "config" => [
        "sort-packages" => true,
        "optimize-autoloader" => true
    ]
];

echo "Package composer.json structure:\n";
echo json_encode($packageConfig, JSON_PRETTY_PRINT) . "\n";

// Example library code structure
$libraryExample = <<<'PHP'
<?php
// src/Calculator.php

namespace MyCompany\AwesomeLibrary;

class Calculator
{
    public function add(int $a, int $b): int
    {
        return $a + $b;
    }
    
    public function multiply(float $a, float $b): float
    {
        return $a * $b;
    }
}
PHP;

echo "\nExample library code:\n$libraryExample\n";

// Publishing checklist
$publishingSteps = [
    "1. Create comprehensive README.md",
    "2. Add LICENSE file (MIT, Apache, GPL, etc.)",
    "3. Set up proper directory structure",
    "4. Write unit tests with PHPUnit",
    "5. Configure CI/CD pipeline",
    "6. Tag stable releases with semantic versioning",
    "7. Submit to Packagist.org",
    "8. Monitor package statistics and issues"
];

echo "\nPackage publishing checklist:\n";
foreach ($publishingSteps as $step) {
    echo "- $step\n";
}
```

Creating installable packages requires proper composer.json metadata,  
PSR-4 autoloading, semantic versioning, and comprehensive documentation.  
Submit packages to Packagist for public distribution or use private  
repositories for internal packages within organizations.  

## Lock files and reproducible builds

Understanding composer.lock for consistent dependency versions.  

```php
<?php

// Example composer.lock structure (simplified)
$lockFileStructure = [
    "_readme" => [
        "This file locks the dependencies to specific versions",
        "It should be committed to version control",
        "Run composer install to install exact versions from lock file"
    ],
    "content-hash" => "abc123def456",
    "packages" => [
        [
            "name" => "monolog/monolog",
            "version" => "3.0.0",
            "source" => [
                "type" => "git",
                "url" => "https://github.com/Seldaek/monolog.git",
                "reference" => "fc9aaaa"
            ],
            "require" => [
                "php" => ">=8.1",
                "psr/log" => "^1.1.4 || ^2.0 || ^3.0"
            ],
            "time" => "2023-07-28T09:28:21+00:00"
        ]
    ],
    "packages-dev" => [],
    "aliases" => [],
    "minimum-stability" => "stable",
    "platform" => [
        "php" => "8.4.0"
    ]
];

echo "Lock file structure overview:\n";
echo json_encode(array_slice($lockFileStructure, 0, 3), JSON_PRETTY_PRINT) . "\n";

// Lock file best practices
$lockFilePractices = [
    "Version Control" => [
        "Always commit composer.lock to repository",
        "Never edit composer.lock manually",
        "Include lock file in deployment packages"
    ],
    "Development Workflow" => [
        "Use 'composer install' in production/CI",
        "Use 'composer update' only when updating deps",
        "Review lock file changes in pull requests"
    ],
    "Team Coordination" => [
        "All team members use same locked versions",
        "Prevents 'works on my machine' issues",
        "Ensures consistent test environments"
    ]
];

echo "\nLock file best practices:\n";
foreach ($lockFilePractices as $category => $practices) {
    echo "\n$category:\n";
    foreach ($practices as $practice) {
        echo "- $practice\n";
    }
}

// Commands for lock file management
$lockCommands = [
    "composer install" => "Install from lock file (production)",
    "composer update" => "Update deps and regenerate lock",
    "composer update vendor/package" => "Update specific package only", 
    "composer install --no-dev" => "Production install without dev deps",
    "composer check-platform-reqs" => "Verify platform requirements"
];

echo "\nLock file commands:\n";
foreach ($lockCommands as $command => $description) {
    echo "$command - $description\n";
}
```

The composer.lock file pins exact dependency versions for reproducible  
builds across environments. It should be committed to version control  
to ensure all team members and deployment environments use identical  
package versions, preventing environment-specific bugs.  

## Configuration and optimization

Optimizing Composer performance and behavior settings.  

```php
<?php

$optimizationConfig = [
    "config" => [
        "optimize-autoloader" => true,
        "classmap-authoritative" => true,
        "apcu-autoloader" => true,
        "sort-packages" => true,
        "cache-dir" => "/tmp/composer-cache",
        "vendor-dir" => "vendor",
        "bin-dir" => "vendor/bin",
        "process-timeout" => 300,
        "use-include-path" => false,
        "preferred-install" => [
            "*" => "dist"
        ],
        "github-protocols" => ["https", "ssh"],
        "platform-check" => true
    ],
    "extra" => [
        "optimize-autoloader" => true,
        "merge-plugin" => [
            "include" => [
                "packages/*/composer.json"
            ],
            "recurse" => true,
            "replace" => false
        ]
    ]
];

echo "Optimization configuration:\n";
echo json_encode($optimizationConfig, JSON_PRETTY_PRINT) . "\n";

// Performance optimization commands
$optimizationCommands = [
    "Production Deploy" => [
        "composer install --no-dev --optimize-autoloader --classmap-authoritative",
        "Enable APCu for autoloader caching",
        "Use --no-scripts if scripts aren't needed"
    ],
    "Development" => [
        "composer dump-autoload --optimize",
        "Enable Xdebug only when debugging", 
        "Use local cache directory"
    ],
    "CI/CD" => [
        "composer install --no-dev --no-interaction --optimize-autoloader",
        "Cache vendor directory between builds",
        "Use parallel installation if supported"
    ]
];

echo "\nOptimization strategies by environment:\n";
foreach ($optimizationCommands as $env => $commands) {
    echo "\n$env:\n";
    foreach ($commands as $command) {
        echo "- $command\n";
    }
}

// Memory and performance monitoring
$performanceMetrics = [
    "autoloader_generation" => "Time to generate optimized autoloader",
    "memory_usage" => "Peak memory usage during operations", 
    "cache_hits" => "Package cache hit rate",
    "download_time" => "Network time for package downloads",
    "install_time" => "Total installation time"
];

echo "\nPerformance metrics to monitor:\n";
foreach ($performanceMetrics as $metric => $description) {
    echo sprintf("%-25s: %s\n", str_replace('_', ' ', $metric), $description);
}
```

Configuration optimization improves Composer performance through  
autoloader caching, classmap generation, and APCu integration.  
Use `--optimize-autoloader` and `--classmap-authoritative` in  
production for faster class loading and reduced file system overhead.  

## Security and auditing

Managing security vulnerabilities in dependencies.  

```php
<?php

// Security configuration
$securityConfig = [
    "config" => [
        "secure-http" => true,
        "github-protocols" => ["https"],
        "platform-check" => true,
        "audit" => [
            "abandoned" => "report"
        ]
    ],
    "require" => [
        "php" => ">=8.4",
        "roave/security-advisories" => "dev-latest"  // Prevents vulnerable packages
    ]
];

echo "Security configuration:\n";
echo json_encode($securityConfig, JSON_PRETTY_PRINT) . "\n";

// Security audit commands
$securityCommands = [
    "composer audit" => "Check for known vulnerabilities",
    "composer audit --format=json" => "Machine-readable audit report",
    "composer show --outdated" => "List outdated packages",
    "composer licenses" => "Show package licenses",
    "composer fund" => "Show packages seeking funding"
];

echo "\nSecurity audit commands:\n";
foreach ($securityCommands as $command => $description) {
    echo "$command - $description\n";
}

// Example vulnerability report structure
$vulnerabilityReport = [
    "advisories" => [
        [
            "package" => "symfony/http-kernel",
            "version" => "5.4.0",
            "advisory" => "CVE-2023-46734",
            "severity" => "high",
            "description" => "HTTP Host Header Injection",
            "fix" => "Update to 5.4.31 or 6.3.8"
        ]
    ],
    "summary" => [
        "total_packages" => 45,
        "vulnerable_packages" => 1,
        "security_advisories" => 1
    ]
];

echo "\nExample vulnerability report:\n";
echo json_encode($vulnerabilityReport, JSON_PRETTY_PRINT) . "\n";

// Security best practices
$securityPractices = [
    "Regular Updates" => [
        "Run composer audit regularly",
        "Subscribe to security advisories",
        "Update packages promptly"
    ],
    "Dependency Management" => [
        "Minimize dependency count",
        "Review package maintenance status",
        "Avoid abandoned packages"
    ],
    "Development" => [
        "Use roave/security-advisories",
        "Enable platform-check in production",
        "Audit licenses for compliance"
    ]
];

echo "\nSecurity best practices:\n";
foreach ($securityPractices as $category => $practices) {
    echo "\n$category:\n";
    foreach ($practices as $practice) {
        echo "- $practice\n";
    }
}
```

Security auditing identifies vulnerable dependencies and provides  
upgrade paths for fixes. Use `composer audit` regularly, integrate  
roave/security-advisories to prevent vulnerable package installation,  
and maintain up-to-date dependencies to minimize security exposure.  

## Monorepo and path repositories

Managing multiple packages in a single repository.  

```php
<?php

// Monorepo structure with path repositories
$monorepoConfig = [
    "name" => "company/monorepo",
    "type" => "project",
    "repositories" => [
        [
            "type" => "path",
            "url" => "./packages/core",
            "options" => [
                "symlink" => true
            ]
        ],
        [
            "type" => "path", 
            "url" => "./packages/api",
            "options" => [
                "symlink" => true
            ]
        ],
        [
            "type" => "path",
            "url" => "./packages/*"
        ]
    ],
    "require" => [
        "php" => ">=8.4",
        "company/core" => "@dev",
        "company/api" => "@dev"
    ],
    "autoload" => [
        "psr-4" => [
            "Company\\Common\\" => "src/"
        ]
    ]
];

echo "Monorepo configuration:\n";
echo json_encode($monorepoConfig, JSON_PRETTY_PRINT) . "\n";

// Individual package configuration
$packageConfig = [
    "name" => "company/core",
    "type" => "library",
    "require" => [
        "php" => ">=8.4"
    ],
    "autoload" => [
        "psr-4" => [
            "Company\\Core\\" => "src/"
        ]
    ]
];

echo "\nIndividual package composer.json:\n";
echo json_encode($packageConfig, JSON_PRETTY_PRINT) . "\n";

// Directory structure example
$directoryStructure = [
    "monorepo/" => [
        "composer.json",
        "packages/" => [
            "core/" => [
                "composer.json",
                "src/",
                "tests/"
            ],
            "api/" => [
                "composer.json", 
                "src/",
                "tests/"
            ],
            "utils/" => [
                "composer.json",
                "src/"
            ]
        ],
        "src/",
        "tests/",
        "vendor/"
    ]
];

function printDirectoryTree($structure, $indent = 0) {
    foreach ($structure as $name => $contents) {
        echo str_repeat("  ", $indent) . $name . "\n";
        if (is_array($contents)) {
            printDirectoryTree($contents, $indent + 1);
        }
    }
}

echo "\nMonorepo directory structure:\n";
printDirectoryTree($directoryStructure);

// Monorepo workflow commands
$monorepoCommands = [
    "composer install" => "Install all dependencies including local packages",
    "composer update company/core" => "Update specific local package",
    "composer require --dev phpunit/phpunit" => "Add dev dep to root",
    "composer run-script test-all" => "Run tests across all packages"
];

echo "\nMonorepo workflow commands:\n";
foreach ($monorepoCommands as $command => $description) {
    echo "$command - $description\n";
}
```

Monorepos use path repositories to manage multiple related packages  
in a single repository. Path repositories can use symlinks for  
development or copy files for distribution. This approach simplifies  
development of interdependent packages and shared tooling.  

## Custom installers and plugins

Creating custom installation behavior and Composer plugins.  

```php
<?php

// Custom installer composer.json
$customInstallerConfig = [
    "name" => "company/custom-installer",
    "type" => "composer-installer",
    "require" => [
        "composer-plugin-api" => "^2.0"
    ],
    "autoload" => [
        "psr-4" => [
            "Company\\Installer\\" => "src/"
        ]
    ],
    "extra" => [
        "class" => "Company\\Installer\\CustomPlugin",
        "installer-name" => "custom-framework"
    ]
];

echo "Custom installer configuration:\n";
echo json_encode($customInstallerConfig, JSON_PRETTY_PRINT) . "\n";

// Example custom installer plugin
$customInstallerCode = <<<'PHP'
<?php

namespace Company\Installer;

use Composer\Composer;
use Composer\IO\IOInterface;
use Composer\Plugin\PluginInterface;
use Composer\Installer\LibraryInstaller;

class CustomPlugin implements PluginInterface
{
    public function activate(Composer $composer, IOInterface $io)
    {
        $installer = new CustomInstaller($io, $composer);
        $composer->getInstallationManager()->addInstaller($installer);
    }

    public function deactivate(Composer $composer, IOInterface $io)
    {
        // Cleanup if needed
    }

    public function uninstall(Composer $composer, IOInterface $io)
    {
        // Cleanup on uninstall
    }
}

class CustomInstaller extends LibraryInstaller
{
    public function supports($packageType)
    {
        return $packageType === 'custom-framework';
    }

    public function getInstallPath(PackageInterface $package)
    {
        $name = $package->getName();
        return 'frameworks/' . basename($name);
    }
}
PHP;

echo "\nCustom installer plugin code:\n$customInstallerCode\n";

// Package using custom installer
$packageUsingCustomInstaller = [
    "name" => "vendor/awesome-framework",
    "type" => "custom-framework",
    "require" => [
        "company/custom-installer" => "^1.0"
    ]
];

echo "\nPackage using custom installer:\n";
echo json_encode($packageUsingCustomInstaller, JSON_PRETTY_PRINT) . "\n";

// Plugin types and capabilities
$pluginTypes = [
    "installer" => "Custom installation paths and behavior",
    "command" => "Add new Composer commands",
    "event" => "React to Composer events", 
    "repository" => "Custom package repositories",
    "downloader" => "Custom download protocols"
];

echo "\nPlugin types and capabilities:\n";
foreach ($pluginTypes as $type => $capability) {
    echo sprintf("%-12s: %s\n", $type, $capability);
}
```

Custom installers and plugins extend Composer functionality for  
specialized package types, installation paths, and workflow integration.  
Plugins implement PluginInterface and can add installers, commands,  
event handlers, and custom repository types for framework ecosystems.  

## Working with private packages

Managing internal packages and private repositories.  

```php
<?php

// Private repository configuration
$privateRepoConfig = [
    "repositories" => [
        [
            "type" => "composer",
            "url" => "https://packages.company.com",
            "options" => [
                "ssl" => [
                    "verify_peer" => true
                ]
            ]
        ],
        [
            "type" => "vcs",
            "url" => "git@github.com:company/private-lib.git"
        ],
        [
            "type" => "git",
            "url" => "https://gitlab.company.com/team/internal-tools.git"
        ]
    ],
    "config" => [
        "github-oauth" => [
            "github.com" => "$GITHUB_TOKEN"
        ],
        "gitlab-token" => [
            "gitlab.company.com" => "$GITLAB_TOKEN"
        ]
    }
];

echo "Private repository configuration:\n";
echo json_encode($privateRepoConfig, JSON_PRETTY_PRINT) . "\n";

// Authentication methods
$authMethods = [
    "Environment Variables" => [
        "COMPOSER_AUTH" => '{"github-oauth":{"github.com":"token"}}',
        "GITHUB_TOKEN" => "GitHub personal access token",
        "GITLAB_TOKEN" => "GitLab access token"
    ],
    "Auth.json File" => [
        "location" => "~/.composer/auth.json or project/auth.json",
        "format" => "Same as config.github-oauth structure",
        "security" => "Never commit auth.json to version control"
    ],
    "Interactive Auth" => [
        "composer config github-oauth.github.com $TOKEN",
        "Stores token in global composer config",
        "Prompts for credentials when needed"
    ]
];

echo "\nAuthentication methods:\n";
foreach ($authMethods as $method => $details) {
    echo "\n$method:\n";
    if (is_array($details)) {
        foreach ($details as $key => $value) {
            echo "  $key: $value\n";
        }
    }
}

// Private package development workflow
$privateWorkflow = [
    "Development" => [
        "Use path repositories for local development",
        "Test packages before tagging releases",
        "Maintain internal package documentation"
    ],
    "Publishing" => [
        "Tag releases with semantic versioning",
        "Update internal package registry",
        "Notify teams of breaking changes"
    ],
    "Security" => [
        "Restrict repository access by team",
        "Use deploy keys for CI/CD",
        "Regular security audits of private packages"
    ]
];

echo "\nPrivate package workflow:\n";
foreach ($privateWorkflow as $phase => $practices) {
    echo "\n$phase:\n";
    foreach ($practices as $practice) {
        echo "- $practice\n";
    }
}

// Example CI/CD configuration for private packages
$ciConfig = <<<'YAML'
# .gitlab-ci.yml example
deploy:
  script:
    - echo '{"github-oauth":{"github.com":"'$GITHUB_TOKEN'"}}' > auth.json
    - composer install --no-dev --optimize-autoloader
    - rm auth.json  # Clean up after install
  variables:
    COMPOSER_CACHE_DIR: ".composer-cache"
  cache:
    paths:
      - .composer-cache/
YAML;

echo "\nExample CI/CD configuration:\n$ciConfig\n";
```

Private packages require authentication and secure repository access.  
Use environment variables, auth.json files, or OAuth tokens for  
authentication. Implement proper access controls, deploy keys for  
CI/CD, and maintain internal package registries for team coordination.  

## Performance monitoring and debugging

Analyzing and optimizing Composer performance issues.  

```php
<?php

// Performance monitoring configuration
$performanceConfig = [
    "config" => [
        "cache-dir" => "/tmp/composer-cache",
        "cache-ttl" => 86400,
        "cache-files-ttl" => 86400,
        "optimize-autoloader" => true,
        "classmap-authoritative" => true,
        "apcu-autoloader" => true,
        "process-timeout" => 600,
        "htaccess-protect" => false
    ]
];

echo "Performance configuration:\n";
echo json_encode($performanceConfig, JSON_PRETTY_PRINT) . "\n";

// Debugging commands
$debugCommands = [
    "composer diagnose" => "System health check and common issues",
    "composer --verbose install" => "Detailed output during operations",
    "composer --profile install" => "Performance profiling information",
    "composer show --tree" => "Dependency tree visualization",
    "composer why vendor/package" => "Why package is installed",
    "composer why-not vendor/package" => "Why package cannot be installed",
    "composer depends vendor/package" => "Packages depending on this one",
    "composer outdated --direct" => "Direct outdated dependencies only"
];

echo "\nDebugging and analysis commands:\n";
foreach ($debugCommands as $command => $description) {
    echo "$command - $description\n";
}

// Performance metrics collection
$performanceMetrics = <<<'PHP'
<?php

class ComposerPerformanceMonitor
{
    private static array $metrics = [];
    
    public static function startTimer(string $operation): void
    {
        self::$metrics[$operation] = [
            'start_time' => microtime(true),
            'start_memory' => memory_get_usage(true)
        ];
    }
    
    public static function endTimer(string $operation): array
    {
        if (!isset(self::$metrics[$operation])) {
            throw new InvalidArgumentException("Timer '$operation' not found");
        }
        
        $start = self::$metrics[$operation];
        $end = [
            'end_time' => microtime(true),
            'end_memory' => memory_get_usage(true)
        ];
        
        return [
            'duration' => $end['end_time'] - $start['start_time'],
            'memory_used' => $end['end_memory'] - $start['start_memory'],
            'peak_memory' => memory_get_peak_usage(true)
        ];
    }
}

// Usage example
ComposerPerformanceMonitor::startTimer('autoload_generation');
// ... Composer operations ...
$metrics = ComposerPerformanceMonitor::endTimer('autoload_generation');

echo "Operation took: " . $metrics['duration'] . " seconds\n";
echo "Memory used: " . number_format($metrics['memory_used'] / 1024 / 1024, 2) . " MB\n";
PHP;

echo "\nPerformance monitoring implementation:\n$performanceMetrics\n";

// Common performance issues and solutions
$performanceIssues = [
    "Slow Installation" => [
        "Enable package caching",
        "Use --prefer-dist for faster downloads",
        "Optimize network connectivity",
        "Use composer-asset-plugin alternatives"
    ],
    "Large Autoloader" => [
        "Use classmap-authoritative in production",
        "Enable APCu autoloader caching",
        "Exclude unnecessary files from autoloading",
        "Optimize PSR-4 namespace structure"
    ],
    "Memory Issues" => [
        "Increase PHP memory_limit",
        "Use --optimize-autoloader flag",
        "Reduce development dependencies in production",
        "Monitor peak memory usage"
    ]
];

echo "\nCommon performance issues and solutions:\n";
foreach ($performanceIssues as $issue => $solutions) {
    echo "\n$issue:\n";
    foreach ($solutions as $solution) {
        echo "- $solution\n";
    }
}
```

Performance monitoring identifies bottlenecks in Composer operations  
through profiling, verbose output, and system diagnostics. Use  
caching, optimization flags, and memory monitoring to improve  
performance in development and production environments.  

## Global packages and tools

Managing system-wide Composer packages and development tools.  

```php
<?php

// Global package installation examples
$globalPackages = [
    "Development Tools" => [
        "friendsofphp/php-cs-fixer" => "Code style fixer",
        "phpstan/phpstan" => "Static analysis tool",
        "vimeo/psalm" => "Static analysis tool",
        "phpmd/phpmd" => "Mess detector",
        "squizlabs/php_codesniffer" => "Code sniffer"
    ],
    "Utilities" => [
        "laravel/installer" => "Laravel application installer",
        "symfony/cli" => "Symfony CLI tool",
        "composer/satis" => "Static Composer repository generator",
        "hirak/prestissimo" => "Parallel download plugin"
    ],
    "Framework Tools" => [
        "laravel/valet" => "Development environment for macOS",
        "drupal/console" => "Drupal Console tool",
        "wp-cli/wp-cli" => "WordPress command line interface"
    ]
];

echo "Useful global packages by category:\n";
foreach ($globalPackages as $category => $packages) {
    echo "\n$category:\n";
    foreach ($packages as $package => $description) {
        echo "  $package - $description\n";
    }
}

// Global package commands
$globalCommands = [
    "composer global require vendor/package" => "Install global package",
    "composer global remove vendor/package" => "Remove global package", 
    "composer global update" => "Update all global packages",
    "composer global show" => "List installed global packages",
    "composer global outdated" => "Show outdated global packages"
];

echo "\nGlobal package management commands:\n";
foreach ($globalCommands as $command => $description) {
    echo "$command - $description\n";
}

// PATH configuration for global binaries
$pathConfiguration = <<<'BASH'
# Add to ~/.bashrc or ~/.zshrc
export PATH="$HOME/.composer/vendor/bin:$PATH"

# Or for specific user directory
export PATH="$HOME/.config/composer/vendor/bin:$PATH"

# Verify global tools are available
which php-cs-fixer
which phpstan
which psalm
BASH;

echo "\nPATH configuration for global tools:\n$pathConfiguration\n";

// Global configuration management
$globalConfigCommands = [
    "composer global config --list" => "Show global configuration",
    "composer global config repositories.packagist false" => "Disable Packagist globally",
    "composer global config minimum-stability dev" => "Allow dev packages globally",
    "composer global config optimize-autoloader true" => "Enable optimization globally"
];

echo "\nGlobal configuration commands:\n";
foreach ($globalConfigCommands as $command => $description) {
    echo "$command - $description\n";
}

// Example global tool usage script
$toolScript = <<<'PHP'
<?php
// tools/quality-check.php

$tools = [
    'php-cs-fixer' => 'fix --dry-run --diff',
    'phpstan' => 'analyse --memory-limit=1G',
    'psalm' => '--show-info=false',
    'phpmd' => 'src/ text cleancode,codesize,design,naming,unusedcode'
];

foreach ($tools as $tool => $args) {
    echo "Running $tool...\n";
    $command = "$tool $args";
    $output = shell_exec($command);
    echo $output . "\n";
}
PHP;

echo "\nExample quality check script using global tools:\n$toolScript\n";
```

Global packages install development tools system-wide for use across  
all projects. Add Composer's global bin directory to your PATH for  
easy access to tools like PHP CS Fixer, PHPStan, and framework CLIs.  
Global packages simplify development environment setup and maintenance.  

## Environment-specific configurations

Configuring Composer for different deployment environments.  

```php
<?php

// Environment-specific configurations
$environmentConfigs = [
    "development" => [
        "config" => [
            "optimize-autoloader" => false,
            "classmap-authoritative" => false,
            "apcu-autoloader" => false,
            "cache-dir" => "var/cache/composer",
            "process-timeout" => 300
        ],
        "scripts" => [
            "post-install-cmd" => [
                "php artisan clear-compiled",
                "php artisan optimize:clear"
            ]
        ]
    ],
    "testing" => [
        "config" => [
            "optimize-autoloader" => true,
            "classmap-authoritative" => false,
            "process-timeout" => 600
        ],
        "scripts" => [
            "pre-install-cmd" => "php artisan down --retry=60",
            "post-install-cmd" => [
                "php artisan migrate --force",
                "php artisan db:seed --force",
                "php artisan up"
            ]
        ]
    ],
    "production" => [
        "config" => [
            "optimize-autoloader" => true,
            "classmap-authoritative" => true,
            "apcu-autoloader" => true,
            "no-dev" => true,
            "prefer-dist" => true
        ],
        "scripts" => [
            "pre-install-cmd" => "php artisan down --retry=60",
            "post-install-cmd" => [
                "php artisan config:cache",
                "php artisan route:cache", 
                "php artisan view:cache",
                "php artisan up"
            ]
        ]
    ]
];

echo "Environment-specific configurations:\n";
foreach ($environmentConfigs as $env => $config) {
    echo "\n" . strtoupper($env) . " Environment:\n";
    echo json_encode($config, JSON_PRETTY_PRINT) . "\n";
}

// Environment detection and configuration
$environmentDetection = <<<'PHP'
<?php

class ComposerEnvironmentManager
{
    public static function getEnvironment(): string
    {
        // Check environment variable
        if ($env = getenv('APP_ENV')) {
            return $env;
        }
        
        // Check for specific files
        if (file_exists('.env.production')) {
            return 'production';
        }
        
        if (file_exists('.env.testing')) {
            return 'testing';
        }
        
        // Default to development
        return 'development';
    }
    
    public static function getInstallCommand(): string
    {
        $env = self::getEnvironment();
        
        return match($env) {
            'production' => 'composer install --no-dev --optimize-autoloader --classmap-authoritative --no-scripts',
            'testing' => 'composer install --optimize-autoloader',
            'development' => 'composer install',
            default => 'composer install'
        };
    }
}

// Usage
$environment = ComposerEnvironmentManager::getEnvironment();
$command = ComposerEnvironmentManager::getInstallCommand();

echo "Detected environment: $environment\n";
echo "Recommended command: $command\n";
PHP;

echo "\nEnvironment detection implementation:\n$environmentDetection\n";

// Docker multi-stage build example
$dockerExample = <<<'DOCKERFILE'
# Multi-stage Dockerfile for different environments
FROM php:8.4-fpm as base
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
WORKDIR /app
COPY composer.json composer.lock ./

FROM base as development
RUN composer install --no-scripts --no-autoloader
COPY . .
RUN composer dump-autoload

FROM base as production  
RUN composer install --no-dev --optimize-autoloader --classmap-authoritative --no-scripts
COPY . .
RUN composer dump-autoload --optimize --classmap-authoritative
DOCKERFILE;

echo "\nDocker multi-stage build example:\n$dockerExample\n";

// CI/CD environment variables
$ciVariables = [
    "COMPOSER_NO_INTERACTION" => "1",
    "COMPOSER_ALLOW_SUPERUSER" => "1", 
    "COMPOSER_CACHE_DIR" => "/tmp/composer-cache",
    "COMPOSER_MEMORY_LIMIT" => "2G",
    "COMPOSER_PROCESS_TIMEOUT" => "600"
];

echo "Useful CI/CD environment variables:\n";
foreach ($ciVariables as $var => $value) {
    echo "$var=$value\n";
}
```

Environment-specific configurations optimize Composer behavior for  
development, testing, and production environments. Use different  
optimization levels, caching strategies, and script execution based  
on deployment context for optimal performance and reliability.  

## Working with git hooks

Integrating Composer with Git hooks for automated workflows.  

```php
<?php

// Pre-commit hook script for Composer validation
$preCommitHook = <<<'BASH'
#!/bin/bash
# .git/hooks/pre-commit

echo "Running pre-commit checks..."

# Validate composer.json syntax
if [ -f "composer.json" ]; then
    echo "Validating composer.json..."
    composer validate --strict --no-check-all
    if [ $? -ne 0 ]; then
        echo "❌ composer.json validation failed"
        exit 1
    fi
    echo "✅ composer.json is valid"
fi

# Check for composer.lock consistency
if [ -f "composer.lock" ]; then
    echo "Checking composer.lock consistency..."
    composer check-platform-reqs --lock
    if [ $? -ne 0 ]; then
        echo "❌ Platform requirements check failed"
        exit 1
    fi
    echo "✅ Platform requirements satisfied"
fi

# Run security audit
echo "Running security audit..."
composer audit --no-dev
if [ $? -ne 0 ]; then
    echo "❌ Security vulnerabilities found"
    exit 1
fi

echo "✅ All pre-commit checks passed"
BASH;

echo "Pre-commit hook script:\n$preCommitHook\n";

// Post-merge hook for dependency updates
$postMergeHook = <<<'BASH'
#!/bin/bash
# .git/hooks/post-merge

echo "Post-merge hook: Checking for dependency changes..."

# Check if composer.json or composer.lock changed
if [ -f "composer.json" ] && [ -f "composer.lock" ]; then
    if git diff-tree -r --name-only --no-commit-id HEAD@{1} HEAD | grep -E "(composer\.(json|lock))" > /dev/null; then
        echo "Dependencies changed, running composer install..."
        composer install --no-interaction
        
        if [ $? -eq 0 ]; then
            echo "✅ Dependencies updated successfully"
        else
            echo "❌ Failed to update dependencies"
            exit 1
        fi
    else
        echo "No dependency changes detected"
    fi
fi
BASH;

echo "Post-merge hook script:\n$postMergeHook\n";

// Automated hook installation
$hookInstaller = <<<'PHP'
<?php

class GitHookInstaller
{
    private const HOOKS = [
        'pre-commit' => 'validateComposerFiles',
        'post-merge' => 'updateDependencies', 
        'pre-push' => 'securityCheck'
    ];
    
    public static function install(): void
    {
        $hooksDir = '.git/hooks';
        
        if (!is_dir($hooksDir)) {
            throw new RuntimeException('Git hooks directory not found');
        }
        
        foreach (self::HOOKS as $hook => $method) {
            $hookFile = "$hooksDir/$hook";
            $content = self::generateHookContent($method);
            
            file_put_contents($hookFile, $content);
            chmod($hookFile, 0755);
            
            echo "Installed $hook hook\n";
        }
    }
    
    private static function generateHookContent(string $method): string
    {
        return "#!/bin/bash\nphp -r \"require 'scripts/hooks.php'; GitHookInstaller::$method();\"";
    }
    
    public static function validateComposerFiles(): void
    {
        system('composer validate --strict');
        system('composer audit');
    }
}

// Install hooks after composer install
GitHookInstaller::install();
PHP;

echo "Hook installer implementation:\n$hookInstaller\n";
```

Git hooks automate Composer workflows by validating composer.json,  
checking dependency consistency, and running security audits on  
commits and merges. Use pre-commit hooks for validation and  
post-merge hooks for automatic dependency updates when files change.  

## Parallel downloads and optimization

Optimizing Composer performance with parallel downloads and caching.  

```php
<?php

// Performance optimization configuration
$performanceConfig = [
    "config" => [
        "preferred-install" => [
            "*" => "dist"
        ],
        "process-timeout" => 600,
        "use-parent-dir" => false,
        "cache-vcs-dirs" => true,
        "cache-ttl" => 15552000,
        "cache-files-ttl" => 15552000,
        "cache-read-only" => false,
        "bin-compat" => "auto",
        "discard-changes" => false,
        "archive-format" => "tar",
        "archive-dir" => "dist"
    ],
    "extra" => [
        "composer-parallel" => [
            "processes" => 4,
            "timeout" => 300
        ]
    ]
];

echo "Performance optimization configuration:\n";
echo json_encode($performanceConfig, JSON_PRETTY_PRINT) . "\n";

// Parallel download implementation example
$parallelDownloader = <<<'PHP'
<?php

class ParallelComposerDownloader
{
    private int $maxProcesses;
    private int $timeout;
    
    public function __construct(int $maxProcesses = 4, int $timeout = 300)
    {
        $this->maxProcesses = $maxProcesses;
        $this->timeout = $timeout;
    }
    
    public function downloadPackages(array $packages): array
    {
        $processes = [];
        $results = [];
        $packageChunks = array_chunk($packages, $this->maxProcesses);
        
        foreach ($packageChunks as $chunk) {
            $processes = [];
            
            foreach ($chunk as $package) {
                $cmd = "composer require {$package['name']}:{$package['version']} --no-interaction";
                $process = proc_open($cmd, [
                    1 => ['pipe', 'w'],
                    2 => ['pipe', 'w']
                ], $pipes);
                
                if (is_resource($process)) {
                    $processes[] = [
                        'process' => $process,
                        'pipes' => $pipes,
                        'package' => $package
                    ];
                }
            }
            
            // Wait for all processes in this chunk
            foreach ($processes as $proc) {
                $output = stream_get_contents($proc['pipes'][1]);
                $error = stream_get_contents($proc['pipes'][2]);
                
                fclose($proc['pipes'][1]);
                fclose($proc['pipes'][2]);
                
                $returnCode = proc_close($proc['process']);
                
                $results[] = [
                    'package' => $proc['package'],
                    'success' => $returnCode === 0,
                    'output' => $output,
                    'error' => $error
                ];
            }
        }
        
        return $results;
    }
}

// Usage example
$downloader = new ParallelComposerDownloader(4, 300);
$packages = [
    ['name' => 'monolog/monolog', 'version' => '^3.0'],
    ['name' => 'guzzlehttp/guzzle', 'version' => '^7.8'],
    ['name' => 'symfony/console', 'version' => '^6.4']
];

$results = $downloader->downloadPackages($packages);
foreach ($results as $result) {
    $status = $result['success'] ? '✅' : '❌';
    echo "$status {$result['package']['name']}\n";
}
PHP;

echo "\nParallel downloader implementation:\n$parallelDownloader\n";

// Caching strategies
$cachingStrategies = [
    "Local Cache" => [
        "cache-dir" => "~/.composer/cache",
        "benefits" => "Fast access to downloaded packages",
        "maintenance" => "composer clear-cache"
    ],
    "Shared Cache" => [
        "cache-dir" => "/shared/composer-cache", 
        "benefits" => "Team-wide cache sharing",
        "considerations" => "File permissions and disk space"
    ],
    "Memory Cache" => [
        "apcu-autoloader" => true,
        "benefits" => "In-memory autoloader caching",
        "requirements" => "APCu extension installed"
    ]
];

echo "\nCaching strategies:\n";
foreach ($cachingStrategies as $strategy => $details) {
    echo "\n$strategy:\n";
    foreach ($details as $key => $value) {
        echo "  " . ucfirst($key) . ": $value\n";
    }
}
```

Parallel downloads and caching significantly improve Composer  
performance by utilizing multiple connections and storing frequently  
accessed data. Configure process limits, timeouts, and cache  
directories based on system resources and network capacity.  

## Vendor autoload customization

Advanced autoloader configuration and custom loading strategies.  

```php
<?php

// Advanced autoload configuration
$autoloadConfig = [
    "autoload" => [
        "psr-4" => [
            "App\\" => "src/",
            "Domain\\" => "domain/",
            "Infrastructure\\" => "infrastructure/"
        ],
        "psr-0" => [
            "Legacy_" => "legacy/lib/"
        ],
        "classmap" => [
            "legacy/classes",
            "external/phpmailer/class.phpmailer.php"
        ],
        "files" => [
            "bootstrap/helpers.php",
            "config/constants.php",
            "legacy/functions.php"
        ],
        "exclude-from-classmap" => [
            "/tests/",
            "/vendor/package/tests/"
        ]
    ],
    "autoload-dev" => [
        "psr-4" => [
            "Tests\\" => "tests/",
            "Benchmarks\\" => "benchmarks/"
        ],
        "files" => [
            "tests/helpers.php"
        ]
    ]
];

echo "Advanced autoload configuration:\n";
echo json_encode($autoloadConfig, JSON_PRETTY_PRINT) . "\n";

// Custom autoloader implementation
$customAutoloader = <<<'PHP'
<?php

class CustomAutoloader
{
    private static array $prefixes = [];
    private static array $fallbacks = [];
    
    public static function register(): void
    {
        spl_autoload_register([self::class, 'loadClass'], true, true);
    }
    
    public static function addPrefix(string $prefix, string $basePath): void
    {
        $prefix = trim($prefix, '\\') . '\\';
        self::$prefixes[$prefix][] = rtrim($basePath, '/') . '/';
    }
    
    public static function addFallback(callable $fallback): void
    {
        self::$fallbacks[] = $fallback;
    }
    
    public static function loadClass(string $className): bool
    {
        // Try PSR-4 autoloading
        foreach (self::$prefixes as $prefix => $basePaths) {
            if (strpos($className, $prefix) === 0) {
                $relative = substr($className, strlen($prefix));
                foreach ($basePaths as $basePath) {
                    $file = $basePath . str_replace('\\', '/', $relative) . '.php';
                    if (file_exists($file)) {
                        require_once $file;
                        return true;
                    }
                }
            }
        }
        
        // Try fallback loaders
        foreach (self::$fallbacks as $fallback) {
            if ($fallback($className)) {
                return true;
            }
        }
        
        return false;
    }
}

// Configuration for custom autoloader
CustomAutoloader::register();
CustomAutoloader::addPrefix('App', 'src');
CustomAutoloader::addPrefix('Tests', 'tests');

// Add fallback for legacy naming conventions
CustomAutoloader::addFallback(function($className) {
    if (strpos($className, 'Legacy_') === 0) {
        $file = 'legacy/' . str_replace('_', '/', $className) . '.php';
        if (file_exists($file)) {
            require_once $file;
            return true;
        }
    }
    return false;
});
PHP;

echo "\nCustom autoloader implementation:\n$customAutoloader\n";

// Autoloader optimization techniques
$optimizationTechniques = [
    "Classmap Authoritative" => [
        "flag" => "--classmap-authoritative",
        "description" => "Only use classmap, ignore PSR-4/0 rules",
        "use_case" => "Production with complete class mapping"
    ],
    "APCu Autoloader" => [
        "flag" => "--apcu-autoloader", 
        "description" => "Cache autoloader in APCu memory",
        "use_case" => "High-traffic production servers"
    ],
    "Optimize Autoloader" => [
        "flag" => "--optimize-autoloader",
        "description" => "Convert PSR-4/0 to classmap",
        "use_case" => "Production deployment optimization"
    ]
];

echo "\nAutoloader optimization techniques:\n";
foreach ($optimizationTechniques as $technique => $details) {
    echo "\n$technique:\n";
    foreach ($details as $key => $value) {
        echo "  " . ucfirst(str_replace('_', ' ', $key)) . ": $value\n";
    }
}

// Performance comparison
$performanceData = [
    "Standard PSR-4" => ["load_time" => "0.15ms", "memory" => "2MB"],
    "Optimized" => ["load_time" => "0.08ms", "memory" => "1.5MB"], 
    "Classmap Auth" => ["load_time" => "0.05ms", "memory" => "1MB"],
    "APCu Cached" => ["load_time" => "0.02ms", "memory" => "0.8MB"]
];

echo "\nPerformance comparison (approximate):\n";
foreach ($performanceData as $method => $metrics) {
    echo sprintf("%-15s: Load: %s, Memory: %s\n", 
        $method, $metrics['load_time'], $metrics['memory']);
}
```

Vendor autoload customization enables complex loading strategies  
combining PSR-4, PSR-0, classmap, and file inclusion. Optimization  
techniques like classmap authoritative mode and APCu caching  
dramatically improve autoloader performance in production environments.  

## Package conflict resolution

Resolving dependency conflicts and version incompatibilities.  

```php
<?php

// Common conflict scenarios and resolutions
$conflictScenarios = [
    "version_conflict" => [
        "problem" => "Package A requires lib ^1.0, Package B requires lib ^2.0",
        "resolution" => "Find compatible versions or use alternatives",
        "command" => "composer why-not vendor/lib:^2.0"
    ],
    "php_version" => [
        "problem" => "Package requires PHP 8.1, project uses PHP 8.4",
        "resolution" => "Update package or use platform config override",
        "command" => "composer config platform.php 8.1.0"
    ],
    "extension_missing" => [
        "problem" => "Package requires ext-mbstring, not installed",
        "resolution" => "Install extension or use alternative package",
        "command" => "composer check-platform-reqs"
    ]
];

echo "Common conflict scenarios:\n";
foreach ($conflictScenarios as $type => $details) {
    echo "\n" . ucwords(str_replace('_', ' ', $type)) . ":\n";
    foreach ($details as $key => $value) {
        echo "  " . ucfirst($key) . ": $value\n";
    }
}

// Conflict resolution tools and commands
$resolutionTools = [
    "Diagnosis" => [
        "composer why vendor/package" => "Why is this package installed?",
        "composer why-not vendor/package" => "Why can't this be installed?",
        "composer depends vendor/package" => "What depends on this package?",
        "composer prohibits vendor/package" => "What prevents installation?"
    ],
    "Resolution" => [
        "composer update --with-dependencies" => "Update with all dependencies",
        "composer require vendor/package --ignore-platform-reqs" => "Ignore platform checks",
        "composer install --ignore-platform-req=ext-mbstring" => "Ignore specific requirement",
        "composer config repositories.foo false" => "Disable problematic repository"
    ]
];

echo "\nConflict resolution tools:\n";
foreach ($resolutionTools as $category => $commands) {
    echo "\n$category:\n";
    foreach ($commands as $command => $description) {
        echo "  $command\n    → $description\n";
    }
}

// Example conflict resolution workflow
$resolutionWorkflow = <<<'PHP'
<?php

class ConflictResolver
{
    public function analyzeConflict(string $package): array
    {
        $analysis = [
            'package' => $package,
            'current_constraints' => $this->getCurrentConstraints($package),
            'conflicting_packages' => $this->findConflictingPackages($package),
            'possible_solutions' => $this->generateSolutions($package)
        ];
        
        return $analysis;
    }
    
    private function getCurrentConstraints(string $package): array
    {
        $output = shell_exec("composer show $package --all 2>/dev/null");
        // Parse composer show output
        return ['version' => 'detected from output'];
    }
    
    private function findConflictingPackages(string $package): array
    {
        $output = shell_exec("composer why-not $package 2>/dev/null");
        // Parse conflicts from output
        return ['conflicting packages parsed'];
    }
    
    private function generateSolutions(string $package): array
    {
        return [
            'Update conflicting packages to compatible versions',
            'Use alternative packages with similar functionality',
            'Downgrade the requested package to compatible version',
            'Use platform config to override requirements'
        ];
    }
    
    public function attemptResolution(string $package, string $strategy): bool
    {
        switch ($strategy) {
            case 'update_dependencies':
                return $this->updateDependencies($package);
            case 'ignore_platform':
                return $this->ignorePlatformReqs($package);
            case 'alternative_package':
                return $this->suggestAlternatives($package);
            default:
                return false;
        }
    }
    
    private function updateDependencies(string $package): bool
    {
        $command = "composer require $package --with-dependencies --no-interaction";
        $result = shell_exec($command);
        return strpos($result, 'error') === false;
    }
    
    private function ignorePlatformReqs(string $package): bool
    {
        $command = "composer require $package --ignore-platform-reqs --no-interaction";
        $result = shell_exec($command);
        return strpos($result, 'error') === false;
    }
    
    private function suggestAlternatives(string $package): bool
    {
        // Logic to suggest alternative packages
        echo "Consider these alternatives to $package:\n";
        return false; // Would return true if alternatives found
    }
}

// Usage
$resolver = new ConflictResolver();
$analysis = $resolver->analyzeConflict('problem/package');
echo "Conflict analysis: " . json_encode($analysis, JSON_PRETTY_PRINT) . "\n";
PHP;

echo "\nConflict resolution workflow:\n$resolutionWorkflow\n";

// Best practices for avoiding conflicts
$avoidanceStrategies = [
    "Dependency Management" => [
        "Keep dependencies minimal and focused",
        "Regularly update packages to latest stable",
        "Use version constraints that allow updates",
        "Monitor deprecated and abandoned packages"
    ],
    "Version Strategy" => [
        "Use semantic versioning constraints properly",
        "Test major version updates in staging first",
        "Document version requirements clearly",
        "Consider backward compatibility impact"
    ],
    "Project Structure" => [
        "Separate concerns into focused packages",
        "Avoid deep dependency trees when possible",
        "Use interfaces to decouple implementations",
        "Consider package alternatives during selection"
    ]
];

echo "\nBest practices for avoiding conflicts:\n";
foreach ($avoidanceStrategies as $category => $strategies) {
    echo "\n$category:\n";
    foreach ($strategies as $strategy) {
        echo "- $strategy\n";
    }
}
```

Package conflict resolution requires systematic analysis of dependency  
requirements, version constraints, and platform compatibility. Use  
Composer's diagnostic tools to identify conflicts and apply appropriate  
resolution strategies like dependency updates or platform overrides.  

## Working with package archives

Creating and consuming package archives for offline deployment.  

```php
<?php

// Archive creation configuration
$archiveConfig = [
    "config" => [
        "archive-format" => "tar",
        "archive-dir" => "dist"
    ],
    "scripts" => [
        "archive" => [
            "composer archive --format=tar --dir=dist",
            "composer archive --format=zip --dir=dist"
        ],
        "create-distribution" => [
            "@archive",
            "tar -czf project-$(date +%Y%m%d).tar.gz dist/ vendor/ src/",
            "echo 'Distribution package created'"
        ]
    ]
];

echo "Archive configuration:\n";
echo json_encode($archiveConfig, JSON_PRETTY_PRINT) . "\n";

// Archive creation and management
$archiveManager = <<<'PHP'
<?php

class ComposerArchiveManager
{
    private string $archiveDir;
    private string $format;
    
    public function __construct(string $archiveDir = 'dist', string $format = 'tar')
    {
        $this->archiveDir = $archiveDir;
        $this->format = $format;
        
        if (!is_dir($this->archiveDir)) {
            mkdir($this->archiveDir, 0755, true);
        }
    }
    
    public function createProjectArchive(string $version = null): string
    {
        $version = $version ?: date('Y.m.d-H.i.s');
        $filename = "project-{$version}.{$this->format}";
        $filepath = "{$this->archiveDir}/$filename";
        
        // Create archive with essential files
        $command = "composer archive --format={$this->format} --file=$filepath";
        $output = shell_exec($command);
        
        if (file_exists($filepath)) {
            return $filepath;
        }
        
        throw new RuntimeException("Failed to create archive: $output");
    }
    
    public function createVendorArchive(): string
    {
        $filename = "vendor-" . date('Y.m.d') . ".tar.gz";
        $filepath = "{$this->archiveDir}/$filename";
        
        if (!is_dir('vendor')) {
            throw new RuntimeException('Vendor directory not found');
        }
        
        $command = "tar -czf $filepath vendor/";
        shell_exec($command);
        
        return $filepath;
    }
    
    public function extractArchive(string $archivePath, string $destination = '.'): bool
    {
        if (!file_exists($archivePath)) {
            throw new InvalidArgumentException("Archive not found: $archivePath");
        }
        
        $extension = pathinfo($archivePath, PATHINFO_EXTENSION);
        
        switch ($extension) {
            case 'tar':
                $command = "tar -xf $archivePath -C $destination";
                break;
            case 'gz':
                $command = "tar -xzf $archivePath -C $destination";
                break;
            case 'zip':
                $command = "unzip -q $archivePath -d $destination";
                break;
            default:
                throw new UnsupportedOperationException("Unsupported format: $extension");
        }
        
        $output = shell_exec($command);
        return empty($output); // Success if no error output
    }
    
    public function getArchiveInfo(string $archivePath): array
    {
        if (!file_exists($archivePath)) {
            throw new InvalidArgumentException("Archive not found: $archivePath");
        }
        
        return [
            'filename' => basename($archivePath),
            'size' => filesize($archivePath),
            'created' => date('Y-m-d H:i:s', filemtime($archivePath)),
            'format' => pathinfo($archivePath, PATHINFO_EXTENSION)
        ];
    }
}

// Usage example
$archiveManager = new ComposerArchiveManager('releases', 'tar');

// Create project archive
$projectArchive = $archiveManager->createProjectArchive('1.0.0');
echo "Project archive created: $projectArchive\n";

// Create vendor archive for offline deployment
$vendorArchive = $archiveManager->createVendorArchive();
echo "Vendor archive created: $vendorArchive\n";

// Get archive information
$info = $archiveManager->getArchiveInfo($projectArchive);
echo "Archive info: " . json_encode($info, JSON_PRETTY_PRINT) . "\n";
PHP;

echo "\nArchive manager implementation:\n$archiveManager\n";

// Offline deployment workflow
$offlineDeployment = [
    "Preparation" => [
        "composer install --no-dev --optimize-autoloader",
        "composer archive --format=tar --dir=releases",
        "tar -czf deployment-package.tar.gz releases/ config/ public/",
        "Include database migrations and configuration"
    ],
    "Transfer" => [
        "Copy deployment package to target server",
        "Verify package integrity with checksums",
        "Backup existing deployment if applicable",
        "Prepare rollback procedures"
    ],
    "Deployment" => [
        "Extract package to deployment directory",
        "Set proper file permissions and ownership",
        "Update configuration for target environment",
        "Run post-deployment scripts and migrations"
    ],
    "Verification" => [
        "Verify application functionality",
        "Check all services are running correctly",
        "Monitor logs for any errors",
        "Confirm rollback procedures if needed"
    ]
];

echo "\nOffline deployment workflow:\n";
foreach ($offlineDeployment as $phase => $steps) {
    echo "\n$phase:\n";
    foreach ($steps as $step) {
        echo "- $step\n";
    }
}
```

Package archives enable offline deployment and distribution by bundling  
project files and dependencies into portable packages. Create archives  
with `composer archive`, include vendor directories, and implement  
extraction workflows for consistent deployment across environments.  

## Composer plugins ecosystem

Exploring useful Composer plugins and creating custom plugins.  

```php
<?php

// Popular Composer plugins
$popularPlugins = [
    "composer/installers" => [
        "purpose" => "Multi-framework installer plugin",
        "description" => "Custom installation paths for various frameworks",
        "usage" => "Automatic installation to framework-specific directories"
    ],
    "wikimedia/composer-merge-plugin" => [
        "purpose" => "Merge multiple composer.json files",
        "description" => "Useful for monorepos and modular applications", 
        "usage" => "Combines dependencies from multiple sources"
    ],
    "phpstan/extension-installer" => [
        "purpose" => "Automatic PHPStan extension configuration",
        "description" => "Installs and configures PHPStan extensions",
        "usage" => "Zero-config PHPStan extension management"
    ],
    "composer/semver" => [
        "purpose" => "Semantic version constraint parser",
        "description" => "Parse and compare semantic version constraints",
        "usage" => "Custom dependency resolution logic"
    ],
    "narrowspark/automatic-composer" => [
        "purpose" => "Framework automation and configuration",
        "description" => "Automatic project configuration based on installed packages",
        "usage" => "Zero-config framework setup"
    ]
];

echo "Popular Composer plugins:\n";
foreach ($popularPlugins as $plugin => $details) {
    echo "\n$plugin:\n";
    foreach ($details as $key => $value) {
        echo "  " . ucfirst($key) . ": $value\n";
    }
}

// Custom plugin development
$customPlugin = <<<'PHP'
<?php

namespace Company\ComposerPlugin;

use Composer\Composer;
use Composer\EventDispatcher\EventSubscriberInterface;
use Composer\IO\IOInterface;
use Composer\Plugin\PluginInterface;
use Composer\Script\Event;
use Composer\Script\ScriptEvents;

class CustomPlugin implements PluginInterface, EventSubscriberInterface
{
    public function activate(Composer $composer, IOInterface $io): void
    {
        $io->write('Custom plugin activated');
    }

    public function deactivate(Composer $composer, IOInterface $io): void
    {
        $io->write('Custom plugin deactivated');
    }

    public function uninstall(Composer $composer, IOInterface $io): void
    {
        $io->write('Custom plugin uninstalled');
    }

    public static function getSubscribedEvents(): array
    {
        return [
            ScriptEvents::POST_INSTALL_CMD => 'onPostInstall',
            ScriptEvents::POST_UPDATE_CMD => 'onPostUpdate',
            ScriptEvents::PRE_PACKAGE_INSTALL => 'onPrePackageInstall'
        ];
    }

    public function onPostInstall(Event $event): void
    {
        $io = $event->getIO();
        $composer = $event->getComposer();
        
        $io->write('Running post-install customizations...');
        
        // Custom logic after installation
        $this->optimizeProject($composer, $io);
        $this->generateConfiguration($composer, $io);
    }

    public function onPostUpdate(Event $event): void
    {
        $io = $event->getIO();
        $io->write('Running post-update customizations...');
        
        // Custom logic after updates
        $this->clearCaches();
        $this->validateConfiguration();
    }

    public function onPrePackageInstall(Event $event): void
    {
        $package = $event->getOperation()->getPackage();
        $io = $event->getIO();
        
        $io->write("Installing package: {$package->getName()}");
        
        // Pre-installation validation or preparation
        $this->validatePackageCompatibility($package);
    }

    private function optimizeProject(Composer $composer, IOInterface $io): void
    {
        // Project optimization logic
        $io->write('Optimizing project structure...');
        
        // Example: Create necessary directories
        $directories = ['storage', 'cache', 'logs'];
        foreach ($directories as $dir) {
            if (!is_dir($dir)) {
                mkdir($dir, 0755, true);
                $io->write("Created directory: $dir");
            }
        }
    }

    private function generateConfiguration(Composer $composer, IOInterface $io): void
    {
        // Configuration generation logic
        $config = [
            'app_name' => $composer->getPackage()->getName(),
            'version' => $composer->getPackage()->getVersion(),
            'dependencies' => count($composer->getRepositoryManager()->getRepositories())
        ];
        
        file_put_contents('config/auto-generated.json', json_encode($config, JSON_PRETTY_PRINT));
        $io->write('Generated configuration file');
    }
    
    private function clearCaches(): void
    {
        $cacheDirectories = ['cache', 'tmp', 'storage/cache'];
        foreach ($cacheDirectories as $dir) {
            if (is_dir($dir)) {
                shell_exec("rm -rf $dir/*");
            }
        }
    }
    
    private function validateConfiguration(): void
    {
        $configFiles = ['config/app.php', 'config/database.php'];
        foreach ($configFiles as $file) {
            if (!file_exists($file)) {
                throw new \RuntimeException("Required configuration file missing: $file");
            }
        }
    }
    
    private function validatePackageCompatibility($package): void
    {
        // Custom package compatibility validation
        $blacklist = ['problematic/package'];
        if (in_array($package->getName(), $blacklist)) {
            throw new \RuntimeException("Package {$package->getName()} is not compatible with this project");
        }
    }
}
PHP;

echo "\nCustom plugin implementation:\n$customPlugin\n";

// Plugin composer.json structure
$pluginComposerJson = [
    "name" => "company/composer-plugin",
    "type" => "composer-plugin",
    "description" => "Custom Composer plugin for project automation",
    "license" => "MIT",
    "require" => [
        "composer-plugin-api" => "^2.0"
    ],
    "autoload" => [
        "psr-4" => [
            "Company\\ComposerPlugin\\" => "src/"
        ]
    ],
    "extra" => [
        "class" => "Company\\ComposerPlugin\\CustomPlugin"
    ]
];

echo "\nPlugin composer.json structure:\n";
echo json_encode($pluginComposerJson, JSON_PRETTY_PRINT) . "\n";

// Plugin development best practices
$developmentPractices = [
    "API Compatibility" => [
        "Target stable composer-plugin-api versions",
        "Test against multiple Composer versions",
        "Handle API changes gracefully"
    ],
    "Event Handling" => [
        "Subscribe only to necessary events",
        "Handle errors gracefully in event handlers",
        "Provide meaningful feedback to users"
    ],
    "Performance" => [
        "Minimize processing time in event handlers",
        "Cache expensive operations when possible",
        "Avoid blocking operations in critical events"
    ],
    "User Experience" => [
        "Provide clear status messages",
        "Allow configuration through composer.json",
        "Include comprehensive documentation"
    ]
];

echo "\nPlugin development best practices:\n";
foreach ($developmentPractices as $category => $practices) {
    echo "\n$category:\n";
    foreach ($practices as $practice) {
        echo "- $practice\n";
    }
}
```

Composer plugins extend functionality through event handling, custom  
installers, and workflow automation. Popular plugins solve common  
problems like multi-framework installation paths and configuration  
management. Custom plugins enable project-specific automation and  
integration with existing development workflows.
