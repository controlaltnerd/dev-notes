# Prevent LocalStorage Data Override

Sort of like a cookie, but worse. Probably.

```
document.addEventListener('DOMContentLoaded', () => {
    // Get today's date
    const today = new Date();

    // Retrieve timestamp from storage
    if (localStorage.getItem("timestamp") !== null) {
        const timestamp = localStorage.getItem("timestamp");

        // Compare timestamp to today's date in milliseconds
        let timeSinceLastVisit =
            (today.getTime() - timestamp.getTime()) / (1000 * 3600 * 24);

        // Check if timestamp exists, indicating a recent visit, and check if it's been in the last 60 days
        if (timeSinceLastVisit <= 60) {
            return; // Nothing to do
        } else {
            storeParams(); // The timestamp is too old, so we need to store param info
            return;
        }
    }
    storeParams(); // Timestamp doesn't exist, so we need to store param info
});

function storeParams() {
    // Store URL param values in local storage

    // Isolate URL parameters from current URL
    const queryString = window.location.search;
    const params = new URLSearchParams(queryString);
    const today = new Date();

    // Set local storage variables
    window.localStorage.setItem("url_params", params);

    // Let's add a timestamp to local storage
    window.localStorage.setItem("timestamp", today);
    console.log(today);
}
```
