# Faker 2.0

Here are some rough ideas on how I think next version of Faker should work.

## Basic usage

```php
use Faker\Factory;

$faker = Factory::create();
$faker->firstname();
$faker->address();
$faker->city();
$faker->maleTitle();
```

## Extensions

All features should be implemented as an "extension". An extension is always single
locale. The "core" `Faker\Generator` contains a set of predefined of methods; `firstname()`,
`address()`, `companyName()` etc, which is just syntactic sugar for calling the
extensions.

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

If a Extension implements `GeneratorAwareInterface`, it will be provided a `Generator`
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

    public function withGenerator(Generator $g)
    {
        $self = clone $this;
        $self->generator = $g;
        return $self;
    }
}
```

Some "core features" in 1.0 like Doctrine support should be moved to an extension
and it will probably live in its own package.

Example implementation of core methods:

```php
public function firstName() {
    return $faker->ext(PersonInterface::class)->firstName();
}
```

## Packages

There should be a `fakerphp/faker` "core" package with one English US Provider.
That way we know that Faker will work "out of the box".

Other languages are split into separate packages like;

- `fakerphp/spanish` (contains es_AR, es_ES, es_PE, es_VE)
- `fakerphp/french` (contains fr_BE, fr_CA, fr_CH, fr_FR)
- `fakerphp/german`
- `fakerphp/swedish`
- etc

The language specific packages contains a localed version of the "core" English
extensions and possibly language specific extensions. They are maintained and versioned
separately from the "core" package.

## Loading multiple languages

The default way of instantiating a Generator is with a factory method.
The factory method loads all default extensions. If the factory
method lives in `Generator::create()` or `Factory::create()` in an implementation
detail.

There could be multiple factory methods, one for each locale.

```php
use Faker\German\GermanDE;
use Faker\English\EnglishNZ;

$fakerEn = EnglishNZ::create();
$fakerDe = GermanDE::create();
```

One could of course build up all the extensions without a factory method, ie
like using dependency injection.

If multiple instances of the `PersonInterface` is provided, we should just pick
one at random.

```php
use Faker\German\German\Person as PersonDe;
use Faker\English\English\Nz\Person as PersonNz;

$faker = // build a generator some how with both PersonDe and PersonNz
$faker->firstName(); // German or English name
$faker->ext(PersonInterface::class)->firstName(); // Same as above

$faker->ext(PersonDe::class)->firstName(); // German name
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

## Magic

All classes has defined methods. No magic `__call()` or `__get()`. It is only the
Modifier proxy classes that includes the magic `__get()`.

This will help IDE auto completion.

## Reproducible builds

If someone uses a "seed" to make sure the the generated content is reproducible
over multiple runs, they can only expect that to be true for the same version of
Faker.

```php
use Faker\Factory;

$faker = Factory::create();
$faker->seed(1);
```
