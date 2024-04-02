Tools include - nmap, owasp amass, gobuster, kiterunner and dev tools

start crAPI - cd /lab/crapi && sudo docker-compose start

## nmap scan - 
    1. nmap -sC -sV 127.0.0.1
    2. nmap -p- 127.0.0.1
    3. nmap -sV -p 8025 127.0.0.1

## OWASP Amass - 
    1. amass enum -list
    2. amass enum -active -d crapi.apisec.api
    3. amass enum -active -d microsoft.com | grep api

## gobuster - 
    1. gobuster dir -u http://127.0.0.1:8888 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  -b 200 
    2. gobuster dir -u http://127.0.0.1:8888/community -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

## Dev tools - 
    1. go to dev tools by pression F12 and go to Network tab. right click and enable URL. 
    2. Now browse the application and look for URLs that make api calls
    3. copy curl request of those api calls and import it in postman to interact with it
