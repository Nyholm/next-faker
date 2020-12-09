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

### Registering an extension

We use PSR-11 to configure extensions:

```php
<?php

use Example\Person;
use Faker\ContainerBuilder;
use Faker\Factory;
use Psr\Container;

$containerBuilder = new ContainerBuilder();

$containerBuilder->register(Person::class);

/** @var Container\ContainerInterface $container */
$container = $containerBuilder->build();

$faker = Factory::create($container);
```

### Implementing an extension

A empty marker interface is provided:

```php
<?php

namespace Faker\Extension;

interface Extension
{
}
```

This marker interface can be implemented to help with auto-discovery of extensions:

```php
<?php

namespace Example;

use Faker\Extension;

final class Person implements Extension\Extension
{
    /**
     * @var array<int, string> 
     */
    private static array $firstNames = [
        // ...        
    ];

    public function firstName(): string
    {
        $key = array_rand(self::$firstNames);
        
        return self::$firstNames[$key];
    }
}
```

A `GeneratorAwareExtension` interface is provided:

```php
<?php

namespace Faker\Extension;

use Faker\Generator;

interface GeneratorAwareExtension extends Extension
{
    public function withGenerator(Generator $generator): self
}
```

This interface can be implemented to gain access to the instance of `Faker\Generator` when returning the extension from the container:

```php
<?php

namespace Example\Person;

use Faker\Extension;
use Faker\Generator;

final class Person implements Extension\GeneratorAwareExtension
{
    private Generator $generator = null;
    
    /**
     * @var array<int, string> 
     */
    private static array $firstNames = [
        // ...        
    ];

    public function firstName(): string
    {
        return $this->generator->randomElement(self::$firstnames);
    }

    public function withGenerator(Generator $generator): self
    {
        $instance  = clone $this;
        
        $instance->generator = $generator;
        
        return $instance;
    }
}
```

A `GeneratorAwareExtensionTrait` is provided with a default implementation of the `GeneratorAwareExtension` interface.

### Using an extension

```php
<?php

use Example\Person;

$faker->ext(Person::class)->firstName();
```

### Example of implementing a core method with an extension

```php
<?php

use Example\Person;

public function firstName() {
    return $faker->ext(Person::class)->firstName();
}
```

Some "core features" in 1.0 like Doctrine support should be moved to an extension
and it will probably live in its own package.

A suggestion about what extension that should be in the core package could be found
in [providers.md](./PROVIDERS.md).

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
$faker->take(5)->name(); // returns 5 names
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
