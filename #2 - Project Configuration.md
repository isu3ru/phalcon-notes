---
tags: [Phalcon, php]
title: '#2 - Project Configuration'
created: '2024-03-02T10:48:23.462Z'
modified: '2024-03-02T20:49:36.350Z'
---

# #2 - Project Configuration

Phalcon configuration can be added to the `index.php `file. But it is much more manageable and clear when defined in a separate `config` folder for each configuration file separately.

There can be many config files. But they must be included in the index.php file.

```php
<?php

    // ... more code here

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

    // ... more code here
```

### config.php

This file has the database connection details and directories of the project.

```php
<?php

/*
 * Modified: prepend directory path of current file, because of this file own different ENV under between Apache and command line.
 * NOTE: please remove this comment.
 */
defined('BASE_PATH') || define('BASE_PATH', getenv('BASE_PATH') ?: realpath(dirname(__FILE__) . '/../..'));
defined('APP_PATH') || define('APP_PATH', BASE_PATH . '/app');

return new \Phalcon\Config\Config([
    'database' => [
        'adapter'     => 'Mysql',
        'host'        => 'localhost',
        'username'    => 'root',
        'password'    => '',
        'dbname'      => 'phalcon-tutorial',
        'charset'     => 'utf8mb4',
    ],
    'application' => [
        'appDir'         => APP_PATH . '/',
        'controllersDir' => APP_PATH . '/controllers/',
        'modelsDir'      => APP_PATH . '/models/',
        'migrationsDir'  => APP_PATH . '/migrations/',
        'viewsDir'       => APP_PATH . '/views/',
        'pluginsDir'     => APP_PATH . '/plugins/',
        'libraryDir'     => APP_PATH . '/library/',
        'cacheDir'       => BASE_PATH . '/cache/',
        'baseUri'        => '/',
    ]
]);

```

### loader.php

This file registers the directories for the `autoloader`. 

Controllers and models are set in the code as an example.

```php
<?php

$loader = new \Phalcon\Autoload\Loader();

/**
 * We're a registering a set of directories taken from the configuration file
 */
$loader->setDirectories(
    [
        $config->application->controllersDir,
        $config->application->modelsDir,
    ]
)->register();

```




