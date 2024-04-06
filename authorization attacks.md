## Object level authorization attacks
This is all about access resources that do not belong to us.
1. Create two user accounts and try accessing data from each other account by IDs
2. Look for all the URLs where there are unique identifiers like IDOR. 
3. Try brute forcing those URLs and look for any responses that were successful
4. If any IDs look comlpex to just iterate, send it to sequencer and check the randomness and try to bruteforce.

## Function level Authorization attacks
This is all about performing unauthorized actions

1. Resource ID
2. Requests that perform unauthorized actions like update, delete, or otherwise alter the resources of other users.
3. Missed or flawed access controls.

Create 2 user accounts and try CRUD operations on each other if possible.