## This package is abandoned

> Since Zend migrated to be [Laminas](https://getlaminas.org/), now I created a new [package](https://github.com/adrosoftware/lmrp-loader) for [Mezzio](https://docs.mezzio.dev/)

# Zend Expressive Routes and Pipeline Loader

[![Build status][Master image]][Master]
[![Coverage Status][Master coverage image]][Master coverage]
[![Latest Stable Version][Stable version image]][Stable version]
[![License][License image]][License]

## Purpose

When building a medium to large applications on Zend Expressive is better if you can organize your routes. By default *expressive* define all the routes in the `routes.php` file under the `config` directory. For me is better if you can at least organize the routes by modules of routes prefix. For example `routes.web.php` for all the web routes and `routes.api.php ` for al the api routes.

## Usage

```bash
$ composer require adrosoftware/zerp-loader
```

The `public/index.php` file by default look like this:

```php
<?php

declare(strict_types=1);

// Delegate static file requests back to the PHP built-in webserver
if (PHP_SAPI === 'cli-server' && $_SERVER['SCRIPT_FILENAME'] !== __FILE__) {
    return false;
}

chdir(dirname(__DIR__));
require 'vendor/autoload.php';

/**
 * Self-called anonymous function that creates its own scope and keep the global namespace clean.
 */
(function () {
    /** @var \Psr\Container\ContainerInterface $container */
    $container = require 'config/container.php';

    /** @var \Zend\Expressive\Application $app */
    $app = $container->get(\Zend\Expressive\Application::class);
    $factory = $container->get(\Zend\Expressive\MiddlewareFactory::class);

    // Execute programmatic/declarative middleware pipeline and routing
    // configuration statements
    (require 'config/pipeline.php')($app, $factory, $container);
    (require 'config/routes.php')($app, $factory, $container);

    $app->run();
})();
```

Assuming you have `config/routes.web.php` and `config/routes.api.php` and so on, then replace:

```php
(require 'config/routes.php')($app, $factory, $container);
```

With something like this:

```php
(new \AdroSoftware\Zerp\Loader('config/routes.*.php'))->load($app, $factory, $container);
```

## Authors:

[Adro Rocker](https://github.com/adrorocker).

  [Master]: https://travis-ci.org/adrosoftware/zerp-loader/
  [Master image]: https://travis-ci.org/adrosoftware/zerp-loader.svg?branch=master
  [Master coverage]: https://coveralls.io/github/adrosoftware/zerp-loader
  [Master coverage image]: https://coveralls.io/repos/github/adrosoftware/zerp-loader/badge.svg?branch=master
  [Stable version]: https://packagist.org/packages/adrosoftware/zerp-loader
  [Stable version image]: https://poser.pugx.org/adrosoftware/zerp-loader/v/stable
  [License]: https://packagist.org/packages/adrosoftware/zerp-loader
  [License image]: https://poser.pugx.org/adrosoftware/zerp-loader/license
