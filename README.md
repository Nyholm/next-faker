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

    public function bar() {
        $format = '...';

        return $this->generator->parse($format);
    }

    public function setGenerator(Generator $g) {
        $this->generator = $g;
    }
}
```

## Modifiers

We can keep the modifier as is:

```php
class Generator {
  private $unique;

  /**
   * @return self The UniqueGenerator is just a proxy
   */
  public function unique()
  {
    return $this->unique ?? $this->unique = new UniqueGenerator($this);
  }
}
```