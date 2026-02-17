# SOC Notes

## Alert Funneling 
First, L1 analysts receive the alerts in a SIEM, EDR, or a ticket management platform. Most of the alerts are closed as False Positives or are handled on L1 level, but complex and threatening ones are sent to L2 that remediate most breaches. And to send the alerts further, you need to learn three new terms: reporting, escalation, and communication.

<img width="1175" height="539" alt="image" src="https://github.com/user-attachments/assets/0db445e3-a3fd-4d91-8ad5-37ea827107a4" />


### Alert Reporting
Before closing or passing the alert to L2, you might have to report it. Depending on team standards and alert severity, instead of a short alert comment, you can be required to document your investigation in detail, ensuring all relevant evidence is included. This is especially important for True Positives, which require escalation.

### Alert Escalation
If the True Positive alert requires additional actions or deeper investigation, escalate it to the L2 analyst for further review following the agreed procedures. That's where your alert report comes in handy since L2 will use it to get the initial context and spend less on the analysis from scratch.

### Communication
You may also need to communicate with other departments during or after the analysis. For example, ask the IT team if they confirm granting administrative privileges to some users or contact HR to get more information about the newly hired employee.

## Report Format
An example of good, structured report following the 5Ws approach

Imagine yourself as an L2 analyst, a DFIR team member, or an IT professional who needs to understand the alert. What would you want to see in the report? We recommend you follow the Five Ws approach and include at least these items in the report:

Who: Which user logs in, runs the command, or downloads the file
What: What exact action or event sequence was performed
When: When exactly did the suspicious activity start and ended
Where: Which device, IP, or website was involved in the alert
Why: The most important W, the reasoning for your final verdict

## Escalation Guide
After you have made a verdict and written your alert report, you must choose whether to escalate the alert to L2. Again, the answer may differ from team to team, but the following recommendations would generally fit most SOC teams. You should escalate the alerts if:

1. The alert is an indicator of a major cyberattack requiring deeper investigation or DFIR
2. Remediation actions like malware removal, host isolation, or password reset are required
3. Communication with customers, partners, management, or law enforcement agencies is required
4. You just do not fully understand
5. the alert and need some help from more senior analysts

### Escalation Steps
To escalate the alert, in most cases, all you have to do is to reassign the alert to the L2 on shift and ping them in corporate chat or in person. In some teams though, you may be required to create a formal written escalation request with dozens of required fields.
<img width="1732" height="407" alt="image" src="https://tryhackme-images.s3.amazonaws.com/user-uploads/678ecc92c80aa206339f0f23/room-content/678ecc92c80aa206339f0f23-1743520297119.svg" />

No matter what the agreements are, L2 will eventually receive the ticket from you, read your report, and contact you in case of any questions. Once everything is clear, the L2 analyst will typically research the alert details further, validate if the alert is indeed a True Positive, communicate with other departments if needed, and, for major incidents, start a formal Incident Response process.
### Requesting L2 Support
It is generally fine for L1 to request senior support if something is unclear. Especially in your first months, it's always better to discuss the alert and clarify SOC procedures than to blindly close the alert you don't understand yourself. The procedures for requesting support may differ, but the flow generally looks like this:

<img width="1732" height="407" alt="image" src="https://github.com/user-attachments/assets/fd9ba790-3b59-4d5e-93ca-227b42efbf5d" />

### SOC Dashboard Escalation
1. Move the alert to In Progress status and do the analysis
2. Write an alert report and set your verdict, such as True Positive
3. If escalation is required, assign the alert to your L2 on shift
4. L2 will receive a notification and start from your alert report


### Communication Cases
**You need to escalate an urgent, critical alert, but L2 is unavailable and does not respond for 30 minutes.**
Ensure you know where to find emergency contacts. First, try to call L2, then L3, and finally your manager.

**The alert about Slack/Teams account compromise requires you to validate the login with the affected user.**
Do not contact the user through the breached chat - use alternative contact methods like a phone call.

**You receive an overwhelming number of alerts during a short period of time, some of which are critical.**
Prioritise the alerts according to the workflow, but inform your L2 on shift about the situation.

**After a few days, you realise that you misclassified the alert and likely missed a malicious action.**
Immediately reach out to your L2 explaining your concerns. Threat actors can be silent for weeks before impact.

**You can not complete the alert triage since the SIEM logs are not parsed correctly or are not searchable.**
Do not skip the alert - investigate what you can and report the issue to your L2 on shift or SOC engineer.

<img width="1445" height="432" alt="image" src="https://github.com/user-attachments/assets/a6f01ed7-113e-4452-be95-82f6d64de35b" />
