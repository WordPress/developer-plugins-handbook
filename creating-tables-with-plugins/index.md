# Creating Tables with Plugins

If you are [writing a plugin](https://developer.wordpress.org/plugins/) for WordPress, you will almost certainly find that you need to store some information in the WordPress database. There are two types of information you could store:

- **Setup information** — user choices that are entered when the user first sets up your plugin, and don’t tend to grow much beyond that (for example, in a tag-related plugin, the user’s choices regarding the format of the tag cloud in the sidebar).
  Setup information will generally be stored using the [WordPress _options_ mechanism](https://developer.wordpress.org/plugins/settings/options-api/).
- **Data** — information that is added as the user continues to use your plugin, which is generally expanded information related to posts, categories, uploads, and other WordPress components (for example, in a statistics-related plugin, the various page views, referrers, and other statistics associated with each post on your site).
  Data can be stored in a separate MySQL/MariaDB table, which will have to be created. Before jumping in with a whole new table, however, consider if storing your plugin’s data in [WordPress’ Post Meta](https://developer.wordpress.org/plugins/metadata/) (a.k.a. Custom Fields) would work. Post Meta is the preferred method; use it when possible/practical.

This article describes how to have your plugin automatically create a MySQL/MariaDB table to store its data. Note that as an alternative to following the steps here, you could have the plugin user run an install script when they install your plugin. Another approach would be to have the user execute an SQL query on their own, using something like [phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/). But neither of those options is very satisfactory, since a user could easily forget to run the install script or mess up the query (and they might not have phpMyAdmin available).

So, it is recommended that you follow the steps below to have your plugin automatically create its database tables:

1. Write a PHP function that creates the table.
2. Ensure that WordPress calls the function when the plugin is activated.
3. Create an upgrade function, if a new version of your plugin needs to have a different table structure.

## Create Database Tables

The first step in making your plugin create database tables automatically is to create a PHP function within your plugin that adds a table or tables to the WordPress MySQL/MariaDB database. For purposes of this article, we’ll assume you want to call this function jal_install.

### Database Table Prefix

In the wp-config.php file, a WordPress site owner can define a database table prefix. By default, the prefix is “wp_”, but you’ll need to check on the actual value and use it to define your database table name. This value is found in the $[wpdb](https://developer.wordpress.org/reference/classes/wpdb/)->prefix variable. (If you’re developing for a version of WordPress older than 2.0, you’ll need to use the $table_prefix global variable, which is deprecated in version 2.1).

So, if you want to create a table called (prefix)liveshoutbox, the first few lines of your table-creation function will be:

```
function jal_install () {
  global $wpdb;

  $table_name = $wpdb->prefix . "liveshoutbox";
}
```

### Creating or Updating the Table

The next step is to actually create the database table. Rather than executing an SQL query directly, we’ll use the dbDelta function in wp-admin/includes/upgrade.php (we’ll have to load this file, as it is not loaded by default). The dbDelta function examines the current table structure, compares it to the desired table structure, and either adds or modifies the table as necessary, so it can be very handy for updates (see wp-admin/upgrade-schema.php for more examples of how to use dbDelta). Note that the dbDelta function is rather picky, however. For instance:

- You must put each field on its own line in your SQL statement.
- You must have two spaces between the words PRIMARY KEY and the definition of your primary key.
- You must use the key word KEY rather than its synonym INDEX and you must include at least one KEY.
- KEY must be followed by a SINGLE SPACE then the key name then a space then open parenthesis with the field name then a closed parenthesis.
- You must not use any apostrophes or backticks around field names.
- Field types must be all lowercase.
- SQL keywords, like CREATE TABLE and UPDATE, must be uppercase.
- You must specify the length of all fields that accept a length parameter. int(11), for example.

With those caveats, here are the next lines in our function, which will actually create or update the table. You’ll need to substitute your own table structure in the $sql variable:

```
global $wpdb;

$charset_collate = $wpdb->get_charset_collate();

$sql = "CREATE TABLE $table_name (
  id mediumint(9) NOT NULL AUTO_INCREMENT,
  time datetime DEFAULT '0000-00-00 00:00:00' NOT NULL,
  name tinytext NOT NULL,
  text text NOT NULL,
  url varchar(55) DEFAULT '' NOT NULL,
  PRIMARY KEY  (id)
) $charset_collate;";

require_once( ABSPATH . 'wp-admin/includes/upgrade.php' );
dbDelta( $sql );
```

**Note:** Above we set the default character set and collation for the table. If we don’t do this, some characters could end up being converted to just ?’s when saved in our table. In this example we use $[wpdb::get_charset_collate()](https://developer.wordpress.org/reference/classes/wpdb/get_charset_collate/) to get the character set and collation. That function was introduced in WordPress 3.5, and if you need to support versions before that you will need create the charset/collate string yourself (you could copy the source of that function).

### Adding Initial Data

Finally, you may want to add some data to the table you just created. Here is an example of how to do that:

```
$welcome_name = 'Mr. WordPress';
$welcome_text = 'Congratulations, you just completed the installation!';

$table_name = $wpdb->prefix . 'liveshoutbox';

$wpdb->insert(
	$table_name,
	array(
		'time' => current_time( 'mysql' ),
		'name' => $welcome_name,
		'text' => $welcome_text,
	)
);
```

**NOTE:** **For more on using WPDB, see [wpdb](https://developer.wordpress.org/reference/classes/wpdb/) class.** In this case, we’re using $[wpdb](https://developer.wordpress.org/reference/classes/wpdb/)->insert, so our data will automatically be escaped. If you need to use another method like $[wpdb](https://developer.wordpress.org/reference/classes/wpdb/)->query instead, it’s a good idea to run the variables through the $[wpdb](https://developer.wordpress.org/reference/classes/wpdb/)->prepare function before passing the query to the database to prevent security problems, even though we defined $welcome_name and $welcome_text in this function and know that there are no SQL special characters in them.

### A Version Option

Another excellent idea is to add an option to record a version number for your database table structure, so you can use that information later if you need to update the table:

```
add_option( "jal_db_version", "1.0" );
```

### The Whole Function

This function is done. Let’s see it all in one piece. Note that the version number is now stored in a global variable.

```
<?php

global $jal_db_version;
$jal_db_version = '1.0';

function jal_install() {
	global $wpdb;
	global $jal_db_version;

	$table_name = $wpdb->prefix . 'liveshoutbox';

	$charset_collate = $wpdb->get_charset_collate();

	$sql = "CREATE TABLE $table_name (
		id mediumint(9) NOT NULL AUTO_INCREMENT,
		time datetime DEFAULT '0000-00-00 00:00:00' NOT NULL,
		name tinytext NOT NULL,
		text text NOT NULL,
		url varchar(55) DEFAULT '' NOT NULL,
		PRIMARY KEY  (id)
	) $charset_collate;";

	require_once ABSPATH . 'wp-admin/includes/upgrade.php';
	dbDelta( $sql );

	add_option( 'jal_db_version', $jal_db_version );
}

function jal_install_data() {
	global $wpdb;

	$welcome_name = 'Mr. WordPress';
	$welcome_text = 'Congratulations, you just completed the installation!';

	$table_name = $wpdb->prefix . 'liveshoutbox';

	$wpdb->insert(
		$table_name,
		array(
			'time' => current_time( 'mysql' ),
			'name' => $welcome_name,
			'text' => $welcome_text,
		)
	);
}
```

## Calling the functions

Now that we have the initialization function defined, we want to make sure that WordPress calls this function when the plugin is activated by a WordPress administrator. To do that, we will use the activate_ action hook. If your plugin file is wp-content/plugins/plugindir/pluginfile.php, you’ll add the following line to the main body of your plugin:

```
register_activation_hook( __FILE__, 'jal_install' );
register_activation_hook( __FILE__, 'jal_install_data' );
```

See [Function_Reference/register_activation_hook](https://developer.wordpress.org/reference/functions/register_activation_hook/) for more details.

## Adding an Upgrade Function

Over the lifetime of your plugin, you may find that you need to change the plugin’s database structure in an upgraded version. To do that, you will need to create update code within your plugin that will detect that a new version has been installed, and upgrade the database structure. The easiest thing to do is to add the code to the jal_install function we just created.

So, let’s assume that the function above was used to create database version 1.0 of your plugin, and you are now upgrading to version 1.1 so that the URL field can be wider (100 characters instead of 55). You will need to add the following lines to the end of your jal_install function, to check the version and upgrade if necessary:

```
<?php

global $wpdb;
$installed_ver = get_option( "jal_db_version" );

if ( $installed_ver != $jal_db_version ) {

	$table_name = $wpdb->prefix . 'liveshoutbox';

	$sql = "CREATE TABLE $table_name (
		id mediumint(9) NOT NULL AUTO_INCREMENT,
		time datetime DEFAULT '0000-00-00 00:00:00' NOT NULL,
		name tinytext NOT NULL,
		text text NOT NULL,
		url varchar(100) DEFAULT '' NOT NULL,
		PRIMARY KEY  (id)
	);";

	require_once( ABSPATH . 'wp-admin/includes/upgrade.php' );
	dbDelta( $sql );

	update_option( "jal_db_version", $jal_db_version );
}
```

You’ll also need to change the global $jal_db_version variable at the top of the file, and of course you’ll want to change the initialization section created above to use the new table structure.

Since 3.1 the activation function registered with [register_activation_hook()](https://developer.wordpress.org/reference/functions/register_activation_hook/) is not called when a plugin is updated. So to run the above code after the plugin is upgraded, you need to check the plugin db version on another hook, and call the function manually if the the database version is old. Like this:

```
function myplugin_update_db_check() {
    global $jal_db_version;
    if ( get_site_option( 'jal_db_version' ) != $jal_db_version ) {
        jal_install();
    }
}
add_action( 'plugins_loaded', 'myplugin_update_db_check' );
```

## Resources

For further reading on plugin development, check out [Plugin Handbook](https://developer.wordpress.org/plugins/), a comprehensive list of plugin resources. You may also find this post from the [wp-hackers mailing list](https://codex.wordpress.org/Mailing_Lists#Hackers) to be helpful: [WordPress Hackers Mailing List: Answer to Plugin Requires Additional Tables](http://lists.automattic.com/pipermail/wp-hackers/2005-May/000940.html). Also see: [Post meta vs separate database tables](http://wordpress.stackexchange.com/questions/4852/post-meta-vs-seperate-database-tables).
