# Pearify — Like Browserify, For PEAR compliance

Pearify is a tool to rewrite PSR-0 and PSR-4 PHP class names to the PEAR naming conventions. Pearify
is based on the excellent PHP class-rewriting project by Michael Tibben.

Requirements
------------

Pearify requires PHP version 5.4 or greater.

Installation
------------

The simplest way to get Pearify is via Packagist. Simply add `mridang/pearify` to your composer JSON's
list of requirements and run `composer install`.

About
------------

Pearify relies heavily on Composer and works only on Composer-based projects. Pearify uses the classmap
generated by Composer to get a list of PHP classes to be renamed.

Pearify requires that you have dumped the optimised autoload classmap so it can include the
array of file paths. You can generate the classmap by invoking

```
composer dump-autoload --optimize
```

Running the aforementioned command, will generate a file called `autoload_classmap.php` in the 
`vendor\composer` directory. More information on the `dump-autoload` directive can be found at
https://getcomposer.org/doc/03-cli.md#dump-autoload. 

The classmap contains an exhaustive list of all the PHP classnames (including paths of dev 
dependencies) and the relative file paths. While this file contains classes from development
dependencies as well, they are automatically excluded by Pearify.

Pearify reads your project's `composer.json` file to recursively deduce all the production
dependencies of your project and only select classes from the classmap belonging to your project's
production dependencies.

If you had a PHP class file called `MyClass.php` in the directory `vendor/myname/myproject/`

```php
namespace MyName\MyProject;

use DateTime;

class MyClass {

    public function getDate() {
        return new DateTime("2014-10-20");
    }
}
```

It would be renamed and copied over to a file name `lib\MyName\MyProject\MyClass.php`

```php
class MyName_MyProject_MyClass {

    public function getDate() {
        return new DateTime("2014-10-20");
    }
}
```

The original file is left untouched and the new file is created (or overwritten) with all the `use`
and `namespace` statements removed.


Gotchas
-------

Pearify has quite a few shortcomings. If you'd like to see support for one of the following added to
Composer, please create a pull-request.

* Only the `vendor` directory is processed. No support for the `vendor-dir` property as shown at 
https://getcomposer.org/doc/06-config.md#vendor-dir
* No support for the suggested dependencies using the `suggests` property as shown at
https://getcomposer.org/doc/04-schema.md#suggest
* Files without any classes are excluded
* Resources in the dependencies are excluded
* Tests in the dependencies are excluded

Authors
-------

* Mridang Agarwalla
* Hannu Pölönen

Credits
-------

* Michael Tibben (@mtibben)

License
-------

Pearify is licensed under the MIT License - see the LICENSE file for details.
