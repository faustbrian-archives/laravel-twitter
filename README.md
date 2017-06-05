# Laravel Twitter

I would appreciate you taking the time to look at my [Patreon](https://www.patreon.com/faustbrian) and considering to support me if I'm saving you some time with my work.

> A [Twitter](https://twitter.com) bridge for Laravel.

## Installation

Require this package, with [Composer](https://getcomposer.org/), in the root directory of your project.

```bash
$ composer require faustbrian/laravel-twitter
```

## Configuration

Laravel Twitter requires connection configuration. To get started, you'll need to publish all vendor assets:

```bash
$ php artisan vendor:publish --provider="BrianFaust\Twitter\TwitterServiceProvider"
```

This will create a `config/twitter.php` file in your app that you can modify to set your configuration. Also, make sure you check for changes to the original config file in this package between releases.

#### Default Connection Name

This option `default` is where you may specify which of the connections below you wish to use as your default connection for all work. Of course, you may use many connections at once using the manager class. The default value for this setting is `main`.

#### Twitter Connections

This option `connections` is where each of the connections are setup for your application. Example configuration has been included, but you may add as many connections as you would like.

## Usage

#### TwitterManager

This is the class of most interest. It is bound to the ioc container as `twitter` and can be accessed using the `Facades\Twitter` facade. This class implements the ManagerInterface by extending AbstractManager. The interface and abstract class are both part of [Graham Campbell's](https://github.com/GrahamCampbell) [Laravel Manager](https://github.com/GrahamCampbell/Laravel-Manager) package, so you may want to go and checkout the docs for how to use the manager class over at that repository. Note that the connection class returned will always be an instance of `Twitter\Twitter`.

#### Facades\Twitter

This facade will dynamically pass static method calls to the `twitter` object in the ioc container which by default is the `TwitterManager` class.

#### TwitterServiceProvider

This class contains no public methods of interest. This class should be added to the providers array in `config/app.php`. This class will setup ioc bindings.

### Examples

Here you can see an example of just how simple this package is to use. Out of the box, the default adapter is `main`. After you enter your authentication details in the config file, it will just work:

```php
// You can alias this in config/app.php.
use BrianFaust\Twitter\Facades\Twitter;

Twitter::get("account/verify_credentials");
// We're done here - how easy was that, it just works!
```

The Twitter manager will behave like it is a `Twitter\Twitter`. If you want to call specific connections, you can do that with the connection method:

```php
use BrianFaust\Twitter\Facades\Twitter;

// Writing this…
Twitter::connection('main')->get("account/verify_credentials");

// …is identical to writing this
Twitter::get("account/verify_credentials");

// and is also identical to writing this.
Twitter::connection()->get("account/verify_credentials");

// This is because the main connection is configured to be the default.
Twitter::getDefaultConnection(); // This will return main.

// We can change the default connection.
Twitter::setDefaultConnection('alternative'); // The default is now alternative.
```

If you prefer to use dependency injection over facades like me, then you can inject the manager:

```php
use BrianFaust\Twitter\TwitterManager;

class Foo
{
    protected $twitter;

    public function __construct(TwitterManager $twitter)
    {
        $this->twitter = $twitter;
    }

    public function bar()
    {
        $this->twitter->get("account/verify_credentials");
    }
}

App::make('Foo')->bar();
```

## Documentation

There are other classes in this package that are not documented here. This is because the package is a Laravel wrapper of [the official Twitter package](https://twitteroauth.com/).

## Testing

``` bash
$ phpunit
```

## Security

If you discover a security vulnerability within this package, please send an e-mail to Brian Faust at hello@brianfaust.de. All security vulnerabilities will be promptly addressed.

## Credits

- [Brian Faust](https://github.com/faustbrian)
- [All Contributors](../../contributors)

## License

[MIT](LICENSE) © [Brian Faust](https://brianfaust.de)
