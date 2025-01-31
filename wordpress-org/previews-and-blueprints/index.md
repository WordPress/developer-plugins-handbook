# Previews and Blueprints

If you haven’t noticed it yet, the WordPress Playground is an amazing feature that lets anyone safely run a temporary WordPress install within their browser. It uses WASM to run a complete WordPress install – PHP, database, and all – entirely from within your web browser. No server needed, nothing to install.

For a while now Playground has supported loading any plugin or theme from the plugin directory; here’s how. 

## The Plugin Preview Button

The Plugin Preview feature adds a convenient button to plugins in the plugin directory, when enabled by a plugin's developers. The button takes the user to Playground with that plugin installed. It’s right beside the Download button.

The Preview button is not shown by default; it must be explicitly enabled. Developers can use blueprint files in order to configure the preview environment and install dependencies (such as other plugins and themes).

## Enabling Plugin Previews

There are two things required for a plugin preview button to appear to all users:

1. A valid `blueprint.json` file must be provided in a blueprints sub-directory of the plugin’s assets folder.
2. The plugin preview must be set to “public” from the plugin’s Advanced view by a committer.

If a valid `blueprint.json` file is present, then the Preview button will be present for plugin committers only. In which case it will look like this:

![The Test Preview button allows plugin authors to showcase what their plugin does with one click.](https://i0.wp.com/developer.wordpress.org/files/2024/03/live-preview.png?resize=554%2C140&ssl=1)

It’s called Test Preview because that’s why it’s there: to allow plugin committers to test their plugin in the Playground environment and decide whether or not to make it easily available to the public.

## Blueprints

Blueprints are json files used to set up a WordPress Playground instance. 

They can be used to specify things like PHP and WP versions, the landing page, and (most importantly) a series of automated steps such as logging in, and installing and activating plugins and themes.

The blueprint for your plugin should be committed to the assets folder with subversion as `assets/blueprints/blueprint.json`. Initially only the one blueprint file is supported, but we expect to allow multiple in future.

Here’s an example of a simple blueprint.json file that you could use as a starting point:

```
{
    "landingPage": "\/wp-admin\/edit.php",
    "preferredVersions": {
        "php": "7.4",
        "wp": "5.9"
    },
    "phpExtensionBundles": [
        "kitchen-sink"
    ],
    "steps": [
        {
            "step": "login",
            "username": "admin",
            "password": "password"
        }
    ]
}
```

The features used here are:

`landingPage`, which specifies the URL of the page that the user will land on when the preview loads.

`preferredVersions`, which specifies versions of PHP and WordPress.

`phpExtensionBundles`, which in this case specifies that we want most common PHP extensions to be available (kitchen-sink).

`steps`, which tells Playground what to do before displaying the landing page. In this case, it will simply log the user in to wp-admin.

Here’s an example of a more advanced blueprint.json that demonstrates some more features you could use to create a rich demo environment for users:

```
{
    "landingPage": "/wp-admin/post.php?post=5&action=edit",
    "preferredVersions": {
        "php": "7.4",
        "wp": "5.9"
    },
    "phpExtensionBundles": [
        "kitchen-sink"
    ],
    "steps": [
        {
            "step": "login",
            "username": "admin",
            "password": "password"
        },
        {
            "step": "installPlugin",
            "pluginZipFile": {
                "resource": "wordpress.org\/plugins",
                "slug": "my-imaginary-plugin-dependency"
            },
            "options": {
                "activate": true
            }
        },
        {
            "step": "installPlugin",
            "pluginZipFile": {
                "resource": "wordpress.org\/plugins",
                "slug": "my-imaginary-plugin"
            },
            "options": {
                "activate": true
            }
        },
        {
            "step": "installTheme",
            "themeZipFile": {
                "resource": "wordpress.org\/themes",
                "slug": "my-imaginary-theme"
            }
        },
        {
            "step": "setSiteOptions",
            "options": {
                "some_required_option_1": "your_favorite_values",
                "some_required_option_2": "your_favorite_values"
            }
        },
        {
            "step": "runPHP",
            "code": "<?php require_once 'wordpress/wp-load.php'; wp_insert_post(array('post_title' => 'wp-load.php required for WP functionality', 'post_status' => 'publish')); ?>"
        }
    ]
}
```

# Using a generated Blueprint file

You might see a notice similar to this on your plugin's page:

```
Your plugin does not yet have a blueprint file for user previews. If you'd like to enable previews, please follow these steps to create a blueprint.

1. Test your plugin in Playground.
2. Fix any bugs in your plugin that prevent it from working in Playground.
3. Download blueprint.json
4. Commit your blueprint to svn.
```

The __Test__ link will use an auto-generated Blueprint file to load your plugin in Playground, with some default configuration values and steps. The __Download blueprint.json__ link will let you download that auto-generated `blueprint.json` file, which you can then modify as needed and commit to Subversion when your plugin is ready for Playground previews.

# Committing a Blueprint to Subversion

You must commit your blueprint.json file to your plugin's assets folder, named like this:

`/assets/blueprints/blueprint.json`
