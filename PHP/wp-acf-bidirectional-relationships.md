# Bi-Directional Relationship Fields in ACF

Set up groups and fields in ACF, then in the code below replace the X and Y in the comments with the appopriate group names, and `field_ABC123` and `field_XYZ789` with the appropriate field keys. Add code to functions.php.

```
// Add ACF bi-directional relationship fields

// Add the filter for the Y relationship field in X ACF group
add_filter('acf/update_value/key=field_ABC123', 'acf_reciprocal_relationship', 10, 3);
// Add the filter for the X relationship field in the Y ACF group
add_filter('acf/update_value/key=field_XYZ789', 'acf_reciprocal_relationship', 10, 3);

function acf_reciprocal_relationship($value, $post_id, $field) {

	// For the last two values (to be set to $key_a and $key_b), set the two fields that you want to create a two way relationship for.
	// These values can be the same field key if you are using a single relationship field on a single post type.

	// Field key of one side of the relationship
	$key_a = 'field_ABC123'; // Y

    // Field key of the other side of the relationship
	// As noted above, this can be the same as $key_a
	$key_b = 'field_XYZ789'; // X

	// Figure out which side we're working on and set up variables.
	// If the keys are the same above then this won't matter.
	// $key_a represents the field for the current posts
	// and $key_b represents the field on related posts
	if ($key_a != $field['key']) {

        // This is side b, swap the value
		$temp = $key_a;
		$key_a = $key_b;
		$key_b = $temp;
	}

	// Get both fields using an ACF function that can get
	// field objects based on field keys. Can be the same field
	// for both sides of the relationship.
	$field_a = acf_get_field($key_a);
	$field_b = acf_get_field($key_b);

	// Set the field names to check for each post
	$name_a = $field_a['name'];
	$name_b = $field_b['name'];

	// Get the old value from the current post and compare it
	// to the new value to see if anything needs to be updated.
	// Use get_post_meta() to a avoid conflicts.
	$old_values = get_post_meta($post_id, $name_a, true);

	// Make sure that the value is an array
	if (!is_array($old_values)) {
		if (empty($old_values)) {
			$old_values = array();
		} else {
			$old_values = array($old_values);
		}
	}

	// Set new values to $value
	// We don't want to mess with $value
	$new_values = $value;

	// Make sure that the value is an array
	if (!is_array($new_values)) {
		if (empty($new_values)) {
			$new_values = array();
		} else {
			$new_values = array($new_values);
		}
	}

	// Get differences:
	// array_diff returns an array of values from the first array that are
	// not in the second array. This gives us lists that need to be added
	// or removed depending on which order we give the arrays in.

	// This line is commented out, and should be used when setting up this
    // filter on a new site. Getting values and updating values on every
    // relationship will cause a performance issue, so you should only use
	// the second line "$add = $new_values" when adding this filter to an
    // existing site, and then you should switch to the first line as soon as
    // you get everything updated. In either case, if you have too many
    // existing relationships checking end updated every one of them will
    // more than likely cause your updates to time out.
	//$add = array_diff($new_values, $old_values);
	$add = $new_values;
	$delete = array_diff($old_values, $new_values);

	// Reorder the arrays to prevent possible invalid index errors
	$add = array_values($add);
	$delete = array_values($delete);

	if (!count($add) && !count($delete)) {
		// There are no changes, so there's nothing to do
		return $value;
	}

	// Do deletes first:
	// Loop through all of the posts that need to have the reciprocal relationship removed
	for ($i=0; $i<count($delete); $i++) {
		$related_values = get_post_meta($delete[$i], $name_b, true);
		if (!is_array($related_values)) {
			if (empty($related_values)) {
				$related_values = array();
			} else {
				$related_values = array($related_values);
			}
		}

		// We use array_diff again
		// This will remove the value without needing to loop through the array and find it
		$related_values = array_diff($related_values, array($post_id));

		// Insert the new value
		update_post_meta($delete[$i], $name_b, $related_values);

		// Insert the acf key reference, just in case
		update_post_meta($delete[$i], '_'.$name_b, $key_b);
	}

	// Do additions, to add $post_id
	for ($i=0; $i<count($add); $i++) {
		$related_values = get_post_meta($add[$i], $name_b, true);
		if (!is_array($related_values)) {
			if (empty($related_values)) {
				$related_values = array();
			} else {
				$related_values = array($related_values);
			}
		}
		if (!in_array($post_id, $related_values)) {

			// Add new relationship if it does not exist
			$related_values[] = $post_id;
		}

		// Update value
		update_post_meta($add[$i], $name_b, $related_values);

		// Insert the acf key reference, just in case
		update_post_meta($add[$i], '_'.$name_b, $key_b);
	}
	return $value;
}
```
