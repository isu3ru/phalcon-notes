---
tags: [Phalcon, php]
title: '#1 - Phalcon Project Setup'
created: '2024-03-02T10:00:06.755Z'
modified: '2024-03-02T20:49:28.477Z'
---

# #1 - Phalcon Project Setup

## Directory Structure

Unlike other frameworks like Codeigniter and Laravel, there is no specific directory structure for the Phalcon framework project.

Following is a **suggested basic directory structure** for a simple Phalcon Project.

```
app
  - config
  - controllers
  - views
  - models
public
  index.php
  
.htrouter.php
```

## Components

### .htrouter.php

this file handles the requests and forwards them to the `public/index.php` file.

```php
<?php

$uri = urldecode(parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH));

if ($uri !== '/' && file_exists(__DIR__ . '/public' . $uri)) {
    return false;
}

$_GET['_url'] = $_SERVER['REQUEST_URI'];

require_once __DIR__ . '/public/index.php';

```

### public/index.php file

This is the front controller for the HTTP requests. Phalcon framework has it inside the `public` folder.

```php
<?php

declare(strict_types=1);

use Phalcon\Di\FactoryDefault;

// turn on reporting ALL errors
error_reporting(E_ALL);

// BASE_PATH will point to the directory where .htrouter.php file is.
define('BASE_PATH', dirname(__DIR__));

// points to the app directory on the same level as .htrouter.php file
define('APP_PATH', BASE_PATH . '/app');

try {
    /**
     * The FactoryDefault Dependency Injector automatically registers
     * the services that provide a full stack framework.
     */
    $di = new FactoryDefault();

    /**
     * Read services
     */
    include APP_PATH . '/config/services.php';

    /**
     * Handle routes
     */
    include APP_PATH . '/config/router.php';

    /**
     * Get config service for use in inline setup below
     */
    $config = $di->getConfig();

    /**
     * Include Autoloader
     */
    include APP_PATH . '/config/loader.php';

    /**
     * Handle the request
     */
    $application = new \Phalcon\Mvc\Application($di);

    echo $application->handle($_SERVER['REQUEST_URI'])->getContent();
} catch (\Exception $e) {
    echo $e->getMessage() . '<br>';
    echo '<pre>' . $e->getTraceAsString() . '</pre>';
}

```
