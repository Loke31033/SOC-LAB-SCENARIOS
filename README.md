🔐 SSH Brute Force Detection using Splunk (SOC Project)

📌 Overview

This project demonstrates a real-world SOC (Security Operations Center) workflow by simulating and detecting an SSH brute force attack using Splunk SIEM.

The workflow includes:
	•	Attack Simulation
	•	Log Ingestion
	•	Detection Engineering
	•	Investigation
	•	Incident Response
	•	Reporting

⸻

🧱 Architecture

Kali Linux (Attacker)
        ↓
Ubuntu Server (Victim + Splunk Forwarder)
        ↓
Splunk Enterprise (SIEM)


⸻

⚙️ Technologies Used
	•	Splunk Enterprise (SIEM)
	•	Splunk Universal Forwarder
	•	Kali Linux (Attacker Machine)
	•	Ubuntu Server (Victim Machine)
	•	Hydra (Brute Force Tool)

⸻

🚀 Attack Simulation

The attack was simulated using Hydra to perform SSH brute force:

hydra -l loki31 -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.20

📸 Screenshot:


⸻

🔍 Detection in Splunk

SPL Query Used

index=* source="/var/log/auth.log" "Failed password"
| rex "from (?<src_ip>\d{1,3}(?:\.\d{1,3}){3})"
| rex "Failed password for (?:invalid user )?(?<user>[^\s]+)"
| stats count as failed_attempts by src_ip, user
| where failed_attempts >= 20
| sort -failed_attempts

📸 Screenshot:




🧠 Investigation

Timeline Analysis

index=* source="/var/log/auth.log" src_ip=192.168.56.10
| sort _time

Key Findings
	•	Multiple failed login attempts detected
	•	Attack originated from a single IP
	•	Possible successful login after brute force

📸 Screenshots

⚡ Incident Response

Actions Taken

sudo ufw deny from 192.168.56.10
sudo passwd -l loki31

Outcome
	•	Attacker IP blocked
	•	User account secured

📸 Screenshot:




📊 Dashboard

A SOC dashboard was created in Splunk to visualize attack patterns.

Panels:
	•	Failed login attempts over time
	•	Top attacker IPs
	•	Successful vs Failed logins

📸 Screenshot:




📄 Incident Report Summary
	•	Attack Type: SSH Brute Force
	•	Source IP: 192.168.56.10
	•	Target User: loki31
	•	Severity: High
	•	Impact: Potential unauthorized access


🎯 Skills Demonstrated
	•	SIEM (Splunk) Configuration
	•	Log Analysis & Parsing (SPL)
	•	Threat Detection & Alerting
	•	Incident Investigation
	•	Incident Response
	•	SOC Workflow Implementation

🏁 Conclusion

This project simulates a real SOC environment, demonstrating the ability to detect and respond to security incidents using Sp
📎 How to Run
	1.	Set up 3 VMs (Kali, Ubuntu Victim, Splunk SIEM)
	2.	Install Splunk Enterprise & Forwarder
	3.	Configure log forwarding
	4.	Run Hydra attack
	5.	Execute SPL queries
	6.	Analyze and respond

👤 Author

Lokeshwar V
🔗 LinkedIn: https://www.linkedin.com/in/lokeshwar-v-011b20289
💻 GitHub: https://github.com/Loke31033
