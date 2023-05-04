# Custom Queries for ACF

Just replace X and Y in the code below with the relevant custom post type information, and put this snippet in functions.php.

```
// Custom queries for using within Elementor to return relationship field contents in a single post

// Must set the query in Elementor Posts widget to Manual Selection, or else all posts of a custom type
// will display when there are none connected to the current post.

// Query for X in Y single posts
add_action( 'elementor/query/X_in_Y', function( $query ) {

	$cast_id = get_the_ID();

	$show_ids = get_post_meta( $X_id, 'X', true );

	if ( $show_ids ) {
		$query->set( 'post__in', $Y_ids );
	}
});
```
