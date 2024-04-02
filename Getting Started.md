## Tools Setup

1. Start off with update, upgrade and dist-upgrade
2. Check burp suite installation 
3. Install Autorize extension in Burp Suite Extender > BApp Store (This will help with automating authorization testing)
4. Install FoxyProxy extension on browser 
5. Config proxy settings in extension for burp with 8080 port and postman with port 5555
6. Set up Burp Suite CA certificate by 

    1. turn on burp suite proxy on foxyproxy
    2. browse to http://burpsuite and download CA certificate
    3. Browser Settings > Certificate > Import

7. Install Postman - sudo wget https://dl.pstmn.io/download/latest/linux64 -O postman-linux-x64.tar.gz

8. sudo tar -xzvf postman-linux-x64.tar.gz -C /opt

9. Linking to postman - sudo ln -s /opt/Postman/Postman /usr/bin/postman
10. Launch Postman with postman command
11. Install Mitmproxy2swagger - sudo pip3 install mitmproxy2swagger
12. Install Git - sudo apt install git
13. Install Docker - sudo apt install docker-compose && sudo apt install docker.io
14. Install go lang - sudo apt install golang-go -y
15. Install jwt_tool - sudo git clone https://github.com/ticarpi/jwt_tool
    1. sudo chmod +x jwt_tool.py
    2. sudo ln -s /opt/jwt_tool/jwt_tool.py /usr/bin/jwt_tool
16. Install Kiterunner - 
    1. sudo git clone https://github.com/assetnote/kiterunner.git
    2. cd kiterunner && sudo make build
    3. sudo ln -s /opt/kiterunner/dist/kr /usr/bin/kiterunner
17. Install Arjun - 
    1. sudo git clone https://github.com/s0md3v/Arjun.git
    2. cd Arjun && sudo pip install .
18. Install OWASP ZAP - sudo apt install zaproxy
    1. Update OpenAPI Support extension

## Lab Setup

## CrAPI
1. Download CrAPI - curl -o docker-compose.yml https://raw.githubusercontent.com/OWASP/crAPI/main/deploy/docker/docker-compose.yml
2. mkdir crapi && mv docker-compose.yml crapi
3. cd crapi
4. sudo docker-compose pull
5. sudo docker-compose -f docker-compose.yml --compatibility up -d 
6. Browse to http://localhost:8888/login for CrAPI
7. Browse to http://localhost:8025/ for MailHog (Mail server)

## Vapi
1. sudo git clone https://github.com/roottusk/vapi.git
2. cd vapi 
3. sudo docker-compose up -d 
4. Browse to http://localhost/vapi

> check running docker images - sudo docker-compose ps

> to stop - sudo docker-compose stop
