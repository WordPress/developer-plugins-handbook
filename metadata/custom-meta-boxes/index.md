# Custom Meta Boxes

## What is a Meta Box?

When a user edits a post, the edit screen is composed of several default boxes: Editor, Publish, Categories, Tags, etc. These boxes are meta boxes. Plugins can add custom meta boxes to an edit screen of any post type.

The content of custom meta boxes are usually HTML form elements where the user enters data related to a Plugin's purpose, but the content can be practically any HTML you desire.

## Why Use Meta Boxes?

Meta boxes are handy, flexible, modular edit screen elements that can be used to collect information related to the post being edited. Your custom meta box will be on the same screen as all the other post related information; so a clear relationship is established.

Meta boxes are easily hidden from users that do not need to see them, and displayed to those that do. Meta boxes can be user-arranged on the edit screen. The users are free to arrange the edit screen in a way that suits them, giving users control over their editing environment.

[alert]All examples on this page are for illustration purposes only. The code is not suitable for production environments.

Operations such as [securing input](https://developer.wordpress.org/apis/security/sanitizing/), [user capabilities](https://developer.wordpress.org/plugins/security/checking-user-capabilities/), [nonces](https://developer.wordpress.org/apis/security/nonces/), and [internationalization](https://developer.wordpress.org/plugins/internationalization/) have been intentionally omitted. Be sure to always address these important operations.[/alert]

## Adding Meta Boxes

To create a meta box use the [`add_meta_box()`](https://developer.wordpress.org/reference/functions/add_meta_box/) function and plug its execution to the [`add_meta_boxes`](https://developer.wordpress.org/reference/hooks/add_meta_boxes/) action hook.

The following example is adding a meta box to the `post` edit screen and the `wporg_cpt` edit screen.

```
function wporg_add_custom_box() {
  $screens = [ 'post', 'wporg_cpt' ];
  foreach ( $screens as $screen ) {
    add_meta_box(
      'wporg_box_id',                 // Unique ID
      'Custom Meta Box Title',      // Box title
      'wporg_custom_box_html',  // Content callback, must be of type callable
      $screen                            // Post type
    );
  }
}
add_action( 'add_meta_boxes', 'wporg_add_custom_box' );
```

The `wporg_custom_box_html` function will hold the HTML for the meta box.

The following example is adding form elements, labels, and other HTML elements.

```
function wporg_custom_box_html( $post ) {
  ?>
  <label for="wporg_field">Description for this field</label>
  <select name="wporg_field" id="wporg_field" class="postbox">
    <option value="">Select something...</option>
    <option value="something">Something</option>
    <option value="else">Else</option>
  </select>
  <?php
}
```

[info]**Note there are no submit buttons in meta boxes.** The meta box HTML is included inside the edit screen's form tags, all the post data including meta box values are transfered via `POST` when the user clicks on the Publish or Update buttons.[/info]

The example shown here only contains one form field, a drop down list. You may create as many as needed in any particular meta box. If you have a lot of fields to display, consider using multiple meta boxes, grouping similar fields together in each meta box. This helps keep the page more organized and visually appealing.

### Getting Values

To retrieve saved user data and make use of it, you need to get it from wherever you saved it initially. If it was stored in the `postmeta` table, you may get the data with [`get_post_meta()`](https://developer.wordpress.org/reference/functions/get_post_meta/).

The following example enhances the previous form elements with pre-populated data based on saved meta box values. You will learn how to save meta box values in the next section.

```
function wporg_custom_box_html( $post ) {
  $value = get_post_meta( $post->ID, '_wporg_meta_key', true );
  ?>
  <label for="wporg_field">Description for this field</label>
    <select name="wporg_field" id="wporg_field" class="postbox">
      <option value="">Select something...</option>
      <option value="something" <?php selected( $value, 'something' ); ?>>Something</option>
      <option value="else" <?php selected( $value, 'else' ); ?>>Else</option>
    </select>
    <?php
}
```

More on the [`selected()`](https://developer.wordpress.org/reference/functions/selected/) function.

### Saving Values

When a post type is saved or updated, several actions fire, any of which might be appropriate to hook into in order to save the entered values. In this example we use the `save_post` action hook but other hooks may be more appropriate for certain situations. Be aware that `save_post` may fire more than once for a single update event. Structure your approach to saving data accordingly.

You may save the entered data anywhere you want, even outside WordPress. Since you are probably dealing with data related to the post, the `postmeta` table is often a good place to store data.

The following example will save the `wporg_field` field value in the `_wporg_meta_key` meta key, which is hidden.

```
function wporg_save_postdata( $post_id ) {
  if ( array_key_exists( 'wporg_field', $_POST ) ) {
    update_post_meta(
      $post_id,
      '_wporg_meta_key',
      $_POST['wporg_field']
    );
  }
}
add_action( 'save_post', 'wporg_save_postdata' );
```

In production code, remember to follow the security measures outlined in the info box!

## Behind the Scenes

You don't normally need to be concerned about what happens behind the scenes. This section was added for completeness.

When a post edit screen wants to display all the meta boxes that were added to it, it calls the [`do_meta_boxes()`](https://developer.wordpress.org/reference/functions/do_meta_boxes/) function. This function loops through all meta boxes and invokes the `callback` associated with each.

In between each call, intervening markup (such as divs, titles, etc.) is added.

## Removing Meta Boxes

To remove an existing meta box from an edit screen use the [`remove_meta_box()`](https://developer.wordpress.org/reference/functions/remove_meta_box/) function. The passed parameters must exactly match those used to add the meta box with [`add_meta_box()`](https://developer.wordpress.org/reference/functions/add_meta_box/).

To remove default meta boxes check the source code for the parameters used. The default [`add_meta_box()`](https://developer.wordpress.org/reference/functions/add_meta_box/) calls are made from `wp-includes/edit-form-advanced.php`.

## Implementation Variants

So far we've been using the procedural technique of implementing meta boxes. Many plugin developers find the need to implement meta boxes using various other techniques.

### OOP

Adding meta boxes using OOP is easy and saves you from having to worry about naming collisions in the global namespace.

To save memory and allow easier implementation, the following example uses an abstract Class with static methods.

```
abstract class WPOrg_Meta_Box {

  /**
   * Set up and add the meta box.
   */
  public static function add() {
    $screens = [ 'post', 'wporg_cpt' ];
    foreach ( $screens as $screen ) {
      add_meta_box(
        'wporg_box_id',          // Unique ID
        'Custom Meta Box Title', // Box title
        [ self::class, 'html' ],   // Content callback, must be of type callable
        $screen                  // Post type
      );
    }
  }

  /**
   * Save the meta box selections.
   *
   * @param int $post_id  The post ID.
   */
  public static function save( int $post_id ) {
    if ( array_key_exists( 'wporg_field', $_POST ) ) {
      update_post_meta(
        $post_id,
        '_wporg_meta_key',
        $_POST['wporg_field']
      );
    }
  }

  /**
   * Display the meta box HTML to the user.
   *
   * @param WP_Post $post   Post object.
   */
  public static function html( $post ) {
    $value = get_post_meta( $post->ID, '_wporg_meta_key', true );
    ?>
    <label for="wporg_field">Description for this field</label>
    <select name="wporg_field" id="wporg_field" class="postbox">
      <option value="">Select something...</option>
      <option value="something" <?php selected( $value, 'something' ); ?>>Something</option>
      <option value="else" <?php selected( $value, 'else' ); ?>>Else</option>
    </select>
    <?php
  }
}

add_action( 'add_meta_boxes', [ 'WPOrg_Meta_Box', 'add' ] );
add_action( 'save_post', [ 'WPOrg_Meta_Box', 'save' ] );
```

### AJAX

Since the HTML elements of the meta box are inside the `form` tags of the edit screen, the default behavior is to parse meta box values from the `$_POST` super global _after the user have submitted the page_.

You can enhance the default experience with AJAX; this allows to perform actions based on user input and behavior; regardless if they've submitted the page.

#### Define a Trigger

First, you must define the trigger, this can be a link click, a change of a value or any other JavaScript event.

In the example below we will define `change` as our trigger for performing an AJAX request.

```
/*jslint browser: true, plusplus: true */
(function ($, window, document) {
  'use strict';
  // execute when the DOM is ready
  $(document).ready(function () {
    // js 'change' event triggered on the wporg_field form field
    $('#wporg_field').on('change', function () {
      // our code
    });
  });
}(jQuery, window, document));
```
#### Client Side Code

Next, we need to define what we want the trigger to do, in other words we need to write our client side code.

In the example below we will make a `POST` request, the response will either be success or failure, this will indicate wither the value of the `wporg_field` is valid.

```
/*jslint browser: true, plusplus: true */
(function ($, window, document) {
  'use strict';
  // execute when the DOM is ready
  $(document).ready(function () {
    // js 'change' event triggered on the wporg_field form field
    $('#wporg_field').on('change', function () {
      // jQuery post method, a shorthand for $.ajax with POST
      $.post(wporg_meta_box_obj.url,                        // or ajaxurl
        {
          action: 'wporg_ajax_change',                // POST data, action
          wporg_field_value: $('#wporg_field').val(), // POST data, wporg_field_value
          post_ID: jQuery('#post_ID').val()           // The ID of the post currently being edited
        }, function (data) {
          // handle response data
          if (data === 'success') {
            // perform our success logic
          } else if (data === 'failure') {
            // perform our failure logic
          } else {
            // do nothing
          }
        }
      );
    });
  });
}(jQuery, window, document));
```

We took the WordPress AJAX file URL dynamically from the `wporg_meta_box_obj` JavaScript custom object that we will create in the next step.

[info]If your meta box only requires the WordPress AJAX file URL; instead of creating a new custom JavaScript object you could use the `ajaxurl` predefined JavaScript variable.

**Available only in the WordPress Administration.** Make sure it is not empty before performing any logic.[/info]

#### Enqueue Client Side Code

Next step is to put our code into a script file and enqueue it on our edit screens.

In the example below we will add the AJAX functionality to the edit screens of the following post types: post, `wporg_cpt`.

The script file will reside at `/plugin-name/admin/meta-boxes/js/admin.js`, `plugin-name` being the main plugin folder, `/plugin-name/plugin.php` the file calling the function.

```
function wporg_meta_box_scripts() {
  // get current admin screen, or null
  $screen = get_current_screen();
  // verify admin screen object
  if (is_object($screen)) {
    // enqueue only for specific post types
    if (in_array($screen->post_type, ['post', 'wporg_cpt'])) {
      // enqueue script
      wp_enqueue_script('wporg_meta_box_script', plugin_dir_url(__FILE__) . 'admin/meta-boxes/js/admin.js', ['jquery']);
      // localize script, create a custom js object
      wp_localize_script(
        'wporg_meta_box_script',
        'wporg_meta_box_obj',
        [
          'url' => admin_url('admin-ajax.php'),
        ]
      );
    }
  }
}
add_action('admin_enqueue_scripts', 'wporg_meta_box_scripts');
```

#### Server Side Code

The last step is to write our server side code that is going to handle the request.

```
// The piece after `wp_ajax_`  matches the action argument being sent in the POST request.
add_action( 'wp_ajax_wporg_ajax_change', 'my_ajax_handler' );

/**
 * Handles my AJAX request.
 */
function my_ajax_handler() {
  // Handle the ajax request here
  if ( array_key_exists( 'wporg_field_value', $_POST ) ) {
    $post_id = (int) $_POST['post_ID'];
    if ( current_user_can( 'edit_post', $post_id ) ) {
      update_post_meta(
        $post_id,
        '_wporg_meta_key',
        $_POST['wporg_field_value']
      );
    }
  }

  wp_die(); // All ajax handlers die when finished
}
```

As a final reminder, the code illustrated on this page lacks important operations that take care of security. Be sure your production code includes such operations.

See [Handbook's AJAX Chapter](https://developer.wordpress.org/plugins/javascript/ajax/) and the [Codex](https://codex.wordpress.org/AJAX_in_Plugins) for more on AJAX.

## More Information

- [Complex Meta Boxes in WordPress](https://www.wproots.com/complex-meta-boxes-in-wordpress/)
- [How To Create Custom Post Meta Boxes In WordPress](https://www.smashingmagazine.com/2011/10/create-custom-post-meta-boxes-wordpress/)
- [WordPress Meta Boxes: a Comprehensive Developer's Guide](https://themefoundation.com/wordpress-meta-boxes-guide/)