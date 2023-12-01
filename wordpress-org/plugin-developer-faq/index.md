Plugin Developer FAQ
--------------------

There are lot of ins and outs to hosting WordPress plugins. Please take a minute to see if your question is answered here before reaching out for assistance.

Last Updated: 10 July 2023

You can contact us by email at `plugins@wordpress.org` – we reply to all emails within 7 business days.

No. The review team is made up of 100% volunteers. Some are compensated by their full-time employers, but no one is hired by WordPress.org, Automattic, or WordPress.com

Please take a look at [this handbook page](https://make.wordpress.org/plugins/handbook/apply/).

Go to the [Add](https://wordpress.org/plugins/developers/add/) page and upload your zip. Your file should be under **10 megs** and be a complete plugin. We do not accept placeholders or plugins that aren’t ready to be used.

Double check that you aren’t including unused files (like test folders, documentation, and full node/vendor folders). The majority of plugins who face this issue have included all sorts of development content that has no place in the final code.

You will get an automated email telling you about the submission immediately. At that point, someone will manually download and review your code. If we find no issues with the security, documentation, or presentation, your plugin will be approved. If we determine there are issues, you will receive a second email with details explaining what needs to be fixed.

When you submit a plugin, you get an automated email telling you what the slug will be. This is populated based on the value of Plugin Name in your main plugin file (the one with the plugin headers). If you set yours as `Plugin Name: Boaty McBoatface` then your URL will be `wordpress.org/plugins/boaty-mcboatface` and your slug will be `boaty-mcboatface` for example. If there is an existing plugin with your name, then you’ll get a warning on submission.

This is _also_ the folder name (in SVN and installed on WordPress) for your plugin and your text-domain, so pay attention carefully.

Once your plugin is approved, this name **cannot** be renamed. Please chose wisely.

If we have to change your permalink (slug) we will always email you to explain why. In general, we change your permalink when you have obvious typos or mistakes (foundre instead of founder, for example) or if there are conflicts with existing trademarks or other plugins. Please make sure you read your review email carefully, as we do explain why we have do to things.

There are two reasons this happens:

1.  You’re trying to use a plugin with a permalink that already exists on WordPress.org
2.  You’re trying to use a plugin with a permalink that exists **outside** WordPress.org and has a significant user base.

The first one is obvious. You can’t have two plugins with the same permalink so you need to pick a new one.

The second one is confusing because it’s telling you that somewhere, not on WordPress.org, that permalink is in use. It’s important to understand that the way the plugin update API works is that it compares the plugin folder name (i.e. the permalink) to every plugin it has hosted on WordPress.org. If there’s a match, then it checks for updates and users are prompted to upgrade.

When that happens, users of the ‘original’ plugin (the one we don’t host) would upgrade to the one from WordPress.org and, if that isn’t what you actually wanted to do, you could break their sites.

Sometimes this situation develops when a company or person releases their plugin privately (via Github for example) and decides they want to re-release it on WordPress.org. In those cases, we recommend you email us and we’ll walk you through how to get past the error.

That error is to inform you that you may not begin your Display Name with someone else’s trademarked term. This is to protect you and the directory from legal issues regarding trademark abuse. To correct the issue, you must change the Display Name in your plugin’s readme and main PHP files.

Please do not try to ‘work around’ this by cleverly renaming your plugin (WuuCommerce for example). All that does is make us worry you’re not going to be able to follow guidelines in the future.

Some trademark owners have requested we no longer permit the use of specific terms in plugin names entirely. If you see this error, then you must remove the term from your plugin name.

> To proceed with this submission you must remove “\[TERM\]” from the Plugin Name: line in both your main plugin file and readme entirely.

If you attempt to get around this by changing your term from ‘Facerange’ to ‘Face-Range’, we will pend your submission and reiterate that you cannot use the term. Please don’t try to be sneaky or clever to get past this restriction.

Log in as the official company user account and submit with that account _only_.

We cannot accept plugins submitted by individual developer accounts, unless they’re clearly company ones as well. For example, submitting your official Facerange plugin with a user that has a gmail address is likely to be flagged for trademark infringement.

Just reply to the email right away and let us know. We can transfer ownership for you. If you forget to do this, you can fix it yourself by [adding the correct account as a committer](https://developer.wordpress.org/plugins/wordpress-org/plugin-developer-faq/#how-do-i-give-someone-else-access-to-my-plugin) and then having that account remove your own.

**DO NOT** resubmit your plugin. Just tell us right away and we’ll fix it.

There’s no official average, as no two plugins are the same. If your plugin is small and all the code is correct, it should be approved within **fourteen** days of _initial review_.

If your plugin has any code issues, it will take as long as it takes for you to correct the issues. Either way, you _will_ get an email from `plugins@wordpress.org` with the status, so please add that to your email whitelist and patiently wait for our response.

We aim to reply to all reviews within seven (7) business days. If it’s been less than that, it just means we’ve been really busy. If it’s been two days, like over a weekend or a holiday, then you should not **reasonably** expect a reply.

Remember the review team is made up of 100% volunteers, all of whom have full time day jobs, and other volunteer duties. We do reply promptly, but we also have lives outside of WordPress.

There’s no timeline and as long as we know you’re working on it and we feel you’re making progress, we’ll leave the review open. Your plugin will be rejected after 3 months, but the review will remain open.

If your plugin review is not complete after three (3) months, we will reject your submission in order to keep the queue maintainable. At any point in time, we have 700 people mid-review, and we figure that 3 months is a pretty reasonable time frame.

No. Reply to the email. Even if it’s been 18 months. The longest time to date has been 3 years. We don’t mind if it takes a while.

Just one.

Allowing people to have multiple submissions at once was proven to be detrimental to the review process. Errors were regularly found in all the plugins, resulting in the same emails being sent multiple times. In addition, people often got confused as to which review they were working on, muddying the waters about what needed to be solved. By changing this to one-at-a-time, confusion in those matters dropped significantly.

In addition, many new users don’t know how to use SVN, and wound up submitting multiple plugins and never using any. That can be a drain on our resources, so we do limit people.

Since all plugins get an initial review within two weeks, this should not be a hardship.

No. And if you do so, we will suspend all your secondary accounts. Don’t try to get around the one-at-a-time rule please.

Submit it as early as possible. Unless the plugin is meant to address a security or legal issue, we don’t permit queue jumping. If it _is_ related to one of those, please email `plugins@wordpress.org` and explain the situation.

We look for some pretty obvious things, all of which are listed [in our guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/). Most can be summed up as “Don’t be a spammer,” but to touch on the ones people do the most:

*   Not including a `readme.txt` file when acting as a service
*   Not testing the plugin with `WP_DEBUG`
*   Including custom versions of packaged JavaScript libraries
*   Calling external files unnecessarily
*   “Powered By” links
*   Phoning home

Again, this is a brief overview. Please read the guidelines, as the full list is quite detailed.

We don’t accept plugins that do ‘nothing,’ are illegal, or encourage bad behavior. This includes black hat SEO spamming, content spinners, hate-plugins, and so on.

Similarly we do not accept framework plugins or library plugins. If your plugin has to require other plugins or themes to edit themselves in order to use your plugin, it’s a library. If your plugin is a template from which more code can be built by customizing the files directly, it’s a framework or boilerplate. Frameworks and libraries should be packaged with each plugin (hopefully in a way that doesn’t conflict with other plugins using the framework or libraries). At least until core supports plugin dependencies.

We also don’t accept 100% copies of other people’s work or plugins that duplicate functionality found in WordPress Core. Basically, your plugin should do something new, or in a new way, or solve a specific issue.

No, you should rewrite and upgrade the existing plugin. Make it a major version release. We can’t rename plugins or transfer users, so a new one wouldn’t carry over any existing users, reviews, support topics, ratings, downloads, favorites, etc. Basically you’d leave _all_ your current users out in the cold, and that’s mean.

Every submission gets an automated reply with directions. Reply to that or email `plugins@wordpress.org` and explain the situation.

We can correct plugin slugs before approval, so we are often able to fix that for you. If not, we’ll let you know what to do. We try to catch typos in names before we approve anything, but we make mistakes too.

We have the following restrictions:

*   Plugins may not use vulgarities in the name or slug
*   Plugins may not use ‘WordPress’ or ‘Plugin’ in their slugs except under extreme situations
*   Plugins may not use version numbers in plugin slugs
*   Due to system limitations, only English letters and Arabic numbers are permitted in the slug
*   Plugins may not **start** with a trademarked term or name of a specific project/library/tool _unless_ submitted by an official representative

We encourage everyone to be creative and come up with unique slugs. We automatically correct any plugin that has an unacceptable slug. If there’s a question as to the best choice, we will contact you to be sure.

Put your code files directly in the `trunk/` directory of your repository. Whenever you release a new version, [tag that release](https://developer.wordpress.org/plugins/wordpress-org/how-to-use-subversion/#task-3) by copying the current trunk revision to a new subdirectory of the `tags/` directory.

Make sure you update [`trunk/readme.txt`](https://wordpress.org/plugins/developers/#readme) to reflect the **new** stable tag.

Images for the readme (such as [screenshots, plugin headers, and plugin icons](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/)), belong in the `assets/` directory (which you may need to create) in the root of your SVN checkout. This will be on the same level as `tags/` and `trunk/`, for example.

No. Doing that will cause the zip generator to break.

If you have complicated plugin with lots of files, you can of course organize them into subdirectories, but the [readme.txt file](https://wordpress.org/plugins/developers/#readme) and the root plugin file should go straight into `trunk/`.

Your Subversion tags should look like version numbers. Specifically, they should only contain **numbers and periods**. `2.8.4` is a good lookin’ tag, `my neato releaso` is a bad lookin’ tag. We recommend you use [Semantic Versioning](http://semver.org/) to keep track of releases, but we do not enforce this.

Note that we’re talking about _Subversion_ tags here, not readme.txt search type tags.

As few as possible. Very rarely does anyone need your old code in the release repository. Remember, SVN is **not** meant for your code versioning. You can use Github for stuff like that. SVN should have your current release versions, but you don’t need all the minor releases to all the previous versions. Just the last one or two for them is good.

No. You can add [svn externals](http://svnbook.red-bean.com/en/1.0/ch07s03.html) to your repository, but they won’t get added to the downloadable zip file.

No.

Yes! However you either have to keep the non-minified in your plugin _or_ direct people via your readme as to where they can get the non-minified files.

It’s fine to minify, but it’s not okay to hide it. All code must be human readable for inclusion in this directory.

As soon as you push code to the SVN folders, your plugin will be live. **DO NOT** push code if you’re not ready, as there’s no ‘off’ switch except to [close the plugin](https://developer.wordpress.org/plugins/wordpress-org/plugin-developer-faq/#closed-plugins). As closing a plugin is permanent, we recommend you not push code until you’re ready to go live.

From the information you specify in the plugin file and in the [readme.txt file](https://wordpress.org/plugins/developers/#readme), and from the Subversion repository itself. Read [about how the readme.txt works](https://developer.wordpress.org/plugins/wordpress-org/how-your-readme-txt-works/) for more information.

You should also make full use of the [Plugin Headers](https://developer.wordpress.org/plugins/the-basics/header-requirements/) in your main plugin file. Those will define how your username shows up on the WordPress.org hosting page, as well as in the WordPress Admin. We recommend using all those headers to fully document your plugin.

Yes, by specifying the `Stable Tag` field in your trunk directory’s [readme.txt file](https://wordpress.org/plugins/developers/#readme).

We ask you **not** use ‘trunk’ as your stable tag, as that makes rollbacks more complicated than they need to be.

Logically, whatever version you tested up to. However, never go above the current release candidate. If there is none, don’t go above the active version. So if WordPress’ stable release is 6.0.9, you can use 6.0 to 6.0.9 and everything will be fine. If there is a release of 6.1-RC then you may use 6.1, however you can go no higher.

Do not attempt to be clever and use 6.5 or 7. This will result in errors on your page.

No. If you’re only making cosmetic changes to the readme or your icons/headers, you _do not_ need to release a new version. Just make sure you update the trunk and tag folders.

Yes. Otherwise no one gets updated.

A changelog is a log or record of all or all notable changes made to your plugin, including records of changes such as bug fixes, new features, etc. If you need help formatting your changelogs, we recommend [Keep A Changelog](http://keepachangelog.com/) as  that’s the format used by many products out there.

Always keep the current major release in your change log. For example, if your current version is 3.9.1, you’ll want that and 3.9 in the change log. Older versions should be removed and migrated to a `changelog.txt` file. That will allow them to be accessible to users, while keeping your readme shorter and more pertinent. At most, keep the most recent version of your plugin and one major version back in your readme’s changelog. Your `changelog.txt` will **not** be visible within the WordPress.org Plugin Directory, but that’s okay. Most users just want to know what’s new.

For YouTube and Vimeo videos, simply paste the video link on a line by itself in your description. Note that the video must be set to allow embedding for the embed process to work. For videos hosted by the WordPress.com VideoPress service, use the `[wpvideo]` shortcode. Shortcodes can also be used for YouTube and Vimeo, if needed, just like in WordPress.

That happens when you neglected to use a proper ‘Tested Up To’ value in your headers in your readme. That value should be the latest version of WordPress that you’ve tested your plugin against. If the latest **major** WordPress version is 4.9, then you should have the value `4.9` to indicate compatibility. You do not need to update for minor releases (if your readme is compatible to 4.9 then that will cover 4.9 through 4.9.1000).

Keep in mind, if you put in non-released versions of WordPress (like 6.0) you’ll see the same message.

The WordPress.org Plugin Directory updates every few minutes. However, it may take longer for your changes to appear depending on the size of the update queue. Please give it at least **6 hours** before contacting us.

You can make your own [plugin headers](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/#plugin-headers) by uploading the correctly named files into the `assets` folder. Read [about plugin headers](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/#plugin-headers) for more information.

You can make your own [plugin icons](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/#plugin-icons) by uploading the correctly named files into the `assets` folder. Read [about plugin icons](https://developer.wordpress.org/plugins/wordpress-org/plugin-assets/#plugin-icons) for more information.

Usually no.

Your plugin icon should _never_ be the unaltered, official logo of, say, Facerange. That would be infringing on their property. You may not use official logos for your branding in your banners or icons. Even if you have permission to do so on your site, _we_ don’t have that permission here.

Much like your plugin name, we recommend your icons and headers be something unique to you. They tend to be more memorable that way.

Per the guidelines, [plugins are limited to 12 tags in their readme](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/#12-public-facing-pages-on-wordpress-org-readmes-must-not-spam). This is to control spam. That said, only the first **FIVE** tags will display on WordPress.org, much for the same reason. The first 12 tags are used for searches, and the rest are ignored, so tag-stuffing won’t help you at all.

In addition, any tags where you are the only one who uses them won’t show, because they’re not going to help anyone find another, similar, plugin.

Yes and no. You can change the display name, but the _slug_ — that part of the plugin URL that is yours — cannot be changed once a plugin is approved. That’s why we warn you, multiple times, upon submission.

To change the display name, edit your main plugin file and change the value of “Plugin Name:” to the new name. You also will want to edit your header in your readme.txt to match.

Simply put, because you’re not them.

If you have written an add-on plugin for BooCommerce, you may not name it “BooCommerce Improved Product Search” as that would generate the slug `boocommerce-improved-product-search` and that would conflict with the trademark of ‘BooCommerce.’ That said, it would be acceptable to submit the name “Boo Improved Product Search” which would use the slug `bc-improved-product-search` (“bc” not being trademarked you see).

As another example, if you have a plugin that integrates a service with a a popular cloud hosting company named Amazorn, you may call it “My Service Integration for Amazorn”, but you may **not** use “Amazorn – My Service Integration”. 

Consider the real life example of Keurig. If you made an eco-friendly brew cup, you could market it “EcoBrew Pod for Keurig” but you could NOT attempt to market it as “Keurig EcoBrew Pod.” The latter implies a direct relationship to Keurig and is actually against the law in some countries. In order to protect you, we need you to tread lightly with recognized brand names and trademarks. Always err on the side of caution; if they come and tell us to close your plugin because you used their term as the _first_ word in the display name, we have to do it.

_Note: We no longer have permission to permit new plugins to use `woo` as the start of their permalink, and are required to enforce the use of `wc` instead._

No.

While we understand that companies can and do grant usage permissions, we do not accept them for permalinks for a really important reason: we _**cannot**_ change your permalink once the plugin is approved. This means if, later on, the company changes their mind and rescinds approval, the plugin will be closed and all of it’s users abandoned.

In order to be forward thinking and proactive about a plugin’s long-term life in the directory, we do not accept ‘permission.’ A permalink may not begin with a trademark (or commonly known brand/term) unless it is by the official owners.

It’s impossible to change a plugin’s URL once it’s approved and we warn you about that in multiple places through the process.

Due to that, we deny most requests for ‘new’ plugins to replace old ones just to get a better slug.

This is because we cannot migrate users between plugins nor can we redirect traffic. This means that submitted a new plugin to change a slug is incredibly detrimental to the plugin’s SEO and reputation, as users will be abandoned. The majority of plugins don’t actually need a new URL, and instead just want to edit their display name.

Unless there’s an egregious typo, language, or legal issue related to your slug, we are **unlikely** to approve a new slug. If we do, we will flag your account to note that future rename requests are to be denied.

You’ll need to change it in the readme _and_ the plugin main file.

Don’t use vulgarities or slurs or other intentionally abusive language. You cannot claim, or appear to claim, to be an official source if you’re not. For example, if you’ve made a plugin that connects to the Frozbaz Service, you should call your plugin “Connector to Frozbaz Service” – in this way, you have made it clear you are making a plugin for a service, rather than being the service.

If you’re combining multiple services (a payment gateway to a popular ecommerce plugin, for example), we strongly recommend you come up with an original, unique, display name.

Currently yes, but you shouldn’t. It’s incredibly redundant and doesn’t actually help your SEO in any way, shape, or form. We already put WordPress _and_ Plugin in your page title.

Assuming you actually did apply for trademarks, you certainly _can_ but it’s not commonly done. Not even Google or Facebook do that. Simply by using your trademark term and having a log of it (like your SVN log), you have usually done the needed legal action required to protect your brand. Consult a lawyer for details.

Usually 6 to 14 days after a plugin is committed to SVN. This is because we have to add your data, parse it, and share it to all of our _heavily_ cached servers. It’s not instantaneous. Also as a new plugin, we have no data on usage, so you may need to wait a bit.

Write a good readme for the language, answer support posts promptly, get good reviews.

Neither. Make your display name memorable and descriptive, while keeping it under 5 words, for maximum benefit.

Go to `https://wordpress.org/support/plugin/YOURPLUGIN` and look at the sidebar on the right. Click the Subscribe to this Plugin button for email alerts.

Every plugin support forum page has a “Subscribe” button at the top of it. Click that and you will be emailed. You can see which plugin forums sets you are subscribed to at `https://wordpress.org/support/users/YOURID/subscriptions`

For RSS, visit `https://wordpress.org/support/view/plugin-committer/YOURID` will list all of the support requests and reviews for any plugin you have commit access. Not a committer, just someone listed as an author? Use `https://wordpress.org/support/view/plugin-contributor/YOURID`

You can also go to `https://profiles.wordpress.org/YOURID/profile/notifications/` and put in any terms you want to be emailed for. Be careful, this can escalate if you use generic terms.

You can add Support Representatives to your plugin. Support representatives can mark forum topics as resolved or sticky (same as plugin authors and contributors), but don’t have commit access to the plugin.

The UI for managing plugin support reps can be found in Advanced View on the plugin page, next to managing committers. Once someone is added as a support rep, they will get a Plugin Support badge when replying to the plugin support topics or reviews.

Generally no. A review is a reflection of an individual’s experience with your product. If they didn’t like it, that’s not for us to change. If you feel that a review is invalid (such as for a different plugin), use the `modlook` button on the post. A member of the **forums** team will investigate.

Abuse of the modlook feature may result in suspension of your plugins. Please, use it wisely.

That’s what happens when someone makes multiple accounts on the forums, usually to give themselves a number of 5-star reviews, or create fake support tickets to appear more responsive. Sockpuppeting is against our guidelines and will result in the reviews and posts being removed, but also may result in your account and all plugins being removed. Don’t do it and don’t flagrantly accuse others of doing it.

As of April 2020, you can close your own plugins at any time. To do so, go to the **advanced** tab on your plugin page (i.e. `https://wordpress.org/plugins/myplugin/advanced/`) and scroll down to the **CLOSE THIS PLUGIN** section. There you will see a warning message and a button.

![Image of the "Close this plugin" feature, with the note "WARNING: Closing your plugin is intended to be a permanent action. You will not be able to reopen it without contacting the plugins team." Below that is a button saying "I understand."](https://i3.wp.com/developer.wordpress.org/files/2020/04/HowtoClose-1024x275.png)

If you agree to the warning, and want to close your plugin, press the button.

Keep in mind, you _will not_ get your plugin restored unless you can justify your situation. Closing a plugin by request is intended to be **permanent**.

Email `plugins@wordpress.org` and ask to please have your plugin reopened. However you will be asked how you managed to do that so that we can improve the functionality of the feature.

Assuming you’re logged in as the correct account, it’s probably because you have too many users. If your plugin has more than 10,000 users, you will need to email `plugins@wordpress.org` and request for us to close it.

No.

We do not permit this as it creates a poor experience for users. Hiding plugins makes users think the plugin has been pulled for security or guideline issues, which causes them not to trust you anymore. We cannot prevent what they think, so instead we prohibit ‘temporary’ closures.

Generally people want to do this when their plugin has a bug that is being fixed, or when they’re unable to support it. We recommend you instead just fix the bug as soon as possible, or if you cannot support the plugin, update the readme to say it’s currently unsupported and why.

If this is for a brand new plugin, you should just call it a ‘public beta’ so people are aware of the status.

When a plugin is closed, the page shows as closed and the zips are no longer generated. No one will be able to download the plugin via the website, nor will they be able to install it via the WordPress admin. The SVN repository will remain accessible to allow others to download and fork the code if desired, per the tenets of the directory.

After 60 days, the closure message will change to alert people as to _why_ it was closed but only in the broadest terms (Guideline Violation, Security, etc) and not with explicit details.

Plugins are closed for guideline violations, security issues, or by author requests. In the case of active issues (such as copyright infringement, abuse, and security), all accounts with commit access to a plugin are notified.

If a plugin has never been used within 6 months (i.e. no code has been pushed to SVN), SVN is broken for upwards of 12 months, or a plugin’s readme indicates it’s deprecated, we _may_ close without notification.

As of 2017, plugin closure reasons are tracked in the plugin database. Sixty days after a plugin is closed, the reason for the closure will be made public:

![Example of a closed plugin with the reason 'Author Request'](https://i3.wp.com/developer.wordpress.org/files/2015/04/not-hello-dolly.jpg)

Please note: We do not publicly disclose the details on exactly why a plugin has been closed.

If you report an [security issue](https://developer.wordpress.org/plugins/wordpress-org/plugin-security/reporting-plugin-security-issues/) or a [guideline violation](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/) in a plugin to `plugins@wordpress.org`, we will review and take appropriate action. Most of the time, this involves closing a plugin. Your name will not be disclosed unless you ask for it to be so, in order to protect you from backlash.

Email `plugins@wordpress.org` with a link to the stolen plugin. Include either a link to where we can download yours or attach the zip. We will compare the two files, as well as all the coding history we have, to determine if the plugin is, indeed, theft, or just an uncredited fork.

Keep in mind, if you licensed your plugin as GPLv2 or later, then it’s perfectly permissible to fork your work, as long as copyright remains intact and you’re credited.

Email `plugins@wordpress.org` right away! **Especially** if your code was non-GPL. While we do permit people to fork other plugins and include that code in their own plugins, it must be credited at all times. Copyright and credits are a requirement.

We do our best to uphold copyright and trademark requirements, as well as prevent brand confusion. Before plugin are approved, we often require them to make some of the more obvious changes. That said, there is a limit to how ‘different’ a URL or name can be when we have 60,000 plugins in the directory, and when some terms are quite common (like ‘popup’ or ‘all-in-one’). Because of that, we require developers to change the plugin’s **display name** to no longer cause conflict or confusion.

If someone is clearly infringing on your copyright or trademark or existing brand, be it by display name or use of trademarked images, please email us at `plugins@wordpress.org` with some proof and we will contact the developer and require changes.

We do expect these to be _reasonable_ requests. That is, if you send us a complaint and list 12 plugins that all use the term ‘best contact form’ because that’s your plugin name, we will review the plugins and only close them if they’re using the phrase excessively. If they use it once (i.e. “This is the best contact form plugin in the Faroe Islands”) then it’s acceptable. If they’re keyword stuffing the phrase, we’re more likely to close them for keyword stuffing. Simply, if your plugin name is super generic, this is going to happen, and it’s usually **not** an infringement case.

Also note that if it’s not **your** trademark, we cannot accept your report. It is the responsibility of the trademark owners, not it’s users, to manage and maintain that.

Email `plugins@wordpress.org` a clear and concise description of the issue. [Please read our document on reporting security issues for details](https://developer.wordpress.org/plugins/wordpress-org/plugin-security/reporting-plugin-security-issues/).

No. We have no relationship with any bug bounty programs, so we don’t file your reports etc to them. The only one with which we work is [hackerone.com/automattic](https://hackerone.com/automattic) and that’s for bugs related to Automattic properties. Everything else is on your own, don’t ask us to submit things.

No. We do not have the ability to assist with CVEs.

Maybe. If it was closed for a security reason, fix the issue, reply to the email, and most of the time we’ll reopen the plugin unless it has more security issues or severe guideline issues. If it was closed for guideline violations, it depends on the severity and nature of the violation. Repeat offenders are less likely to have a plugin reopened, for example, than first-timers.

If you asked for the plugin to be closed, you will be expected to explain why the change of heart. Plugins are intended to remain closed when a developer requests it, and not reopened again a month later.

_All_ plugins must pass a current standards and security review in order to be restored. This is not optional. Users will lose more faith in you for having your plugin closed multiple times than they would for one longer closure where you address all the potential issues.

Everyone who represents a plugin, from support tech to developer, is the responsibility of the plugin owner. If they violate the guidelines egregiously, then the owners are expected to accept those consequences and correct course. When that doesn’t happen, plugins get closed. We notify the plugin owners in these cases and explain why and do our best to keep plugins open.

It’s exceptionally rare that we close all of a developer’s plugins. In general it happens because of the following:

1.  You asked us to close all your plugins
2.  Email issues
    1.  The email bounced and we were unable to get in touch
    2.  The email sent us auto-replies and warnings were sent at least twice to fix that
3.  Guideline issues
    1.  Previous censuring for behaviour and/or a final warning was issued
    2.  Delivering legal threats to the directory and/or the volunteers
    3.  The violation was deemed ‘egregious’ (death threats, hundreds of sock puppets, harassment, etc)

If you asked us to close them, you have to explain _why_ the change of heart.

If you’re having email issues, you have to resolve them and you’ll be required to bring all your plugins up to current standards of security and guidelines.

As for that last one … Generally you don’t get to come back from that. If we deliver you a final warning for your behaviour and, within less than a year, you start up again with the issues (or fail to resolve all the issues we mentioned), we’re not going to reopen your plugins.

First and foremost, _take it seriously_. The email will list exactly what the problems have been and why we’ve chosen to escalate to a final warning. Plugin Owners are expected to resolve all the issues, to cease causing new guideline violations, and to closely monitor the actions of any coworkers. In short, stop breaking the guidelines, stop making excuses, apologize for any misbehaviour, and correct course.

The last thing we want to do is ban someone and disable all their plugins. It’s not healthy for the community. At the same time, if a developer is unable or unwilling to play by the same rules as everyone else, it’s detrimental to keep then in the directory and disrespectful to everyone else.

To add users as committers, that is give them access to update code, go to `https://wordpress.org/plugins/YOURPLUGIN/advanced` and add their username in as a committer.

To have them show up as an author, add their username to the `readme.txt` file.

_Do not add regular users as authors._ It’s meant for people who help with development only. This means if someone ‘inspired’ you, you should not add them as an author.

Anyone with commit access can do this. Go to `https://wordpress.org/plugins/YOURPLUGIN/advanced` and hover over their ID. A delete link will appear. Click on it.

Please don’t delete yourself.

Go to the Advanced tab and scroll down to the Danger Zone. There you will see a section for **Transfer Your Plugin**. Pick someone from the dropdown and click the button.

For more details, please read the [documentation on transferring plugins](https://developer.wordpress.org/plugins/wordpress-org/transferring-your-plugin-to-a-new-owner/).

Plugins with a large number of users (over 10,000) or ones that are deemed critical to the WordPress project (such as featured or beta plugins) can only be transfered via written request to the plugins team. [Please read the documentation on transfering plugins for details](https://developer.wordpress.org/plugins/wordpress-org/transferring-your-plugin-to-a-new-owner/).

[We permit users to adopt existing plugins that are no longer currently developed](https://developer.wordpress.org/plugins/wordpress-org/take-over-an-existing-plugin/).

We ask you try to connect with the original developers first, so they can add you. In some case, that’s not possible and you should start with fixing the plugin. Make sure it meets coding standards, is secure, and update the copyright information to include yourself. Then you can contact us regarding [plugin adoption](https://developer.wordpress.org/plugins/wordpress-org/take-over-an-existing-plugin/).

We offer **no** guarantee that you will be given anyone’s plugin, even following a successful review.

Short answer: Probably not.

Many developers receive unsolicited emails or offers to purchase their plugin. We have found the vast majority of these to be fraudulent and do _not_ recommend you follow up with them.

While legitimate offers do come, they’re usually from the official company to whom a plugin is related, or from a well established plugin company. The ones that start “We’re reaching out to the WordPress community …” or “We are looking to acquire existing WordPress plugins …” should not be trusted. Such purchases have often destroyed the reputation of the plugin (and the original developer) by engaging in sleazy tactics such as tracking users or other serious guideline violations.

If you do choose to sell your plugin (or give it away to someone else), please make sure the new owners understand all the [guidelines of the repository](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/). Should they violate our terms the plugin will be removed, and we may not give it back depending on the level of the violation. Whomever has commit access to a plugin has the ownership and responsibility of it’s behavior for users. Spamming, inserting tracking data, and adding junk features are the fastest way to ruin your plugin.

We advocate only giving your plugin to people you _personally_ have vetted, and that you trust with being responsible with your code and your users.

When a developer is determined to have died, they are removed from their own plugins in order to prevent the unethical from gaining access and harming users. If they are the only developer, the plugin may be closed. All attempts are made to find their friends and coworkers, to offer them a chance to adopt the code first, but if no one reliable or willing can be found the plugin is closed.