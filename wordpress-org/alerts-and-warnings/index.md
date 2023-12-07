# Alerts and Warnings

When you visit plugin pages on WordPress.org, you may notice special alerts or warnings. These exist to help visitors understand the status of various plugins.

## Approved and Pending Data

![Blue background - This plugin is approved and awaiting data upload but not visible to the public yet. Once you make your first commit, the plugin will become public.](https://developer.wordpress.org/files/2018/02/approved.jpg)

Plugins that have been approved but no code has yet been uploaded will see this message:This _only_ displays to the plugin owner and will go away once code has been pushed via SVN.

## Closed

As of November 2017, plugins that are closed display a notice:

![Red background: This plugin has been closed and is no longer available for download.](https://i3.wp.com/developer.wordpress.org/files/2018/02/closed.png)

This is viewable by all visitors and indicates a plugin was closed. Plugins closed after January 2018 will include a date:

![Red background: This plugin was closed on February 7, 2018 and is no longer available for download.](https://i3.wp.com/developer.wordpress.org/files/2018/02/closed-alt.jpg)

After 60 days, the alert will be updated to explain _why_ the plugin was closed:

![Alert detailing why a plugin was closed](https://i3.wp.com/developer.wordpress.org/files/2018/02/why-closed.png)

Plugin committers will see the following additional note:

![Blue background: If you did not request this change, please contact plugins@wordpress.org for a status. All developers with commit access are contacted when a plugin is closed, with the reasons why, so check your spam email too.](https://i3.wp.com/developer.wordpress.org/files/2018/02/closed-owner.png)

### Reasons why plugins are closed

- Author Request – the author has asked the plugin to be closed.
- Guideline Violation – a violation of any of the guideline.
- Licensing/Trademark Violation – non-GPL code in use, or trademarks are being misused.
- Merged Into Core – the plugin is now a part of core (reserved for feature projects).
- Security Issue – a security concern has been found in this plugin.

Additional details on why a plugin is closed are not provided to anyone outside the WordPress.org security team or the plugin authors, unless there is an extreme circumstance.

## Out of Date

Plugins that do not support the last 3 major releases of WordPress have the following notice:

![Yellow background: This plugin hasn’t been tested with the latest 3 major releases of WordPress. It may no longer be maintained or supported and may have compatibility issues when used with more recent versions of WordPress.](https://i3.wp.com/developer.wordpress.org/files/2018/02/old.jpg)

Previously this message alerted users to plugins not updated within the last 2 years. In 2018 it was modified to rely on more pertinent data. Since WordPress updates major releases 2 to 3 times per year, and a maintained a plugin should be testing with the recent versions, this alert can be avoided by updating a plugin readme when new versions of WordPress is released.

Developers are emailed before every major release of WordPress and asked to update this value. They _do not_ need to push a new version, just [update the readme](https://developer.wordpress.org/plugins/wordpress-org/how-your-readme-txt-works/) and edit the value of `Tested up to:` to the latest version of WordPress.