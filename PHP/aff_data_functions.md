# Cross-Subdomain URL Params

Add custom fields to WooCommerce checkout and save their values with the order metadata.

**Note: Use with JS snippet in order-form.md.**

```
<?php
/**
 * Add affiliate fields to WooCommerce checkout to capture URL affiliate params, and display them on the order edit page
 */

// Let's make some new fields, add them to checkout, and save

add_action("woocommerce_after_order_notes", "x_custom_checkout_fields");

function x_custom_checkout_fields($checkout)
{
    // Output the hidden field
    echo '<div id="user_link_hidden_checkout_fields">
            <input type="hidden" class="input-hidden" name="param1" id="param1" value="{affiliateID.param1}"><br>
            <input type="hidden" class="input-hidden" name="param2" id="param2" value="{affiliateID.param2}"><br>
            <input type="hidden" class="input-hidden" name="param3" id="param3" value="{affiliateID.param3}"><br>
            <input type="hidden" class="input-hidden" name="param4" id="param4" value="{affiliateID.param4}"><br>
            <input type="hidden" class="input-hidden" name="param5" id="param5" value="{affiliateID.param5}"><br>
    </div>';
}

// Saving the field values in the order metadata
add_action(
    "woocommerce_checkout_update_order_meta",
    "x_save_custom_checkout_fields"
);

function x_save_custom_checkout_fields($order_id)
{
    update_post_meta(
        $order_id,
        "_param1",
        sanitize_text_field($_POST["param1"])
    );
    update_post_meta(
        $order_id,
        "_param2",
        sanitize_text_field($_POST["param2"])
    );
    update_post_meta(
        $order_id,
        "_param3",
        sanitize_text_field($_POST["param3"])
    );
    update_post_meta(
        $order_id,
        "_param4",
        sanitize_text_field($_POST["param4"])
    );
    update_post_meta(
        $order_id,
        "_param5",
        sanitize_text_field($_POST["param5"])
    );
}

// Now let's add the fields to the order edit page
add_action(
    "woocommerce_admin_order_data_after_billing_address",
    "x_display_verification_id_in_admin_order_meta",
    10,
    1
);

function x_display_verification_id_in_admin_order_meta($order)
{
    $order_id = method_exists($order, "get_id") ? $order->get_id() : $order->id;
    echo "<p><strong>" .
        __("param1", "woocommerce") .
        ":</strong> " .
        get_post_meta($order_id, "_param1", true) .
        "</p>";
    echo "<p><strong>" .
        __("param2", "woocommerce") .
        ":</strong> " .
        get_post_meta($order_id, "_param2", true) .
        "</p>";
    echo "<p><strong>" .
        __("param3", "woocommerce") .
        ":</strong> " .
        get_post_meta($order_id, "_param3", true) .
        "</p>";
    echo "<p><strong>" .
        __("param4", "woocommerce") .
        ":</strong> " .
        get_post_meta($order_id, "_param4", true) .
        "</p>";
    echo "<p><strong>" .
        __("param5", "woocommerce") .
        ":</strong> " .
        get_post_meta($order_id, "_param5", true) .
        "</p>";
}
```
