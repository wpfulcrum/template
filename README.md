# Template Module

[![Build Status](https://travis-ci.org/wpfulcrum/template.svg?branch=develop)](https://travis-ci.org/wpfulcrum/template) 
[![Latest Stable Version](https://poser.pugx.org/wpfulcrum/template/v/stable)](https://packagist.org/packages/wpfulcrum/template) 
[![License](https://poser.pugx.org/wpfulcrum/template/license)](https://packagist.org/packages/wpfulcrum/template)

**Empowering your WordPress plugin to load templates.**

Does your plugin create a custom post type or taxonomy? Or maybe you want to add a page template into the page's template selector?  The Fulcrum Template Module makes it easy for you.

## Features

- Theme override - if the template exists in the theme, it loads that one and not the plugin. 
- Loads single, post type archive, and taxonomy templates from your plugin.
- Loads your custom template into the admin's page template select, allowing authors to select and assign your template to the page.
- If the page being loaded has a template assigned to it, it will load the plugin's template when available.

## Installation

The best way to use this component is through Composer:

```
composer require wpfulcrum/template
```

## Dependencies

This module requires:
 
- at least PHP 5.6
- WordPress 4.8+

## Configuration

There are 2 types of loaders, each requiring a separate configuration file in your plugin:

- Template loader - for the front-end
- Admin page template loader - for the back-end

### Template Loader Configuration

The following example is for a "Book" custom post type with a "Genre" taxonomy:

```
<?php

return [
    'autoload' => true,
    'config'   => [
        'templateFolderPath' => __DIR__ . '/path/to/the/templates/',
        'postType'           => 'book',
        'useSingle'          => true,
        'useArchive'         => true,
        'useTax'             => true,
        'taxonomy'           => 'genre',
        'usePageTemplates'   => true, // set to false when there are no custom page templates.
    ],
];
```

The above configuration tells the module that this plugin has the following template files that should be loaded when needed:

- `single-book.php`
- `archive-book.php`
- `taxonomy-genre.php`
- and page templates, which are configured next.

### Admin (Back-end) Template Loader

When you're plugin needs to load custom page templates into the back-end, use the following configuration example:

```
<?php

return [
    'autoload' => true,
    'config'   => [
        'usePageTemplates'   => true,
        'templates'          => [
            'template-books.php' => __("Book Entry Template"),
        ],
    ],
];
```

The above configuration tells the module that the plugin has the following templates available for Pages:

- `template-books.php`

#### What Happens?

When an author opens a page in the back-end admin area, the "Book Entry Template" is available for selection as the page's template.  If that template is selected, the plugin's template file will load when that page is being rendered in the browser (on the front-end). 

## Making It Work

There are 2 ways to utilize this module:

1. With the full [Fulcrum plugin](https://github.com/wpfulcrum/fulcrum).
2. Or on its own without Fulcrum.

### With Fulcrum

In Fulcrum, your plugin is an Add-on.  In your plugin's configuration file, you will have a parameter for the `serviceProviders`, where you list each of the service providers you want to use.  In this case, you'll use the `provider.template` for the front-end loading and `provider.adminTemplate` for the back-end admin loading.  

For example, using our configurations above, this would be the configuration:

```
	'serviceProviders' => [
        'template.book'   => [
            'provider' => 'provider.template',
            'config'   => __DIR__ . '/template/book-genre.php',
        ],
        'adminTemplate.books'   => [
            'provider' => 'provider.adminTemplate',
            'config'   => __DIR__ . '/template/page-templates.php',
        ],        
	],
```

### Without Fulcrum

Without Fulcrum, you'll need to instantiate each of the Loaders.

For the front-end template loader, you would do the following:

```
TemplateLoader(
    ConfigFactory::create('/path/to/config/template/book-genre.php'),
);
```

For the back-end admin template loader, you would do the following:

```
AdminTemplate(
    ConfigFactory::create('/path/to/config/template/page-templates.php'),
);
```

## Contributing

All feedback, bug reports, and pull requests are welcome.