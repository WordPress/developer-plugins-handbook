# Common issues

## Data Must be Sanitized, Escaped, and Validated
 
When you include POST/GET/REQUEST/FILE calls in your plugin, it's important to sanitize, validate, and escape them. The goal here is to prevent a user from accidentally sending trash data through the system, as well as protecting them from potential security issues. 
 
SANITIZE: Data that is input (either by a user or automatically) must be sanitized as soon as possible. This lessens the possibility of XSS vulnerabilities and MITM attacks where posted data is subverted.
 
VALIDATE: All data should be validated, no matter what. Even when you sanitize, remember that you don’t want someone putting in ‘dog’ when the only valid values are numbers.
 
ESCAPE: Data that is output must be escaped properly when it is echo'd, so it can't hijack admin screens. There are many esc_*() functions you can use to make sure you don't show people the wrong data. 

To help you with this, WordPress comes with a number of sanitization and escaping functions. You can read about those here: 

https://developer.wordpress.org/apis/security/sanitizing/
https://developer.wordpress.org/apis/security/escaping/

Remember: You must use the most appropriate functions for the context. If you’re sanitizing email, use sanitize_email(), if you’re outputting HTML, use wp_kses_post(), and so on.

An easy mantra here is this:

Sanitize early
Escape Late
Always Validate
 
Clean everything, check everything, escape everything, and never trust the users to always have input sane data. After all, users come from all walks of life.

## Using CURL Instead of HTTP API

WordPress comes with an extensive HTTP API that should be used instead of creating your own curl calls. It’s both faster and more extensive. It’ll fall back to curl if it has to, but it’ll use a lot of WordPress’ native functionality first.

