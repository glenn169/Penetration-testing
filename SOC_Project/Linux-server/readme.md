# Goal

The goal of this project is to build a realistic, production-like security testing lab that hosts multiple intentionally vulnerable web applications using Docker on a Linux server.

The lab is designed to simulate how modern web applications are deployed and accessed in real-world environments. Instead of exposing services via IP addresses and open ports, applications are accessed through domain names using a centralized NGINX reverse proxy. This approach mirrors real production architectures commonly found in cloud platforms, bug bounty targets and enterprise environments.

Key objectives of this lab include:
- Hosting multiple vulnerable web applications in isolated Docker containers
- Using a central NGINX reverse proxy for domain-based routing
- Preventing direct exposure of backend service ports
- Simulating real-world DNS-based application discovery from an attacker’s perspective
- Providing a controlled environment for offensive security testing, SOC analysis and detection engineering

By replicating real deployment patterns rather than simplified lab setups, this project aims to improve practical skills in web application security testing, infrastructure awareness and defensive monitoring.

# How can I configure it ?
1. copy the docker-compose.yml file and paste it on your file system, then run `docker compose up -d`
2. I have added the required files in the respective folders, you can follow the steps provided there
3. run `docker ps` and you should be able to see the running docker containers
4. Once you have completed the step 2, then in attacker machine,
5. add the below given to `/etc/hosts`  Make sure to replace `<target-IP>` with the ip address of machine you have started docker on.
   ``` <target-IP>  dvwa.lab zerohealth.lab juiceshop.lab pixi.lab vulnbank.lab yrprey.lab vulnlab.lab webgoat.lab ```
