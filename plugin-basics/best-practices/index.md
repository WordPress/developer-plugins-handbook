# Best Practices

Here are some best practices to help organize your code so it works well alongside WordPress core and other WordPress plugins.

## Avoid Naming Collisions

A naming collision happens when your plugin is using the same name for a variable, function or a class as another plugin.

Luckily, you can avoid naming collisions by using the methods below.

## Procedural Coding Method

By default, all variables, functions and classes are defined in the **global namespace**, which means that it is possible for your plugin to override variables, functions and classes set by another plugin and vice-versa. Variables that are defined _inside_ of functions or classes are not affected by this.

### Prefix Everything

All globally accessible code should be prefixed with a _unique_ identifier. Prefixes prevent conflicts with other plugins and prevents them from overwriting your variables and accidentally calling your functions and classes.

In order to prevent conflicts with other plugins, your prefix should be at least 4 letters long, though we recommend 5. You should avoid using a common English word, and instead choose something unique to your plugin. We host tens of thousands of plugins on WordPress.org alone. There are hundreds of thousands more outside our servers. You're _going_ to run into conflicts.

A good way to do this is with a prefix. For example, if your plugin is called "Easy Custom Post Types" then you could use names like these:

- `function ecpt_save_post()`
- `define( 'ECPT_LICENSE', true );`
- `class ECPT_Admin{}`
- `namespace EasyCustomPostTypes;`
- `update_option( 'ecpt_settings', $settings );`

[info]Because you are making code as a part of the **WordPress** project, you must avoid the use of prefixes that have a high probability of conflicting with the core WordPress. This includes but is not limited to: `__` (double underscores), `wp_` , `WordPress`, or `_` (single underscore)

If you are making code for a 'sub' plugin (such as a WooCommece extension), you would similarly need to avoid using any of their normal/common prefixes (i.e. Woo, WooCommerce).

You can use them _inside_ your classes or namespace, but not as stand-alone function/namespace/class.[/info]

If you're using `_n()` or `__()` for translation, that's fine. We're **only** talking about functions you've created for your plugin, not the core functions from WordPress. In fact, those core features are _why_ you need to not use those prefixes in your own plugin! You wouldn't want to break WordPress for your users.

Remember: Good prefix names are unique and distinct to your plugin. This will help you and the next person in debugging, as well as prevent conflicts.

Code that **must** be prefixed includes:

- Functions (unless namespaced)
- Classes, interfaces, and traits (unless namespaced)
- Namespaces
- Global variables
- Options and transients

### Check for Existing Implementations

PHP provides a number of functions to verify existence of variables, functions, classes and constants. All of these will return true if the entity exists.

