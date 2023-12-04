# Sub-Menus

## Add a Sub-Menu

To add a new Sub-menu to WordPress Administration, use the `add_submenu_page()` function.

```
add_submenu_page(
  string $parent_slug,
  string $page_title,
  string $menu_title,
  string $capability,
  string $menu_slug,
  callable $function = ''
);
```
### Example

Lets say we want to add a Sub-menu "WPOrg Options" to the "Tools" Top-level menu.

**The first step** will be creating a function which will output the HTML. In this function we will perform the necessary security checks and render the options we’ve registered using the [Settings API](https://developer.wordpress.org/plugins/settings/).

[info]We recommend wrapping your HTML using a `<div>` with a class of `wrap`.[/info]

```
function wporg_options_page_html() {
  // check user capabilities
  if ( ! current_user_can( 'manage_options' ) ) {
    return;
  }
  ?>
  <div class="wrap">
    <h1><?php echo esc_html( get_admin_page_title() ); ?></h1>
    <form action="options.php" method="post">
      <?php
      // output security fields for the registered setting "wporg_options"
      settings_fields( 'wporg_options' );
      // output setting sections and their fields
      // (sections are registered for "wporg", each field is registered to a specific section)
      do_settings_sections( 'wporg' );
      // output save settings button
      submit_button( __( 'Save Settings', 'textdomain' ) );
      ?>
    </form>
  </div>
  <?php
}
```

**The second step** will be registering our WPOrg Options Sub-menu. The registration needs to occur during the `admin_menu` action hook.

```
function wporg_options_page() {
  add_submenu_page(
    'tools.php',
    'WPOrg Options',
    'WPOrg Options',
    'manage_options',
    'wporg',
    'wporg_options_page_html'
  );
}
add_action( 'admin_menu', 'wporg_options_page' );
```

For a list of parameters and what each do please see the [`add_submenu_page()`](https://developer.wordpress.org/reference/functions/add_submenu_page/) in the reference.

## Predefined Sub-Menus

Wouldn’t it be nice if we had helper functions that define the `$parent_slug` for WordPress built-in Top-level menus and save us from manually searching it through the source code?

Below is a list of parent slugs and their helper functions:

- [`add_dashboard_page()`](https://developer.wordpress.org/reference/functions/add_dashboard_page/) – `index.php`
- [`add_posts_page()`](https://developer.wordpress.org/reference/functions/add_posts_page/) – `edit.php`
- [`add_media_page()`](https://developer.wordpress.org/reference/functions/add_media_page/) – `upload.php`
- [`add_pages_page()`](https://developer.wordpress.org/reference/functions/add_pages_page/) – `edit.php?post_type=page`
- [`add_comments_page()`](https://developer.wordpress.org/reference/functions/add_comments_page/) – `edit-comments.php`
- [`add_theme_page()`](https://developer.wordpress.org/reference/functions/add_theme_page/) – `themes.php`
- [`add_plugins_page()`](https://developer.wordpress.org/reference/functions/add_plugins_page/) – `plugins.php`
- [`add_users_page()`](hhttps://developer.wordpress.org/reference/functions/add_users_page/) – `users.php`
- [`add_management_page()`](https://developer.wordpress.org/reference/functions/add_management_page/) – `tools.php`
- [`add_options_page()`](https://developer.wordpress.org/reference/functions/add_options_page/) – `options-general.php`
- [`add_options_page()`](https://developer.wordpress.org/reference/functions/add_options_page/) – `settings.php`
- [`add_links_page()`](https://developer.wordpress.org/reference/functions/add_links_page/) – `link-manager.php` – requires a plugin since WP 3.5+
- Custom Post Type – `edit.php?post_type=wporg_post_type`
- Network Admin – `settings.php`

## Remove a Sub-Menu

The process of removing Sub-menus is exactly the same as [removing Top-level menus](https://developer.wordpress.org/plugins/administration-menus/top-level-menus/#remove-a-top-level-menu).

## Submitting forms

The process of handling form submissions within Sub-menus is exactly the same as [Submitting forms within Top-Level Menus](https://developer.wordpress.org/plugins/administration-menus/top-level-menus/#submitting-forms).

`add_submenu_page()` along with all functions for pre-defined sub-menus (`add_dashboard_page`, `add_posts_page`, etc.) will return a `$hookname`, which you can use as the first parameter of `add_action` in order to handle the submission of forms within custom pages:

```
function wporg_options_page() {
  $hookname = add_submenu_page(
    'tools.php',
    'WPOrg Options',
    'WPOrg Options',
    'manage_options',
    'wporg',
    'wporg_options_page_html'
    );

  add_action( 'load-' . $hookname, 'wporg_options_page_html_submit' );
}

add_action( 'admin_menu', 'wporg_options_page' );
```

As always, do not forget to check whether the form is being submitted, do CSRF verification, [validation](https://developer.wordpress.org/apis/security/data-validation/), and [sanitization](https://developer.wordpress.org/apis/security/sanitizing/).