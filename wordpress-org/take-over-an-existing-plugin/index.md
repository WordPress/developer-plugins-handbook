# Take Over an Existing Plugin

People cease development on their plugins for a variety of reasons. Instead of letting those plugins stagnate, we encourage people to instead list them for adoption by another, more active, developer.

In adopting a plugin, you are promising to be responsible for all future development, and to ensure the plugin (and you) comply with all [plugin guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/).

[alert]Not all requests will be approved, even following a successful review.[/alert]

## The Adoption Process

There are two ways a plugin can be adopted.

1. You ask the developer directly, they say yes and add you
2. You ask the Plugin Review team to assist you

Since you’re reading this, you likely are working on the second method, so keep reading. You will be expected to have followed **all** the steps herein.

### 1. Check the Plugin Status

If the plugin is open and active, give it a full review on your own before you go any further. Make sure you feel comfortable and capable of maintaining the code long term.

If a plugin is closed because it was **unused**, you can skip the rest of this and email `plugins@wordpress.org` right away and attach your version of the proposed plugin.

If the plugin was closed for security issues, we require **all** those issues to be resolved, so put your best foot forward and demonstrate you have the impetus to find and fix those issues.

Closed plugins are _less_ likely to be able to be adopted, as the nature of their closures may be more complex and intricate. If a plugin was closed for license issues, for example, we may not be permitted to reopen it for anyone except the license holders.

Larger plugins (100k users or more) are also less likely to be adopted, as that is a not-insignificant userbase, and we need to be sure you really are capable of managing a plugin of that size.

### 2. Contact the Original Developer

You _must_ attempt to contact the original developer, as they can [give you access to the plugin](https://developer.wordpress.org/plugins/wordpress-org/plugin-developer-faq/#plugin-ownership). We recommend trying:

- email
- leaving a comment on the plugin support page
- opening a GitHub issue

We expect you to make all reasonable efforts to reach out to them. If the plugin page says the plugin has no active developer, then you’re fine.

If you _do_ get in touch with the developer, ask them to [transfer ownership to you](https://developer.wordpress.org/plugins/wordpress-org/plugin-developer-faq/#plugin-ownership). They can do this on their own and, once it’s done, you may manage the plugin. If they have issues, have them contact the plugin team via email and we will assist them.

If there’s no way to get in touch, or they don’t reply, move to step 3.

### 3. Update The Code

Even if the plugin has been given to you by the developer, you must review the code from the top down to make sure it’s safe, secure, and meets our current [guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/).

Your update must include editing the readme to ensure it documents the new ownership (and preferably when it takes place), removing all links to their site/support resources, as well as updating the copyright information to include you. Remember, copyright is additive. You keep the old and add yours on.

If your plugin is a major upgrade, you _must_ provide an upgrade path. Just wanting a name-slug is not sufficient reason to take over a plugin. We care deeply about our users, and violating their trust in us by breaking their existing sites with your upgrades is to be avoided at all costs.

Remember you need to _increase_ the version number so people are prompted to upgrade to your new version.

### 4. Submit Your Code for Review

If you still can’t get in touch with the original developer, you’ll need to ask the Plugin Review Team for help.

Once you’ve finished updating the code, email `plugins@wordpress.org` explaining how you tried to contact the original developer and with your code attached as a zip. If you can’t email zips, then upload it to some file service (Google Drive, Dropbox, etc) or provide a link to your code repository (Github, Bitbucket, etc). Make sure the link is public!

After we receive your version of the code as a zip, we will review it and test it. At this point, you will go through a _normal_ review process. That is, we will treat you like any new plugin and we will check the whole plugin for security and guidelines. Even if those issues are found in the original plugin, you will be required to fix them.

At this stage, some plugins are determined to have existing security flaws. We may close those plugins, depending on the nature of the issues, and you will be trusted to not publicly disclose those issues.

### 5. We Contact the Original Developer

Once we feel the code is acceptable, and that you are capable of sustaining that specific plugin in a secure manner that adheres to our guidelines, we will contact the original developer on your behalf and give them your information, explaining that you want to take over development.

We’ll do everything we can to ensure the original plugin author has been notified, but sometimes that’s just not possible.

### 6. Wait Patiently

We give the original developer 30 days (1 month) to reply to our inquiry. Should they reply and deny the request, we will honour their decision and ask you to convert the plugin into a forked version. We do our best to respect them as much as we respect you as a developer, and honor their wishes with their work.

If they approve it, we will assist in transitioning the plugin to your account.

If they don’t reply, and you’ve made it this far, we will likely transfer the plugin to you.

### 7. Update the Plugin

In order to _safely_ update the plugin, we will close it before we add you. You will then be required to update via SVN. Once that’s done, we’ll double check everything is correct and reopen it. The plugin will now be yours.

## Frequently Asked Questions

### Will old reviews/support posts be removed?

No. You inherit the good and the bad.

### Do I have to keep the original developer on?

No. You can (and in fact should) remove commit access from anyone who is not actively maintaining the plugin. However. Per copyright restrictions, you must retain their credit in the code. We recommend you also leave them listed as a contributor.

### The original developer is dead. Does that change anything?

Yes, but not how you’re thinking. You (obviously) can skip asking them for permissions first, but we actually reach out to the developers’ coworkers and teams to see if they want to continue maintaining the plugin. In some cases, a developer will ask us to permanently close their plugins in the event of their death. We respect their wishes.

### Why was my request denied?

In cases where we deny an adoption, it’s usually for the following reasons.

- The requesting developer does not have the experience we feel the plugin requires
- The requested plugin is deemed high-risk
- The existing developer is a company or legal entity who owns the trademark
- The plugin has legal issues preventing us from from any transfers
- The requesting developer has had multiple guideline infractions
- The original developer asked us not to

If we don’t feel comfortable handing over a plugin, we will inform you as soon as possible.

There are rare cases where the plugin has already been given to new owners, but they have not yet deployed code. In general, if you’re told that a specific plugin is not available, there is a long history concerning the plugin preventing us from permitting takeover. In those cases, we recommend you submit your plugin as a fork.
