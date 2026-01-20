# File Inclusion Vulnerability 
File inclusion vulnerabilities are commonly found and exploited in various programming languages for web applications, such as PHP that are poorly written and implemented. The main issue of these vulnerabilities is the input validation, in which the user inputs are not sanitized or validated, and the user controls them.

# Path Traversal
Also known as Directory traversal, a web security vulnerability allows an attacker to read operating system resources, such as local files on the server running an application. The attacker exploits this vulnerability by manipulating and abusing the web application's URL to locate and access files or directories stored outside the application's root directory.
- Usually in PHP `file_get_contents` fuction will cause Path Traversal vulnerability

| Location | Description |
|--------|-------------|
| `/etc/issue` | Contains a message or system identification printed before the login prompt |
| `/etc/profile` | Controls system-wide default variables such as exported variables, file creation mask (umask), terminal types, and mail notifications |
| `/proc/version` | Specifies the version of the Linux kernel |
| `/etc/passwd` | Contains all registered users that have access to the system |
| `/etc/shadow` | Contains information about system users' passwords |
| `/root/.bash_history` | Contains the command history for the root user |
| `/var/log/dmessage` | Contains global system messages, including messages logged during system startup |
| `/var/mail/root` | Stores all emails for the root user |
| `/root/.ssh/id_rsa` | Private SSH keys for the root or any valid user on the server |
| `/var/log/apache2/access.log` | Contains access request logs for the Apache web server |
| `C:\boot.ini` | Contains boot options for computers using BIOS firmware |

#Local File Inclusion (LFI)
LFI attacks against web applications are often due to a developers' lack of security awareness. With PHP, using functions such as `include`, `require`, `include_once`, and `require_once` often contribute to vulnerable web applications.

### Suppose the web application provides two languages and the user can select between the EN and AR.

``` php
<?PHP 
	include($_GET["lang"]);
?>
```
1. The PHP code above uses a GET request via the URL parameter lang to include the file of the page.
2. The call can be done by sending the following HTTP request as follows: `http://webapp.thm/index.php?lang=EN.php` to load the English page or `http://webapp.thm/index.php?lang=AR.php` to load the Arabic page, where EN.php and AR.php files exist in same directory.

### In the above case we analysed the code and checked for vulneability, In this case we will be doing black box testing, in which we wont have source code. We will use the errors to understand how the data is passed and process in to the web app.
1. we have the following entry point: `http://webapp.thm/index.php?lang=EN`. If we enter an invalid input, such as `THM`, we get the following error
``` Warning: include(languages/THM.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12 ```
2. The error message discloses significant information. By entering THM as input, an error message shows what the include function looks like: include(languages/THM.php);
3. If you look closely in the error message it will add .php at the end of the parameter value and also the error shows the path `/var/www/html/THM-4/index.php`
`eg: If you input the file welcome.php the error would look something like this ->  include(includes/welcome.php.php)`
4. To exploit this we need to use ../../../../etc/passwd (../ = 4 since we need to go back 4 directories before getting to the root directory)
``` Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12```
5. It seems we could move out of the PHP directory but still, the include function reads the input with .php at the end! This tells us that the developer specifies the file type to pass to the include function. To bypass this scenario, we can use the NULL BYTE, which is %00.
6. the url will look like `http://webapp.thm/index.php?lang=../../../../etc/passwd%00` where the code looks like `include("languages../../../../etc/passwd%00").".php");`  !!THIS DOES NOT WORK ABOVE PHP 5.3.4

# Remote File Inclusion - RFI
It is similar to LFI, here we can add the external files to include function. This is basically something where the attacker can inject an external url into `include` function. One requirement for RFI is that the `allow_url_fopen` option needs to be ON.
Risk of RFI is higher than LFI since it can allow an attacker to gain RCE on the server 

# Methodology
1. Find an entry point that could be via `GET`, `POST`, `COOKIE`, or `HTTP` header values!
2. Enter a valid input to see how the web server behaves.
3. Enter invalid inputs, including special characters and common file names.
4. Don't always trust what you supply in input forms is what you intended! Use either a browser address bar or a tool such as Burpsuite.
5. Look for errors while entering invalid input to disclose the current path of the web application; if there are no errors, then trial and error might be your best option.
6. Understand the input validation and if there are any filters!
7. Try the inject a valid entry to read sensitive files


# Remediation
1. Keep system and services, including web application frameworks, updated with the latest version. 
2. Turn off PHP errors to avoid leaking the path of the application and other potentially revealing information.
3. A Web Application Firewall (WAF) is a good option to help mitigate web application attacks.
4. Disable some PHP features that cause file inclusion vulnerabilities if your web app doesn't need them, such as `allow_url_fopen ON` and `allow_url_include`.
5. Carefully analyze the web application and allow only protocols and PHP wrappers that are in need.
6. Never trust user input, and make sure to implement proper input validation against file inclusion.
7. Implement whitelisting for file names and locations as well as blacklisting.
