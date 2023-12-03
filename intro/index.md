# Introduction to Plugin Development

Welcome to the Plugin Developer Handbook. Whether you're writing your first plugin or your fiftieth, we hope this resource helps you write the best plugin possible.

The Plugin Developer Handbook covers a variety of topics — everything from what should be in the plugin header, to security best practices, to tools you can use to build your plugin. It's also a work in progress — if you find something missing or incomplete, please notify the documentation team in slack and we'll make it better together.

## Why We Make Plugins

If there's one cardinal rule in WordPress development, it's this: **Don't touch WordPress core**. This means that you don't edit core WordPress files to add functionality to your site. This is because WordPress overwrites core files with each update. Any functionality you want to add or modify should be done using plugins.

WordPress plugins can be as simple or as complicated as you need them to be, depending on what you want to do. The simplest plugin is a single PHP file. The [Hello Dolly](https://wordpress.org/plugins/hello-dolly/ "Hello Dolly Plugin") plugin is an example of such a plugin. The plugin PHP file just needs a [Plugin Header](https://developer.wordpress.org/plugins/plugin-basics/header-requirements/), a couple of PHP functions, and some [hooks](https://developer.wordpress.org/plugins/hooks/) to attach your functions to.

Plugins allow you to greatly extend the functionality of WordPress without touching WordPress core itself.