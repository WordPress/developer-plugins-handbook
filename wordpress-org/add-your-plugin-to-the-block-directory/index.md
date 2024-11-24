# Add Your Plugin to the Block Directory

There are two main directory listings of block related plugins.

- Plugins that are **only** blocks
- Plugins that contain blocks

A plugin can be listed in both, however there will be no benefit to your plugin SEO or ranking due to either. The only benefit to being a plugin that is only a block is that you gain the ability to be added to the Block Directory in the Block Editor itself, for immediate installation.

## Block Only Plugins

Block Only plugins are plugins that **only** contain blocks.

Block Plugins are required to be much smaller and more minimalist than a regular WordPress plugin in order to be safely installed with a single click. That means as well as keeping to the regular [plugin guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/) you’ll also need to follow some additional rules. In particular, you should stick to mostly JavaScript code and keep PHP to the bare minimum; and not add any UI or other code outside of the Gutenberg editor.

If you’re a committer of a block plugin that does meet the criteria for adding it to the [Block Directory](https://wordpress.org/plugins/browse/block/) as confirmed by the Checker tool, you can then add it yourself [using the Block Checker tool](https://wordpress.org/plugins/developers/block-plugin-validator/):

![Block checker tool interface with a "Add Plugin Name to the Block Directory" button](https://i0.wp.com/developer.wordpress.org/files/2020/08/Screen-Shot-2020-07-10-at-1.29.25-pm.png?resize=1024%2C308&ssl=1)

Likewise you can remove it at any time using that same tool if you notice problems or would prefer it wasn’t included.

**Helpful tools:**

- [Block Creation tutorial](https://github.com/WordPress/gutenberg/pull/22831/files?short_path=c4d2c28#diff-c4d2c286eac33acdc7571032a984e0ca) – how to write a block plugin
- [Block Submission tutorial](https://github.com/WordPress/gutenberg/pull/23545/files?short_path=555f1c3#diff-555f1c31856d86ed5ff0d492b5a127c1) – tips and suggestions for ensuring your block is ready for the Block Directory
- [Block Checker tool](https://make.wordpress.org/plugins/2020/07/11/you-can-now-add-your-own-plugins-to-the-block-directory/) – validate plugin for inclusion

## Plugins Containing Blocks

Many older plugins, as well as larger and more complex plugins, may contain blocks. They also will contain other features, like widgets. An example of this sort of plugin would be Jetpack or Yoast SEO. While they have a large number of features, they also contain some blocks.

If you’ve written a plugin that introduces or improves blocks, or know of a plugin that does, **email us at [plugins@wordpress.org](mailto:plugins@wordpress.org)** and request your plugin be added. At that time, your plugin will be reviewed to confirm this request, but also to ensure you meet all current guideline standards.

Before you email, please make certain your plugin is compatible with the latest version of WordPress and that it is free from all security issues. If there are security or guideline issues in your plugin, it may be closed pending your corrections.
