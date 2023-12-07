# Debug Bar and Add-Ons

The Debug Bar is a plugin that adds a debug menu to the admin bar that shows query, cache, and other helpful debugging information.

## Debug Bar

This is the main plugin, adding the base functionality that is extended by the remaining plugins listed on this page.

When `WP_DEBUG` is enabled it also tracks PHP Warnings and Notices to make them easier to find.

When `SAVEQUERIES` is enabled the mysql queries are tracked and displayed.

[Visit Debug Bar](https://wordpress.org/plugins/debug-bar/)

## Debug Bar Console

This plugin adds a console in which you can run arbitrary PHP. This is excellent for testing the contents of variables, among many other uses.

[Visit Debug Bar Console](https://wordpress.org/plugins/debug-bar-console/)

## Debug Bar Shortcodes

This plugin adds a new panel to the Debug Bar that displays the registered shortcodes for the current request.

Additionally it will show you:

- Which function/method is called by the shortcode.
- Whether the shortcode is used on the current post/page/post type and how (only when on singular).
- Any additional information available about the shortcode, such as a description, which parameters it takes, whether or not it is self-closing.
- Find out all pages/posts/etc on which a shortcode is used.

[Visit Debug Bar Shortcodes](https://wordpress.org/plugins/debug-bar-shortcodes/)

## Debug Bar Constants

This plugin adds three new panels to the Debug Bar that display the defined constants available to you as a developer for the current request:

- WP Constants
- WP Class Constants
- PHP Constants

[Visit Debug Bar Constants](https://wordpress.org/plugins/debug-bar-constants/)

## Debug Bar Post Types

This plugin adds a new panel to the Debug Bar that displays detailed information about the registered post types for your site.

[Visit Debug Bar Post Types](https://wordpress.org/plugins/debug-bar-post-types/)

## Debug Bar Cron

This plugin adds a new panel in the Debug Bar displaying information about WordPress' scheduled events.

Once installed, you will have access to the following information:

- Number of scheduled events.
- If cron is currently running.
- Time of next event.
- Current time.
- List of custom scheduled events.
- List of core scheduled events.
- List of schedules.

[Visit Debug Bar Cron](https://wordpress.org/plugins/debug-bar-cron/)

## Debug Bar Actions and Filters Addon

This plugin adds two more tabs in the Debug Bar to display hooks (Actions and Filters) attached to the current request. Actions tab displays the actions hooked to current request. Filters tab displays the filter tags along with the functions attached to it with respective priority.

[Visit Debug Bar Actions and Filters Addon](https://wordpress.org/plugins/debug-bar-actions-and-filters-addon/)

## Debug Bar Transients

This plugin adds information about WordPress transients to a new panel in the Debug Bar.

Once installed, you will have access to the following information:

- Number of existing transients.
- List of custom transients.
- List of core transients.
- List of custom site transients.
- List of core site transients.
- An option to delete a transient.

[Visit Debug Bar Transients](https://wordpress.org/plugins/debug-bar-transients/)

## Debug Bar List Script & Style Dependencies

This plugin lists scripts and styles that are loaded, in which order they're loaded, and what dependencies exist.

[Visit Debug Bar List Script & Style Dependencies](https://wordpress.org/plugins/debug-bar-list-dependencies/)

## Debug Bar Remote Requests

This plugin will add a new panel to Debug Bar that will display and profile remote requests made through the HTTP API.

Once installed, you will have access to the following information:

- Request method (GET, POST, etc).
- URL.
- Time per request.
- Total time for all requests.
- Total number of requests.

Optionally, you can add `?dbrr_full=1` to your URL to get additional information, including all request parameters and a full dump of the response with headers.

[Visit Debug Bar Remote Requests](https://wordpress.org/plugins/debug-bar-remote-requests/)