- **Variables**: [isset()](https://www.php.net/manual/en/function.isset.php) (includes arrays, objects, etc.)
- **Functions**: [function_exists()](https://www.php.net/manual/en/function.function-exists.php)
- **Classes**: [class_exists()](https://www.php.net/manual/en/function.class-exists.php)
- **Constants**: [defined()](https://www.php.net/manual/en/function.defined.php)

Keep in mind that using `(!function_exists('NAME')) {` around all your functions and classes sounds like a great idea until you realize the fatal flaw. If something else has a function with the same name and their code loads first, your plugin will break. Using if-exists to replace/override a function or class should be reserved for _shared_ libraries only.

### Example

```
// Create a function called "wporg_init" if it doesn't already exist
if ( ! function_exists( 'wporg_init' ) ) {
  function wporg_init() {
    register_setting( 'wporg_settings', 'wporg_option_foo' );
  }
}

// Create a function called "wporg_get_foo" if it doesn't already exist
if ( ! function_exists( 'wporg_get_foo' ) ) {
  function wporg_get_foo() {
    return get_option( 'wporg_option_foo' );
  }
}
```

## Object Oriented Programming Method

An easier way to tackle the naming collision problem is to use a [class](https://www.php.net/manual/en/language.oop5.php) for the code of your plugin.

You will still need to take care of checking whether the name of the class you want is already taken but the rest will be taken care of by PHP.

### Example

```
if ( ! class_exists( 'WPOrg_Plugin' ) ) {
  class WPOrg_Plugin {
    public static function init() {
      register_setting( 'wporg_settings', 'wporg_option_foo' );
    }

    public static function get_foo() {
      return get_option( 'wporg_option_foo' );
    }
  }

  WPOrg_Plugin::init();
  WPOrg_Plugin::get_foo();
}
```
## File Organization

The root level of your plugin directory should contain your `plugin-name.php` file and, optionally, your [uninstall.php](https://developer.wordpress.org/plugins/plugin-basics/uninstall-methods/) file. All other files should be organized into sub folders whenever possible.

### Folder Structure

A clear folder structure helps you and others working on your plugin keep similar files together.

Here's a sample folder structure for reference:

```
/plugin-name
  plugin-name.php
  uninstall.php
  /languages
  /includes
  /admin
    /js
    /css
    /images
  /public
    /js
    /css
    /images
```

## Plugin Architecture

The architecture, or code organization, you choose for your plugin will likely depend on the size of your plugin.

For small, single-purpose plugins that have limited interaction with WordPress core, themes or other plugins, there's little benefit in engineering complex classes; unless you know the plugin is going to expand greatly later on.

For large plugins with lots of code, start off with classes in mind. Separate style and scripts files, and even build-related files. This will help code organization and long-term maintenance of the plugin.

### Conditional Loading

It's helpful to separate your admin code from the public code. Use the conditional [is_admin()](https://developer.wordpress.org/reference/functions/is_admin/). You must still perform capability checks as this doesn't indicate the user is authenticated or has Administrator-level access. See [Checking User Capabilities](https://developer.wordpress.org/plugins/security/checking-user-capabilities/).

For example:

```
if ( is_admin() ) {
  // we are in admin mode
  require_once __DIR__ . '/admin/plugin-name-admin.php';
}
```

### Avoiding Direct File Access

As a security precaution, it's a good practice to disallow access if the `ABSPATH` global is not defined. This is only applicable to files which contain code outside of class or function definitions, such as the main plugin file.

You can implement this by including this code at the top of the file:

```
if ( ! defined( 'ABSPATH' ) ) {
  exit; // Exit if accessed directly
}
```

### Architecture Patterns

While there are a number of possible architecture patterns, they can broadly be grouped into three variations:

- [Single plugin file, containing functions](https://github.com/GaryJones/move-floating-social-bar-in-genesis/blob/master/move-floating-social-bar-in-genesis.php)
- [Single plugin file, containing a class, instantiated object and optionally functions](https://github.com/norcross/wp-comment-notes/blob/master/wp-comment-notes.php)
- [Main plugin file, then one or more class files](https://github.com/DevinVinson/WordPress-Plugin-Boilerplate)

### Architecture Patterns Explained

Specific implementations of the more complex of the above code organizations have already been written up as tutorials and slides:

- [Slash – Singletons, Loaders, Actions, Screens, Handlers](https://jjj.blog/2012/12/slash-architecture-my-approach-to-building-wordpress-plugins/)
- [Implementing the MVC Pattern in WordPress Plugins](https://iandunn.name/content/presentations/wp-oop-mvc/mvc.php)

## Boilerplate Starting Points

Instead of starting from scratch for each new plugin you write, you may want to start with a **boilerplate**. One advantage of using a boilerplate is to have consistency among your own plugins. Boilerplates also make it easier for other people to contribute to your code if you use a boilerplate they are already familiar with.

These also serve as further examples of different yet comparable architectures.

- [WordPress Plugin Boilerplate](https://github.com/DevinVinson/WordPress-Plugin-Boilerplate): A foundation for WordPress Plugin Development that aims to provide a clear and consistent guide for building your plugins.
- [WordPress Plugin Bootstrap](https://github.com/claudiosanches/wordpress-plugin-boilerplate): Basic bootstrap to develop WordPress plugins using Grunt, Compass, GIT, and SVN.
- [WP Skeleton Plugin](https://github.com/ptahdunbar/wp-skeleton-plugin): Skeleton plugin that focuses on unit tests and use of composer for development.
- [WP CLI Scaffold](https://developer.wordpress.org/cli/commands/scaffold/plugin/): The Scaffold command of WP CLI creates a skeleton plugin with options such as CI configuration files.

Of course, you could take different aspects of these and others to create your own custom boilerplate.