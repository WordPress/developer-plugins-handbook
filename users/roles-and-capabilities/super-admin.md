### Super Admin

Super Admin is not a typical role like subscriber, to which capabilities can be granted or revoked; rather, super admin is a status that can be granted to users when WordPress is in multisite mode. These users are then granted all capabilities on the network, except where explicitly denied.

A capability such as **edit_others_posts** can be granted to the subscriber role, by the way of:

```php
get_role( 'subscriber' )->add_cap( 'edit_others_posts' );
```

By default, super admins would have the **edit_others_posts** capability and any other capabilities. WordPress uses the **current_user_can()** function to check if the current user has the passed capability, and this function is what grants all capabilities to super admins. **current_user_can()** is a wrapper function for **user_can()**, which calls the **has_cap** method on the **WP_User** object. In the **has_cap()** method, there exists the code that grants super admin all capabilities:

```php
//Multisite super admin has all caps by definition, Unless specifically denied.
if ( is_multisite() && is_super_admin( $this->ID ) ) {
  if ( in_array( 'do_not_allow', $caps, true ) ) {
   return false;
  }
  return true;
}
```

The code above checks if WordPress is in multisite mode and if the user in question is a super admin. If both conditions are met, **has_cap()** returns true if the user is not explicitly denied the capability, which would be the case if **do_not_allow** is a required capability. For users who are not super admins, has_cap continues executing and checks if the user has been explicitly granted the capability, ending with:

```php
// Must have ALL requested caps.
foreach ( (array) $caps as $cap ) {
	if ( empty( $capabilities[ $cap ] ) ) {
		return false;
	}
}

return true;
```
As a result, there is no need to explicity grant capablities to super admins, because super admins are already granted all capabilities by WordPress.
