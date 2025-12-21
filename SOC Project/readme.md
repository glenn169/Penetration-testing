# Project Description
I am currently designing and building a **SOC Analyst home lab project** to gain hands-on experience with real-world detection, monitoring and incident analysis.

The lab simulates an enterprise-style environment consisting of:

- A **Windows Active Directory server** to generate authentication, privilege escalation and lateral movement events
- A **Linux web server** hosting intentionally vulnerable applications such as **DVWA, OWASP Juice Shop and other vulnerable web apps** using Docker
- A **Kali Linux attacker machine** to perform realistic attack scenarios against both the web applications and the AD environment

For centralized monitoring, I am using **Wazuh as the SIEM platform**. Wazuh agents collect security logs from both Windows and Linux systems, including Windows Event Logs, Sysmon telemetry, auditd events, authentication logs and web server logs. These logs are analyzed, correlated and visualized through the Wazuh dashboard to detect suspicious activity and security incidents.

As an extension, I am integrating an **LLM-based analysis layer** to assist with SOC workflows. The LLM will analyze triggered alerts, summarize incidents, map them to **MITRE ATT&CK techniques**, assess severity and recommend response actions. Alert summaries and analysis will be delivered via **Discord or Telegram** for near real-time SOC-style notifications.

The goal of this project is to:

- Understand end-to-end log collection and detection pipelines
- Practice real SOC analyst workflows (detection, triage, investigation, response)
- Gain hands-on experience with Active Directory attacks, web attacks and SIEM alerting
- Explore how AI can assist, not replace, SOC analysts in alert triage and incident analysis

This project is focused on building **practical, interview-ready SOC experience** rather than theoretical knowledge and I plan to document detections, attack simulations and incident reports as part of the learning process.

If anyone has **suggestions, feedback, or ideas for improvement**, please feel free to **DM me** - I would be very happy to hear your thoughts and learn from the community.


# References
1. Web application Penetraton testing labs - [labs](https://www.hackingarticles.in/web-application-pentest-lab-setup-using-docker/)
2. 
