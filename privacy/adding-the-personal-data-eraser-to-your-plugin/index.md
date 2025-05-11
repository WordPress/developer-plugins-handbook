# Adding the Personal Data Eraser to Your Plugin

In WordPress 4.9.6, new tools were added to make compliance easier with laws like the European Union's General Data Protection Regulation, or GDPR for short. Among the tools added is a Personal Data Removal tool which supports erasing/anonymizing personal data for a given user. It does NOT delete registered user accounts – that is still a separate step the admin can choose whether or not to do.

In addition to the personal data stored in things like WordPress comments, plugins can also hook into the eraser feature to erase the personal data they collect, whether it be in something like postmeta or even an entirely new Custom Post Type (CPT).

Like the exporters, the "key" for all the erasers is the user's email address – this was chosen because it supports erasing personal data for both full-fledged registered users and also unregistered users (e.g. like a logged out commenter).

However, since performing a personal data erase is a destructive process, we don't want to just do it without confirming the request, so the admin-facing user interface starts all requests by having the admin enter the username or email address making the request and then sends then a link to click to confirm their request. Once a request has been confirmed, the admin can kick off personal data erasure for the user, or force one if the need arises.

The way the personal data export is erased is similar to how the personal data exporters – and relies on hooking "eraser" callbacks to do the dirty work of erasing the data. When the admin clicks on the remove personal data link, an AJAX loop begins that iterates over all the erasers registered in the system, one at a time. In addition to erasers built into core, plugins can register their own eraser callbacks.

The eraser callback interface is designed to be as simple as possible. An eraser callback receives the email address we are working with, and a page parameter as well. The page parameter (which starts at 1) is used to avoid plugins potentially causing timeouts by attempting to erase all the personal data they've collected at once. A well behaved plugin will limit the amount of data it attempts to erase per page (e.g. 100 posts, 200 comments, etc.)

The eraser callback replies whether items containing personal data were erased, whether any items containing personal data were retained, an array of messages to present to the admin (explaining why items that were retained were retained) and whether it is done or not. If an eraser callback reports that it is not done, it will be called again (in a separate request) with the page parameter incremented by 1.

When all the erasers have been called to completion, the admin user interface is updated to show whether or not all personal data found was erased, and any messages explaining why personal data was retained.

Let's work on a hypothetical plugin which adds commenter location data to comments. Let's assume the plugin has used [`add_comment_meta`](https://developer.wordpress.org/reference/functions/add_comment_meta/) to add location data using `meta_key`'s of `latitude` and `longitude`.

The first thing the plugin needs to do is to create an eraser function that accepts an email address and a page, e.g.:

```
/**
 * Removes any stored location data from a user's comment meta for the supplied email address.
 *
 * @param string $email_address   email address to manipulate
 * @param int    $page            pagination
 *
 * @return array
 */
function wporg_remove_location_meta_from_comments_for_email( $email_address, $page = 1 ) {
  $number = 500; // Limit us to avoid timing out
  $page   = (int) $page;

  $comments = get_comments(
    array(
      'author_email' => $email_address,
      'number'       => $number,
      'paged'        => $page,
      'order_by'     => 'comment_ID',
      'order'        => 'ASC',
    )
  );

  $items_removed = false;

  foreach ( (array) $comments as $comment ) {
    $latitude  = get_comment_meta( $comment->comment_ID, 'latitude', true );
    $longitude = get_comment_meta( $comment->comment_ID, 'longitude', true );

    if ( ! empty( $latitude ) ) {
      delete_comment_meta( $comment->comment_ID, 'latitude' );
      $items_removed = true;
    }

    if ( ! empty( $longitude ) ) {
      delete_comment_meta( $comment->comment_ID, 'longitude' );
      $items_removed = true;
    }
  }

  // Tell core if we have more comments to work on still
  $done = count( $comments ) < $number;
  return array(
    'items_removed'  => $items_removed,
    'items_retained' => false, // always false in this example
    'messages'       => array(), // no messages in this example
    'done'           => $done,
  );
}
```

The next thing the plugin needs to do is to register the callback by filtering the eraser array using the [`wp_privacy_personal_data_erasers`](https://developer.wordpress.org/reference/hooks/wp_privacy_personal_data_erasers/) filter.

When registering you provide a friendly name for the eraser (to aid in debugging – this friendly name is not shown to anyone at this time) and the callback, e.g.

```
/**
 * Registers all data erasers.
 *
 * @param array $exporters
 *
 * @return mixed
 */
function wporg_register_privacy_erasers( $erasers ) {
  $erasers['my-plugin-slug'] = array(
    'eraser_friendly_name' => __( 'Comment Location Plugin', 'text-domain' ),
    'callback'             => 'wporg_remove_location_meta_from_comments_for_email',
  );
  return $erasers;
}

add_filter( 'wp_privacy_personal_data_erasers', 'wporg_register_privacy_erasers' );
```

And that's all there is to it! Your plugin will now clean up its personal data!
