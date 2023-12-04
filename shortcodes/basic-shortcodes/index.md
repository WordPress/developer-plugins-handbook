# Basic Shortcodes

## Add a Shortcode

It is possible to add your own shortcodes by using the Shortcode API. The process involves registering a callback `$func` to a shortcode `$tag` using `add_shortcode()`.

```
add_shortcode(
  string $tag,
  callable $func
);
```

`[wporg]` is your new shortcode. The use of the shortcode will trigger the `wporg_shortcode` callback function.

```
add_shortcode( 'wporg', 'wporg_shortcode' );
function wporg_shortcode( $atts = [], $content = null) {
  // do something to $content
  // always return
  return $content;
}
```

## Remove a Shortcode

It is possible to remove shortcodes by using the Shortcode API. The process involves removing a registered `$tag` using [`remove_shortcode()`](https://developer.wordpress.org/reference/functions/remove_shortcode/).

```
remove_shortcode(
  string $tag
);
```

Make sure that the shortcode have been registered before attempting to remove. Specify a higher priority number for [`add_action()`](https://developer.wordpress.org/reference/functions/add_action/) or hook into an action hook that is run later.

## Check if a Shortcode Exists

To check whether a shortcode has been registered use [`shortcode_exists()`](https://developer.wordpress.org/reference/functions/shortcode_exists/).