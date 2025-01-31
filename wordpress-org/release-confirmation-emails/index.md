# Release Confirmation Emails

In order to better protect plugins from inadvertent releases, we’ve added a new **opt-in** feature for plugins: Release Confirmation.

When enabled on a plugin, to release a new version of a plugin you’ll need to tag a new release in SVN as normal, and then confirm on the [Release Management](https://wordpress.org/plugins/developers/releases/) dashboard.

Access to the Release Management dashboard is limited to plugin committers, and all actions require confirmation via a tokenised link which is emailed to you as needed.

When WordPress.org detects a new version has been tagged in SVN, all committers are automatically sent an email notifying them that a new release is pending, who made it, and a link to the dashboard to confirm the release.
Plugin committers will also see a banner on the top of your WordPress.org plugins page directing you to the Release Management dashboard.

Once confirmed, the release will proceed as usual and should update within a few minutes.

## Requirements

-   Plugins are required to use tags for new versions, you cannot release your plugin from trunk.
-   You must still update the `Stable Tag:` header in your trunk/readme.txt file.
-   Once released, alterations cannot be made to the tagged version. This includes changing the readme (for tested up to).
-   Committers must be able to receive emails directly, and not have it go to a shared inbox or support ticket system.

## Requiring double approval

Release Confirmations are opt-in, but only requires a singular committer to confirm the release.
If a group would like to require multiple committers to approve a release, please contact the Plugins team with your request.
