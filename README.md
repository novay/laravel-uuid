# Laravel UUID (Universally Unique Identifier)

[![Total Downloads](https://poser.pugx.org/novay/uuid/d/total.svg)](https://packagist.org/packages/novay/uuid)
[![Build Status](https://travis-ci.org/novay/laravel-uuid.svg?branch=master)](http://travis-ci.org/novay/laravel-uuid)
[![Latest Stable Version](https://poser.pugx.org/novay/uuid/v/stable.svg)](https://packagist.org/packages/novay/uuid)
[![Latest Unstable Version](https://poser.pugx.org/novay/uuid/v/unstable.svg)](https://packagist.org/packages/novay/uuid)
[![License](https://poser.pugx.org/novay/uuid/license.svg)](https://raw.githubusercontent.com/novay/laravel-auth/LICENSE)

Laravel package to generate a UUID according to the RFC 4122 standard. UUID Versions 1, 3, 4 and 5 are supported. With MIT license.

- [About](#about)
- [Requirements](#requirements)
- [Installation](#installation)
    - [Laravel 5.5 and above](#laravel-5.5-and-above)
    - [Laravel 5.4 and below](#laravel-5.4-and-below)
- [Basic Usage](#basic-usage)
- [Advanced Usage](#advanced-usage)
    - [UUID creation](#uuid-creation)
        - [UUID V1](#uuid-v1)
        - [UUID V3](#uuid-v3)
        - [UUID V4](#uuid-v4)
        - [UUID V5](#uuid-v5)
- [Additional Features](#additional-features)
    - [Import UUID](#import-uuid)
    - [Extract time](#extract-time)
    - [Extract Version](#extract-version)
    - [Eloquent UUID Generation](#eloquent-uuid-generation)
    - [Model Binding to UUID instead of Primary Key](#model-binding-to-uuid-instead-of-primary-key)
    - [Validation](#validation)
- [License](#license)
- [Notes](#notes)
- [Credits](#credits)

### About
Since Laravel `4.*` and `5.*` both rely on either `OpenSSL` or `Mcrypt`, the pseudo random byte generator now tries to use one of them. If both cannot be used (not a Laravel project?), the 'less random' `mt_rand()` function is used.

### Requirements
* [Laravel 5.3, 5.4 or 5.5+](https://laravel.com/docs/installation)

### Installation

##### Laravel 5.5 and above
1. From your projects root folder in terminal run:

```bash
    composer require novay/laravel-uuid
```

* Uses package auto discovery feature, no need to edit the `config/app.php` file.

##### Laravel 5.4 and below
1. From your projects root folder in terminal run:

```bash
    composer require novay/laravel-uuid:2.0
```

2. Register the package with laravel in `config/app.php` under `aliases` with the following:

```php
    'aliases' => [
        'Uuid' => novay\Uuid\Uuid::class,
    ];
```

### Basic Usage

To quickly generate a UUID just do

```php
    Uuid::generate()
```
* This will generate a version 1 with a random ganerated MAC address.

To echo out the generated Uuid cast it to a string

```php
(string) Uuid::generate()
```

or

```php
Uuid::generate()->string
```

### Advanced Usage

#### UUID creation

##### UUID V1
Generate a version 1, time-based, UUID. You can set the optional node to the MAC address. If not supplied it will generate a random MAC address.

```php
Uuid::generate(1,'00:11:22:33:44:55');
```

##### UUID V3
Generate a version 3, name-based using MD5 hashing, UUID

```php
Uuid::generate(3,'test', Uuid::NS_DNS);
```

##### UUID V4
Generate a version 4, truly random, UUID

```php
Uuid::generate(4);
```

##### UUID V5
Generate a version 5, name-based using SHA-1 hashing, UUID

```php
Uuid::generate(5,'test', Uuid::NS_DNS);
```

### Additional Features
###### Import UUID
* To import a UUID

```php
$uuid = Uuid::import('d3d29d70-1d25-11e3-8591-034165a3a613');
```

###### Extract Time
* Extract the time for a time-based UUID (version 1)

```php
$uuid = Uuid::generate(1);
dd($uuid->time);
```

###### Extract Version
* Extract the version of an UUID

```php
$uuid = Uuid::generate(4);
dd($uuid->version);
````

###### Eloquent UUID Generation

If you want an UUID magically be generated in your Laravel models, just add this boot function to your Model.

```php
/**
 *  Setup model event hooks
 */
public static function boot()
{
    parent::boot();
    self::creating(function ($model) {
        $model->uuid = (string) Uuid::generate(4);
    });
}
```
This will generate a version 4 UUID when creating a new record.

###### Model Binding to UUID instead of Primary Key

If  you want to use the UUID in URLs instead of the primary key, you can add this to your model (where 'uuid' is the column name to store the UUID)

```php
/**
 * Get the route key for the model.
 *
 * @return string
 */
public function getRouteKeyName()
{
    return 'uuid';
}
```

When you inject the model on your resource controller methods you get the correct record

```php
public function edit(Model $model)
{
   return view('someview.edit')->with([
        'model' => $model,
    ]);
}
```

###### Validation

Just use like any other Laravel validator.

``'uuid-field' => 'uuid'``

Or create a validator from scratch. In the example an Uuid object in validated. You can also validate strings `$uuid->string`, the URN `$uuid->urn` or the binary value `$uuid->bytes`

```php
$uuid = Uuid::generate();
$validator = Validator::make(['uuid' => $uuid], ['uuid' => 'uuid']);
dd($validator->passes());
```

### Credits
* Full development credit must go to [webpatser](https://github.com/webpatser). This package was forked and modified to be compliant with [MIT](https://opensource.org/licenses/MIT) licensing standards for production use.

## Notes
Full details on the UUID specification can be found [here](http://tools.ietf.org/html/rfc4122)

## License
Laravel UUID is licensed under the MIT license for both personal and commercial products. Enjoy!