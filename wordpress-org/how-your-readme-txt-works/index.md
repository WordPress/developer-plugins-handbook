# Plugin Readmes

This page will explain some aspects of the plugin directory, and explain of the more obvious aspects which a lot of people miss.

To make your entry in the plugin browser most useful, each plugin should have a readme file named `readme.txt` that adheres to the [WordPress plugin readme file standard](https://wordpress.org/plugins/readme.txt). This file controls the output on the front-facing part of the directory. Writing a description in the readme determines exactly what will be displayed on `wordpress.org/plugins/Your-Plugin`

You can use the [plugin readme generator](https://generatewp.com/plugin-readme/) and put your completed result through the [official readme validator](https://wordpress.org/plugins/developers/readme-validator/) to check it. If you need more visual assistance you can use the tool [wpreadme.com](https://wpreadme.com/)

Since WordPress 5.8 plugin [readme files are not parsed for requirements](https://core.trac.wordpress.org/ticket/48520). This means that headers `Requires PHP` and `Requires at least` are going to be parsed from plugin’s main PHP file.

## Section Details

All plugins contain a main PHP file, and almost all plugins have a `readme.txt` file as well. The `readme.txt` file is intended to be written using a subset of markdown.

### Readme Header Information

The Plugin readme header consists of this information:

```
=== Plugin Name ===
Contributors: (this should be a list of wordpress.org userid's)
Donate link: https://example.com/
Tags: tag1, tag2
Requires at least: 4.7
Tested up to: 5.4
Stable tag: 4.3
Requires PHP: 7.0
License: GPLv2 or later
License URI: https://www.gnu.org/licenses/gpl-2.0.html
Here is a short description of the plugin.  This should be no more than 150 characters.  No markup here.
```

- **Contributors** – a case sensitive, comma separated list of all WordPress.org usernames who have contributed to the code. It is generally considered respectful to include the names of people who worked on forked projects. Some developers will ask to be removed from the list, as they don’t want other plugins showing up on their profile page. It’s best to honor those requests. Remember to _only_ use the WordPress.org username – anything else will show up sans profile link and gravatar. To change someone’s display name (which shows on the front facing pages for the plugin), edit the profile `https://wordpress.org/support/users/YOURID/edit/` and change the display name.
- **Donate link** – (OPTIONAL) Makes a “Donate to this plugin” link in the sidebar. If there is no link, nothing shows up.
- **Tags** – 1 to 5 comma separated terms that describe the plugin. Plugins must refrain from using competitors plugin names as tags. Plugins should not use tags which are unique to that plugin, as these will not be shown.
- **Tested up to** – The version of WordPress that the plugin has been tested against. This field _ignores_ minor versions, as plugins shouldn’t break with a minor update. This means a plugin only need to define the major version it is tested against and the WordPress.org plugin directory will automatically add the minor version. This should be numbers _only_, so ‘4.9’ and not ‘WP 4.9’
- **Requires PHP** – (OPTIONAL) The required _minimum_ version of PHP needed for use with this plugin. This should be numbers _only_, so ‘7.0’ and not ‘PHP 7.0’
- **Stable Tag** – The stable version of the plugin. This is _not_ the version of WordPress, but the version of the plugin itself. Only use numbers and periods, and [SemVer formatting](https://semver.org/) is recommended.
- **License** – The GPLV2 (or later) compatible license used for the plugin.
- **License URI** – (OPTIONAL) A link to the license. This is optional, but if a plugin uses a more rare license, strongly recommended.

At the end of the header section is a place for a _short_ description of a plugin. The example recommends no more than 150 characters and to not use markup. That line of text is the single line description of the plugin which shows up right under the plugin name. If it’s longer than 150 characters, it gets cut off, so keep it short.

### Installation

If your plugin has no custom install settings, it’s okay to omit this section. If your plugin has custom configuration notes post install, this is a great place to put that information.

### Custom Sections

While custom sections are permitted and supported, please use them in moderation. People get used to seeing how every other plugin looks, and when yours is weird, they’ll miss important information.

## Technical Details

While most of the readme details are self evident, there are a few sections that trip people up.

### How The Readme Is Parsed

[warning]While using the stable tag of trunk currently works in the Plugin Directory, it’s not a supported or recommended method to indicate new versions and has been known to cause issues with automatic updates. At this time, we are actively discouraging and (in the case of new plugins) prohibiting it’s use[/warning]

WordPress.org’s Plugin Directory works based on the information found in the field **Stable Tag** in the readme. When WordPress.org parses the `readme.txt`, the very first thing it does is to look at the `readme.txt` in the `/trunk` directory, where it reads the “Stable Tag” line.

When the Stable Tag is properly set, WordPress.org will go and look in `/tags/` for the referenced version. So a Stable Tag of “1.2.3” will make it look for `/tags/1.2.3/`.

[tip]The readme.txt in the tag folder must also be properly updated to have the correct “Stable Tag” — failing to do so may cause your plugin to not be updatable.[/tip]

If the Stable Tag is 1.2.3 and `/tags/1.2.3/` exists, then nothing in trunk will be read any further for parsing by any part of the system. If you try to change the description of the plugin in `/trunk/readme.txt` then your changes won’t do anything on your plugin page. Everything comes from the `readme.txt` in the file being pointed to by the Stable Tag.

The WordPress.org Plugin Directory reads the main plugin PHP file to get things like the Name of the plugin, the Plugin URI, and most importantly, the version number. On the plugin page, you’ll see the download button which reads “Download Version 1.2.3” or similar. That version number comes from the plugin’s main PHP file, _not_ the readme!

The Stable Tag points to a subdirectory in the `/tags` directory. But the version of the plugin is not actually set by that folder name. Instead, it’s the version that is listed in the plugin’s PHP file itself which determines the name. If you have changed Stable Tag to 1.4 and the plugin still says 1.3 in the PHP file, then the version listed will be 1.3.

### Videos

You can embed videos from YouTube, Vimeo, and anywhere else [WordPress supports by default](https://wordpress.org/support/article/embeds/). All you have to do is paste the video URL onto it’s own line in your readme.

We recommend you NOT have the video as the final line in a FAQ section, as sometimes formatting gets weird.

### Markdown

The readmes use a customized version of [Markdown](https://daringfireball.net/projects/markdown/). Most Markdown calls work as expected.

Markdown allows for easy linking in your readme.txt as well. Just write like this to link a word to a URL:

\[WordPress\](http://wordpress.org)

Videos can be put into your readme.txt too. A YouTube or Vimeo link on a line by itself will be auto-embedded. It’s also possible to embed videos hosted on VideoPress using the wpvideo shortcode.

### Field Details

For those who want to know exactly what gets parsed into what:

- **Authors**
  Author field from the plugin header and Contributors field from the readme file.
- **Version**
  Version field from the plugin header.
- **Tags** (as in categories)
  Tags field from the readme file.
- **Plugin Name**
  The Plugin Name from the readme file falling back on the Plugin Name specified in the plugin header.
- **Author and Plugin Homepages**
  The Author URI and Plugin URI fields of the plugin header. The Plugin URI should be _unique_ to each plugin. Do **not** use the same URI for your free and pro plugin. It ends badly.
- **Last updated time**
  Time of last check in to the appropriate directory after a version number change.
- **Creation time**
  Time of _first_ check in.

### File Size

While readmes are simple text files, having a file larger than 10k may result in errors. Your readme should be brief and to the point. The description should not be a sales pitch as much as a description of the plugin, what it does, and how to use it. Your install directions should be direct. Your FAQ should actually address issues.

As for your changelog, we recommend keeping the current release in the readme and splitting the rest out out into it’s own file — `changelog.txt` for example. By storing all the older changelog data there, you keep your readme small and allow the people who get really into long changelogs to read things on their own.

Similarly, if you need in-depth documentation with inline images and so on, direct people to your own website.

## See Also

- [Plugin Headers](https://developer.wordpress.org/plugins/plugin-basics/header-requirements/) (found in the main file of the plugin)
