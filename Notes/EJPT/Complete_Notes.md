
# 1. Information Gathering.

Information gathering is nothing but gathering information about our target either by active of passive recon.

1. Passive Information Gathering:
    - Gathering information about the target without directly engaging with it.
    - Gaining information using google, shodan or some other tools which doesnt directly contact with the server
2. Active Information Gathering:
    - In active information gathering we will actively engage with the target system.
    - we can gain information sing some tools like: gaining IP addresses, port scans etc.
    
    ### Footprinting:
    
    Footprinting is similar to information gathering but in footprinting we identify more important information related to our target.
    
    ## Passive Recon:
    
    Things that you should look in passive recon
    
    - IP addresses
    - Directories hidden from search engines (robots.txt)
    - Names
    - Email Addresses
    - Phone numbers
    - Physical address
    - web technologies.
    
    ### Practice Steps
    
    1. Obtain the IP address of the server using `host` 
    
    ```bash
    host <domain_name>
    ```
    
    → i f you get 2 IP addresses for the server, then it shows that the server is using WAF or any proxy services.
    
    1. Check for `robots.txt`  → it allows you what specific file you don't want the web crawlers to crawl
    2. Check for `sitemap.xml` → it helps search engine to provied results in a organised way of indexing the website 
    3. Add ons: `Builtwith`  : to identify technologies.
        
                          `wappalyzer`: to identify the technologies.
        
    4. Use `whatweb` 
    
    ```bash
    whatweb <domain_name>
    ```
    
     6. `HTTrack` is used to download the complete website to your local machine.
    
    ### Whois enumeration:
    
    It enumerates the db that stores the registrar, registered users, ip and other important information.
    
    ```bash
    whois <domain_name> OR <IP_Address>
    ```
    
    ### Website footprinting with netcraft
    
    Netcraft is essentially used to gather information about a target domain, including email, technologies used and other things.
    
    go to [sitereport.netcraft.com](http://sitereport.netcraft.com) and enter the DOMAIN_NAME
    
    ### DNS Recon
    
    it is used to identify the records associated with the domain like A, AAAA, MX, TXT records, Name Server etc.
    
    **Practical:**
    
    1. [dnsdumpster.com](http://dnsdumpster.com) → used for bug bounty too. It gives the well structured results and its really helpful.
