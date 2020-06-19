# Magento2 GrumPHP
GrumPHP configuration for Magento 2 Module Automated Quality Check

## What is GrumPHP ?

- [GrumPHP](https://github.com/phpro/grumphp) has a set of common tasks built-in.

- GrumPHP will run some tests on the committed code. If the tests fail, you won't be able to commit your changes.

- This handy tool will not only improve your codebase, it will also teach your co-workers to write better code following the best practices you've determined as a team.

- GrumPHP can be configured at 2 levels:
    
   + module level (`magento-2-root/app/code/MyVendor/MyModule`).
   
   + project level (`magento-2-root`).

## Setup

1. Install GrumPHP by running `composer require --dev phpro/grumphp` or `composer global require --dev phpro/grumphp`

2. GrumPHP can monitor each git repository push action by initializing it in the repository. 
   
  Goto the module directory and Run: `php vendor/bin/grumphp git:init` or `grumphp git:init`

3. GrumPHP is a task runner and all task needs to be defined via `grumphp.yml` file in the module directory (`app/code/MyVendor/MyModule`) for module level configuration.
 Use the below example to start with:

```yaml
parameters:
  git_dir: .
  root_dir: ../../../..
  bin_dir: '%root_dir%/vendor/bin'
  hide_circumvention_tip: true
  process_timeout: 120
  tasks:
    composer:
      file: ./composer.json
      no_check_all: true
      no_check_lock: false
      no_check_publish: false
      with_dependencies: false
      strict: false
    git_blacklist:
      keywords:
        - "die("
        - "var_dump("
        - "print_r("
        - "exit;"
        - "console.log("
        - "_objectManager"
        - "ObjectManagerInterface"
    phpcs:
      standard: Magento2
      tab_width: 4
      severity: 10
      error_severity: 10
      warning_severity: ~
      report: full
      triggered_by: [phtml, xml, css, js, php]
    phpcsfixer2:
      allow_risky: ~
      cache_file: ~
      config: '%root_dir%/.php_cs.dist'
      using_cache: ~
      verbose: true
      config_contains_finder: false
      triggered_by: ['php', 'phtml']

#    phpmd:
#      ruleset: ['%magento_dir%/dev/tests/static/testsuite/Magento/Test/Php/_files/phpmd/ruleset.xml']
#    phpstan:
#      autoload_file: ~
#      configuration: ~
#      level: 0
#      force_patterns: []
#      ignore_patterns: []
#      triggered_by: ['php']
#      memory_limit: "-1"
```

4. Add bin path (`magento2-root/vendor/bin`) to `composer.json`: 
```json
{
    "config": {
            "bin-dir": "../../../../vendor/bin"
        }
}
```

5. Test by running `php ../../../../vendor/bin/grumphp run` or `grumphp run`

## Dependencies
    
#### 1. PHPCS

- Goto Magento2 root run following commands:

- Install [PHP CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) (skip if already installed) : `composer require --dev "squizlabs/php_codesniffer=*"`

- Install [Magento2 coding standard](https://github.com/magento/magento-coding-standard) :
  
  `composer require --dev magento/magento-coding-standard`
  
- Set Magento2 Standard in PHP CodeSniffer available standards:

  `phpcs --config-set installed_paths ../../magento/magento-coding-standard/`


#### 2. PHPCS FIXER 2

- Install [PHP Coding Standards Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer) (skip if already installed) :
 
  `composer require --dev friendsofphp/php-cs-fixer`

#### 3. PHPMD

#### 4. PHPSTAN
