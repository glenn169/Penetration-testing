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
