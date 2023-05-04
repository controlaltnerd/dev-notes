# Save Related Fields in ACF

This snippet allows information to be automatically mapped from one post to another in ACF fields. The use case here is that one post is of a custom post type that is the parent of the custom post type of the other post, and that when creating the child post, selecting its parent auto-populates some number of fields with relevant information about the parent, to make it easier to retrieve that information for later use in things like Elementor post queries, where using complex queries can be cumbersome.

Note that all fields have to be created in the relevant ACF groups and added to the custom post types before adding this code to functions.php.

```
// Save X parent's name to ACF field in X posts
function acf_related_info_X_parent( $post_id ) {

	// Let's check the relationship fields for a parent and store its ID
	if ( get_field('Y') ) {
		$parent_id = get_field ('Y', $post_id, false);
	} else if ( get_field('Z') ) {
		$parent_id = get_field ('Z', $post_id, false);
	}

	if ( is_array($parent_id) ) {
		$parent_id = $parent_id[0];
	}

	// Now let's look up the name of the parent
	if ( $parent_id) {
		$parent_name = get_the_title ( $parent_id );
		$parent_url = get_the_permalink ( $parent_id );
	}

	// Then let's look up other info about the parent
	if ( $parent_id) {
		$parent_thumbnail = get_field ('thumbnail', $parent_id, false);
	}

	// Finally we'll save the parent name to a text field, URL to a URL field, and the thumbnail to an image field
	update_field ('parent_name', $parent_name, $post_id);
	update_field ('parent_url', $parent_url, $post_id);
	update_field ('parent_thumbnail', $parent_thumbnail, $post_id);
}

add_action('acf/save_post', 'acf_related_info_X_parent', 11);
```
