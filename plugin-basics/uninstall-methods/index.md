# Uninstall Methods

Your plugin may need to do some clean-up when it is uninstalled from a site.

A plugin is considered uninstalled if a user has deactivated the plugin, and then clicks the delete link within the WordPress Admin.

When your plugin is uninstalled, you'll want to clear out any plugin options and/or settings specific to the plugin, and/or other database entities such as tables.

Less experienced developers sometimes make the mistake of using the deactivation hook for this purpose.

This table illustrates the differences between deactivation and uninstall.

| Scenario | Deactivation Hook | Uninstall Hook |
| --- | --- | --- |
| Flush Cache/Temp | Yes | No |
| Flush Permalinks | Yes | No |
| Remove Options from {[$wpdb](https://developer.wordpress.org/reference/classes/wpdb/)→prefix}_options | No | Yes |
| Remove Tables from [$wpdb](https://developer.wordpress.org/reference/classes/wpdb/) | No | Yes |

## Method 1: register_uninstall_hook

To set up an uninstall hook, use the [register\_uninstall\_hook()](https://developer.wordpress.org/reference/functions/register_uninstall_hook/) function:

```
register_uninstall_hook(
  __FILE__,
  'pluginprefix_function_to_run'
);
```

## Method 2: uninstall.php

To use this method you need to create an `uninstall.php` file inside the root folder of your plugin. This magic file is run automatically when the users deletes the plugin.

For example: `/plugin-name/uninstall.php`

[alert]Always check for the constant `WP_UNINSTALL_PLUGIN` in `uninstall.php` before doing anything. This protects against direct access.

The constant will be defined by WordPress during the `uninstall.php` invocation.

The constant is **NOT** defined when uninstall is performed by [register\_uninstall\_hook()](https://developer.wordpress.org/reference/functions/register_uninstall_hook/).[/alert]

Here is an example deleting option entries and dropping a database table:

```
// if uninstall.php is not called by WordPress, die
if ( ! defined( 'WP_UNINSTALL_PLUGIN' ) ) {
  die;
}

$option_name = 'wporg_option';

delete_option( $option_name );

// for site options in Multisite
delete_site_option( $option_name );

// drop a custom database table
global $wpdb;
$wpdb->query( "DROP TABLE IF EXISTS {$wpdb->prefix}mytable" );
```

[info]In Multisite, looping through all blogs to delete options can be very resource intensive.[/info]