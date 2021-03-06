NewRelic module for ZF2
=======================

NewRelic module provide an object-oriented PHP wrapper for [New Relic](http://newrelic.com/) monitoring service.

[![Build Status](https://secure.travis-ci.org/neeckeloo/NewRelic.png?branch=master)](http://travis-ci.org/neeckeloo/NewRelic)
[![Latest Stable Version](https://poser.pugx.org/neeckeloo/NewRelic/v/stable.png)](https://packagist.org/packages/neeckeloo/NewRelic)
[![Coverage Status](https://coveralls.io/repos/neeckeloo/NewRelic/badge.png)](https://coveralls.io/r/neeckeloo/NewRelic)
[![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/neeckeloo/NewRelic/badges/quality-score.png?s=d8f10c2b5c49a2cebe53b533b7a281368b8ddb07)](https://scrutinizer-ci.com/g/neeckeloo/NewRelic/)
[![Dependencies Status](https://d2xishtp1ojlk0.cloudfront.net/d/6979063)](http://depending.in/neeckeloo/NewRelic)

Introduction
------------

NewRelic module provide a logger and a wrapper for [New Relic PHP API](https://newrelic.com/docs/php/the-php-api).

The current route is used to set the name of each transaction. Moreover, the module allow exceptions logging if enabled.

Default configuration
---------------------

```php
return array(
    'newrelic' => array(
        // Sets the newrelic app name.  Note that this will discard metrics
        // collected before the name is set.  If empty then your php.ini
        // configuration will take precedence.
        'application_name' => null,

        // May be null and will only be set if application name is also given.
        'license' => null,

        // If false then neither change the auto_instrument or manually
        // instrument the real user monitoring.
        'browser_timing_enabled' => false,

        // When true tell the newrelic extension to insert Real User Monitoring
        // scripts automatically.
        'browser_timing_auto_instrument' => true,

        // When true, a logger with the newrelic writer will be called for
        // dispatch error events.
        'exceptions_logging_enabled' => false,

        // Defines ignored transactions
        'ignored_transactions' => array(),

        // Defines background job transactions
        'background_jobs' => array(),
    ),
);
```

Usage
-----

### Ignore transactions

NewRelic API allows to ignore some transactions. This configuration defines some routes and controllers of transactions that will be ignored.

#### Ignore routes

```php
return array(
    'newrelic' => array(
        'ignored_transations' => array(
            'routes' => array(
                'admin*',
                'user/login',
            ),
        ),
    ),
);
```

Those rules ignore all admin routes and the "user/login" route.

#### Ignore controllers

```php
return array(
    'newrelic' => array(
        'ignored_transations' => array(
            'controllers' => array(
                'FooController',
                'BarController',
                'BazController',
            ),
        ),
    ),
);
```

You can also ignore some actions of specified controllers :

```php
return array(
    'newrelic' => array(
        'ignored_transations' => array(
            'controllers' => array(
                array('FooController', array('foo', 'bar')),
                array('BarController', array('baz')),
            ),
        ),
    ),
);
```

#### Ignore a transaction manually

You can ignore a transaction manually by calling ```ignoreTransaction()``` method of NewRelic client.

```php
$client = $this->getServiceLocator()->get('NewRelic\Client');
$client->ignoreTransaction();
```

### Define background jobs

The configuration of background jobs is identical to ignored transactions but use the key ```background_jobs``` as below.

```php
return array(
    'newrelic' => array(
        'background_jobs' => array(),
    ),
);
```

#### Defines a background job manually

You can define a transaction as background job by calling ```backgroundJob()``` method of NewRelic client.

```php
$client = $this->getServiceLocator()->get('NewRelic\Client');
$client->backgroundJob(true);
```

### Add custom metric

```php
$client = $this->getServiceLocator()->get('NewRelic\Client');
$client->addCustomMetric('salesprice', $price);
```
