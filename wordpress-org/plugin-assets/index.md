The `assets` folder in your plugin is where you can store images (like plugin headers, icons, and screenshots) used on your plugin’s display page.

All files should be placed into the `assets` directory of your SVN directory and will work for all versions of your plugin. This is a top level directory, just like `trunk`. You would **not** place the screenshots into `trunk/assets` or `tags/1.0/assets`.

All images are served through a CDN and cached heavily, so it may take a some time to update when changed or added. Please give the proxy some time to retrieve your images and cache them before reporting it’s not working. It should only take a few minutes, but 6 hours is not unheard of when the servers are under high load (like the week before and during a release of a major version of WordPress).

[Default Image Sizes](#default-image-sizes)
-------------------------------------------

Image sizes should be the same as implied by the names. That is, `banner-772x250.png` should be 772 pixels wide by 250 pixels high. Similarly, `icon-256x256.png` should be a 256×256 square.

We have **not** defined any new banner sizes so please don’t try to be clever and rename yours thinking they’ll work. They simply won’t show. Similarly, don’t make your images larger (or smaller) and use the existing names. Things will look terrible.

[Plugin Headers](#plugin-headers)
---------------------------------

Plugin headers are those images you see at the top of a plugin page:

[![](https://i0.wp.com/developer.wordpress.org/files/2015/05/Screenshot-2024-07-02-at-12.50.04%E2%80%AFPM.png?resize=1024%2C472&ssl=1)](https://i0.wp.com/developer.wordpress.org/files/2015/05/Screenshot-2024-07-02-at-12.50.04%E2%80%AFPM.png?ssl=1)

When designing your header image, keep in mind the use of international plugin directories. Some of these, like [Hebrew](https://he.wordpress.org/plugins/) and [Arabic](https://ar.wordpress.org/plugins/), use Right-To-Left (RTL) languages. Ideally, design your banner such that the elements included in the image can be positioned from right to left or from left to right. You can create a different image for RTL pages, with `-rtl` in the name.

### [Filenames](#banner-filenames)

* Normal Banner: `banner-772x250.(jpg|png)`
* Normal Banner (Localized): `banner-772x250-(rtl|es|es_ES).(jpg|png)`
* High-DPI (Retina): `banner-1544x500.(jpg|png)`
* High-DPI (Retina Localized): `banner-1544x500-(rtl|es|es_ES).(jpg|png)`

Images can be localised to a specific language, or for all RTL languages.  
The locale can be specified as a full locale (`es_ES`) or as a partial locale (`es`), if the language is RTL and a locale-specific image isn’t provided, the `rtl` image will be checked for.  
Do not duplicate English images into RTL and locale-specific files without making alterations, the English variant will be used instead.

For example of an RTL image, look at bbPress [in English](https://wordpress.org/plugins/bbpress/) and then [in Arabic](https://ar.wordpress.org/plugins/bbpress/).

For an example of Retina images, [check out Hello Dolly](https://wordpress.org/extend/plugins/hello-dolly/) or [Pluginception](https://wordpress.org/plugins/pluginception/). You _cannot_ use the retina image alone, it only works as an “add-on” to the 772×250 image. The larger ‘retina’ image is only used on displays that can show the higher detail.

4MB is the maximum size for headers images, but smaller is better.

[Plugin Icons](#plugin-icons)
-----------------------------

Plugin icons are square images that show on the side of the plugin in searches on WordPress.org but also on the back-end of WordPress.org.

[![Akismet with it's Plugin Icon](https://i0.wp.com/developer.wordpress.org/files/2015/05/akismet1.png?resize=651%2C235&ssl=1)](https://i0.wp.com/developer.wordpress.org/files/2015/05/akismet1.png?ssl=1)

In addition to JPG and PNG formats, you can also use **SVG**. Vectors are perfect for icons, as they can be scaled to any size and the file itself is small. If you chose to use SVGs, you _must_ also use a PNG icon as a fallback, otherwise your plugin icon will not display properly in older browsers or on Facebook.

If you do not use an icon, an auto-generated one will be made for you. Some examples are the circled icons below:

[![Example of auto-generated icons](https://i0.wp.com/developer.wordpress.org/files/2015/05/auto-generated-icons.jpg?resize=798%2C332&ssl=1)](https://i0.wp.com/developer.wordpress.org/files/2015/05/auto-generated-icons.jpg?ssl=1)

1MB is the maximum file size for icons, but as with headers, the smaller the better.

### [Filenames](#filenames)

* Normal: `icon-128x128.(png|jpg|gif)`
* High-DPI (Retina): `icon-256x256.(png|jpg|gif)`
* SVG: `icon.svg`

There are no plans to change these sizes.

[Screenshots](#screenshots)
---------------------------

Screenshots show on the main page of your plugin, and are used to illustrate aspects of the plugin admin dashboard or live examples. There should be **one** screenshot for every line in your `readme.txt` file. The content of the lines will become the captions of the screenshots on your plugin’s page.

For example: `1. This is a monkey`

That would show the caption ‘This is a monkey’ under the first screenshot. Presumably of a monkey.

Screenshots _must_ be local to display. Links to external files won’t work.

10MB is maximum file size for screenshots, but as always, the smaller the better.

### [Filenames](#filenames-2)

* `screenshot-1.(png|jpg)`
* `screenshot-2.(png|jpg)`

All filenames should be lowercase; uppercase names won’t work.

Screenshots can be localized [similar to banners](#banner-filenames), the following filenames would take priority over the above English names when the plugin is viewed in German:

* `screenshot-1-de.(png|jpg)`
* `screenshot-2-de.(png|jpg)`

[Issues](#issues)
-----------------

If you find your images are downloading when people click on them from your WordPress.org Plugin Directory page, you’ll need to make a change in how you upload them via SVN. It’s due to how some images are sent with the `Content-Type application/octet-stream`.

To fix it, run this command:

    svn propset svn:mime-type image/png *.png
    svn propset svn:mime-type image/jpeg *.jpg

Alternatively, plugin authors can set this in their ~/.subversion/config file:

    *.png = svn:mime-type=image/png
    *.jpg = svn:mime-type=image/jpeg

That’ll apply to only new files though. Fixing already-committed files will require the command above.

