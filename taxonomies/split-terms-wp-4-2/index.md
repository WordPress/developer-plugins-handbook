# Term Splitting (WordPress 4.2)

This information is here for historical purposes. If you're not interested in how terms worked prior to 2015, you can skip this section.

## Prior to WordPress 4.2

Terms in different taxonomies with the same slug shared a single term ID. For instance, a tag and a category with the slug "news" had the same term ID.

## WordPress 4.2+

Beginning with 4.2, when one of these shared terms is updated, it will be split: the updated term will be assigned a new term ID.

## What does it mean for you?

In the vast majority of situations, this update was seamless and uneventful. However, some plugins and themes who store term IDs in options, post meta, user meta, or elsewhere might have been affected.

## Handling the Split

WordPress 4.2 includes two different tools to help authors of plugins and themes with the transition.

### The `split_shared_term` hook

When a shared term is assigned a new term ID, a new `split_shared_term` action is fired.

Here are a few examples of how plugin and theme authors can leverage this hook to ensure that stored term IDs are updated.

#### Term ID stored in an option

Let's say your plugin stores an option called `featured_tags` that contains an array of term IDs (`[4, 6, 10]`) that serve as the query parameter for your homepage featured posts section.

In this example, you'll hook to `split_shared_term` action, check whether the updated term ID is in the array, and update if necessary.

```
/**
 * Update featured_tags option when a shared term gets split.
 *
 * @param int    $term_id          ID of the formerly shared term.
 * @param int    $new_term_id      ID of the new term created for the $term_taxonomy_id.
 * @param int    $term_taxonomy_id ID for the term_taxonomy row affected by the split.
 * @param string $taxonomy         Taxonomy for the split term.
 */
function wporg_featured_tags_split( int $term_id, int $new_term_id, int $term_taxonomy_id, string $taxonomy ): void {
  // we only care about tags, so we'll first verify that the taxonomy is post_tag.
  if ( 'post_tag' === $taxonomy ) {

    // get the currently featured tags.
    $featured_tags = get_option( 'featured_tags' );

    // if the updated term is in the array, note the array key.
    $found_term = array_search( $term_id, $featured_tags, true );
    if ( false !== $found_term ) {

      // the updated term is a featured tag! replace it in the array, save the new array.
      $featured_tags[ $found_term ] = $new_term_id;
      update_option( 'featured_tags', $featured_tags );
    }
  }
}
add_action( 'split_shared_term', 'wporg_featured_tags_split', 10, 4 );
```

#### Term ID stored in post meta

Let's say your plugin stores a term ID in post meta for pages so that you can show related posts for a certain page.

In this case, you need to use the `get_posts()` function to get the pages with your `meta_key` and update the `meta_value` matching the split term ID.

```
/**
 * Update related posts term ID for pages
 *
 * @param int    $term_id          ID of the formerly shared term.
 * @param int    $new_term_id      ID of the new term created for the $term_taxonomy_id.
 * @param int    $term_taxonomy_id ID for the term_taxonomy row affected by the split.
 * @param string $taxonomy         Taxonomy for the split term.
 */
function wporg_page_related_posts_split( int $term_id, int $new_term_id, int $term_taxonomy_id, string $taxonomy ): void {
  // find all the pages where meta_value matches the old term ID.
  $page_ids = get_posts(
    array(
      'post_type'  => 'page',
      'fields'     => 'ids',
      'meta_key'   => 'meta_key',
      'meta_value' => $term_id,
    )
  );

  // if such pages exist, update the term ID for each page.
  if ( $page_ids ) {
    foreach ( $page_ids as $id ) {
      update_post_meta( $id, 'meta_key', $new_term_id, $term_id );
    }
  }
}
add_action( 'split_shared_term', 'wporg_page_related_posts_split', 10, 4 );
```

### The `wp_get_split_term` function

[info]Using the `split_shared_term` hook is the preferred method for processing Term ID changes.

However, there may be cases where Terms are split without your plugin having a chance to hook to the `split_shared_term` action.[/info]

WordPress 4.2 stores information about taxonomy terms that have been split, and provides the `wp_get_split_term()` utility function to help developers retrieve this information.

Consider the case above, where your plugin stores term IDs in an option named `featured_tags`. You may want to build a function that validates these tag IDs (perhaps to be run on plugin update), to be sure that none of the featured tags has been split:

```
/**
 * Retrieve information about split terms and udpates the featured_tags option with the new term IDs.
 *
 * @return void
 */
function wporg_featured_tags_check_split() {
  $featured_tag_ids = get_option( 'featured_tags', array() );

  // check to see whether any IDs correspond to post_tag terms that have been split.
  foreach ( $featured_tag_ids as $index => $featured_tag_id ) {
    $new_term_id = wp_get_split_term( $featured_tag_id, 'post_tag' );

    if ( $new_term_id ) {
      $featured_tag_ids[ $index ] = $new_term_id;
    }
  }

  // save
  update_option( 'featured_tags', $featured_tag_ids );
}
```

Note that `wp_get_split_term()` takes two parameters, `$old_term_id` and `$taxonomy` and returns an integer.

If you need to retrieve a list of all split terms associated with an old Term ID, regardless of taxonomy, use `wp_get_split_terms()`.