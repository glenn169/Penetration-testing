# Insecure Direct Object Reference (IDOR)
This type of vulnerability can occur when a web server receives user-supplied input to retrieve objects (files, data, documents), too much trust has been placed on the input data, and it is not validated on the server-side to confirm the requested object belongs to the user requesting it.

- Usually ID is encoded using base64
- If ID is hashed then you can crack it using http://crackstation.net . MD5 is most commonly used to hash the ID


# Methodology
1. create two accounts and swap the Id numbers between them.
2. You can usually check for API call using burpsuite or browser network tool to find the api call and modify it
3. If you can view the other users' content using their Id number while still being logged in with a different account (or not logged in at all),
4. CONGRATULATIONS!!🎊🎊 you've found a valid IDOR vulnerability.

