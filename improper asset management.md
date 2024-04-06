Check if downgrading of version numbers is possible.

Examples - 
        
    /api/v3/accounts
    /v2/accounts
    api2.target.com/accounts
    apiv3.target.com/accounts
    Accept:version=2.0
    Accept:api-version=3
    /api/accounts?ver=2

Look for non production version of an API include any API version that was not meant for end-user consumption. Like -
    
    api.test.target.com
    api.uat.target.com
    beta.api.target.com
    delta.api.target.com
    /api/private
    /api/test

1. In Postman look for places where such version numbers are being used.
2. Create a new environment if not created and add a variable for version.
3. Use "Find and Replace" functionality in Postman to search all v2 and replace with the variable created before.
4. See if the requests with lower version numbers are still getting valid responses.
5. Look for differences in HTTP Headers in different versions for lack of security headers.
