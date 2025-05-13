# CyberSec-Writeups
Index

- [Linux Distro's](#linux-distro)
- [Packages](#packages)

## Base Station
Some people choose to have a dedicated machine, some choose to run everything in virtual machines. As I will not go over everyt part of the setup of a base station, I will reference other peoples work that I found helpfull.

### Virtual Machines
For the people running from a virtual machine, there are a couple of free options. 
- Vmware
  
- Virtual Box

- WSL (2.0)
Windows subsystem for Linux is also a handy way to start with a Cyber Securtiy workstation. This will allow you to run linux native commands from within the native Winodws Command line.
_[Helpfull Tutorial](https://medium.com/@gulfsteve/hacking-with-wsl2-ede3e649e08d)_

### Linux Distro

- Kali
  
    :arrow_down: [Choose your ISO - Download Page](https://www.kali.org/get-kali/#kali-platforms)
- Parrot OS

    :arrow_down: [Choose your ISO - Download Page](https://parrotsec.org/download/)
- Build your own
    You can start setting up your own station by just downloading a clean Ubuntu or Debian Distro.


## Cyber Kill Chain
According to Lockheed Martin, the Cyber Kill Chain
- Recon: The first step is discovering and collecting information on the system and the victim. The reconnaissance phase is the planning phase for the adversaries.

- Weaponization: This step refers to preparing a file with a malicious component, for example, to provide the attacker with remote access.

- Delivery: Delivery means delivering the “weaponized” file to the target via any feasible method, such as email or USB flash memory.

- Exploitation: When the user opens the malicious file, their system executes the malicious component.

- Installation: The previous step should install the malware on the target system.

- Command & Control (C2): The successful installation of the malware provides the attacker with a command and control ability over the target system.

- Actions on Objectives: After gaining control over one target system, the attacker has achieved their objectives. One example objective is Data Exfiltration (stealing target’s data).


### Recon
Recon, short for reconnaissance, refers to the step where the attacker tries to learn as much as possible about the target. Information such as the types of servers, operating system, IP addresses, names of users, and email addresses, can help the attack’s success.
During the recon stage, a lot of enumeration will be done. Enumeration is the process of gathering information on a target in order to find potential attack vectors and aid in exploitation. This process is essential for an attack to be successful, as wasting time with exploits that either don't work or can crash the system can be a waste of energy. Enumeration can be used to gather usernames, passwords, network information, hostnames, application data, services, or any other information that may be valuable to an attacker.

One part of Recon is Network reconnaissance. A network interface has 65.535 ports which are used for UDP User Datagram Protocol, a connectionless communication, and TCP conection oriented traffic. 
Ports 0 through 1023 are defined as well-known ports. Registered ports are from 1024 to 49151. The remainder of the ports from 49152 to 65535 can be used dynamically by applications. A brief description of these are as follows:
- Port 20 and 21: FTP data and FTP control, respectively
- Port 22: Remote login protocol secure shell (SSH)
- Port 23: Telnet, used for accessing system remotely but is not very secure
- Port 25: Simple Mail Transfer Protocol (SMTP) used by e-mail servers
- Port 53: DNS protocol
- Port 80: Used for accessing Web servers
- Port 110: The POP service or Post Office Protocol used by local e-mail clients to retrieve mail from servers
- Port 123: NTP to synchronize time with remote time servers
- Port 143: E-mail clients can use the Internet Message Access Protocol (IMAP) to retrieve mail from servers
- Port 443: This is the Hypertext Transfer Protocol (HTTP) Secure that combines the HTTP with a cryptographic protocol, which can be used for payment transactions and other secure transmission of data from Web pages.
- Port 631: The Internet Printing Protocol (IPP) used to print to printers located remotely on the network
- Port 3306: The standard port for MySQL
- Port 3389: The standard port for RDP connections.

<ins>Website reconnaissance</ins>
When visiting a website via a browser, when clicking or typing the URL a lot of stuff goes on in the back. GET and REDIRECT request happen in a split second before you see the website apear in your browser.
To summarise, when you request a website, your computer needs to know the server's IP address it needs to talk to; for this, it uses DNS. Your computer then talks to the web server using a special set of commands called the HTTP protocol; the webserver then returns HTML, JavaScript, CSS, Images, etc., which your browser then uses to correctly format and display the website to you.
A lot of other tools can be used to view a information of a website besides a browser. I am referring to tools like: curl, wget, nslookup, dig, who can give info on and many others. 
Some curl examples:
- curl http://10.10.44.138/cookie-test
- curl -H "Cookie: logged_in=true; admin=true" http://10.10.44.138/cookie-test


#### Recon Tools
<ins>NMAP</ins>
Nmap (“Network Mapper”) is an open source tool for network exploration and security auditing... more info on [the nmap website ](https://nmap.org)

<ins>Enum4linux</ins>
With this tool enumeration of SMB shares, on both Windows and Linux systems, is made straigh forward. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB. It's installed by default on Parrot and Kali, however if you need to install it, you can do so from the official github.
The syntax of Enum4Linux is nice and simple: "enum4linux [options] ip"

<ins>FFUF</ins>
Short for Fuzz Faster U Fool. A Go Language user enumeration tool for web applications

<ins>Smbmap / smbclient</ins>
These tools are used for enumerating smbshares on default ports 139 and 445 from a targeted devices.
    smbmap -H [TARGET-IP]

<ins>Wpscan</ins>


#### Exploitation Tools
<ins>Metasploit</ins>

Installing metasploit 
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && chmod 755 msfinstall && ./msfinstall

<ins>Smbclient</ins>

smbclient //[TARGET-IP/SHARE -U %

Once you are connected to the SMB server, you can find all the commands that smbclient knows by typing ‘help’. Some commenly used are ls, cd, chmod, mget

### Brute force tools
In brute force tools we use wordlist or dictionaries to guess what

<ins>Dirbuster</ins>
This tools is used for website file and folder enumeration which can be usefull to find hidden folders on a webserver. It uses a wordlist to go through GET request.


<ins>Gobuster</ins>

Example:
jack@baby-blue:~$ gobuster dir -u http://overwrite.uploadvulns.thm -w /home/jack/sjan/_wordlist/directory-list-2.3-medium.txt

<ins>Hydra</ins>
Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more. Hydra comes by default on both Parrot and Kali, however if you need it, you can find the GitHub here.
The syntax for the command we're going to use to find the passwords is this:
hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV [TARGET IP] ftp

<ins>John</ins>
Also known as John the ripper is a multi brute force tool




