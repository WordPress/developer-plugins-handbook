# Common issues

This is a compilation of some of the most common issues the Plugin Review Team encounters when reviewing plugins.

This list contains excerpts from the team's email messages, and should not be considered a complete or exhaustive list; the outcome of the reviews depends on the manual review performed by the team.

## Security

### Sanitize

**Input data must be Sanitized, Validated, and Escaped on output**

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

#### Sanitize: Confusion about escape and sanitize functions

**Note**: escape functions cannot be used to sanitize. They serve different purposes. Even if they seem to be perfect for this purpose, most of the functions are filterable and people expect to use them to escape. Therefore, another plugin may change what they do and make yours at risk and exploitable.

If you are trying to echo the variable, you have to first sanitize it and then escape it, as for example:


```php
echo esc_html(sanitize_text_field($_POST['example']));
```

#### Sanitize: Using filter functions to sanitize

**Note**: When using functions like `filter_var`, `filter_var_array`, `filter_input` and/or `filter_input_array` you will need to [set the FILTER parameter to any kind of filter that sanitizes the input](https://www.php.net/manual/en/filter.filters.php).

Leaving the filter parameter empty, PHP by default will apply the filter "FILTER_DEFAULT" **which is not sanitizing at all**.

```php
$post_id = filter_input(INPUT_GET, 'post_id', FILTER_SANITIZE_NUMBER_INT);
```

#### Sanitize: Nonces

**Note**: When checking a nonce using `wp_verify_nonce` you will need to sanitize the input using `wp_unslash` AND `sanitize_text_field`, [this is because this function is pluggable, and extenders should not trust its input values](https://developer.wordpress.org/news/2023/08/understand-and-use-wordpress-nonces-properly/#verifying-the-nonce).

Example:
```php
if ( ! isset( $_POST['prefix_nonce'] ) || ! wp_verify_nonce( sanitize_text_field( wp_unslash ( $_POST['prefix_nonce'] ) ) , 'prefix_nonce' ) )
```

### Processing the whole input

We strongly recommend you **never** attempt to process the whole $_POST/$_REQUEST/$_GET stack. This makes your plugin slower as you're needlessly cycling through data you don't need. Instead, you should only be attempting to process the items within that are required for your plugin to function.

### Escape

**Variables and options must be escaped when echo'd**

Much related to sanitizing everything, all variables that are echoed need to be escaped when they're echoed, so it can't hijack users or (worse) admin screens. There are many esc_*() functions you can use to make sure you don't show people the wrong data, as well as some that will allow you to echo HTML safely.

At this time, we ask you escape **all $-variables, options, and any sort of generated data when it is being echoed**. That means you should not be escaping when you build a variable, but when you output it at the end. We call this 'escaping late.'

Besides protecting yourself from a possible XSS vulnerability, escaping late makes sure that you're keeping the future you safe. While today your code may be only outputted hardcoded content, that may not be true in the future. By taking the time to properly escape **when** you echo, you prevent a mistake in the future from becoming a critical security issue.

This remains true of options you've saved to the database. Even if you've properly sanitized when you saved, the tools for sanitizing and escaping aren't interchangeable. Sanitizing makes sure it's safe for processing and storing in the database. Escaping makes it safe to output.

Also keep in mind that sometimes a function is echoing when it should really be returning content instead. This is a common mistake when it comes to returning JSON encoded content. Very rarely is that actually something you should be echoing at all. Echoing is because it needs to be on the screen, read by a human. Returning (which is what you would do with an API) can be json encoded, though remember to **sanitize** when you save to that json object!

There are a number of options to secure all types of content (html, email, etc). Yes, even HTML needs to be properly escaped.

[https://developer.wordpress.org/apis/security/escaping/](https://developer.wordpress.org/apis/security/escaping/)

Remember: You must use the most appropriate functions for the context. There is pretty much an option for everything you could echo. Even echoing HTML safely.

#### Escape: Use of esc_url_raw

We know this is confusing, the `esc_url_raw` function is not an escaping function, but a sanitizing function similar to `sanitize_url`. Specifically it is used to sanitize a URL for use in a database or a redirection.

The appropriate function to escape a URL is `esc_url`.

#### Escape: Use of __

The function `__` retrieves the translation without escaping, please either:

- Use an alternative function that escapes the resulting value such as `esc_html__` or `esc_attr__`.
- Or wrap the `__` function with a proper escaping function such as `esc_html`, `esc_attr`, `wp_kses_post`, etc.

Examples:
```php
<h2><?php echo esc_html__('Settings page', 'plugin-slug'); ?></h2>
<h2><?php echo esc_html(__('Settings page', 'plugin-slug')); ?></h2>
```

#### Escape: Use of _e and _ex

The functions `_e` and `_ex` output the translation without escaping, please use an alternative function that escapes the output.

- An alternative to `_e` would be `esc_html_e`, `esc_attr_e` or simply using `__` wrapped by an escaping function and inside an `echo`.
- An alternative to `_ex` would be using `_x` wrapped by an escaping function and inside an `echo`.

Examples:
```php
<h2><?php esc_html_e('Settings page', 'plugin-slug'); ?></h2>
<h2><?php echo esc_html(__('Settings page', 'plugin-slug')); ?></h2>
<h2><?php echo esc_html(_x('Settings page', 'Settings page title', 'plugin-slug')); ?></h2>
```

#### Escape: Use json_encode

When you need to echo a JSON, it's better to make use of the function `wp_json_encode`, also, make sure you are not avoiding escaping with the options passed on the second parameter.

```php
echo wp_json_encode($array_or_object);
```

#### Escape: HTML

When escaping, there are cases where your plugin will need to output HTML. This can be done using the functions `wp_kses_post` or `wp_kses`. The function `wp_kses_post` will allow any common HTML that can go inside a post content, `wp_kses` will allow any HTML that you set up using its second and third parameters, please [refer to its documentation](https://developer.wordpress.org/reference/functions/wp_kses/).

A common mistake is to use `esc_html` to escape HTML. This function is not intended for that, it's intended to escape the output that will go **inside** an HTML tag, therefore it will strip any HTML tags.

Examples:
```php
echo wp_kses_post($html_content);
echo wp_kses($html_content, array( 'a', 'div', 'span' ));
```

#### Escape: Using sanitizing functions

Sanitize functions cannot be used to escape. They serve different purposes. Even if they seem to be perfect for this purpose, most of the functions are filterable and people expect to use them to sanitize. Therefore, another plugin may change what they do and make yours at risk and exploitable.

If you are trying to echo the variable, you have to first sanitize it and then escape it, as for example:

```php
echo esc_html(sanitize_text_field($_POST['example']));
```

### Files

#### Files: Use the WordPress file uploader

**Please use WordPress' file uploader**

When plugins use `move_uploaded_file(), they exclude their uploads from the built-in checks and balances with WordPress's functions. Instead of that, you should use the built in function:

[https://developer.wordpress.org/reference/functions/wp_handle_upload/](https://developer.wordpress.org/reference/functions/wp_handle_upload/)

#### Files: Unfiltered uploads

**ALLOW_UNFILTERED_UPLOADS is not allowed.**

Setting this constant to true will allow the user to upload any type of file (including PHP and other executables), creating serious potential security risks. As developers, we should not use or allow the use of this constant in any kind of logic, not even in a conditional.

WordPress includes a list of safe files, as you can see in the function [wp_get_mime_types](https://developer.wordpress.org/reference/functions/wp_get_mime_types).

If you need to add a specific file that is not in the list and that won't represent a security risk, you can do so using the [upload_mimes](https://developer.wordpress.org/reference/hooks/upload_mimes/) filter.

#### Files: Calling files remotely

Offloading images, js, css, and other scripts to your servers or any remote service (like Google, MaxCDN, jQuery.com etc) is disallowed. When you call remote data you introduce an unnecessary dependency on another site. If the file you're calling isn't a part of WordPress Core, then you should include it -locally- in your plugin, not remotely. If the file IS included in WordPress core, please call that instead.

An exception to this rule is if your plugin is performing a service. We will permit this on a case by case basis. Since this can be confusing we have some examples of what are not permitted:

- Offloading jquery CSS files to Google - You should include the CSS in your plugin.
- Inserting an iframe with a help doc - A link, or including the docs in your plugin is preferred.
- Calling images from your own domain - They should be included in your plugin.

Here are some examples of what we would permit:

- Calling font families from Google or their approved CDN (if GPL compatible)
- API calls back to your server to process possible spam comments (like Akismet)
- Offloading comments to your own servers (like Disqus)
- oEmbed calls to a service provider (like Twitter or YouTube)

Please remove external dependencies from your plugin and, if possible, include all files within the plugin (that is not called remotely). If instead you feel you are providing a service, please re-write your readme.txt in a manner that explains the service, the servers being called, and if any account is needed to connect.

### Libraries

#### Libraries: Using development versions

**Using Beta / Alpha / Development versions of libraries**

We do not recommend you use the beta version of a library unless it has features required by your plugin. Instead, you should be using the most stable release of the library.

If there is a technical reason you must use the beta version, please explain why. Otherwise, please change your library to the stable release.

#### Libraries: Not maintained

**Libraries that are no longer maintained are not permitted**

We no longer accept using any library that is no longer supported or maintained by their developers, as they pose a significant security risk. Please consider other options.

#### Libraries: Out of Date

At least one of the 3rd party libraries you're using is out of date. Please upgrade to the latest stable version for better support and security. We do not recommend you use beta releases.

### WPDB: Unsafe SQL calls

When making database calls, it's highly important to protect your code from SQL injection vulnerabilities. You need to update your code to use wpdb calls and prepare() with your queries to protect them.

Please review the following:

*   [https://developer.wordpress.org/reference/classes/wpdb/#protect-queries-against-sql-injection-attacks](https://developer.wordpress.org/reference/classes/wpdb/#protect-queries-against-sql-injection-attacks)
*   [https://codex.wordpress.org/Data\_Validation#Database](https://codex.wordpress.org/Data_Validation#Database)
*   [https://make.wordpress.org/core/2012/12/12/php-warning-missing-argument-2-for-wpdb-prepare/](https://make.wordpress.org/core/2012/12/12/php-warning-missing-argument-2-for-wpdb-prepare/)
*   [https://ottopress.com/2013/better-know-a-vulnerability-sql-injection/](https://ottopress.com/2013/better-know-a-vulnerability-sql-injection/)

#### WPDB: Arrays of placeholders

**Note**: Passing individual values to wpdb::prepare using placeholders is fairly straightforward, but what if we need to pass an array of values instead?

You'll need to create a placeholder for each item of the array and pass all the corresponding values to those placeholders, this seems tricky, but here is a snippet to do so.

```php
$wordcamp_id_placeholders = implode( ', ', array_fill( 0, count( $wordcamp_ids ), '%d' ) );
$prepare_values = array_merge( array( $new_status ), $wordcamp_ids );
$wpdb->query( $wpdb->prepare( "
    UPDATE `$table_name`
    SET `post_status` = %s
    WHERE ID IN ( $wordcamp_id_placeholders )",
    $prepare_values
) );
```
There is a core ticket that could make this easier in the future: [https://core.trac.wordpress.org/ticket/54042](https://core.trac.wordpress.org/ticket/54042)

### Not use HEREDOC-NOWDOC

**Do not use HEREDOC or NOWDOC syntax in your plugins**

While both are totally valid, and in many ways desirable features of PHP that allow you to output content, it comes with a cost that is too high for most plugins.

The primary issue is that most (if not all) codesniffers won't detect lack of escaping in code when you use HEREDOC or NOWDOC. While there are ways around this they have the end result of dashing all that readability to the rubbish pile and leaving you with a jumbled mess that won't properly be scanned.

We feel the risk here is much higher than the benefits, which is why we don't permit their use.

### Direct file access

**Allowing Direct File Access to plugin files**

Direct file access occurs when someone directly queries a PHP file. This can be done by entering the complete path to the file in the browser's URL bar or by sending a POST request directly to the file. 

For files that only contain class or function definitions, the risk of something funky happening when accessed directly is minimal. However, for files that contain executable code (e.g., function calls, class instance creation, class method calls, or inclusion of other PHP files), the risk of security issues is hard to predict because it depends on the specific case, but it can exist and it can be high.

You can easily prevent this by adding the following code at the top of all PHP files that could potentially execute code if accessed directly:

```php
if ( ! defined( 'ABSPATH' ) ) exit; // Exit if accessed directly
```

## Compatibility

### Prefixing

**Generic function/class/define/namespace/option names**

All plugins must have unique function names, namespaces, defines, class and option names. This prevents your plugin from conflicting with other plugins or themes. We need you to update your plugin to use more unique and distinct names.

A good way to do this is with a prefix. For example, if your plugin is called "Easy Custom Post Types" then you could use names like these:

- function ecpt_save_post()
- class ECPT_Admin{}
- namespace ECPT;
- update_option( 'ecpt_settings', $settings );
- define( 'ECPT_LICENSE', true );
- global $ecpt_options;

Don't try to use two (2) or three (3) letter prefixes anymore. We host nearly 100-thousand plugins on WordPress.org alone. There are tens of thousands more outside our servers. Believe us, you’re going to run into conflicts.

You also need to avoid the use of __ (double underscores), wp_ , or _ (single underscore) as a prefix. Those are reserved for WordPress itself. You can use them inside your classes, but not as stand-alone function.

Please remember, if you're using _n() or __() for translation, that's fine. We're **only** talking about functions you've created for your plugin, not the core functions from WordPress. In fact, those core features are why you need to not use those prefixes in your own plugin! You don't want to break WordPress for your users.

Related to this, using if (!function_exists('NAME')) { around all your functions and classes sounds like a great idea until you realize the fatal flaw. If something else has a function with the same name and their code loads first, your plugin will break. Using if-exists should be reserved for shared libraries only.

Remember: Good prefix names are unique and distinct to your plugin. This will help you and the next person in debugging, as well as prevent conflicts.

### PHP

#### PHP: Do not use Short Tags

The primary issue with PHP's short tags is that PHP managed to choose a tag (`<?`) that was used by another syntax: XML. This is a big issue when you consider how common XML parsing and management is.

We know as of PHP 5.4, `<?= ... ?>` tags are supported everywhere, regardless of short tags settings. This should mean they're safe to use in portable code but in reality that has proven not to be the case. Add on to that the fact that many codesniffers won't detect lack of escaping in code when you use short-tags, and it becomes not worth the headache for anyone.

Basically the risk here is way higher than the benefits, which is why we don't permit their use.

#### PHP: Changing Settings globally

**Don't Force Set PHP Settings Globally**

While many plugins can need optimal settings for PHP, we ask you please not set them as global defaults.

Having defines like ini_set('memory_limit', '-1'); run globally (like on init or in the __construct() part of your code) means you'll be running that for everything on the site, which may cause your users to fall out of compliance with any limits or restrictions on their host.

If you must use those, you need to limit them specifically to only the exact functions that require them.

#### PHP: Setting a default timezone

This is rarely a good idea. People should be able to define their own timezones in WordPress.

Also WordPress explicitly sets and expects the default timezone to be UTC (in settings.php) and the date/time functions sometimes rely on the fact that the default timezone is UTC. For instance if you do date_default_timezone_set(get_option('timezone_string')) and then later try to get a GMT timestamp from get_post_time() or get_post_modified_time(), it will fail to give you the right date.

#### PHP: Error reporting

**Don't Use Error Reporting in Production Code**

While error_reporting() is a great tool in PHP ( [https://www.php.net/manual/en/function.error-reporting.php](https://www.php.net/manual/en/function.error-reporting.php) ) but if you set it permanently in your plugin, you mess things up for everyone who uses your code. Should they have a reason to try to debug their site which happens to use your code, they won't be able to get a clean test because you're messing with the output. It has no place in the day to day function of your plugin.

### Plugin standards

#### Main file convention

**The main file of the plugin has a name that does not follow the convention.**

We expect the main plugin file (the file containing the plugin headers) to have the same name as the plugin folder, which is also the same name as the slug / permalink of the plugin.

For example, if your plugin slug is `ecpt-social-manager` we expect your main plugin filename to be `ecpt-social-manager.php`.

Note that using some common names as the filename for the main plugin file can lead to issues in some configurations.

Please check out our tips on how to [structure files and folders in a plugin](https://developer.wordpress.org/plugins/plugin-basics/best-practices/#folder-structure).

#### Incomplete Headers

Your headers are either missing or incomplete.

Please review [Header Requirements](https://developer.wordpress.org/plugins/the-basics/header-requirements/) and update your plugin accordingly, putting the headers in only the main file.

#### Incomplete Readme

Your readme is either missing or incomplete.

In some cases, such as for first plugins, ones with dependencies, or plugins that call external services, we require you to provide a complete readme. This means your readme has to have headers as well as a proper description and documentation as to how it works and how one can use it.

Our goal with this is to make sure everyone knows what they're installing and what they need to do before they install it. No surprises. This is especially important if your plugin is making calls to other servers. You are expected to provide users with all the information they need before they install your plugin.

Your readme also must validate per [Validator](https://wordpress.org/plugins/about/validator/) or we will reject it. Keep in mind, we don't want to see a readme.MD. While they can work, a readme.txt file will always be given priority, and not all of the markdown will work as expected.

We ask you please create your readme one based on this: [https://wordpress.org/plugins/readme.txt](https://wordpress.org/plugins/readme.txt)

#### No GPL-compatible license declared

It is necessary to declare the license of this plugin. You can do this using the fields available both in the plugin readme and in the plugin headers.

Remember that all code, data, and images — anything stored in the plugin directory hosted on WordPress.org — must comply with the GPL or a GPL-Compatible license. Included third-party libraries, code, images, or otherwise, must be also compatible.

For a specific list of compatible licenses, [please read the GPL-Compatible license list on gnu.org](https://www.gnu.org/licenses/license-list.html#GPLCompatibleLicenses).

#### Incorrect Stable Tag

In your readme, your 'Stable Tag' does not match the Plugin Version as indicated in your main plugin file.

Your Stable Tag is meant to be the stable version of your plugin, not of WordPress. For your plugin to be properly downloaded from WordPress.org, those values need to be the same. If they're out of sync, your users won't get the right version of your code.

We recommend you use Semantic Versioning (aka SemVer) for managing versions:

- [Software Versioning](https://en.wikipedia.org/wiki/Software_versioning)
- [SemVer](https://semver.org/)

Please note: While currently using the stable tag of trunk currently works in the Plugin Directory, it's not actually a supported or recommended method to indicate new versions and has been known to cause issues with automatic updates.

We ask you please properly use tags and increment them when you release new versions of your plugin, just like you update the plugin version in the main file. Having them match is the best way to be fully forward supporting.

#### Declared license mismatched

When declaring the license for this plugin, it has to be the same.

Please make sure that you are declaring the same license in both the readme file and the plugin headers.

It is fine for this plugin to contain code from other sources under other licenses as long those are well documented and are under GPL or GPL-Compatible license, but we need a sole license declared for your code.

### Use HTTP API

**Using CURL Instead of HTTP API**

WordPress comes with an extensive HTTP API that should be used instead of creating your own curl calls. It’s both faster and more extensive. It’ll fall back to curl if it has to, but it’ll use a lot of WordPress’ native functionality first.

[https://developer.wordpress.org/plugins/http-api/](https://developer.wordpress.org/plugins/http-api/)

In case you need, you can use setopt with [https://developer.wordpress.org/reference/hooks/http_api_curl/](https://developer.wordpress.org/reference/hooks/http_api_curl/)

Please note: If you're using CURL in 3rd party vendor libraries, that's permitted. It's in your own code unique to this plugin (or any dedicated WordPress libraries) that we need it corrected.

### Use wp_enqueue commands

Your plugin is not correctly including JS and/or CSS. You should be using the built in functions for this:

When including **JavaScript code** you can use:

- wp_register_script() and [wp_enqueue_script()](https://developer.wordpress.org/reference/functions/wp_enqueue_script/) to add JavaScript code from a file.
- [wp_add_inline_script()](https://developer.wordpress.org/reference/functions/wp_add_inline_script/) to add inline JavaScript code to previous declared scripts.

When including **CSS** you can use:

- wp_register_style() and [wp_enqueue_style()](https://developer.wordpress.org/reference/functions/wp_enqueue_style/) to add CSS from a file.
- [wp_add_inline_style()](https://developer.wordpress.org/reference/functions/wp_add_inline_style/) to add inline CSS to previously declared CSS.

Note that as of WordPress 5.7, you can pass attributes like async, nonce, and type by using new functions and filters: [Script Attributes Related Functions in WordPress 5.7](https://make.wordpress.org/core/2021/02/23/introducing-script-attributes-related-functions-in-wordpress-5-7/)

If you're trying to enqueue on the admin pages you'll want to use the admin enqueues.

- [admin_enqueue_scripts](https://developer.wordpress.org/reference/hooks/admin_enqueue_scripts/)
- [admin_print_scripts](https://developer.wordpress.org/reference/hooks/admin_print_scripts/)
- [admin_print_styles](https://developer.wordpress.org/reference/hooks/admin_print_styles/)

### Including Libraries Already In Core

Your plugin has included a copy of a library that WordPress already includes.

WordPress includes a number of useful libraries, such as jQuery, Atom Lib, SimplePie, PHPMailer, PHPass, and more. For security and stability reasons, plugins may not include those libraries in their own code, but instead must use the versions of those libraries packaged with WordPress.

You can see the list of JS Libraries here:

[Default Scripts and JS Libraries included and registered by WordPress](https://developer.wordpress.org/reference/functions/wp_enqueue_script/#default-scripts-and-js-libraries-included-and-registered-by-wordpress)

While we do not YET have a decent public facing page to list all these libraries, we do have a list here:

[Core Credits](https://meta.trac.wordpress.org/browser/sites/trunk/api.wordpress.org/public_html/core/credits/wp-59.php#L739)

It’s fine to locally include add-ons not in core, but please ONLY add those additional files. For example, you do not need the entire jQuery UI library for one file. If your code doesn't work with the built-in versions of jQuery, it's most likely a noConflict issue.

### Internationalization

#### Internationalization: Using variables

**Internationalization: Don't use variables or defines as text, context or text domain parameters.**

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

## Compliance

### Changing Active Plugins

It is not allowed for plugins to change the activation status of other plugins, this is an action that must be performed by the user.

It is also not allowed to interfere with the user's actions when activating or deactivating a plugin, unless that's done to prevent errors (For example: When your plugin depends on another plugin, deactivate your own plugin when that other plugin is not active).

WordPress 6.5 introduces [Plugin Dependencies](https://make.wordpress.org/core/2024/03/05/introducing-plugin-dependencies-in-wordpress-6-5/), you can use it to manage dependencies (although it's fine if you use this as a fallback).

### Update checker

**Including An Update Checker / Changing Updates functionality**

Please remove the checks you have in your plugin to provide for updates.

We do not permit plugins to phone home to other servers for updates, as we are providing that service for you with WordPress.org hosting. One of our guidelines is that you actually use our hosting, so we need you to remove that code.

We also ask that plugins not interfere with the built-in updater as it will cause users to have unexpected results with WordPress 5.5 and up.

### Undocumented 3rd party

**Undocumented use of a 3rd Party or external service**

We permit plugins to require the use of 3rd party (i.e. external) services, provided they are properly documented in a clear manner.

We require plugins that reach out to other services to disclose this, in clear and plain language, so users are aware of where data is being sent. This allows them to ensure that any legal issues with data transmissions are covered. This is true even if you are the 3rd party service.

In order to do so, you must update your readme to do the following:

- Clearly explain that your plugin is relying on a 3rd party as a service and under what circumstances
- Provide a link to the service.
- Provide a link to the service terms of use and/or privacy policies.

Remember, this is for your own legal protection. Use of services must be upfront and well documented.

### Included Unneeded Folders

This plugin includes folders and files that looks like are not required for the running of your plugin. Some examples are:

- development tools
- unneeded vendor folders for production (bower, node, grunt, etc)
- demos
- unit tests

If you're trying to include the human-readable version of your own code (in order to comply with our guidelines) that's fine, remember that we also permit you to put links to them in your readme.

You should also keep and/or link configuration files, as for example, the `composer.json` file in order to allow others to review, study, and yes, fork this code.

[Detailed Plugin Guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/#4-code-must-be-mostly-human-readable)

But you can, and should, safely remove those other unneeded folders from your plugins.

### Not permitted files

A plugin typically consists of files related to the plugin functionality (php, js, css, txt, md) and maybe some multimedia files (png, svg, jpg) and / or data files (json, xml).

We have detected files that are not among of the files normally found in a plugin, are they necessary? If not, then those won't be allowed.

#### Including code from a "premium" source

Some premium libraries are specifically not permitted to be included in free (WordPress.org hosted) plugins. Those must be removed.

### GPL

#### GPL: Non-GPL compatible Code Included

It's required that all code be compatible with the GPLv2 (or later) license in order to be included in our directory.

You must remove the code and alter the plugin so it is not required. We suggest you find code that is GPL compatible and use that instead. For more information on what types of licenses are compatible with GPL, please review the following links:

- [GNU Licenses - License List](https://www.gnu.org/licenses/license-list.html)
- [GPL FAQ - All Compatibility](https://www.gnu.org/licenses/gpl-faq.html#AllCompatibility)

#### GPL: No publicly documented resource

**No publicly documented resource for your compressed content**

In reviewing your plugin, we cannot find a non-compiled version of your javascript and/or css related source code.

In order to comply with our guidelines of human-readable code, we require you to include the source code and/or a link to the non-compressed, developer libraries you’ve included in your plugin. If you include a link, this may be in your source code, however we require you to also have it in your readme.

[Detailed Plugin Guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/#4-code-must-be-mostly-human-readable)

We strongly feel that one of the strengths of open source is the ability to review, observe, and adapt code. By maintaining a public directory of freely available code, we encourage and welcome future developers to engage with WordPress and push it forward.

That said, with the advent of larger and larger plugins using more complex libraries, people are making good use of build tools (such as composer or npm) to generate their distributed production code. In order to balance the need to keep plugin sizes smaller while still encouraging open source development, we require plugins to make the source code to any compressed files available to the public in an easy to find location, by documenting it in the readme.

For example, if you’ve made a Gutenberg plugin and used npm and webpack to compress and minify it, you must either include the source code within the published plugin or provide access to a public maintained source that can be reviewed, studied, and yes, forked.

We strongly recommend you include directions on the use of any build tools to encourage future developers.

#### GPL: Using composer but no composer.json file

We noticed that your plugin is using Composer to handle library dependencies, that's great as it will help maintaining and updating your plugin in the future while avoiding collisions with other plugins that are using same libraries.

The `composer.json` file describes the dependencies of your project and may contain other metadata as well. It's a small file that typically can be found in the top-most directory of your plugin.

As one of the strengths of open source is the ability to review, observe, and adapt code, **we would like to ask you to include that file in your plugin**, even if it is only used for development purposes. This will allow others to exercise the open source freedoms from which we all benefit.
