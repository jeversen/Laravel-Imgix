# Laravel Imgix

Laravel package for generating [Imgix](https://www.imgix.com) URLs for your images.

Multi-domains is supported!

## Dependencies

- [PHP](https://secure.php.net): ^7.1
- [imgix/imgix-php](https://github.com/imgix/imgix-php): ^2.1
- [illuminate/support](https://github.com/illuminate/support): ^5.0

## Install

You can install the package via [Composer](https://getcomposer.org/)
```bash
$ composer require otisz/laravel-imgix
```

In Laravel 5.5 or above the service provider will automatically get registered. In older versions of the framework just add the service provider in `config/app.php` file:
```php
'providers' => [
    ...
    Otisz\Laravel\Imgix\ImgixServiceProvider::class,
    ...
],

'aliases' => [
    ...
    'Imgix' =>  Otisz\Laravel\Imgix\Facades\Imgix::class,
    ...
],
```

You can publish the config file with:
```bash
$ php artisan vendor:publish --tag=imgix
```

When published, [the `config/imgix.php` config](config/imgix.php) file contains:

```php
return [
    /*
    |--------------------------------------------------------------------------
    | ImgIX Domains
    |--------------------------------------------------------------------------
    |
    | Domain sharding enables you to spread image requests across
    | multiple domains. This allows you to bypass the requests-per-host
    | limits of browsers. We recommend 2-3 domain shards maximum if you are
    | going to use domain sharding.
    |
    | @link https://github.com/imgix/imgix-php#domain-sharded-urls
    |
    */
    'domains' => [
        env('IMGIX_DOMAIN', 'example.imgix.net'),
        env('IMGIX_DOMAIN2'),
        env('IMGIX_DOMAIN3'),
    ],

    /*
    |--------------------------------------------------------------------------
    | ImgIX Domains
    |--------------------------------------------------------------------------
    |
    | By default, shards are calculated using a checksum so that the image
    | path always resolves to the same domain. This improves caching in the
    | browser. However, you can supply a different strategy that cycles
    | through domains instead.
    |
    | Supported: \Imgix\ShardStrategy::CRC, \Imgix\ShardStrategy::CYCLE
    |
    */
    'shardStrategy' => \Imgix\ShardStrategy::CRC,

    /*
    |--------------------------------------------------------------------------
    | ImgIX HTTPS
    |--------------------------------------------------------------------------
    |
    | For HTTPS support.
    |
    */
    'useHttps' => env('IMGIX_HTTPS', false),

    /*
    |--------------------------------------------------------------------------
    | ImgIX Signed URLs
    |--------------------------------------------------------------------------
    |
    | To produce a signed URL, you must enable secure URLs on your source
    | and then provide your signature key to the URL builder.
    |
    | @link https://github.com/imgix/imgix-php#signed-urls
    |
    */
    'signKey' => env('IMGIX_SIGNKEY', ''),

    /*
    |--------------------------------------------------------------------------
    | ImgIX Library Param
    |--------------------------------------------------------------------------
    |
    | For security and diagnostic purposes, we default to signing all requests
    | with the language and version of library used to generate the URL.
    |
    */
    'includeLibraryParam' => env('IMGIX_LIBRARY', true),
];
```
    
## Usage

### Facade

```php
Imgix::createUrl(string $path, array $params = []): string
```

`$path`: The path of the image \
`$params`: The parameters provided by [Imgix](https://docs.imgix.com/apis/url)

```php
Imgix::createUrl('bridge.png', ['w' => 100, 'h' => 100])

// Prints out:
// http://example.imgix.net/bridge.png?w=100&h=100
```

### Helper

```php
imgix(string $path, array $params = []): string
```

`$path`: The path of the image \
`$params`: The parameters provided by [Imgix](https://docs.imgix.com/apis/url)

```php
Imgix::createUrl('bridge.png', ['w' => 100, 'h' => 100])

// Prints out:
// http://example.imgix.net/bridge.png?w=100&h=100
```
    
## Testing

``` bash
$ composer lint
$ composer test
```

## Contributing

### Security

If you discover any security-related issues, please email [leventeotta@gmail.com](mailto:leventeotta@gmail.com) instead of using the issue tracker.

## Licence

The MIT Licence (MIT). Please see [License File](LICENSE.md) for more information.