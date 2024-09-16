# Working with User Metadata 

## Introduction

WordPress' `users` table was designed to contain only the essential information about the user.

[info]As of WP 4.7 the table contains: `ID`, `user_login`, `user_pass`, `user_nicename`, `user_email`, `user_url`, `user_registered`, `user_activation_key`, `user_status` and `display_name`.[/info]

Because of this, to store additional data, the `usermeta` table was introduced, which can store any arbitrary amount of data about a user.

Both tables are tied together using one-to-many relationship based on the `ID` in the `users` table.

## Manipulating User Metadata

There are two main ways for manipulating User Metadata.

1. A form field in the user's profile screen.
2. Programmatically, via a function call.

### via a Form Field

The form field option is suitable for cases where the user will have access to the WordPress admin area, in which he will be able to view and edit profiles.

Before we dive into an example, it's important to understand the hooks involved in the process and why they are there.

#### `show_user_profile` hook

This action hook is fired whenever a user edits **it's own** user profile.

**Remember,** a user that doesn't have the capability of editing his own profile won't fire this hook.

#### `edit_user_profile` hook

This action hook is fired whenever a user edits a user profile of **somebody else**.

**Remember,** a user that doesn't have the capability for editing 3rd party profiles won't fire this hook.

#### Example Form Field

In the example below we will be adding a birthday field to the all profile screens. Saving it to the database on profile updates.

```
/**
 * The field on the editing screens.
 *
 * @param $user WP_User user object
 */
function wporg_usermeta_form_field_birthday( $user ) {
  ?>
  <h3>It's Your Birthday</h3>
  <table class="form-table">
    <tr>
      <th>
        <label for="birthday">Birthday</label>
      </th>
      <td>
        <input type="date"
          class="regular-text ltr"
          id="birthday"
          name="birthday"
          value="<?= esc_attr( get_user_meta( $user->ID, 'birthday', true ) ) ?>"
          title="Please use YYYY-MM-DD as the date format."
          pattern="(19[0-9][0-9]|20[0-9][0-9])-(1[0-2]|0[1-9])-(3[01]|[21][0-9]|0[1-9])"
          required>
        <p class="description">
          Please enter your birthday date.
        </p>
      </td>
    </tr>
  </table>
  <?php
}

/**
 * The save action.
 *
 * @param $user_id int the ID of the current user.
 *
 * @return bool Meta ID if the key didn't exist, true on successful update, false on failure.
 */
function wporg_usermeta_form_field_birthday_update( $user_id ) {
  // check that the current user have the capability to edit the $user_id
  if ( ! current_user_can( 'edit_user', $user_id ) ) {
    return false;
  }

  // create/update user meta for the $user_id
  return update_user_meta(
    $user_id,
    'birthday',
    $_POST['birthday']
  );
}

// Add the field to user's own profile editing screen.
add_action(
  'show_user_profile',
  'wporg_usermeta_form_field_birthday'
);

// Add the field to user profile editing screen.
add_action(
  'edit_user_profile',
  'wporg_usermeta_form_field_birthday'
);

// Add the save action to user's own profile editing screen update.
add_action(
  'personal_options_update',
  'wporg_usermeta_form_field_birthday_update'
);

// Add the save action to user profile editing screen update.
add_action(
  'edit_user_profile_update',
  'wporg_usermeta_form_field_birthday_update'
);
```

### Programmatically

This option is suitable for cases where you're building a custom user area and/or plan to disable access to the WordPress admin area.

The functions available for manipulating User Metadata are: [`add_user_meta()`](https://developer.wordpress.org/reference/functions/add_user_meta/), [`update_user_meta()`](https://developer.wordpress.org/reference/functions/update_user_meta/), [`delete_user_meta()`](https://developer.wordpress.org/reference/functions/delete_user_meta/) and [`get_user_meta()`](https://developer.wordpress.org/reference/functions/get_user_meta/).

#### Add

```
add_user_meta(
  int $user_id,
  string $meta_key,
  mixed $meta_value,
  bool $unique = false
);
```

Please refer to the Function Reference about [`add_user_meta()`](https://developer.wordpress.org/reference/functions/add_user_meta/) for full explanation about the used parameters.

#### Update

```
update_user_meta(
  int $user_id,
  string $meta_key,
  mixed $meta_value,
  mixed $prev_value = ''
);
```

Please refer to the Function Reference about [`update_user_meta()`](https://developer.wordpress.org/reference/functions/update_user_meta/) for full explanation about the used parameters.

#### Delete

```
delete_user_meta(
  int $user_id,
  string $meta_key,
  mixed $meta_value = ''
);
```

Please refer to the Function Reference about [`delete_user_meta()`](https://developer.wordpress.org/reference/functions/delete_user_meta/) for full explanation about the used parameters.

#### Get

```
get_user_meta(
  int $user_id,
  string $key = '',
  bool $single = false
);
```

Please refer to the Function Reference about [`get_user_meta()`](https://developer.wordpress.org/reference/functions/get_user_meta/) for full explanation about the used parameters.

Please note, if you pass only the `$user_id`, the function will retrieve all Metadata as an associative array.

You can render User Metadata anywhere in your plugin or theme.