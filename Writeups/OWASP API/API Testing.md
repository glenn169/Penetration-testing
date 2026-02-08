# Broken Object Level Authorisation(BOLA)
### How does it Happen?
Generally, API endpoints are utilised for a common practice of retrieving and manipulating data through object identifiers. BOLA refers to **Insecure Direct Object Reference (IDOR)** - which creates a scenario where the user uses the input functionality and gets access to the resources they are not authorised to access. In an API, such controls are usually implemented through programming in Models (Model-View-Controller Architecture) at the code level.

### Likely Impact
The absence of controls to prevent unauthorised object access can lead to data leakage and, in some cases, complete account takeover. User's or subscribers' data in the database plays a critical role in an organisation's brand reputation; if such data is leaked over the internet, that may result in substantial financial loss.

### Practical 
1. Go to the `api` endpoint such as `/apirule1/users/{ID}` it allows the user to request information by sending an employee ID. You can send any employee ID and it will return with their details.
2. What is the issue with the above API call? The problem is that the endpoint is not validating any incoming API call to confirm whether the request is valid. It is not checking for any authorisation whether the person requesting the API call can ask for it or not.
3. If it asks for authorization token then it will be secure.

# Broken User Authenticaton (BUA)
### How does it happen?
User authentication is the core aspect of developing any application containing sensitive data. Broken User Authentication (BUA) reflects a scenario where an API endpoint allows an attacker to access a database or acquire a higher privilege than the existing one. The primary reason behind BUA is either invalid implementation of authentication like using incorrect email/password queries etc., or the absence of security mechanisms like authorisation headers, tokens etc.

### Likely Impact 
In broken user authentication, attackers can compromise the authenticated session or the authentication mechanism and easily access sensitive data. Malicious actors can pretend to be someone authorised and can conduct an undesired activity, including a complete account takeover.

### Methodology
1. go to the login api endpoint eg: `/apirule2/user/login_v` which authenticates based on email and password.
2. The endpoint will return the token which will be used as `Authorization-Token` header in GET request to `apirule2/user/details` to show the specific employee.
3. You can test by sending the POST request only with the vaid email and password as `' or 1=1;--` in the form parameter
