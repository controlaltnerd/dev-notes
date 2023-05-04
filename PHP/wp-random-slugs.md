# Generate Random Slugs for Custom Post Types

Replace `CUSTOM_POST_TYPE_NAME` below and add this snippet to functions.php.

```
/**
 *  Generate random slugs for custom post types
 */

 // Filter post slug
 // This needs saving as post meta to use that value rather than regenerate a new one

add_filter( 'wp_unique_post_slug', function( $slug, $post_ID, $post_status, $post_type, $post_parent, $original_slug ) {

    if ( ! in_array( $post_type, array( 'CUSTOM_POST_TYPE_NAME' ) ) ) {
        return $slug;
    }

    if ( ! $slug = esc_attr( get_post_meta( $post_ID, '_slug', true ) ) ) {

	// The chance of this returning the same URL twice is 1 in 20,000,000,000. Unlikely, but possible..
	$slug = esc_attr( wordwrap( strtolower( wp_generate_password( 21, false, false ) ), 7 , '-' , true ) );

        update_post_meta( $post_ID, '_slug', $slug );

    }

    return $slug;

}, 10, 6 );
```
