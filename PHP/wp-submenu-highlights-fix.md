# Fix for Submenu Highlights in WordPress

This seems to be primarily an issue with custom taxonomies. Fixes an issue where custom post types set as children of other post types don't trigger highlighting on their associated submenu items in WP-Admin.

## PHP

```
/ Add fix for CPT submenu highlights
add_filter( 'submenu_file', 'highlight_cpt_submenu_file' );

function highlight_cpt_submenu_file( $submenu_file ){

	// Get current screen
	global $current_screen, $self;

	if ( in_array( $current_screen->base, array( 'post', 'edit' ) ) && 'cpt_a' == $current_screen->post_type ) {
		$submenu_file = 'edit.php?post_type=cpt_a';
	}

	if ( in_array( $current_screen->base, array( 'post', 'edit' ) ) && 'cpt_b' == $current_screen->post_type ) {
		$submenu_file = 'edit.php?post_type=cpt_b';
	}

	if ( in_array( $current_screen->base, array( 'post', 'edit' ) ) && 'cpt_c' == $current_screen->post_type ) {
		$submenu_file = 'edit.php?post_type=cpt_c';
	}

	if ( in_array( $current_screen->base, array( 'post', 'edit' ) ) && 'cpt_d' == $current_screen->post_type ) {
		$submenu_file = 'edit.php?post_type=cpt_d';
	}

	return $submenu_file;
}
```
