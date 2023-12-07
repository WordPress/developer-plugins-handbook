# Localization

## What is localization?

Localization describes the subsequent process of translating an internationalized plugin. Localization is often abbreviated as `l10n` (because there are 10 letters between the l and the n).

## Localization files

### POT (Portable Object Template) files

This file contains the original strings (in English) in your plugin.

### PO (Portable Object) files

Every translator will take the `POT` file and translate the `msgstr` sections into their own language. The result is a `PO` file with the same format as a `POT`, but with translations and some specific headers. There is one `PO` file per language.

### MO (Machine Object) files

From every translated `PO` file a `MO` file is built. These are machine-readable, binary files that the gettext functions actually use (they don't care about `.POT` or `.PO` files), and are a "compiled" version of the `PO` file. The conversion is done using the `msgfmt` command line tool. In general, an application may use more than one large logical translatable module and a different `MO` file accordingly. A text domain is a handle of each module, which has a different `MO` file.

## Generating the POT file

The `POT` file is the one you need to hand to translators, so that they can do their work. The `POT` and `PO` files can easily be interchangeably renamed to change file types without issues.

[info]It is a good idea to offer the POT file along with your plugin, so that translators won't have to ask you specifically about it.[/info]

There are a couple of ways to generate a `POT` file for your plugin:

### WP-CLI

Install [WP-CLI](https://make.wordpress.org/cli/handbook/guides/installing/) and use the `wp i18n make-pot` command according to the [documentation](https://developer.wordpress.org/cli/commands/i18n/make-pot/).

### Poedit

You can also use [Poedit](https://poedit.net/) locally when translating. This is an open source tool for all major OSs. The free Poedit default version supports manual scanning of all source code with Gettext functions. A pro version of it also features one-click scanning for WordPress plugins. After generating the `PO` file you can rename the file to `POT`. If a `MO` was generated then you can delete that file as it is not needed. If you don't have the pro version you can easily get the [Blank POT](https://github.com/fxbenard/Blank-WordPress-Pot) and use that as the base of your `POT file`. Once you have placed the blank `POT` in the languages folder you can click "Update" in Poedit to update the `POT` file with your strings.

### Grunt Tasks

There are even some grunt tasks that you can use to create the POTs. [grunt-wp-i18n](https://github.com/cedaro/grunt-wp-i18n) and [grunt-pot](https://www.npmjs.com/package/grunt-pot). Steps on setting up grunt are beyond the scope of this documentation, but just be aware that it is possible. Here is an [example Grunt.js and package.json](https://gist.github.com/grappler/10187003) that you can place in the root of your plugin.

## Translate the PO file

Save the translated file as `my-plugin-{locale}.mo`. The locale is the language code and/or country code. For example, the locale for German is `de_DE`. From the code example above the text domain is 'my-plugin' therefore the German MO and PO files should be named `my-plugin-de_DE.mo` and `my-plugin-de_DE.po`. For more information about language and country codes, see [Installing WordPress in Your Language](https://developer.wordpress.org/advanced-administration/before-install/in-your-language/).

There are multiple ways to translate a `PO` file.

### Manually

You can use a text editor to enter the translation. In a text editor it will look like this.

```
#: plugin-name.php:123
msgid "Page Title"
msgstr ""
```

You enter the translation between the quotation marks. For the German translation it would look like this.

```
#: plugin-name.php:123
msgid "Page Title"
msgstr "Seitentitel"
```

### Poedit

You can also use [Poedit](https://poedit.net/) when translating. The free Poedit default version supports manual scanning of all source code with Gettext functions. A pro version of it also features one-click scanning for WordPress plugins and themes.

### Online Services

A third option is to use an online translation service. The general idea is that you upload the `POT` file and then you can give permission to users or translators to translate your plugin. This allows you to track the changes, always have the latest translation and reduce the translation being done twice.

Here are a few tools that can be used to translate PO files online:

- [Transifex](https://www.transifex.com/)
- [WebTranslateIt](https://webtranslateit.com/)
- [Poeditor](https://poeditor.com/)
- [GlotPress](https://wordpress.org/plugins/glotpress/)

## Generate MO file

### Command line


`msgfmt` is used to create the MO file. `msgfmt` is part of Gettext package. Otherwise command line can be used. A typical `msgfmt` command looks like this:

#### Unix Operating Systems

```
msgfmt -o filename.mo filename.po
```

#### Windows Operating Systems

```
msgfmt -o filename.mo filename.po
```

If you have a lot of `PO` files to convert at once, you can run it as a batch. For example, using a `bash` command:

#### Unix Operating Systems (multiple files)

```
# Find PO files, process each with msgfmt and rename the result to MO
for file in `find . -name "*.po"` ; do msgfmt -o ${file/.po/.mo} $file ; done
```

#### Windows Operating Systems (multiple files)

For Windows you need to install [Cygwin](https://www.cygwin.com/) first.

Create a file called `potomo.sh` and put the following into it:

```
#! /bin/sh
# Find PO files, process each with msgfmt and rename the result to MO
for file in `/usr/bin/find . -name '*.po'` ; do /usr/bin/msgfmt -o ${file/.po/.mo} $file ; done
```

You can run this command in the command line.

```
cd C:/path/to/language/folder/my-plugin/languages & C:/cygwin/bin/bash -c /cygdrive/c/path/to/script/directory/potomo.sh
```

### Poedit

`msgfmt` is also integrated in [Poedit](https://poedit.net/) allowing you to use it to generate the MO file. There is a setting in the preferences where you can enable or disable it.

![Internationalization Localization](https://i3.wp.com/developer.wordpress.org/files/2014/10/internationalization-localization-04.jpg)

### Grunt task

There is [grunt-po2mo](https://www.npmjs.com/package/grunt-po2mo) that will convert all of the files.

## Tips for Good Translations

### Don't translate literally, translate organically

Being multi-lingual, you undoubtedly know that the languages you speak have different structures, rhythms, tones, and inflections. Translated messages don't need to be structured the same way as the English ones: take the ideas that are presented and come up with a message that expresses the same thing in a natural way for the target language. It's the difference between creating an equal message and an equivalent message: don't replicate, replace. Even with more structural items in messages, you have creative license to adapt and change if you feel it will be more logical for, or better adapted to, your target audience.

### Try to keep the same level of formality (or informality)

Each message has a different level of formality or informality. Exactly what level of formality or informality to use for each message in your target language is something you'll have to figure out on your own (or with your team), but WordPress messages (informational messages in particular) tend to have a politely informal tone in English. Try to accomplish the equivalent in the target language, within your cultural context.

### Don't use slang or audience-specific terms

Some amount of terminology is to be expected in a blog, but refrain from using colloquialisms that only the "in" crowd will get. If the uninitiated blogger were to install WordPress in your language, would they know what the term means? Words like pingback, trackback, and feed are exceptions to this rule; they're terminology that are typically difficult to translate, and many translators choose to leave in English.

### Read other software's localizations in your language

If you get stuck or need direction, try reading through the translations of other popular software tools to get a feel for what terms are commonly used, how formality is addressed, etc. Of course, WordPress has its own tone and feel, so keep that in mind when you're reading other localizations, but feel free to dig up UI terms and the like to maintain consistency with other software in your language.

## Using Localizations

Place the localization files in the language folder, either in the plugin `languages` folder or as of WordPress 3.7 in the plugin `languages` folder normally under `wp-content`. The full path would be `wp-content/languages/plugins/my-plugin-fr_FR.mo`.

You can change the language in the "General Settings". If you do not see this option, or the language into which you want to switch i snot listed, do it manually:

- Define `WPLANG` inside of `wp-config.php` to your chosen language. For example, if you wanted to use French, you would have: `define ('WPLANG', 'fr_FR');`
- Go to `wp-admin/options-general.php` or "Settings" -> "General"
- Select your language in "Site Language" dropdown
- Go to `wp-admin/update-core.php`
- Click "Update translations", when available
- Core translations files are downloaded, when available

## Resources

- [Creating .pot file for your theme or plugin](https://foxland.fi/creating-pot-file-for-your-theme-or-plugin/)
- [How To Internationalize WordPress Plugins](https://tommcfarlin.com/internationalize-wordpress-plugins/)
- [Translating Your Theme](https://code.tutsplus.com/translating-your-theme--wp-25014t)
- [Blank WordPress POT](https://github.com/fxbenard/Blank-WordPress-Pot)
- [Improved i18n WordPress tools](https://github.com/grappler/i18n)
- [How to update translations quickly](https://ulrich.pogson.ch/update-translations-quickly)
- [Gist: Complete Localization Grunt task](https://gist.github.com/grappler/10187003)
- WordPress.tv
  - [i18n](https://wordpress.tv/tag/i18n/)
  - [internationalization](https://wordpress.tv/tag/internationalization/)
  - [translation](https://wordpress.tv/tag/translation/)