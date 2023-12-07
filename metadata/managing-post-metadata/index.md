# Managing Post Metadata

## Adding Metadata

Adding metadata can be done quite easily with [`add_post_meta()`](https://developer.wordpress.org/reference/functions/add_post_meta/). The function accepts a `post_id`, a `meta_key`, a `meta_value`, and a `unique` flag.

The `meta_key` is how your plugin will reference the meta value elsewhere in your code. Something like `mycrazymetakeyname` would work, however a prefix related to your plugin or theme followed by a description of the key would be more useful. `wporg_featured_menu` might be a good one. It should be noted that the same `meta_key` may be used multiple times to store variations of the metadata (see the unique flag below).

The `meta_value` can be a string, integer, or an array. If it's an array, it will be automatically serialized before being stored in the database.

The `unique` flag allows you to declare whether this key should be unique. A **non** unique key is something a post can have multiple variations of, like price.

If you only ever want **one** price for a post, you should flag it `unique` and the `meta_key` will have one value only.

## Updating Metadata

If a key already exists and you want to update it, use [`update_post_meta()`](https://developer.wordpress.org/reference/functions/update_post_meta/). If you use this function and the key does **NOT** exist, then it will create it, as if you'd used [`add_post_meta()`](https://developer.wordpress.org/reference/functions/add_post_meta/).

Similar to [`add_post_meta()`](https://developer.wordpress.org/reference/functions/add_post_meta/), the function accepts a `post_id`, a `meta_key`, and `meta_value`. It also accepts an optional `prev_value` – which, if specified, will cause the function to only update existing metadata entries with this value. If it isn't provided, the function defaults to updating all entries.

## Deleting Metadata

[`delete_post_meta()`](https://developer.wordpress.org/reference/functions/delete_post_meta/) takes a `post_id`, a `meta_key`, and optionally `meta_value`. It does exactly what the name suggests.

## Character Escaping

Post meta values are passed through the [`stripslashes()`](https://www.php.net/manual/en/function.stripslashes.php) function upon being stored, so you will need to be careful when passing in values (such as JSON) that might include escaped characters.

Consider the JSON value `{"key":"value with \"escaped quotes\""}`:

```
$escaped_json = '{"key":"value with \"escaped quotes\""}';
update_post_meta( $id, 'escaped_json', $escaped_json );
$broken = get_post_meta( $id, 'escaped_json', true );
/*
$broken, after stripslashes(), ends up unparsable:
{"key":"value with "escaped quotes""}
*/
```

### Workaround

By adding one more level of escaping using the function [`wp_slash()`](https://developer.wordpress.org/reference/functions/wp_slash/) (introduced in WP 3.6), you can compensate for the call to [`stripslashes()`](https://www.php.net/manual/en/function.stripslashes.php):

```
$escaped_json = '{"key":"value with \"escaped quotes\""}';
update_post_meta( $id, 'double_escaped_json', wp_slash( $escaped_json ) );
$fixed = get_post_meta( $id, 'double_escaped_json', true );
/*
$fixed, after stripslashes(), ends up as desired:
{"key":"value with \"escaped quotes\""}
*/
```

## Hidden Custom Fields

If you are a plugin or theme developer and you are planning to use custom fields to store parameters, it is important to note that WordPress will not show custom fields which have `meta_key` starting with an "_" (underscore) in the custom fields list on the post edit screen or when using the [`get_post_meta()`](https://developer.wordpress.org/reference/functions/get_post_meta/) template function.

This can be useful in order to show these custom fields in an unusual way by using the [`add_meta_box()`](https://developer.wordpress.org/reference/functions/add_meta_box/) function.

The example below will add a unique custom field with the `meta_key` name '_color' and the `meta_value` of 'red' but this custom field will not display in the post edit screen:

```
add_post_meta( 68, '_color', 'red', true );
```

### Hidden Arrays

In addition, if the `meta_value` is an array, it will not be displayed on the page edit screen, even if you don't prefix the `meta_key` name with an underscore.