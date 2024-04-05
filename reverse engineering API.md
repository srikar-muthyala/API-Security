If api documentation is not available we need to build our own to know how to use the API.

Two ways to go about it - 
1. Use Postman to collect API requests and manually build out an API collection.
2. Automatically build out an API specification using mitmproxy2swagger.

## Manual Method
1. In Postman, Capture Requests > Enable Proxy > Change URL > Start proxy

2. Enable postman proxy in foxyproxy and start browsing the application to populate the requests to work with.

3. After browsing the application and getting enough requests populated in postman, we can stop the capture.

4. Select the API requests and add them to collection.

5. Create folders and organize the API requests in them to make it cleaner and easier to work with.

## Automated Method
1. Open mitmweb
2. enable burp proxy in foxyproxy and start browsing the application just like before.
3. All the traffic will be logged in on mitmweb application.
4. File > Save
5. mitmproxy2swagger -i ~/Downloads/flows -o spec.yml -p http://localhost:8888 -f flow
6. clean the spec.yml file and remove ignore flags to all the endpoints that are needed. (only remove ignore not '-' before it)
7. Run this after cleaning the file - mitmproxy2swagger -i ~/Downloads/flows\ \(1\) -o spec1.yml -p http://localhost:8888 -f flow --examples
8. Import in editor.swagger.io to check the documentation.
9. Import into Postman to work with it
