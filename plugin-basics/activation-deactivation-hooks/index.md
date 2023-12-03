# Activation / Deactivation Hooks

Activation and deactivation hooks provide ways to perform actions when plugins are activated or deactivated.

- On _activation_, plugins can run a routine to add rewrite rules, add custom database tables, or set default option values.
- On _deactivation_, plugins can run a routine to remove temporary data such as cache and temp files and directories.

[alert]The deactivation hook is sometimes confused with the [uninstall hook](https://developer.wordpress.org/plugins/plugin-basics/uninstall-methods/). The uninstall hook is best suited to **delete all data permanently** such as deleting plugin options and custom tables, etc.[/alert]

## Activation

To set up an activation hook, use the [register\_activation\_hook()](https://developer.wordpress.org/reference/functions/register_activation_hook/) function:

```
register_activation_hook(
  __FILE__,
  'pluginprefix_function_to_run'
);
```

## Deactivation

To set up a deactivation hook, use the [register\_deactivation\_hook()](https://developer.wordpress.org/reference/functions/register_deactivation_hook/) function:

```
register_deactivation_hook(
  __FILE__,
  'pluginprefix_function_to_run'
);
```

The first parameter in each of these functions refers to your main plugin file, which is the file in which you have placed the [plugin header comment](https://developer.wordpress.org/plugins/plugin-basics/header-requirements/). Usually these two functions will be triggered from within the main plugin file; however, if the functions are placed in any other file, you must update the first parameter to correctly point to the main plugin file.

## Example

One of the most common uses for an activation hook is to refresh WordPress permalinks when a plugin registers a custom post type. This gets rid of the nasty 404 errors.

Let's look at an example of how to do this:

```
/**
 * Register the "book" custom post type
 */
function pluginprefix_setup_post_type() {
  register_post_type( 'book', ['public' => true ] ); 
} 
add_action( 'init', 'pluginprefix_setup_post_type' );

/**
 * Activate the plugin.
 */
function pluginprefix_activate() { 
  // Trigger our function that registers the custom post type plugin.
  pluginprefix_setup_post_type(); 
  // Clear the permalinks after the post type has been registered.
  flush_rewrite_rules(); 
}
register_activation_hook( __FILE__, 'pluginprefix_activate' );
```

If you are unfamiliar with registering custom post types, don't worry – this will be covered later. This example is used simply because it's very common.

Using the example from above, the following is how to reverse this process and deactivate a plugin:

```
/**
 * Deactivation hook.
 */
function pluginprefix_deactivate() {
  // Unregister the post type, so the rules are no longer in memory.
  unregister_post_type( 'book' );
  // Clear the permalinks to remove our post type's rules from the database.
  flush_rewrite_rules();
}
register_deactivation_hook( __FILE__, 'pluginprefix_deactivate' );
```

For further information regarding activation and deactivation hooks, here are some excellent resources:

- [register\_activation\_hook()](https://developer.wordpress.org/reference/functions/register_activation_hook/) in the WordPress function reference.
- [register\_deactivation\_hook()](https://developer.wordpress.org/reference/functions/register_deactivation_hook/) in the WordPress function reference.