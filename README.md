# Awin Product SDK
An inofficial PHP SDK to grab product data from Awin.

## Installation
```
composer require ckdot/awin-product-sdk
```

## Usage

```php
<?php
use AwinProductSdk\ValueObjects\Advertiser;
use AwinProductSdk\ValueObjects\Product;

require_once 'vendor/autoload.php';

$apiKey = 'your-api-key';
$factory = new \AwinProductSdk\Factory\Factory();
$config = new \AwinProductSdk\ValueObjects\Config(
    'https://productdata.awin.com/datafeed/list/apikey/' . $apiKey,
    new \Symfony\Component\Cache\Simple\FilesystemCache()
);
$aWin = $factory->createFacade($config);


$advertisers = $aWin->getAdvertisers();
$activeAdvertisers = $aWin->filterAdvertisers(Advertiser::KEY_STATUS, 'active', $advertisers);

/**
 * @var Advertiser $advertiser
 */
foreach ($activeAdvertisers as $index => $advertiser) {
    $products = $aWin->getProducts($advertiser);
    $shirts   = $aWin->filterProductsByRegExp(Product::KEY_NAME, '/shirt/i', $products);

    echo $advertiser->getId().': ' . $advertiser->getName() .PHP_EOL;

    /**
     * @var Product $shirt
     */
    foreach ($shirts as $shirt) {
        echo '- ' . $shirt->getId() . ': ' . $shirt->getName().PHP_EOL;
    }
}

```

## How it works
The SDK is using the Awin product feed CSV files to read out advertiser and product data.
Basically parsing of these data is quite fast but still you should be aware that some product feeds have a size of several megabytes.
So I recommand to fetch data via SDK in background (cronjob) and store them into your own database.
Make sure you set the memory limit of your PHP instance to a proper size. 
## Documentation

There is no documention. If you want to know how it works in detail, read the code.


## License

MIT:
https://en.wikipedia.org/wiki/MIT_License
