[![Latest Stable Version](https://poser.pugx.org/arrilot/laravel-data-anonymization/v/stable.svg)](https://packagist.org/packages/arrilot/laravel-data-anonymization/)
[![CircleCI](https://circleci.com/gh/atabix/laravel-data-anonymization.svg?style=svg)](https://github.com/atabix/laravel-data-anonymization)
[![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/arrilot/laravel-data-anonymization/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/arrilot/laravel-data-anonymization/)

# Laravel Data Anonymization

* This is a bridge package for a full integration of `arrilot/data-anonymization` into Laravel framework.

## Installation

1. ```composer require arrilot/laravel-data-anonymization```

2. Add `database/anonymizatio`n to `composer.json -> autoload -> classmap`

3. `php artisan anonymization:install`


## Usage

The package is designed to be as much consistent with Laravel built-in seeders as possible.

### Bootstrapping

`php artisan anonymization:install` creates two files:

1) `database/anonymization/DatabaseAnonymizer.php`

```php
<?php

use Arrilot\LaravelDataAnonymization\AbstractAnonymizer;

class DatabaseAnonymizer extends AbstractAnonymizer
{
    /**
     * Run the database anonymization.
     *
     * @return void
     */
    public function run()
    {
        $this->call(UserTableAnonymizer::class);
    }
}

```

2) `database/anonymization/UserTableAnonymizer.php`

```php
<?php

use Arrilot\DataAnonymization\Blueprint;
use Arrilot\LaravelDataAnonymization\AbstractAnonymizer;
use Faker\Generator as Faker;

class UserTableAnonymizer extends AbstractAnonymizer
{
    /**
     * Run the database anonymization.
     *
     * @return void
     */
    public function run()
    {
        // For more info about this part read here https://github.com/arrilot/data-anonymization
        $this->table('users', function (Blueprint $table) {

            $table->column('email')->replaceWith(function(Faker $faker) {
                return $faker->unique()->email;
            });

            $table->column('name')->replaceWith('John Doe');
        });
    }
}

```

`DatabaseAnonymizer` is an entry point into anonymization. It runs other anonymizers.
`UserTableAnonymizer` is a useful built-in example. You can modify it and create other anonymizers for other table using generator.

### Generator command

`php artisan make:anonymizer ProfileTableAnonymizer`. Similar to `make:seeder`

### Running the anonymization

Anonymization is performed using `php artisan db:anonymize` command.
Its signature is identical with `db:seed` command.

