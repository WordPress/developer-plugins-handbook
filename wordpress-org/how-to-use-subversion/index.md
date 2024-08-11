# Using Subversion

SVN, or Subversion, is a version control system similar to Git. It can be used via command line, or one of numerous GUI applications, such as [Tortoise SVN](https://tortoisesvn.net/), [SmartSVN](https://www.smartsvn.com/), and more. If you're new to SVN, we recommend reviewing a [comparison of SVN clients](https://en.wikipedia.org/wiki/Comparison_of_Subversion_clients) before deciding which is best for you.

This document is _not_ a complete and robust explanation for using SVN, but more a quick primer to get started with plugins on WordPress.org. For more comprehensive documentation, see [The SVN Book](https://svnbook.red-bean.com/).

We'll describe here some of the basics about using SVN as it relates to WordPress.org hosting. The basic concepts of SVN, and nearly all code repository services, remain the same.

For additional information, please see these documents:

- [How the readme.txt works](https://developer.wordpress.org/plugins/wordpress-org/how-your-readme-txt-works/).
- [How plugin assets (header images and icons) work](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/).

[warning]SVN and the Plugin Directory are a _release_ repository. Unlike Git, you shouldn't commit every small change, as doing so can degrade performance. Please only push **finished** changes to your SVN repository.[/warning]

## Overview

All your files will be centrally stored in the **svn repository** on our servers. From that repository, anyone can **check out** a copy of your plugin files onto their local machine, but, as a plugin author, only you have the authority to **check in**. That means you can make changes to the files, add new files, and delete files on your local machine and upload those changes back to the central server. It's this process of checking in that updates both the files in the repository and also the information displayed in the WordPress.org Plugin Directory.

Subversion keeps track of all these changes so that you can go back and look at old versions or **revisions** later if you ever need to. In addition to remembering each individual revision, you can tell subversion to **tag** certain revisions of the repository for easy reference. Tags are great for [labeling different releases of your plugin](https://developer.wordpress.org/plugins/wordpress-org/how-to-use-subversion/#tags) and are the only fully supported method of ensuring the correct versions are seen on WordPress.org and updated for users.

## Your Account

Your account for SVN will be the same username (not the email) of the account you used when you submitted the plugin. This is the user ID you use for the WordPress forums as well.

Remember, _capitalization matters_ — if your username is JaneDoe, then you must use the capital J and D or else SVN will fail. You can see the specific capitalization of your name at [Your Profile](https://profiles.wordpress.org/me/profile/edit/group/1/).

If you need to reset your password, go to [login.wordpress.org](https://login.wordpress.org/lostpassword)

## SVN Folders

There are three directories created by default in all SVN repositories.

```
/assets/
/tags/
/trunk/
```

- Use `assets` for [screenshots, plugin headers, and plugin icons](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/).
- Development work belongs in `trunk`.
- Releases go in `tags`.

_A `/branches/` directory that was used for divergent code is no longer created by default, as it was unused._

### Trunk

[warning]Do not put your _main_ plugin file in a subfolder of trunk, like `/trunk/my-plugin/my-plugin.php` as that will break downloads. You may use subfolders for included files.[/warning]

The `/trunk` directory is where your plugin code should live. The trunk can be considered to be the latest and greatest code, however this is not necessarily the latest _stable_ code. Trunk is for the development version. Hopefully, the code in trunk should always be working code, but it may be buggy from time to time because it's not necessarily the "stable" version. For simple plugins, the trunk may be the only version of the code that exists, and that's fine as well.

Even if you do your development work elsewhere (like a git repository), we recommend you keep the trunk folder up to date with your code for easy SVN compares.

### Tags

The `/tags` directory is where you put versions of the plugin. You will use the same version numbers for the sub-directories here as you do for your plugin versioning. It is important that you always use tag folders and proper versioning to ensure your users get the correct code.

Version 1.0 of the plugin will be in `/tags/1.0`, version 1.1 would be in `/tags/1.1`, and so forth.

We **strongly** encourage the use of [semantic software versioning](https://semver.org/).

### Assets

[info]See also: [How Your Plugin Assets Work](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/)[/info]

Assets is where your screenshots, header images, and plugin icons reside. Some older plugins in the directory may have screenshot files in /trunk instead, however this is not recommended. All new plugins should put their screenshots in /assets. This keeps the filesizes of plugins small, as it is not necessary to send screenshots to WordPress installations along with the plugin itself.

### Branches

[alert]The `/branches/` directory is no longer created by default, as it was largely unused. This section can be considered deprecated and is available only for informational purposes.[/alert]

The `/branches` directory is a place that you can use to store branches of the plugin. Perhaps versions that are in development, or test code, etc.

The WordPress.org system **does not** use the branches directory for anything at all, it's considered to be strictly for developers to use as they need it. As it is no longer created by default, you can ignore it as you do not need it any longer.

## Best Practices

In order to make your code the most accessible for other developers, the following practices are considered to be optimum.

### Don't use SVN for development

This is often confusing. Unlike GitHub, SVN is meant to be a _release_ system, not a development system. You don't need to commit and push every small change, and in fact doing so is detrimental to the system. Every time you push code to SVN, it rebuilds _all_ your zip files for all versions in SVN. This is why sometimes your plugin updates don't show for up to 6 hours. Instead, you should push one time, when you're ready to go.

### Use the trunk folder for code

Many people use `trunk` as a placeholder. While it's possible to simply update the `readme.txt` file in trunk and put everything in tag folders, doing so makes it more difficult to compare any changes in your code. Instead, trunk should contain the latest version of your code, even if that version is a beta.

### Always Tag Releases

While it's possible to use trunk as a stable tag for plugins, this feature is not actually supported nor recommended. Instead, releases should be properly tagged and iterated. This will ensure full compatibility with any automatic updater, as well as allow for rollbacks should there be an issue with your code.

### Create tags from trunk

Instead of pushing your code directly to a tag folder, you should edit the code in trunk, complete with the stable version in the readme, and _then_ copy the code from trunk to the new tag.

Not only will this make it easier see any changes, you will be making smaller commits as SVN will only update the changed code. This will save you time and reduce potential errors (such as updating to the wrong stable tag and pushing bad code to users).

Don't worry about the tag folder not existing for a short while. You can use `svn cp` to copy trunk to the tag and then push them up to SVN at the same time.

If you are operating locally, then you can update trunk and create tags from it all in one go. Checkout the root of your repository, update the files in /trunk, then `svn copy /trunk /tags/1.2.3` (or whatever the version number is) and then commit the whole thing in one go. SVN is a system based on differences, and as long as you use svn to do the copy operation, then it preserves history and makes everything easy for others to follow along with.

### Delete old versions

Since SVN is a release repository, many developers chose to remove older (non-supported) versions of their plugins. As of 2019, this no longer speeds up releases, as the build process only addresses tags with changed files.

## Examples

### Starting a New Plugin

To start your plugin, you need to add the files you already have to your new SVN repository.

First create a local directory on your machine to house a copy of the SVN repository:

```
mkdir my-local-dir
```

Next, check out the pre-built repository

```
svn co https://plugins.svn.wordpress.org/your-plugin-name my-local-dir
> A my-local-dir/trunk
> A my-local-dir/branches
> A my-local-dir/tags
> Checked out revision 11325.
```

In our example, subversion has added ("A" for "add") all of the directories from the central SVN repository to your local copy.

To add your code, navigate into the `my-local-dir` folder:

```
cd my-local-dir
```

Now you can add your files to the `trunk/` directory of your local copy of the repository using copy/paste commands via command line, or dragging and dropping. Whatever you're comfortable with.

[warning]Do not put your _main_ plugin file in a subfolder of trunk, like `/trunk/my-plugin/my-plugin.php` as that will break downloads. You may use subfolders for included files.[/warning]

Once your files are in the trunk folder, you must let subversion know you want to add those new files back into the central repository.

```
cd my-local-dir
svn add trunk/*
> A trunk/my-plugin.php
> A trunk/readme.txt
```

After you add all your files, you'll check in the changes back to the central repository.

```
svn ci -m 'Adding first version of my plugin'
> Adding trunk/my-plugin.php
> Adding trunk/readme.txt
> Transmitting file data .
> Committed revision 11326.
```

It's required to include a commit message for all checkins.

If the commit fails because of ‘Access forbidden' and you **know** you have commit access, add your username and password to the check-in command.

```
svn ci -m 'Adding first version of my plugin' --username your_username --password your_password
```

Remember your username is _case sensitive_.

### Editing Existing Files

Once your plugin is in the directory, you will likely need to edit the code at some point.

First go into your your local copy of the repository and make sure it's up to date.

```
cd my-local-dir/
svn up
> At revision 11326.
```

In the above example, we're all up to date. If there had been changes in the central repository, they would have been downloaded and merged into your local copy.

Now you can edit the file that needs changing using whatever editor you prefer.

If you're not using an SVN GUI tool (like SubVersion or Coda) you can still check and see what's different between your local copy and the central repository after you make changes. First we check the status of the local copy:

```
svn stat
> M trunk/my-plugin.php
```

This tells us that our local `trunk/my-plugin.php` is different from the copy we downloaded from the central repository ("M" for "modified").

Let's see what exactly has changed in that file, so we can check it over and make sure things look right.

```
svn diff
> * What comes out is essentially the result of a
  * standard `diff -u` between your local copy and the
  * original copy you downloaded.
```

If it all looks good then it's time to check in those changes to the central repository.

```
svn ci -m "fancy new feature: now you can foo *and* bar at the same time"
> Sending trunk/my-plugin.php
> Transmitting file data .
> Committed revision 11327.
```

And now you've successfully updated trunk.

### "Tagging" New Versions

Each time you make a formal release of your plugin, you should tag a copy of that release's code. This lets your users easily grab the latest (or an older) version, it lets you keep track of changes more easily, and lets the WordPress.org Plugin Directory know what version of your plugin it should tell people to download.

First copy your code to a subdirectory in the `tags/` directory. For the sake of the WordPress.org plugin browser, the new subdirectory should always look like a version number. `2.0.1.3` is good. `Cool hotness tag` is **bad**.

We want to use `svn cp` instead of the regular `cp` in order to take advantage of SVN's features.

```
svn cp trunk tags/2.0
> A tags/2.0
```

As always, check in the changes.

```
svn ci -m "tagging version 2.0"
> Adding tags/2.0
> Adding tags/2.0/my-plugin.php
> Adding tags/2.0/readme.txt
> Committed revision 11328.
```

When tagging a new version, **remember to update** the `Stable Tag` field in [`trunk/readme.txt`](https://wordpress.org/plugins/developers/#readme) to the new version.

Congratulations! You've updated your code!

## Notes

Don't put anything in SVN that you're not willing and prepared to have deployed to everyone who uses your plugin. This _includes_ vendor files, `.gitignore` and everything else.

You also should never upload zip files. Like most code repository systems, SVN expects you to upload individual files.

### See Also

- [How the readme.txt works](https://developer.wordpress.org/plugins/wordpress-org/how-your-readme-txt-works/).
- [How plugin assets (header images and icons) work](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/).
- [The SVN Book](https://svnbook.red-bean.com/).