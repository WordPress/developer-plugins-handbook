# Compliance Disclaimers

As of **April 12, 2018**, [Guideline 9](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/#9-developers-and-their-plugins-must-not-do-anything-illegal-dishonest-or-morally-offensive) (Developers and their plugins must not do anything illegal, dishonest, or morally offensive.) has been amended to include the following new prohibition:

- implying that a plugin can create, provide, automate, or guarantee legal compliance

Some plugins offer to assist a site with being ‘compliant’ with laws like the ADA, GDPR, EU Cookie Law, VAT, HIPPA, PCI, and so on.

No plugin can offer 100% legal compliance. They can (and do) assist sites with being more compliant. Still, at the end of the day, the responsibility remains with the site administrators to ensure their sites meet the qualifications for any compliance. Even services are not providing full compliance, just compliance when _their_ service is in use.

In order to make this more clear to site administrators, we recommend that plugins do **not** claim to be 100% compliant, and instead to explain that the plugin itself will assist in compliance. This has the added benefit of protecting developers in the case where compliance guidelines change and the plugin has not yet been updated.

## What do I need to do?

tl;dr: Update your readme, documentation, assets (banners, screenshots, etc), and descriptions to clearly state that your plugin is meant to **assist** in compliance.

For example, if your plugin says it “will make your website ADA compliant.” you should change that to “will help make your website more ADA compliant.” In addition, it would be wise to add in a note that “no plugin can provide 100% compliance” and then enumerate what yours does to get people closer. It’ll help your sales pitches too!

## Do I need to change my plugin display name?

If your plugin name claims or implies compliance, yes.

For example, if your plugin display name is “100% EU Cookie Law Compliance” or “VAT-MOSS Compliance” then you should change the display name to “Improve EU Cookie Law Compliance” or “VAT-MOSS Compliance Assistant”

Keep in mind, a BETTER plugin name would be one that is unique to you. Remember, people can find “Foobar’s EU Cookie Law Tools” easier than “EU Cookie Law Tools” – they’ll remember your name easier.

## My plugin’s a service and is 100% compliant. Do I still need to do this?

Yes, but in a slightly different way.

A service assumes the responsibility for the compliance needed in those cases, and that’s what needs to be clear in the readme: it’s not the plugin code, it’s the service. We strongly recommend you link to your proof of compliance, and that is has a date on it.

For example, instead of saying “Foobar Payment Plugin is PCI compliant.” you could say “The Foobar Payment Gateway Service handles PCI compliance, however this only pertains to purchases made using our gateway. Full details on our compliance can be found in our `[documentation](https://service-name.com/doc/)`.”

## My plugin uses a 3rd party library that claims 100% compliance. Do I need to change that?

Not unless you also claim it in your own readme/documentation. Though you should be a good human and warn them that what they’re doing isn’t really accurate.

## I only talk about 100% XHTML compliance. Do I have to change things?

Technically no, but … can you **really** promise 100% compliance forever and ever? Probably best to change that and just say “Updated for further XHTML compliance.” Check out how WordPress.org handles [Accessibility compliance](https://wordpress.org/about/accessibility/) for a good place to start.

## What happens if I don’t do this?

If we find you’re making inaccurate claims, we will warn you. Then if it’s not fixed in a reasonable amount of time (60 days) your plugin may be closed. Worst than that, however, you open yourself up for legal disputes. This is not actually our responsibility to protect you from, however we feel that being good stewards of open source includes educating you as to these things. Essential, this is something that could seriously hurt you (or your company), and we’d rather that not happen.

In addition, from this point forward, if your plugin is closed for other issues (guideline or security related), we will require you to update the readme before we will reopen the plugin.

## How do I report other plugins for this violation?

Same way you would anything. Email `plugins@wordpress.org` with a link to their plugin and an explanation as to why you feel they’re in violation. Keep in mind, closing a plugin is a last resort, so we may already be talking to them about it. Also remember pointing out other people making this mistake doesn’t give you a free pass. You have to fix your own stuff too.
