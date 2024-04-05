# Nikto 
    nikto <url>
Skim through the found misconfigurations out of which some might be interesting.

# ZAP
## Automated mode
1. Quick Start > Automated Scan > URL > Attack

This will give some results but wont be effective.

2. Export > Import OpenAPI Defination 

Provide the spec.yml file here and ZAP will include all the requests into Sites tab.

From here we can do directory bruteforcing with Spider and try Active Scanning to scan for vulnerabilities.

## Semi Manual Mode
1. Open ZAP Browser with HUD enabled.
2. Browse through the complete application just like before to collect all the endpoints with active scan "ON".
3. We can initiate Spider while in HUD mode as well to brute force directories.
4. After browing throughout the application wait for Active scan to complete and check Alerts generated.