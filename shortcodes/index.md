# Shortcodes

As a security precaution, running PHP inside WordPress content is forbidden; to allow dynamic interactions with the content, Shortcodes were presented in WordPress version 2.5.

Shortcodes are macros that can be used to perform dynamic interactions with the content. i.e creating a gallery from images attached to the post or rendering a video.

## Why Shortcodes?

Shortcodes are a valuable way of keeping content clean and semantic while allowing end users some ability to programmatically alter the presentation of their content.

When the end user adds a photo gallery to their post using a shortcode, they’re using the least data possible to indicate how the gallery should be presented.

Advantages:

- No markup is added to the post content, which means that markup and styling can easily be manipulated on the fly or at a later state.
- Shortcodes can also accept parameters, allowing users to modify how the shortcode behaves on an instance by instance basis.

## Built-in Shortcodes

By default, WordPress includes the following shortcodes:

- `[caption]` – allows you to wrap captions around content
- `[gallery]` – allows you to show image galleries
- `[audio]` – allows you to embed and play audio files
- `[video]` – allows you to embed and play video files
- `[playlist]` – allows you to display collection of audio or video files
- `[embed]` – allows you to wrap embedded items

## Shortcode Best Practices

Best practices for developing shortcodes include the [plugin development best practices](https://developer.wordpress.org/plugins/plugin-basics/best-practices/) and the list below:

- **Always return!** Shortcodes are essentially filters, so creating "[side effects](https://en.wikipedia.org/wiki/Side_effect_(computer_science))" will lead to unexpected bugs.
- Prefix your shortcode names to avoid collisions with other plugins.
- Sanitize the input and escape the output.
- Provide users with clear documentation on all shortcode attributes.

## Quick Reference

See the complete example of using a [basic shortcode structure, taking care of self-closing and enclosing scenarios, shortcodes within shortcodes and securing output](https://developer.wordpress.org/plugins/shortcodes/shortcodes-with-parameters/#complete-example).

## External Resources

- [WordPress Shortcodes Generator](https://generatewp.com/shortcodes/)