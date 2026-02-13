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


# Bypassing Client-side Filters 
### There are four easy ways to bypass your average client-side file upload filter:
1. Turn off Javascript in your browser this will work provided the site doesn't require Javascript in order to provide basic functionality. If turning off Javascript completely will prevent the site from working at all then one of the other methods would be more desirable; otherwise, this can be an effective way of completely bypassing the client-side filter.
2. Intercept and modify the incoming page. Using Burpsuite, we can intercept the incoming web page and strip out the Javascript filter before it has a chance to run. The process for this will be covered below.
3. Intercept and modify the file upload. Where the previous method works before the webpage is loaded, this method allows the web page to load as normal, but intercepts the file upload after it's already passed (and been accepted by the filter
4. Send the file directly to the upload point. Why use the webpage with the filter, when you can send the file directly using a tool like `curl`? Posting the data directly to the page which contains the code for handling the file upload is another effective method for completely bypassing a client side filter. `curl -X POST -F "submit:<value>" -F "<file-parameter>:@<path-to-file>" <site>`.To use this method you would first aim to intercept a successful upload (using Burpsuite or the browser console) to see the parameters being used in the upload, which can then be slotted into the above command.

## Methodology
1. lets assume that we found an upload page on a website
  
   <img width="534" height="320" alt="image" src="https://github.com/user-attachments/assets/ca3c151b-ca24-47f9-8836-95af102475a3" />
4. check for the javascript filters in the source code.
   <img width="755" height="576" alt="image" src="https://github.com/user-attachments/assets/1e28c8db-8b3c-4148-b400-c4555573109a" />
In this instance we can see that the filter is using a whitelist to exclude any MIME type that isn't image/jpeg.
5. Our next step is to attempt a file upload. As expected, if we choose a JPEG, the function accepts it. Anything else and the upload is rejected.
6. Having established this, let's start Burpsuite and reload the page. We will see our own request to the site, but what we really want to see is the server's response, so right click on the intercepted data, scroll down to "Do Intercept", then select "Response to this request":
   <img width="581" height="687" alt="image" src="https://github.com/user-attachments/assets/ea05f369-eb44-40e8-8c22-a301597de84e" />
5.When we click the "Forward" button at the top of the window, we will then see the server's response to our request. Here we can delete, comment out, or otherwise break the Javascript function before it has a chance to load:<img width="877" height="742" alt="image" src="https://github.com/user-attachments/assets/3cd1dbfb-bb88-491c-869b-b0f51ae113dd" />
6. Having deleted the function, we once again click "Forward" until the site has finished loading, and are now free to upload any kind of file to the website
7. It's worth noting here that Burpsuite will not, by default, intercept any external Javascript files that the web page is loading. If you need to edit a script which is not inside the main page being loaded, you'll need to go to the "Options" tab at the top of the Burpsuite window, then under the "Intercept Client Requests" section, edit the condition of the first line to remove `^js$|`:
   <img width="821" height="296" alt="image" src="https://github.com/user-attachments/assets/0056fe2d-78cd-4f69-8652-9b3e39c90098" />


We've already bypassed this filter by intercepting and removing it prior to the page being loaded, but let's try doing it by uploading a file with a legitimate extension and MIME type, then intercepting and correcting the upload with Burpsuite.

1. Having reloaded the webpage to put the filter back in place, let's take the reverse shell that we used before and rename it to be called "shell.jpg". As the MIME type (based on the file extension) automatically checks out, the Client-Side filter lets our payload through without complaining:
<img width="473" height="306" alt="image" src="https://github.com/user-attachments/assets/cc531360-79de-4764-ac62-6e532900677a" />
2. Once again we'll activate our Burpsuite intercept, then click "Upload" and catch the request:
   <img width="756" height="462" alt="image" src="https://github.com/user-attachments/assets/ae2e651c-1d41-48ae-a9a1-455dae8073b6" />
3. Observe that the MIME type of our PHP shell is currently image/jpeg. We'll change this to text/x-php, and the file extension from .jpg to .php, then forward the request to the server:
   <img width="587" height="225" alt="image" src="https://github.com/user-attachments/assets/e288c0cd-78c8-4627-8a76-166576215bc5" />
4. Now, when we navigate to http://demo.uploadvulns.thm/uploads/shell.php having set up a netcat listener, we receive a connection from the shell! 

# Bypassing Server-side Filtering: File Extensions
For the first part of this task we'll take a look at a website that's using a blacklist for file extensions as a server side filter. There are a variety of different ways that this could be coded, and the bypass we use is dependent on that. In the real world we wouldn't be able to see the code for this, but for this example, it will be included here:
``` php
<?php
    //Get the extension
    $extension = pathinfo($_FILES["fileToUpload"]["name"])["extension"];
    //Check the extension against the blacklist -- .php and .phtml
    switch($extension){
        case "php":
        case "phtml":
        case NULL:
            $uploadFail = True;
            break;
        default:
            $uploadFail = False;
    }
?>
```
In this instance, the code is looking for the last period (.) in the file name and uses that to confirm the extension, so that is what we'll be trying to bypass here.

