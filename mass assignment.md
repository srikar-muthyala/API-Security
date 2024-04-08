Ability to add additional parameters to requests than the user can usually do.

The user registration request may contain key-values for username, email address, and password. An attacker could intercept this request and add parameters like **"isadmin": "true"**. If the data object has a corresponding value and the API provider does not sanitize the attacker's input then there is a chance that the attacker could register their own admin account.

1. Taking an example of a login form, initiate a signup request.
2. Look at the request body parameters and try adding additional parameters and observer if the application responds differently to it.
3. Mine for parameters in request with **Param Miner** extension in Burp Suite.
4. Try changing GET to POST and add body data to see if it gets reflected in response.
