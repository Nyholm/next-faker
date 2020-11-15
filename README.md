# Faker 2.0

Here are some rough ideas on how next version of Faker will work.

## Basic usage

```php
use Faker\Factory;

$faker = Factory::create();
$faker->firstname();
$faker->address();
$faker->city();
$faker->maleTitle();
```

### Language specific

Get faker with the German provider for names, address etc.

```php
use Faker\Factory;
use Faker\German\Provider\GermanDE;

$faker = Factory::create(new GermanDE());
$faker->firstname(); // German first name
```

Faker can support multiple languages
```php
use Faker\Factory;
use Faker\German\Provider\GermanDE;
use Faker\Swedish\Provider\SwedishSE;
use Faker\Swedish\English\EnglishNZ;
use Faker\Provider\EnglishUS;

$faker = Factory::create([new EnglishUS(), new GermanDE(), new SwedishSE()]);
$faker->firstname(); // random locale.

// Will return an German only clone.
$faker->withLocale(GermanDE::class);
$faker->withLocale(GermanDE::class)->firstname(); // German first name

// Throws an exception since the provider is not configured.
$faker->withLocale(EnglishNZ::class)->firstname();
```

## Extensions

The "core" `Faker\Generator` contains a set of predefined of methods; `firstname()`,
`address()`, `companyName()` etc.

They can be localized with "Language providers". To extend functionality, you need
an `Extension`.

Here is how to use an extension:

```php
$faker->ext(Foo::class)->bar();
```

We use PSR-11 to configure extensions:

```php
use Faker\Factory;

$builder = new ContainerBuilder();
$builder->register(Foo:class);
$psr11Container = $builder->build();

$faker = Factory::create(null, $psr11Container);
```

If a Extension implements `GeneratorAwareInterface`, it will be provider a generator
just after it is returned from the container.

```php
use Faker\Extension\GeneratorAwareInterface;
use Faker\Generator;

class Foo implements GeneratorAwareInterface
{
    private $generator;

    public function bar()
    {
        $format = '...';

        return $this->generator->parse($format);
    }

    public function setGenerator(Generator $g)
    {
        $this->generator = $g;
    }
}
```

## Modifiers

```php

use Faker\Factory;

$faker = Factory::create();
$faker->withUnique()->name();
$faker->withMaybe(0.8)->name();
$faker->withValid(fn($v) => strlen($v) > 3))->name();
```

We can keep the modifier as Proxy classes like they are in 1.0.

```php
class Generator {
  private $unique;

  public function __construct()
  {
     // ..

     $this->unique = new UniqueGenerator($this);
  }

  /**
   * @return self The UniqueGenerator is just a proxy
   */
  public function withUnique()
  {
    return $this->unique;
  }

  /**
   * @return self The UniqueGenerator is just a proxy
   */
  public function withUnique()
  {
    return $this->unique;
  }

  /**
   * @return self
   */
  public function withMaybe($weight = 0.5, $default = null)
  {
    if (is_int($weight) && mt_rand(1, 100) <= $weight) {
      return $this;
    }

    return new DefaultGenerator($default);
  }

  /**
   * @return self
   */
  public function withValid(callable $validator, int $maxRetries = 10000)
  {
    return new ValidGenerator($this, $validator, $maxRetries);
  }
}
```

## Packages

There should be a `fakerphp/faker` "core" package with one English US Provider.

Other languages are split into separate packages like;

- `fakerphp/spanish` (contains es_AR, es_ES, es_PE, es_VE)
- `fakerphp/frensh` (contains fr_BE, fr_CA, fr_CH, fr_FR)
- `fakerphp/german`
- `fakerphp/swedish`
- etc

The language specific packages contains language Providers and possibly langauge
specific extensions. They are maintained and versioned separately from the "core"
package.

## Magic

All classes has defined methods. No magic `__call()` or `__get()`. It is only the
Modifier proxy classes that includes the magic `__get()`.

This will help IDE auto completion.

