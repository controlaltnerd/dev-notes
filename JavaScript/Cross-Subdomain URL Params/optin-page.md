# Optin Page

Retrieve data from cookies, or retrieve from URL parameters if cookies don't exist and create them.

**Note: this snippet was created to work with the snippets from order-form.md and aff_data_functions.md**

This code can be used for any data passed through URL params, not just affiliate data.

```
// Check for affiliate cookies, and set them when they don't exist

// Let's set up our variables - grab the query string from current URL and set an array of parameters
const queryString = window.location.search;
const params = new URLSearchParams(queryString);
console.log("params found");

// Evaluate each parameter/value pair for cookie
for (const [key, value] of params) {
  // Set an expiration of 60 days
  setAffCookie(key, value, 60);
}

function setAffCookie(cName, cValue, expDays) {
  let date = new Date();
  date.setTime(date.getTime() + expDays * 24 * 60 * 60 * 1000);
  const expires = "expires=" + date.toUTCString();

  // Check to see if the cookie already exists
  const cExists = getCookie(cName);
  if (!cExists) {
    // It doesn't exist, so let's create it
    document.cookie =
      cName +
      "=" +
      cValue +
      "; " +
      expires +
      "; domain=EXAMPLE.COM;path=/";
    console.log("New cookie: " + cName);
  } else {
    // It does exist, so log it and leave
    console.log("Cookie exists: " + cName);
  }
}

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
```
