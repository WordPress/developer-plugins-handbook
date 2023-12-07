# Working with Users

## Adding Users

To add a user you can use `wp_create_user()` or `wp_insert_user()`.

`wp_create_user()` creates a user using only the username, password and email parameters while `wp_insert_user()` accepts an array or object describing the user and its properties.

### Create User

`wp_create_user()` allows you to create a new WordPress user.

[info]It uses [`wp_slash()`](https://developer.wordpress.org/reference/functions/wp_slash/) to escape the values. The PHP compact() function to create an array with these values. The [`wp_insert_user()`](https://developer.wordpress.org/reference/functions/wp_insert_user/) to perform the insert operation.[/info]

Please refer to the Function Reference about `wp_create_user()` for full explanation about the used parameters.

#### Example Create

```
// check if the username is taken
$user_id = username_exists( $user_name );

// check that the email address does not belong to a registered user
if ( ! $user_id && email_exists( $user_email ) === false ) {
  // create a random password
  $random_password = wp_generate_password( 12, false );
  // create the user
  $user_id = wp_create_user(
    $user_name,
    $random_password,
    $user_email
  );
}
```

## Insert User

```
wp_insert_user( $userdata );
```

[info]The function calls a filter for most predefined properties.

The function performs the action `user_register` when creating a user (user ID does not exist).

The function performs the action `profile_update` when updating the user (user ID exists).[/info]

Please refer to the Function Reference about `wp_insert_user()` for full explanation about the used parameters.

### Example Insert

Below is an example showing how to insert a new user with the website profile field filled in.

```
$username  = $_POST['username'];
$password  = $_POST['password'];
$website   = $_POST['website'];
$user_data = [
  'user_login' => $username,
  'user_pass'  => $password,
  'user_url'   => $website,
];

$user_id = wp_insert_user( $user_data );

// success
if ( ! is_wp_error( $user_id ) ) {
  echo 'User created: ' . $user_id;
}
```

## Updating Users

`wp_update_user()` Updates a single user in the database. The update data is passed along in the `$userdata` array/object.

To update a single piece of user meta data, use `update_user_meta()` instead. To create a new user, use `wp_insert_user()` instead.

[info]If current user's password is being updated, then the cookies will be cleared![/info]

Please refer to the Function Reference about `wp_update_user()` for full explanation about the used parameters.

### Example Update

Below is an example showing how to update a user's website profile field.

```
$user_id = 1;
$website = 'https://wordpress.org';

$user_id = wp_update_user(
  array(
    'ID'       => $user_id,
    'user_url' => $website,
  )
);

if ( is_wp_error( $user_id ) ) {
  // error
} else {
  // success
}
```

## Deleting Users

`wp_delete_user()` deletes the user and optionally reassign associated entities to another user ID.

[info]The function performs the action `deleted_user` after the user have been deleted.[/info]

[alert]If the `$reassign` parameter is not set to a valid user ID, then all entities belonging to the deleted user will be deleted![/alert]

Please refer to the Function Reference about `wp_delete_user()` for full explanation about the used parameters.