We can see that the code is filtering out the .php and .phtml extensions, so if we want to upload a PHP script we're going to have to find another extension. The wikipedia page for PHP gives us a few common extensions that we can try; however, there are actually a variety of other more rarely used extensions available that webservers may nonetheless still recognise. These include: `.php3`, `.php4`, `.php5`, `.php7`, `.phps`, `.php-s`, `.pht` and `.phar`. Many of these bypass the filter (which only blocks`.php` and `.phtml`), but it appears that the server is configured not to recognise them as PHP files, as in the below example:
<img width="805" height="329" alt="image" src="https://github.com/user-attachments/assets/16b29e11-b719-43cd-99f9-839a51f7af2c" />
This is actually the default for Apache2 servers, at the time of writing; however, the sysadmin may have changed the default configuration (or the server may be out of date), so it's well worth trying.

Eventually we find that the .phar extension bypasses the filter and works thus giving us our shell:
<img width="1041" height="630" alt="image" src="https://github.com/user-attachments/assets/c234f4e2-14bc-4e7e-87b7-516e4885ae94" />

## Black-Box testing for file extension filter
Let's have a look at another example, with a different filter. This time we'll do it completely black-box: i.e. without the source code.
Once again, we have our upload form:
<img width="476" height="278" alt="image" src="https://github.com/user-attachments/assets/734c01da-8ec9-4f00-ae63-77b7f3c685c9" />
Ok, we'll start by scoping this out with a completely legitimate upload. Let's try uploading the spaniel.jpg image from before:
<img width="562" height="292" alt="image" src="https://github.com/user-attachments/assets/e1a4a0a3-c55d-432b-bb37-98057b16bef2" />
Well, that tells us that JPEGS are accepted at least. Let's go for one that we can be pretty sure will be rejected (`shell.php`):
<img width="572" height="314" alt="image" src="https://github.com/user-attachments/assets/3764b947-7968-4cf5-b4c2-cb912bc1ca4e" />
In the previous example we saw that the code was using the `pathinfo()` PHP function to get the last few characters after the `.`, but what happens if it filters the input slightly differently?
Let's try uploading a file called `shell.jpg.php`. We already know that `JPEG` files are accepted, so what if the filter is just checking to see if the `.jpg` file extension is somewhere within the input?

Pseudocode for this kind of filter may look something like this:
```
ACCEPT FILE FROM THE USER -- SAVE FILENAME IN VARIABLE userInput
IF STRING ".jpg" IS IN VARIABLE userInput:
    SAVE THE FILE
ELSE:
    RETURN ERROR MESSAGE
```


When we try to upload our file we get a success message. Navigating to the /uploads directory confirms that the payload was successfully uploaded:
when we click on the file, by enabling the reverse listner, we will get the reverse shell 

This is by no means an exhaustive list of upload vulnerabilities related to file extensions. As with everything in hacking, we are looking to exploit flaws in code that others have written; this code may very well be uniquely written for the task at hand. This is the really important point to take away from this task: there are a million different ways to implement the same feature when it comes to programming -- your exploitation must be tailored to the filter at hand. The key to bypassing any kind of server side filter is to enumerate and see what is allowed, as well as what is blocked; then try to craft a payload which can pass the criteria the filter is looking for.

# Bypassing Server-side Filtering: Magic Numbers
Magic numbers are used as a more accurate identifier of files. The magic number of a file is a string of hex digits, and is always the very first thing in a file. Knowing this, it's possible to use magic numbers to validate file uploads, simply by reading those first few bytes and comparing them against either a whitelist or a blacklist. Bear in mind that this technique can be very effective against a PHP based webserver


- As expected, if we upload our standard shell.php file, we get an error; however, if we upload a JPEG, the website is fine with it. All running as per expected so far.
- From the previous attempt at an upload, we know that JPEG files are accepted, so let's try adding the JPEG magic number to the top of our shell.php file. A quick look at the list of file signatures on Wikipedia shows us that there are several possible magic numbers of JPEG files. It shouldn't matter which we use here, so let's just pick one (FF D8 FF DB). We could add the ASCII representation of these digits (ÿØÿÛ) directly to the top of the file but it's often easier to work directly with the hexadecimal representation, so let's cover that method.
- Before we get started, let's use the Linux `file` command to check the file type of our shell
- We can see that the magic number we've chosen is four bytes long, so let's open up the `reverse_shell.php` script and add four random characters on the first line. These characters do not matter, so for this example we'll just use four "A"s
- Save the file and exit. Next we're going to reopen the file in hexeditor (which comes by default on Kali), or any other tool which allows you to see and edit the shell as hex. In hexeditor the file looks like this
  <img width="718" height="82" alt="image" src="https://github.com/user-attachments/assets/5e9d7835-f266-40e4-92ca-dbb4cccbc386" />
- **Note** the four bytes in the red box: they are all 41, which is the hex code for a capital "A" -- exactly what we added at the top of the file previously.
- Change this to the magic number we found earlier for JPEG files: `FF D8 FF DB` you can find using `hexeditor <file_name>` 
- Perfect. Now let's try uploading the modified shell and see if it bypasses the filter!



