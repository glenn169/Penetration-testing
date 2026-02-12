# Find Upload Vulnerability
File uploads can also open up severe vulnerabilities in the server. This can lead to anything from relatively minor, nuisance problems; all the way up to full Remote Code Execution (RCE) if an attacker manages to upload and execute a shell. With unrestricted upload access to a server (and the ability to retrieve data at will), an attacker could deface or otherwise alter existing content -- up to and including injecting malicious webpages, which lead to further vulnerabilities such as XSS or CSRF. By uploading arbitrary files, an attacker could potentially also use the server to host and/or serve illegal content, or to leak sensitive information. Realistically speaking, an attacker with the ability to upload a file of their choice to your server -- with no restrictions -- is very dangerous indeed.

## Methodology 
1. If you find any function to upload the file, check for the source code if there is any client-side filters, make sure to intercept the request while uploading the file, and use gobuster to check where the files are being uploaded.
2. poke around and see what you can and can't upload, If the website has server-side filtering in place then we may need to take a guess at what the filter is looking for, upload a file, then try something slightly different based on the error message if the upload fails. **USE OWASP ZAP**


# Overwriting Existing Files
When files are uploaded to the server, a range of checks should be carried out to ensure that the file will not overwrite anything which already exists on the server. Common practice is to assign the file with a new name -- often either random, or with the date and time of upload added to the start or end of the original filename. Alternatively, checks may be applied to see if the filename already exists on the server; if a file with the same name already exists then the server will return an error message asking the user to pick a different file name. File permissions also come into play when protecting existing files from being overwritten. Web pages, for example, should not be writeable to the web user, thus preventing them from being overwritten with a malicious version uploaded by an attacker.

# RCE using File upload
Remote Code Execution (as the name suggests) would allow us to execute code arbitrarily on the web server. Whilst this is likely to be as a low-privileged web user account (such as www-data on Linux servers), it's still an extremely serious vulnerability. Remote code execution via an upload vulnerability in a web application tends to be exploited by uploading a program written in the same language as the back-end of the website (or another language which the server understands and will execute). Traditionally this would be PHP, however, in more recent times, other back-end languages have become more common (Python Django and Javascript in the form of Node.js being prime examples). It's worth noting that in a routed application (i.e. an application where the routes are defined programmatically rather than being mapped to the file-system), this method of attack becomes a lot more complicated and a lot less likely to occur. Most modern web frameworks are routed programmatically.
There are two basic ways to achieve RCE on a webserver when exploiting a file upload vulnerability: webshells, and reverse/bind shells. Realistically a fully featured reverse/bind shell is the ideal goal for an attacker; however, a webshell may be the only option available (for example, if a file length limit has been imposed on uploads, or if firewall rules prevent any network-based shells). We'll take a look at each of these in turn. As a general methodology, we would be looking to upload a shell of one kind or another, then activating it, either by navigating directly to the file if the server allows it (non-routed applications with inadequate restrictions), or by otherwise forcing the webapp to run the script for us (necessary in routed applications).

## Methodology
_Bind Shell_
1. upload a random image or file, check where it is being stored.
2. Then you can upload the webshell file i.e., for php the code will be,
  ``` php
<?php
    echo system($_GET["cmd"]);
?>
```
save it to a .php file and upload it.
3. Go to upload folder for example ` http://shell.uploadvulns.thm/<upload_folder>/file_name.php?cmd=whoami ` in webshell you need to use the parameter `?cmd=<command>`

_Reverse Shell_
1. go to https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php and copy the code and replace with `attacker_ip` and `Listerner_port` and save it to a .php file
2. Go to the uploaded folder or try to access the file while keeping the netcat listerner enabled `nc -lvnp <port_number>`

# Filtering 
Client-side filters: Basically filters the file brefore it is uploaded to the server. It takes place in the client side
Server-side filters: These filters are on the server side, since the code is not visible, it is difficult to bypass the server-side filters completly.

### Extension Validation
File extensions are used (in theory) to identify the contents of a file. In practice they are very easy to change, so actually don't mean much; however, MS Windows still uses them to identify file types, although Unix based systems tend to rely on other methods, which we'll cover in a bit. Filters that check for extensions work in one of two ways. They either blacklist extensions (i.e. have a list of extensions which are not allowed) or they whitelist extensions (i.e. have a list of extensions which are allowed, and reject everything else)

### File Type Filtering 
Similar to Extension validation, but more intensive, file type filtering looks, once again, to verify that the contents of a file are acceptable to upload. We'll be looking at two types of file type validation:

_MIME validation:_ MIME (Multipurpose Internet Mail Extension) types are used as an identifier for files -- originally when transfered as attachments over email, but now also when files are being transferred over HTTP(S). The MIME type for a file upload is attached in the header of the request, and looks something like this:
<img width="762" height="216" alt="image" src="https://github.com/user-attachments/assets/659675f3-8c2e-4dc3-aaa7-33b871395071" />
_Magic Number validation:_ Magic numbers are the more accurate way of determining the contents of a file; although, they are by no means impossible to fake. The "magic number" of a file is a string of bytes at the very beginning of the file content which identify the content. For example, a PNG file would have these bytes at the very top of the file: `89 50 4E 47 0D 0A 1A 0A`.

_- File Length Filtering_
_- File Name Filtering_
_- File Content Filtering_
