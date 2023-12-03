# What is a Plugin?

Plugins are packages of code that extend the core functionality of WordPress. WordPress plugins are made up of PHP code and can include other assets such as images, CSS, and JavaScript.

By making your own plugin you are _extending_ WordPress, for example, building additional functionality on top of what WordPress already offers, or you could write a plugin that displays links to the ten most recent posts on your site.

Or, using WordPress' custom post types, you could write a plugin that creates a full-featured support ticketing system with email notifications, custom ticket statuses, and a client-facing portal. The possibilities are _endless_!_

Most WordPress plugins are composed of many files, but a plugin really only _needs_ one main file with a specifically formatted [DocBlock](https://en.wikipedia.org/wiki/PHPDoc) in the header.

[Hello Dolly](https://wordpress.org/plugins/hello-dolly/ "Hello Dolly"), one of the first plugins, is only [100 lines](https://plugins.trac.wordpress.org/browser/hello-dolly/trunk/hello.php) long. Hello Dolly shows lyrics from [the famous song](https://en.wikipedia.org/wiki/Hello,_Dolly!_(song)) in the WordPress admin. Some CSS is used in the PHP file to control how the lyric is styled.

As a WordPress.org plugin author, you have an amazing opportunity to create a plugin that will be installed, tinkered with, and loved by millions of WordPress users. All **you** need to do is turn your great idea into code. The Plugin Handbook is here to help you with that.