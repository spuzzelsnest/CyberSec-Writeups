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
Nmap is a tool used for network port enumeration

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
Smbclient
smbclient //[TARGET-IP/SHARE -U %
Once you are connected to the SMB server, you can find all the commands that smbclient knows by typing ‘help’. Some commenly used are ls, cd, chmod, mget
Brute force tools
In brute force tools we use wordlist or dictionaries to guess what
Dirbuster
This tools is used for website file and folder enumeration which can be usefull to find hidden folders on a webserver. It uses a wordlist to go through GET request.
Gobuster

Example:
jack@baby-blue:~$ gobuster dir -u http://overwrite.uploadvulns.thm -w /home/jack/sjan/_wordlist/directory-list-2.3-medium.txt
Hydra
Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more. Hydra comes by default on both Parrot and Kali, however if you need it, you can find the GitHub here.
The syntax for the command we're going to use to find the passwords is this:
hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV [TARGET IP] ftp
John
Also known as John the ripper is a multi brute force tool



## Packages
### Recon
- nmap
  Nmap (“Network Mapper”) is an open source tool for network exploration and security auditing... more info on [the nmap website ](https://nmap.org)






TCP 3 Way Handshake
Three-Way HandShake or a TCP 3-way handshake is a process which is used in a TCP/IP network to make a connection between the server and client. It is a three-step process that requires both the client and server to exchange synchronization and acknowledgment packets before the real data communication process starts.
Three-way handshake process is designed in such a way that both ends help you to initiate, negotiate, and separate TCP socket connections at the same time. It allows you to transfer multiple TCP socket connections in both directions at the same time.
Message	Description
SYN	Used to initiate and establish a connection. It also helps you to synchronize sequence numbers between devices.
SYN-ACK	SYN message from local device and ACK of the earlier packet.
ACK	Helps to confirm to the other side that it has received the SYN.
FIN	Used to terminate a connection.
 
•	Step 1: The client establishes a connection with a server. It sends a segment with SYN and informs the server about the client should start communication, and with what should be its sequence number.
•	Step 2: The server responds to the client request with SYN-ACK signal set. ACK helps you to signify the response of segment that is received and SYN signifies what sequence number it should able to start with the segments.
•	Step 3: The client acknowledges the response of the Server, and they both create a stable connection will begin the actual data transfer process.
Website reconnaissance
When visiting a website via a browser, when clicking or typing the URL a lot of stuff goes on in the back. GET and REDIRECT request happen in a split second before you see the website apear in your browser.
To summarise, when you request a website, your computer needs to know the server's IP address it needs to talk to; for this, it uses DNS. Your computer then talks to the web server using a special set of commands called the HTTP protocol; the webserver then returns HTML, JavaScript, CSS, Images, etc., which your browser then uses to correctly format and display the website to you.
A lot of other tools can be used to view a information of a website besides a browser. I am referring to tools like: curl, wget, nslookup, dig, who can give info on and many others. 
Some curl examples:
•	curl http://10.10.44.138/cookie-test
•	curl -H "Cookie: logged_in=true; admin=true" http://10.10.44.138/cookie-test
Data Integrity Failures
Let's think of how web applications maintain sessions. Usually, when a user logs into an application, they will be assigned some sort of session token that will need to be saved on the browser for as long as the session lasts. This token will be repeated on each subsequent request so that the web application knows who we are. These session tokens can come in many forms but are usually assigned via cookies. Cookies are key-value pairs that a web application will store on the user's browser and that will be automatically repeated on each request to the website that issued them.
IDOR
IDOR stands for Insecure Direct Object Reference and is a type of access control vulnerability.
This type of vulnerability can occur when a web server receives user-supplied input to retrieve objects (files, data, documents), too much trust has been placed on the input data, and it is not validated on the server-side to confirm the requested object belongs to the user requesting it.

Cookies
For example, if you were creating a webmail application, you could assign a cookie to each user after logging in that contains their username. In subsequent requests, your browser would always send your username in the cookie so that your web application knows what user is connecting. This would be a terrible idea security-wise because, as we mentioned, cookies are stored on the user's browser, so if the user tampers with the cookie and changes the username, they could potentially impersonate someone else and read their emails! This application would suffer from a data integrity failure, as it trusts data that an attacker can tamper with.

One solution to this is to use some integrity mechanism to guarantee that the cookie hasn't been altered by the user. To avoid re-inventing the wheel, we could use some token implementations that allow you to do this and deal with all of the cryptography to provide proof of integrity without you having to bother with it. One such implementation is JSON Web Tokens (JWT).

JWTs are very simple tokens that allow you to store key-value pairs on a token that provides integrity as part of the token. The idea is that you can generate tokens that you can give your users with the certainty that they won't be able to alter the key-value pairs and pass the integrity check. The structure of a JWT token is formed of 3 parts:
 
JSON Web Tokens
The header contains metadata indicating this is a JWT, and the signing algorithm in use is HS256. The payload contains the key-value pairs with the data that the web application wants the client to store. The signature is similar to a hash, taken to verify the payload's integrity. If you change the payload, the web application can verify that the signature won't match the payload and know that you tampered with the JWT. Unlike a simple hash, this signature involves the use of a secret key held by the server only, which means that if you change the payload, you won't be able to generate the matching signature unless you know the secret key.

Notice that each of the 3 parts of the token is simply plaintext encoded with base64. You can use this online tool to encode/decode base64. Try decoding the header and payload of the following token:

eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNjY1MDc2ODM2fQ.C8Z3gJ7wPgVLvEUonaieJWBJBYt5xOph2CpIhlxqdUw

Note: The signature contains binary data, so even if you decode it, you won't be able to make much sense of it anyways.

JWT and the None Algorithm
A data integrity failure vulnerability was present on some libraries implementing JWTs a while ago. As we have seen, JWT implements a signature to validate the integrity of the payload data. The vulnerable libraries allowed attackers to bypass the signature validation by changing the two following things in a JWT:

1.	Modify the header section of the token so that the alg header would contain the value none.
2.	Remove the signature part.
Taking the JWT from before as an example, if we wanted to change the payload so that the username becomes "admin" and no signature check is done, we would have to decode the header and payload, modify them as needed, and encode them back. Notice how we removed the signature part but kept the dot at the end.
 

It sounds pretty simple! Let's walk through the process an attacker would have to follow in an example scenario.
File Share enumeration
Fileshare protocols like FTP or SMB are commenly exploited for unauthorised access to directorys that should not be access by non privilegde users.
SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. Servers make file systems and other resources (printers, named pipes, APIs) available to clients on the network. Client computers may have their own hard disks, but they also want access to the shared file systems and printers on the servers.
The SMB protocol is known as a response-request protocol, meaning that it transmits multiple messages between the client and server to establish a connection. Clients connect to servers using TCP/IP (actually NetBIOS over TCP/IP as specified in RFC1001 and RFC1002), NetBEUI or IPX/SPX.
FTP – File Transfer Protocol – is a protocol used to allow remote transfer of files over a network. It uses a client-server model to do this, and- as we'll come on to later- relays commands and data in a very efficient way.

A Real life Example: The security bootcamp
https://en.wikipedia.org/wiki/List_of_file_signatures
Say we are given a website to out we would follow the next steps.
The first we need to take a look at the website as a whole. We would look for indicators of what languages and frameworks the web application might have been built with. A good start to enumerate a website manually would be by making a request to the website and intercepting the response with Burpsuite. Headers such as server or x-powered-by can be used to gain information about the server. We would also be looking for vectors of attack, like, for example, an upload page.
Having found an upload page, we would then aim to inspect it further. Looking at the source code for client-side scripts to determine if there are any client-side filters to bypass would be a good thing to start with, as this is completely in our control.
We would then attempt a completely innocent file upload. From here we would look to see how our file is accessed. In other words, can we access it directly in an uploads folder? Is it embedded in a page somewhere? What's the naming scheme of the website? This is where tools such as Gobuster might come in if the location is not immediately obvious. This step is extremely important as it not only improves our knowledge of the virtual landscape we're attacking, it also gives us a baseline "accepted" file which we can base further testing on.
An important Gobuster switch here is the -x switch, which can be used to look for files with specific extensions. For example, if you added -x php,txt,html to your Gobuster command, the tool would append .php, .txt, and .html to each word in the selected wordlist, one at a time. This can be very useful if you've managed to upload a payload and the server is changing the name of uploaded files.
Having ascertained how and where our uploaded files can be accessed, we would then attempt a malicious file upload, bypassing any client-side filters we found in step two. We would expect our upload to be stopped by a server side filter, but the error message that it gives us can be extremely useful in determining our next steps.
Injection
Injection vulnerabilities are quite dangerous to a company as they can potentially cause downtime and/or loss of data. Identifying injection points within a web application is usually quite simple, as most of them will return an error. There are many types of injection attacks, some of them are:

SQL Injection
SQL Injection is when an attacker enters a malicious or malformed query to either retrieve or tamper data from a database. And in some cases, log into accounts.
Examples:
•	The character ' will close the brackets in the SQL query
•	'OR' in a SQL statement will return true if either side of it is true. As 1=1 is always true, the whole statement is true. Thus it will tell the server that the email is valid, and log us into user id 0, which happens to be the administrator account.
•	The -- character is used in SQL to comment out data, any restrictions on the login will no longer work as they are interpreted as a comment. This is like the # and // comment in python and javascript respectively.
Command Injection
Command Injection is when web applications take input or user-controlled data and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests. 
Email Injection
Email injection is a security vulnerability that allows malicious users to send email messages without prior authorization by the email server. These occur when the attacker adds extra data to fields, which are not interpreted by the server correctly.
Exploitation
After the Recon, some more investigation will lead to vulnerabilities which are described in CVE’s. A CVE, short for Common Vulnerabilities and Exposures, is a list of publicly disclosed computer security flaws. When someone refers to a CVE, they usually mean the CVE ID number assigned to a security flaw.

Reverse Shell
A Shell can simply be described as a piece of code or program which can be used to gain code or command execution on a device. A reverse shell is a type of shell in which the target machine communicates back to the attacking machine. Commonly a script is run on the target machine which tries to call back to the attachers machine. On the attakers machine a listening tool is waiting for the incomming connection.
Priviledge escalation 
When Broken Access Control exploits or bugs are found, it will be categorized into one of two types:
Horizontal Privilege Escalation: Occurs when a user can perform an action or access data of another user with the same level of permissions.
Vertical Privilege Escalation: Occurs when a user can perform an action or access data of another user with a higher level of permissions.
SUID
So, what are files with the SUID bit set? Essentially, this means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!


Why crack on GPUs?
Graphics cards have thousands of cores. Although they can’t do the same sort of work that a CPU can, they are very good at some of the maths involved in hash functions. This means you can use a graphics card to crack most hash types much more quickly. Some hashing algorithms, notably bcrypt, are designed so that hashing on a GPU is about the same speed as hashing on a CPU which helps them resist cracking.
Cracking on VMs?
It’s worth mentioning that virtual machines normally don’t have access to the host's graphics card(s) (You can set this up, but it’s a lot of work). If you want to run hashcat, it’s best to run it on your host (Windows builds are available on the website, run it from powershell). You can get Hashcat working with OpenCL in a VM, but the speeds will likely be much worse than cracking on your host. John the ripper uses CPU by default and as such, works in a VM out of the box although you may get better speeds running it on the host OS as it will have more threads and no overhead from running in a VM.


Scripting your own tools
File Obfuscation

Example 
    echo “ <?php echo system($_GET["cmd"]);?>” > myCutePicture.jpg
Changing the header of a file with hexeditor to make the file look like a other file. List of Extensions headers can be found her.
Example 


Python
Python is whiledly used as a scripting language for creating or automating a lot of the processes involed with hacking. It has a lot of the libraries available for download.
Docker
On the internet there are a lot of docker 
 
Prep of a Linux system
Windows subsystem for Linux
Windows subsystem for linux is  one of the ways of running linux on windows. Here is a short description on how it is installed. By default WSL does not support bridged neworks witch breaks reverse shell explotation. Once WSL is setup I will go over some options on how to bridge the network connections

Open an elevated powershell and run the following commands to install WSL.
wsl --install
Enable wsl
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
check current version of WSL and enable version 2 if necessary
wsl --list --verbose
wsl --set-version Debian 2
wsl --set-default-version 2
install linux distro, you can easily do so by going through the Microsoft Store. ( kali is not needed – Debian or Ubuntu also do the trick)
install the linux kernel update for windows
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
Network Settings
By default WSL does not create a bridge between the physical and virtual network adaptor. To do this, you can open network connections in the settings menu and open the window where you can view and change the network adaptors. When you select your prefered network adaptor ( wired or wireless) and the vEthernet (WSL) network connection.
 
To be sure you have the right interfaces selected in the Network Bridge view the properties.
 

Run the following script to make your machine have a virtual IP in your own network range.

To test everything out, try to ping google.
If sudo is needed to run the ping command you can solve this by changing the permissions on the executable /bin/ping: sudo chmod u+s /bin/ping

Install linux packages
System packages
The following system packages will be needed to run most of the tools that are available on linux.
sudo apt install -y curl file wget tmux git arp-scan net-tools nfs-common tcpdump telnet traceroute smbclient basez mcrypt python3-pip python3-setuptools hashid python3-impacket python3-ldap3 openvpn default-mysql-client build-essential libssl-dev yasm libgmp-dev libpcap-dev libnss3-dev libkrb5-dev pkg-config firefox-esr openjdk-17-jdk ruby-full hexedit bash-completion postgresql gcc-mingw-w64-x86-64 g++-mingw-w64-x86-64 make
Hacking packages
These are most commonly used and can be easily installed through Aptitude.
sudo apt install -y hydra nmap ftp smbmap sqlmap ncat dirb gobuster samdump2
Installing John the Ripper
    wget https://github.com/openwall/john/archive/bleeding-jumbo.zip
unzip bleeding-jumbo.zip
rm bleeding-jumbo.zip
sudo mv john-bleeding-jumbo /opt/john
cd /opt/john/src/
./configure && make
The application gets compiled in the folder run 1 level up
    cd ../run
Now we can benchmark John by running the following command ( this will take a while + 30min )
john –test
Install hashcat
To install hashcat for windows follow the following steps 
git clone https://github.com/hashcat/hashcat
git clone https://github.com/win-iconv/win-iconv
cd win-iconv/
patch < ../hashcat/tools/win-iconv-64.diff
sudo make install
cd ../hashcat/
sudo make win
If you install it on WSL, make sure you have OpenGL or Cuda installed.

Install Burp Suite Community
Download the installer from portswigger.net. 
You can choose to use the windows installer, or you can use the Linux 64x bit version.
Move the installer to the Linux environment
    mv /mnt/c/tmp/burpsuite_community_linux_v2023_10_3_6.sh ~/Downloads
start the installer 
    sudo ./burpsuite_community_linux_v2023_10_3_6.sh
Launch Burp Suite Community from Windows Subsystem for Linux
    java -jar -Xmx4g burpsuite_community.jar
Installing Metasploit

curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall
chmod 755 msfinstall
sudo ./msfinstall 

If you want to use the database, make sure you run a msfdb init before running the software itself 
If  you love the banners, you can start metasploit by running 
Msfconsole 
If you want to run in it without viewing the banner:

Msfconsole -q
Using TMUX
Tmux is used to have multiple screens running on a single command promt window, its like having tabs in a browser or screens on a desktop. This allows to run multiple longer lasting commands at the same machine at the same time.
Installing enum4linux-ng 

Installing Go Language

Install FFUF
go install github.com/ffuf/ffuf@latest
