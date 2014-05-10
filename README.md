![Hoa](http://static.hoa-project.net/Image/Hoa_small.png)

Hoa is a **modular**, **extensible** and **structured** set of PHP libraries.
Moreover, Hoa aims at being a bridge between industrial and research worlds.

# Hoa\Test ![state](http://central.hoa-project.net/State/Test)

This library provides tools to create and run tests for Hoa libraries.

In each library, a `Test/` directory contains test suites. So far, only unit
tests are supported. They are written with [atoum](http://atoum.org/).

## Quick usage

As a quick overview, we see how to execute, write and generate unit tests. Let
`Hoa\Foo` be a library.

### Execute tests

To execute some tests, we will use the `hoa test:run` command. We have several
options to select a set of tests:

  * `-f`/`--files` to select files,
  * `-d`/`--directories` to select directories,
  * `-n`/`--namespaces` to select classes in some namespaces,
  * `-l`/`--libraries` to select all tests of some libraries,
  * `-a`/`--all` to select all tests of all libraries.

Most of the time, we will run all tests of a library, and then all the tests of
all libraries. Thus:

```sh
$ hoa test:run --libraries Foo
# do something
$ hoa test:run --all
```

### Manual unit tests

First, let's create the `Hoa/Foo/Test/Unit/Bar.php` file, that contains:

```php
namespace Hoa\Foo\Test\Unit {

class Bar extends \Hoa\Test\Unit\Suite {

    public function caseBaz ( ) {

        $this->integer(7 * 3 * 2)->isEqualTo(42);
    }
}

}
```

A class represents a test suite (that extends the `Hoa\Test\Unit\Suite` class).
A method represents a test case, where its name must be prefixed by `case`.

The `Hoa\Test` library enables the [Praspel extension for
atoum](http://central.hoa-project.net/Resource/Contributions/Atoum/PraspelExtension).
Consequently, we have the `realdom`, `sample`, `sampleMany` etc. asserters to
automatically generate data.

### Automatically generate unit tests

Thanks to [Praspel](http://central.hoa-project.net/Resource/Library/Praspel), we
are able to automatically generate test suites. Those test suites are compiled
into executable test suites written with atoum's API with the help of the
[Praspel extension for
atoum](http://central.hoa-project.net/Resource/Contributions/Atoum/PraspelExtension).

Let `Hoa\Foo\Baz` be the following class:

```php
namespace Hoa\Foo {

class Baz {

    /**
     * @requires x: /foo.+ba[rz]/;
     * @ensures  \result: true;
     */
    public function qux ( ) {

        // …
    }
}

}
```

Then, to automatically generate a test suite, we will use the `hoa
test:generate` command. It has the following options:

  * `-c`/`--classes` to generate tests of some classes,
  * `-n`/`--namespaces` to generate tests of all classes in some namespaces,
  * `-d`/`--dry-run` to generate tests but output them instead of save them.

The dry-run mode is very helpful. We encourage you to often generate tests with
this option to see what happens. This option is also helpful when having some
errors.

Thus, to automatically generate tests of the `Hoa\Foo\Baz` class, we will make:

```sh
$ hoa test:generate --classes Hoa.Foo.Baz
```

`Hoa.Foo.Baz` is equivalent to `Hoa\\Foo\\Baz`, it avoids to escape backslashes.
Then to execute this test suite, nothing new:

```sh
$ hoa test:run --libraries Foo
```

or

```sh
$ hoa test:run --directories Test/Praspel/
```

to only run the test suite generated by the Praspel tools.

## Environment variables

  * `HOA_ATOUM_BIN`: this variable represents the path to the atoum binary,
  * `HOA_ATOUM_PRASPEL_EXTENSION`: this variable indicates the root of the
    `Atoum\PraspelExtension` library (do not forget the trailing `/`!); useful
    when Hoa is installed through the Central and not Composer.

## Documentation

Different documentations can be found on the website:
[http://hoa-project.net/](http://hoa-project.net/).

## License

Hoa is under the New BSD License (BSD-3-Clause). Please, see
[`LICENSE`](http://hoa-project.net/LICENSE).
