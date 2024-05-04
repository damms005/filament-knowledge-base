![filament-knowledge-base Banner](docs/images/banner.jpg)


# A filament plugin that adds a knowledge base and documentation to your filament panel(s).

[![Latest Version on Packagist](https://img.shields.io/packagist/v/guava/filament-knowledge-base.svg?style=flat-square)](https://packagist.org/packages/guava/filament-knowledge-base)
[![GitHub Tests Action Status](https://img.shields.io/github/actions/workflow/status/guava/filament-knowledge-base/run-tests.yml?branch=main&label=tests&style=flat-square)](https://github.com/guava/filament-knowledge-base/actions?query=workflow%3Arun-tests+branch%3Amain)
[![GitHub Code Style Action Status](https://img.shields.io/github/actions/workflow/status/guava/filament-knowledge-base/fix-php-code-style-issues.yml?branch=main&label=code%20style&style=flat-square)](https://github.com/guava/filament-knowledge-base/actions?query=workflow%3A"Fix+PHP+code+style+issues"+branch%3Amain)
[![Total Downloads](https://img.shields.io/packagist/dt/guava/filament-knowledge-base.svg?style=flat-square)](https://packagist.org/packages/guava/filament-knowledge-base)

Did your filament panel ever get complex real quick? Ever needed to a place to document all your features in one place?

Filament Knowledge Base is here for exactly this reason!

Using our Knowledge Base package, you can write markdown documentation files to document every feature of your package and give your users a comprehensive knowledge base tailored for your product. Right inside Filament!

## Showcase

This is where your screenshots and videos should go. Remember to add them, so people see what your plugin does.

## Support us

Your support is key to the continual advancement of our plugin. We appreciate every user who has contributed to our journey so far.

While our plugin is available for all to use, if you are utilizing it for commercial purposes and believe it adds significant value to your business, we kindly ask you to consider supporting us through GitHub Sponsors. This sponsorship will assist us in continuous development and maintenance to keep our plugin robust and up-to-date. Any amount you contribute will greatly help towards reaching our goals. Join us in making this plugin even better and driving further innovation.

## Installation

You can install the package via composer:

```bash
composer require guava/filament-knowledge-base
```

You can publish the config file with:

```bash
php artisan vendor:publish --tag="filament-knowledge-base-config"
```

This is the contents of the published config file:

```php
return [
];
```

## Introduction
### Knowledge Base Panel
We register a separate panel for your entire Knowledge Base. This way you have a single place where you can in detail document your functionalities.

### Modal Previews

## Storage
We currently support flat file (stored inside the source project) storage out of the box.

You can choose to store your documentation in:
 - Markdown files (Preferred method)
 - PHP classes (for complex cases)

In the future, we plan to also ship a Database Driver so you can store your documentation in the database.

## Usage

### Register plugin
To begin, register the `KnowledgeBasePlugin` in all your Filament Panels from which you want to access your Knowledge Base / Documentation.

```php
use Filament\Panel;
use Guava\FilamentKnowledgeBase\KnowledgeBasePlugin;
 
public function panel(Panel $panel): Panel
{
    return $panel
        ->plugins([
            // ...
            KnowledgeBasePlugin::make(),
        ])
}
```

### Create documentation
To create your first documentation, simply run the `docs:make`, such as:
```bash
php artisan docs:make prologue.getting-started
```
This will create a file in `/docs/en/prologue/getting-started.md`.

If you want to create the file for a specific locale, you can do so using the `--locale` option (can be repeated for multiple locales):
```bash
php artisan docs:make prologue.getting-started --locale=de --locale=en
```
This would create the file for both the `de` and `en` locale.

If you **don't** pass any locale, it will automatically create the documentation file for every locale in `/docs/{locale}`.

### Markdown
After you generated your documentation file, it's time to edit it.

A markdown file consists of two sections, the `Front Matter` and `Content`.

#### Front Matter
In the front matter, you can customize the documentation page, such as the title, the icon and so on:
```md
---
// Anything between these dashes is the front matter
title: My example documentation
icon: heroicon-o-book-open
---
```

#### Content
Anything after the front matter is your content, written in markdown:
```md
---
// Front matter ... 
---
# Introduction
Lorem ipsum dolor ....
```
And that's it! You've created a simple knowledge base inside Filament.

### Accessing the knowledge base
In every panel you registered the Knowlege Base plugin, we automatically inject a documentation button at the very bottom of the sidebar.

[SCREENSHOT HERE]

But we offer a deeper integration to your panels.

#### Integrating into resources
You will most likely have a section in your knowledge base dedicated to each of your resource (at least to the more complex ones).

To integrate your resource with the documentation, all you need to do is implement the `HasKnowledgeBase` contract in your resource.

This will require you to implement the `getDocumentation` method, where you simply return the documentation pages you want to integrate. You can either return the `IDs` as strings (dot-separated path inside `/docs/{locale}/`) or use the helper to retrieve the model:
```php
use use Guava\FilamentKnowledgeBase\Contracts\HasKnowledgeBase;

class UserResource extends Resource implements HasKnowledgeBase
{
    // ...
    
    // 
    public static function getDocumentation(): array
    {
        return [
            'users.introduction',
            'users.authentication',
            FilamentKnowledgeBase::model()::find('users.permissions'),
        ];
    }
}
```

### Accessing the documentation models
We use the `sushi` package in the background to store the documentations. This way, they behave almost like regular `Eloquent models`.

#### Get model using our helper
To get the model, simply use our helper `FilamentKnowledgeBase::model()`:
```php
use \Guava\FilamentKnowledgeBase\FilamentKnowledgeBase;

// find specific model
FilamentKnowledgeBase::model()::find('<id>');
// query models
FilamentKnowledgeBase::model()::query()->where('title', 'Some title');
// etc.
```


## Testing

```bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits

- [Lukas Frey](https://github.com/GuavaCZ)
- [All Contributors](../../contributors)
- Spatie - Our package skeleton is a modified version of [Spatie's Package Tools](https://github.com/spatie/laravel-package-tools)
- Spatie shiki and markdown packages

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.