[https://developer.wordpress.org/plugins/http-api/](https://developer.wordpress.org/plugins/http-api/)

In case you need, you can use setopt with [https://developer.wordpress.org/reference/hooks/http_api_curl/](https://developer.wordpress.org/reference/hooks/http_api_curl/)

Please note: If you're using CURL in 3rd party vendor libraries, that's permitted. It's in your own code unique to this plugin (or any dedicated WordPress libraries) that we need it corrected.

## Changing Active Plugins

It is not allowed for plugins to change the activation status of other plugins, this is an action that must be performed by the user.

It is also not allowed to interfere with the user's actions when activating or deactivating a plugin, unless that's done to prevent errors (For example: When your plugin depends on another plugin, deactivate your own plugin when that other plugin is not active).

WordPress 6.5 introduces [Plugin Dependencies](https://make.wordpress.org/core/2024/03/05/introducing-plugin-dependencies-in-wordpress-6-5/), you can use it to manage dependencies (although it's fine if you use this as a fallback).

## ALLOW_UNFILTERED_UPLOADS is not allowed.

Setting this constant to true will allow the user to upload any type of file (including PHP and other executables), creating serious potential security risks. As developers, we should not use or allow the use of this constant in any kind of logic, not even in a conditional.

WordPress includes a list of safe files, as you can see in the function [wp_get_mime_types](https://developer.wordpress.org/reference/functions/wp_get_mime_types).

If you need to add a specific file that is not in the list and that won't represent a security risk, you can do so using the [upload_mimes](https://developer.wordpress.org/reference/hooks/upload_mimes/) filter.

## Setting a default timezone

This is rarely a good idea. People should be able to define their own timezones in WordPress.

Also WordPress explicitly sets and expects the default timezone to be UTC (in settings.php) and the date/time functions sometimes rely on the fact that the default timezone is UTC. For instance if you do date_default_timezone_set(get_option('timezone_string')) and then later try to get a GMT timestamp from get_post_time() or get_post_modified_time(), it will fail to give you the right date.

## Please use WordPress' file uploader

When plugins use move_uploaded_file(), they exclude their uploads from the built-in checks and balances with WordPress's functions. Instead of that, you should use the built in function:

[https://developer.wordpress.org/reference/functions/wp_handle_upload/](https://developer.wordpress.org/reference/functions/wp_handle_upload/)

## Don't Use Error Reporting in Production Code

While error_reporting() is a great tool in PHP ( [https://www.php.net/manual/en/function.error-reporting.php](https://www.php.net/manual/en/function.error-reporting.php) ) but if you set it permanently in your plugin, you mess things up for everyone who uses your code. Should they have a reason to try to debug their site which happens to use your code, they won't be able to get a clean test because you're messing with the output. It has no place in the day to day function of your plugin.

## Don't Force Set PHP Limits Globally

While many plugins can need optimal settings for PHP, we ask you please not set them as global defaults.

Having defines like ini_set('memory_limit', '-1'); run globally (like on init or in the __construct() part of your code) means you'll be running that for everything on the site, which may cause your users to fall out of compliance with any limits or restrictions on their host.

If you must use those, you need to limit them specifically to only the exact functions that require them.

## Including An Update Checker / Changing Updates functionality

Please remove the checks you have in your plugin to provide for updates.

We do not permit plugins to phone home to other servers for updates, as we are providing that service for you with WordPress.org hosting. One of our guidelines is that you actually use our hosting, so we need you to remove that code.

We also ask that plugins not interfere with the built-in updater as it will cause users to have unexpected results with WordPress 5.5 and up.

## Allowing Direct File Access to plugin files

Direct file access is when someone directly queries your file. This can be done by simply entering the complete path to the file in the URL bar of the browser but can also be done by doing a POST request directly to the file. For files that only contain a PHP class the risk of something funky happening when directly accessed is pretty small. For files that contain procedural code, functions and function calls, the chance of security risks is a lot bigger.

You can avoid this by putting this code at the top of all PHP files that could potentially execute code if accessed directly :

```php
if ( ! defined( 'ABSPATH' ) ) exit; // Exit if accessed directly
```

## Internationalization: Don't use variables or defines as text, context or text domain parameters.

In order to make a string translatable in your plugin you are using a set of special functions. These functions collectively are known as "gettext".

There is a [dedicated team in the WordPress community](https://make.wordpress.org/polyglots/) to translate and help other translating strings of WordPress core, plugins and themes to other languages.

To make them be able to translate this plugin, please **do not use variables or function calls** for the text, context or text domain parameters of any gettext function, all of them **NEED to be strings**. Note that the translation parser reads the code without executing it, so it won't be able to read anything that is not a string within these functions.

## Internationalization: Don't use variables or defines as text, context or text domain parameters.

In order to make a string translatable in your plugin you are using a set of special functions. These functions collectively are known as "gettext".

There is a [dedicated team in the WordPress community](https://make.wordpress.org/polyglots/) to translate and help other translating strings of WordPress core, plugins and themes to other languages.

To make them be able to translate this plugin, please **do not use variables or function calls** for the text, context or text domain parameters of any gettext function, all of them **NEED to be strings**. Note that the translation parser reads the code without executing it, so it won't be able to read anything that is not a string within these functions.

For example, if your gettext function looks like this...

`esc_html__( $greetings , 'plugin-slug' );`

...the translator won't be able to see anything to be translated as `$greetings` is not a string, it is not something that can be translated.
You need to give them the string to be translated, so they can see it in the translation system and can translate it, the correct would be as follows...

`esc_html__( 'Hello, how are you?' , 'plugin-slug' );`

This also applies to the translation domain, this is a bad call:

`esc_html__( 'Hello, how are you?' , $plugin_slug );`

The fix here would be like this:

`esc_html__( 'Hello, how are you?' , 'plugin-slug' );`

Also note that the translation domain needs to be the same as your plugin slug.

What if we want to include a dynamic value inside the translation? Easy, you need to add a placeholder which will be part of the string and change it after the gettext function does its magic, you can use `printf` to do so, like this:

```php
printf(
    /* translators: %s: First name of the user */
    esc_html__( 'Hello %s, how are you?', 'plugin-slug' ),
    esc_html( $user_firstname )
);
```

You can read [more information here](https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/#text-domains).