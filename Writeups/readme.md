# Methodology

# 1. Authentication Bypass
## Username Enumeration
- ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u <URL> -mr "username already exists"
    * `-w` -> input path to wordlist
    * `-X` -> Request method eg: POST, GET, PUT.
    * `-d` -> input data parameters (eg: username, password,email..etc)
    * `-H` -> Input the headers (eg: Content-Type: application/x-www-form-urlencoded , Content-Type: application/json {Use when you input data in JSON format} )
    * `-u` -> Enter the URL
    * `-mr` -> to match the regexp
