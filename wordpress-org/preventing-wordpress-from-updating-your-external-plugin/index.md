# Preventing WordPress from Updating Your External Plugin

[warning]The information on this page is meant for use only on plugins **not** hosted on WordPress.org. Do not attempt to use this on your code hosted in the directory.[/warning]

If you host your plugin on WordPress.org, we handle all updates for you. As such, the steps in this document are **prohibited** in all plugins submitted to the directory. Any plugin hosted here that is found to include this plugin will be closed and the developer required to remove it in order for their plugin to be restored.

We have chosen to document it here for the education of developers, as well as to ensure the global WordPress community can be safer.

## Always Use Good Folder Names

Before we get into the code, we must stress the absolute best way to ensure your plugin won’t get overwritten by an update from WordPress.org is to use a good name. If you’re making a plugin for your company, give it a folder name like `companyname-function-plugin` — for example, if you work for FaceRange and you’re making a status plugin, you could name it `facerange-status-plugin`

Not only would we not accept it for using the prohibited term ‘plugin’, the plugin team would validate that the plugin owner **legally** represents FaceRange.

## Update URI

As of WordPress 5.8, we have added in a feature to how the WordPress.org API checks for updates, and allowed it to be blocked by the use of a new header: Update URI.

Let’s say you have a plugin you made for your own site, and you gave it the folder name of `my-plugin`. That is a generic folder name, and has a high probability that someone else may use it. It’s also not a name we would allow you to block in our system, due to it’s generic nature.

The Update URI header can be added to the plugin headers. Look in your main plugin file for this section:

```
/**
 * Plugin Name: My Cool Plugin
 * Plugin URI: https://example.com/my-plugin/
 * Description: My Plugin does cool things.
 * Version: 1.0
 * Author: the team
 * Author URI: https://example.com/
 * Text Domain: my-plugin
 * License: GPLv2
 * License URI: https://opensource.org/licenses/gpl-2.0.php
 */
```

To apply it, add a new header for **Update URI** and put a **non** WordPress.org URI in the value:

```
 * Update URI: https://example.com/my-updater/
```

You can also set it to `Update URI: false` if you want. As long as it does not include `worpress.org/plugins` or `w.org/plugins` it will be protected.

## Filtering Updates

Another method, which is supported on older versions of WordPress, is to filter external API requests and discard any for your plugin.

This code, which was written by [Mark Jaquith](https://markjaquith.wordpress.com/2009/12/14/excluding-your-plugin-or-theme-from-update-checks/), can be added to your own plugin:

```
function example_hidden_plugin_12345( $r, $url ) {
    if ( 0 !== strpos( $url, 'https://api.wordpress.org/plugins/update-check' ) )
        return $r; // Not a plugin update request. Bail immediately.

    $plugins = unserialize( $r['body']['plugins'] );
    unset( $plugins->plugins[ plugin_basename( __FILE__ ) ] );
    unset( $plugins->active[ array_search( plugin_basename( __FILE__ ), $plugins->active ) ] );
    $r['body']['plugins'] = serialize( $plugins );
    return $r;
}

add_filter( 'http_request_args', 'example_hidden_plugin_12345', 5, 2 );
```

What that does is check if the update request is from the WordPress.org api, and if it matches the plugin folder and file name of _this_ plugin. If it does, the plugin is removed from the list of plugins to check for updates.
