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

# Excessive data exposure 
### How does it happen?
Excessive data exposure occurs when applications tend to disclose more than desired information to the user through an API response. The application developers tend to expose all object properties (considering the generic implementations) without considering their sensitivity level. They leave the filtration task to the front-end developer before it is displayed to the user. Consequently, an attacker can intercept the response through the API and quickly extract the desired confidential data. The runtime detection tools or the general security scanning tools can give an alert on this kind of vulnerability. However, it cannot differentiate between legitimate data that is supposed to be returned or sensitive data. 

### Likely Impact 
A malicious actor can successfully sniff the traffic and easily access confidential data, including personal details, such as account numbers, phone numbers, access tokens and much more. Typically, APIs respond with sensitive tokens that can be later on used to make calls to other critical endpoints.

### Methodology 
follow the same method as other vulnerabilities, here the endpoint exposes excess data than it is intended 

# Lack of Resources and Rate Limiting
### How does it happen?
Lack of resources and rate limiting means that APIs do not enforce any restriction on the frequency of clients' requested resources or the files' size, which badly affects the API server performance and leads to the DoS (Denial of Service) or non-availability of service. Consider a scenario where an API limit is not enforced, thus allowing a user (usually an intruder) to upload several GB files simultaneously or make any number of requests per second. Such API endpoints will result in excessive resource utilisation in network, storage, compute etc.

# Broken Function Level Authorisation
### How does it happen?
Broken Function Level Authorisation reflects a scenario where a low privileged user (e.g., sales) bypasses system checks and gets access to confidential data by impersonating a high privileged user (Admin). Consider a scenario of complex access control policies with various hierarchies, roles, and groups and a vague separation between regular and administrative functions leading to severe authorisation flaws. By taking advantage of these issues, the intruders can easily access the unauthorised resources of another user or, most dangerously – the administrative functions. 
Broken Function Level Authorisation reflects IDOR permission, where a user, most probably an intruder, can perform administrative-level tasks. APIs with complex user roles and permissions that can span the hierarchy are more prone to this attack. 

### Likely Impact 
The attack primarily targets the authorisation and non-repudiation principles of security. Broken Functional Level Authorisation can lead an intruder to impersonate an authorised user and let the malicious actor get administrative rights to perform sensitive tasks. 

### Practical
In an endpoint `/apirule5/users_v` to fetch data of all employees from the database. To add protection, he added another layer to security by adding a special header `isAdmin` in each request. The API only fetches employee information from the database if `isAdmin=1` and `Authorization-Token` are correct. The authorisation token for HR user Alice is `YWxpY2U6dGVzdCFAISM6Nzg5Nzg=`.

# Mass Assignment 
### ﻿How does it happen?
Mass assignment reflects a scenario where client-side data is automatically bound with server-side objects or class variables. However, hackers exploit the feature by first understanding the application's business logic and sending specially crafted data to the server, acquiring administrative access or inserting tampered data. This functionality is widely exploited in the latest frameworks like Laravel, Code Ignitor etc.
Consider a user's profiles dashboard where users can update their profile like associated email, name, address etc. The username of the user is a read-only attribute and cannot be changed; however, a malicious actor can edit the username and submit the form. If necessary filtration is not enabled on the server side (model), it will simply insert/update the data in the database. 

### Practical
1. Bob has been assigned to develop a signup API endpoint `/apirule6/user` that will take a name, username and password as input parameters (POST). The user's table has a credit column with a default value of 50. Users will upgrade their membership to have a larger credit value.
2. Here basically you should not be able to tamper the values of the existing credits or points

# Security Misconfiguration
### How does it happen?
Security misconfiguration depicts an implementation of incorrect and poorly configured security controls that put the security of the whole API at stake. Several factors can result in security misconfiguration, including improper/incomplete default configuration, publically accessible cloud storage, Cross-Origin Resource Sharing (CORS), and error messages displayed with sensitive data. Intruders can take advantage of these misconfigurations to perform detailed reconnaissance and get unauthorised access to the system. 
Security misconfigurations are usually detected by vulnerability scanners or auditing tools and thus can be curtailed at the initial level. API documentation, a list of endpoints, error logs etc., must not be publically accessible to ensure safety against security misconfigurations. Typically, companies deploy security controls like web application firewalls, which are not configured to block undesired requests and attacks.

### Practical
1. The company MHT is facing serious server availability issues. Therefore, they assigned Bob to develop an API endpoint `/apirule7/ping_v` (GET) that will share details regarding server health and status.
2. Bob successfully designed the endpoint; however, he forgot to implement any error handling to avoid any information leakage.

# Improper Assets Management
### ﻿How does it happen?
Inappropriate Asset Management refers to a scenario where we have two versions of an API available in our system; let's name them APIv1 and APIv2. Everything is wholly switched to APIv2, but the previous version, APIv1, has not been deleted yet. Considering this, one might easily guess that the older version of the API, i.e., APIv1, doesn't have the updated or the latest security features. Plenty of other obsolete features of APIv1 make it possible to find vulnerable scenarios, which may lead to data leakage and server takeover via a shared database amongst API versions.
It is essentially about not properly tracking API endpoints. The potential reasons could be incomplete API documentation or absence of compliance with the Software Development Life Cycle. A properly maintained, up-to-date API inventory and proper documentation are more critical than hardware-based security control for an organisation.

# Insufficient Logging & Monitoring 
### How does it happen?
Insufficient logging & monitoring reflects a scenario when an attacker conducts malicious activity on your server; however, when you try to track the hacker, there is not enough evidence available due to the absence of logging and monitoring mechanisms. Several organisations only focus on infrastructure logging like network events or server logging but lack API logging and monitoring. Information like the visitor's IP address, endpoints accessed, input data etc., along with a timestamp, enables the identification of threat attack patterns. If logging mechanisms are not in place, it would be challenging to identify the attacker and their details. Nowadays, the latest web frameworks can automatically log requests at different levels like error, debug, info etc. These errors can be logged in a database or file or even passed to a SIEM solution for detailed analysis.
   
