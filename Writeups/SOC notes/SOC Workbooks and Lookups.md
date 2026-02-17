# SOC Workbooks and Lookups

### Assets & Identities 
You will have the Identity Inventory where information about all the people working in the company will be stored including their dept, access and other necessary information 

Asset Inventory, here all the information about the device will be stored, like, IP address, location of server, use of the server..etc.

Continuing identity and asset inventory topics, you might also need to look at the alert from a network point of view, especially in bigger companies. Consider the scenario where you are investigating a chain of related alerts based on firewall logs and want to give some meaning to the IPs you see:

1. 08:00: An IP 103.61.240.174 is repeatedly connecting to a corporate firewall via port TCP/10443
2. 08:23: Firewall logs show that the IP 103.61.240.174 was translated to an internal 10.10.0.53 IP
3. 08:25: The IP 10.10.0.53 is scanning the 172.16.15.0/24 network range but does not find open ports
4. 08:32: The same IP is now scanning the 172.16.23.0/24 network range, and the attack seems to be ongoing

## Network Diagrams
To investigate the case above, you will have to find out what service is running at the 10443 port and why anyone would connect there. Then, identify the subnet the 10.10.0.53 IP belongs to and why it would ever try to connect to other subnets. A network diagram, a visual schema presenting existing locations, subnets, and their connections, is an answer to your questions:
<img width="3560" height="1480" alt="image" src="https://github.com/user-attachments/assets/23d228fa-275c-4cda-813f-8085ee95abf9" />

Depending on a company's size and structure, you may see more complex diagrams, but their use for SOC analysts remains the same - to help understand suspicious network activity. In our scenario, you can refer to the network diagram and reconstruct the attack path as follows:

1. Threat actor behind the 103.61.240.174 IP performed VPN brute force, targeting `vpn.tryhatme.thm`
2. After a successful brute force and VPN login, the threat actor was assigned an IP from the VPN Subnet
3. Then, the adversary tried to scan the Database Subnet, but was likely blocked by the firewall rules
4. Seeing no success, the threat actor switches to the Office Subnet, looking for their next target

## SOC Workbooks
SOC workbook, also called playbook, runbook, or workflow, is a structured document that defines the steps required to investigate and remediate specific threats efficiently and consistently. Since L1 analysts are considered junior specialists and are not expected to triage every possible attack scenario perfectly, senior analysts often prepare workbooks to support their less experienced teammates. L1 analysts are recommended and sometimes even required to triage the alerts precisely according to workbooks to avoid mistakes and streamline the analysis.
## Workbook Example
<img width="1342" height="690" alt="image" src="https://github.com/user-attachments/assets/4f60feda-d717-4fd0-a115-5460466635de" />
The diagram above is a typical example of an investigation workbook aimed to help L1 analysts triage alerts about atypical email, web, or corporate VPN login. Most workbook diagrams are supplemented with a detailed textual guide and links to the mentioned resources. Also, note how the workbook is divided into three logical groups. By following the steps in the correct order, you can guarantee high-quality alert triage and eliminate cases where the verdict is made without enough evidence:
1. Enrichment: Use Threat Intelligence and identity inventory to get information about the affected user
2. Investigation: Using the gathered data and SIEM logs, make your verdict if the login is expected
3. Escalation: Escalate the alert to L2 or communicate the login with the user if necessary

