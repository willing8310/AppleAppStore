Apple AppStore
==============

AppStore element
----------------

This component created for control apple AppStores

Each AppStore has country name by [ISO_3166-1_alpha-2] (http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)

All AppStores implements of AppStoreInterface and have PriceTrasnformer for this AppStore

Example usage US Store:

```php
use Apple\AppStore\Stores\USStore;

$usStore = new USStore;

var_dump($usStore);

// Get price transformer
$usPriceTransformer = $usStore->getPriceTransformer();
```


US Store have a default PriceTransformer


PriceTransformer component
--------------------------

Each price transformer can transform and reverse transform price.
Default price transformer can't trasnforms.

Example usage Russian price transformer:

```php
use Apple\AppStore\Stores\RUPriceTransformer;

$ruPrice = new RUPriceTransformer;

$ruPrice
    ->setPrices(array(
        '0.99' => 33,
        '1.99' => 66
        // Etc...
    ));

var_dump($ruPrice);

// Transform price
// RUB -> USD
var_dump($ruPrice->transform(1.99));

// Reverse transform
// USD -> RUB
var_dump($ruPrice->reverse(66));
```

#### ATTENTION:

Prices map usage string variables, because not correct work float variables as array key.


Create new AppStore
-------------------

AppStore must be implements AppStore interface

Example:

```php
use Apple\AppStore\Stores\AbstractStore;
use Apple\AppStore\Stores\DefaultPriceTransformer;


class MyAppStore extends AbstractStore
{
    /**
     * Get county code in ISO2A
     *
     * @return string
     */
    public function getCountryISO()
    {
        // Example as United States
        return 'us';
    }

    /**
     * Get URI prefix for usage other components
     * (AppleParse)
     *
     * @return string
     */
    public function getUriPrefix()
    {
        // Example as United States
        return '/us/';
    }

    /**
     * Get default price transformer
     *
     * @return PriceTransformerInterface
     */
    public function getDefaultPriceTransformer()
    {
        return new DefaultPriceTransformer();
    }
}


$myAppStore = new MyAppStore;

var_dump($myAppStore);
```

Create new PriceTransformer
---------------------------

Example:

#### ATTENTION:

Prices map usage string variables, because not correct work float variables as array key.

```php
<?php

$loader = include __DIR__ . '/vendor/autoload.php';

use Apple\AppStore\Stores\AbstractPriceTransformer;


class MyPriceTransformer extends AbstractPriceTransformer
{
    /**
     * Get default prices
     *
     * @return array
     */
    public function getDefaultPrices()
    {
        return array(
            '0.99' => 1,
            '1.99' => 2,
            '2.99' => 3
            // etc...
        );
    }
}


$myPriceTransformer = new MyPriceTransformer;

var_dump($myPriceTransformer);
```