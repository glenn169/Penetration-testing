# Disclaimer
This project is built strictly for educational and defensive security purposes. All attacks are performed in a controlled lab environment on systems owned by the author.

# Project Description 
I am currently designing and building a **SOC Analyst home lab project** to gain hands-on experience with real-world detection, monitoring and incident analysis.

The lab simulates an enterprise-style environment consisting of:

- A **Windows Active Directory server** to generate authentication, privilege escalation and lateral movement events
- A **Linux web server** hosting intentionally vulnerable applications such as **DVWA, OWASP Juice Shop and other vulnerable web applications** using Docker
- A **Kali Linux attacker machine** to perform realistic attack scenarios against both the web applications and the AD environment

For centralized monitoring, I am using **Wazuh as the SIEM platform**. Wazuh agents collect security logs from both Windows and Linux systems, including Windows Event Logs, Sysmon telemetry, auditd events, authentication logs and web server logs. These logs are analyzed, correlated and visualized through the Wazuh dashboard to detect suspicious activity and security incidents.

As an extension, I am integrating an **LLM-based analysis layer** to assist with SOC workflows. The LLM will analyze triggered alerts, summarize incidents, map them to **MITRE ATT&CK techniques**, assess severity and recommend response actions. Alert summaries and analysis will be delivered via **Discord or Telegram** for near real-time SOC-style notifications.

The goal of this project is to:

- Understand end-to-end log collection and detection pipelines
- Practice real SOC analyst workflows (detection, triage, investigation, response)
- Gain hands-on experience with Active Directory attacks, web attacks and SIEM alerting
- Explore how AI can assist, not replace, SOC analysts in alert triage and incident analysis

This project is focused on building **practical, interview-ready SOC experience** rather than theoretical knowledge and I plan to document detections, attack simulations, and incident reports as part of the learning process.

If anyone has **suggestions, feedback, or ideas for improvement**, please feel free to **DM me** - I would be very happy to hear your thoughts and learn from the community.

# Network Architecture

<img width="650" height="650" alt="architecture or workflow" src="https://github.com/user-attachments/assets/9e9ef990-f91f-42fd-84b2-65f6e5cdb534" />

Here I will be using VMware to host all the VMs

## 1. Linux Server 
1. depoly vulnerable web applications uing docker
2. setup port forwarding using nginx

## 2. Windows server
1. Install AD windows server
2. create demo users
3. create different roles for different users
4. You can host 1 or 2 web servers for practicing 

## 3.Attacker Machine 
1. This is the kali linux machine which you will be using to attack widnows and linux servers
2. Install and configure tools as per your need 

## 4.Wazuh Server
1. I will be using Ubuntu server to host Wazuh dashboard
2. once wazuh is configured in ubuntu, you can add agents on linux and windows server so that you can capture the logs from them.


# References
1. Vulnerable Web application Labs (Docker based) -[Labs](https://www.hackingarticles.in/web-application-pentest-lab-setup-using-docker/)
2. One of the fast and best video to setup wazuh -[Wazuh Installation](https://youtu.be/BTR2JSNgld4?si=2szztfHzWOBKiC_7)
   
