# Order Form

Get values from cookies and update hidden fields to be submitted with order data.

**Note: Use with PHP snippet in aff_data_functions.md.**

```
// Retrieve affiliate cookie values and update hidden fields
jQuery(document).ready(function ($) {
  // Set relevant cookie names to look for
  const paramKeys = [
    "param1",
    "param2",
    "param3",
    "param4",
    "param5"
  ];
  const params = {};

  paramKeys.forEach(function (param) {
    // Returns the cookie with the given name, or undefined if not found
    const pd = getCookie(param);
    params[param] = pd;
  });

  function getCookie(name) {
    let matches = document.cookie.match(
      new RegExp(
        "(?:^|; )" +
          name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, "\\$1") +
          "=([^;]*)"
      )
    );
    return matches ? decodeURIComponent(matches[1]) : undefined;
  }

  // Remove any undefined keys from params
  Object.keys(params).forEach(
    (key) => params[key] === undefined && delete params[key]
  );

  // Find hidden fields and check their values to find corresponding cookies
  const hiddenFields = document.querySelectorAll("input[type=hidden]");
  for (const field of hiddenFields) {
    const value = field.value;
    const name =
      value && value.indexOf("{affiliateID.") !== -1
        ? value.substring(13, value.length - 1)
        : undefined;

    const paramCookie = params[name];

    // If corresponding cookie exists, update hidden field's value
    if (paramCookie) {
      document.getElementById(field.id || field.name).value = paramCookie;
    }
  }

  // Clean up any hidden fields that didn't get their values updated, to prevent {affiliateID.*} from being added to order records
  $(".input-hidden").each(function () {
    const $this = $(this);
    const valueCheck = $this.val();
    console.log("Original value: " + valueCheck);

    let valReg = valueCheck.match(/^\{/);
    if (valReg) {
      $this.val("");
    }
    console.log("New value: " + $this.val());
  });
});
```
