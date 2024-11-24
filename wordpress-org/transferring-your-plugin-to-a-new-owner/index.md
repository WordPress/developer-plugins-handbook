# Transferring Your Plugin to a New Owner

While any plugin can have an unlimited number of committers and support reps, there is only one official owner of a plugin at any time. This is akin to how a post on WordPress can only have one official post author.

## For Plugins With Under 10,000 Users

If you’re transferring your plugin to a new owner, there are two steps that must take place.

First, add the new user as a **committer** to the plugin:

- go to `https://wordpress.org/plugins/YOURPLUGIN/advanced` and add their username in as a committer
- update the `readme.txt` to add their userID as an author

Next, go to the Advanced tab and scroll down to the Danger Zone. There you will see a section for **Transfer Your Plugin**. Pick someone from the dropdown and click the button.

If there are no other committers, the plugin will not be available to be transferred, so you must do that first.

![Transfer this plugin interface with a selector for the new owner and a "Please transfer -Plugin Name-" button](https://i0.wp.com/developer.wordpress.org/files/2020/04/transfer.jpeg?resize=1024%2C558&ssl=1)

## For Plugins with OVER 10,000 Users (or are beta/featured)

In order to prevent abuse, larger plugins and those officially recognized as featured/beta are restricted from these changes.

To transfer a plugin in this case, you will need to email `plugins@wordpress.org` from the **CURRENT** owner’s email the following information:

1. A _brief_ explanation of the reason for the transfer
2. The user ID of the new owner
3. If applicable, any changes to the status of being a featured/beta plugin

Most requests are processed without issue, however should a plugin be determined to be critical to the WordPress.org project, or should there be reason to believe the request was invalid (i.e. not sent from the current owner’s email, or an email address positively connected back to them), it may be denied or delayed.
