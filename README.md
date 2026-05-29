# HACK THE BOX - Kush7711

Description

MonitorsFour is an Easy-rated Windows machine from Hack The Box focused on web exploitation, credential discovery, Docker abuse, and privilege escalation techniques. The machine demonstrates how multiple small misconfigurations can be chained together to gain full system compromise.

The attack path begins with reconnaissance and web enumeration, leading to the discovery of vulnerable services and exposed credentials. Further exploitation provides access to containerized environments, eventually resulting in Docker escape and SYSTEM-level privileges on the host machine.

This lab is useful for learning:

Web Enumeration
Virtual Host Discovery
Credential Harvesting
Docker Abuse
Privilege Escalation
Windows Post Exploitation
Skills Learned
Nmap Enumeration
HTTP Service Analysis
Host File Configuration
Credential Discovery
Container Enumeration
Docker Privilege Escalation
WinRM Access
SYSTEM Privilege Escalation
Enumeration

Initial port scan using Nmap:

nmap -sS -sV -Pn <IP>

Discovered Services:

HTTP - Port 80
WinRM - Port 5985

The website initially failed to load because the hostname monitorsfour.htb was unresolved. Adding the target IP and domain to /etc/hosts resolved the issue.

echo "<IP> monitorsfour.htb" | sudo tee -a /etc/hosts
Disclaimer

This write-up is intended for educational purposes only. All activities were performed in a legal Hack The Box lab environment.
