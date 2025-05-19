# CyberSec-Writeups
This is an introductional writeup on how to setup a base station for a Capture the Flag game, with the introduction of some base tools.

## Base Station
Some people choose to have a dedicated machine, some choose to run everything in virtual machines. As I will not go over every part of the setup of a base station, I will reference other peoples work that I found helpfull.

### Linux Distro's

- <ins>Kali Linux</ins>
  
    :arrow_down: [Choose your ISO - Download Page](https://www.kali.org/get-kali/#kali-platforms)
- <ins>Parrot OS</ins>

    :arrow_down: [Choose your ISO - Download Page](https://parrotsec.org/download/)
- <ins>Build your own</ins>

    You can start setting up your own base station by just downloading a clean Ubuntu or Debian Distro and add the described tools.

### Virtual Machines
You can run your base station as a virtual machine on Windows, there are a couple of free options. All come with their own drawbacks.

- <ins>VMWare</ins>

    [By NAKIVO Team](https://www.nakivo.com/blog/install-kali-linux-vmware/)

- <ins>Virtual Box</ins>

    [By Marko Aleksic](https://phoenixnap.com/kb/how-to-install-kali-linux-on-virtualbox)

- <ins>WSL (2.0)</ins>

    Windows subsystem for Linux is also a handy way to start with a Cyber Securtiy workstation. This will allow you to run linux native commands from within the native Winodws Command line.

    [For The Kali linux website](https://www.kali.org/docs/wsl/wsl-preparations/)
  or
    [By Steve McIlwain](https://medium.com/@gulfsteve/hacking-with-wsl2-ede3e649e08d)

## Cyber Kill Chain
Developed by Lockheed Martin, the Cyber Kill Chain framework is part of the Intelligence Driven Defense model for identification and prevention of cyber intrusions activity. The model identifies what the adversaries must complete in order to achieve their objective.The seven steps of the Cyber Kill Chain enhance visibility into an attack and enrich an analyst’s understanding of an adversary’s tactics, techniques and procedures.

According to Lockheed Martin, the Cyber Kill Chain looks like this:

- <ins>Recon:</ins> The first step is discovering and collecting information on the system and the victim. The reconnaissance phase is the planning phase for the adversaries.

- <ins>Weaponization:</ins> This step refers to preparing a file with a malicious component, for example, to provide the attacker with remote access.

- <ins>Delivery:</ins> Delivery means delivering the “weaponized” file to the target via any feasible method, such as email or USB flash memory.

- <ins>Exploitation:</ins> When the user opens the malicious file, their system executes the malicious component.

- <ins>Installation:</ins> The previous step should install the malware on the target system.

- <ins>Command & Control (C2):</ins> The successful installation of the malware provides the attacker with a command and control ability over the target system.

- <ins>Actions on Objectives:</ins> After gaining control over one target system, the attacker has achieved their objectives. One example objective is Data Exfiltration (stealing target’s data).


### Recon
Recon, short for reconnaissance, refers to the step where the attacker tries to learn as much as possible about the target. Information such as the types of servers, operating system, IP addresses, names of users, and email addresses, can help the attack’s success. During the recon stage, a lot of enumeration will be done.

Enumeration is the process of gathering information on a target in order to find potential attack vectors and aid in exploitation. This process is essential for an attack to be successful, as wasting time with exploits that either don't work or can crash the system can be a waste of energy. Enumeration can be used to gather usernames, passwords, network information, hostnames, application data, services, or any other information that may be valuable to an attacker.

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

An example of a wget command, which is nothing more then a command line URI request, that will save the page requested.

```
    wget google.com

    Prepended http:// to 'google.com'
    --YYYY-MM-DD HH:MM:SS--  http://google.com/
    Resolving google.com (google.com)... 172.217.23.206
    Connecting to google.com (google.com)|172.217.23.206|:80... connected.
    HTTP request sent, awaiting response... 301 Moved Permanently
    Location: http://www.google.com/ [following]
    --YYYY-MM-DD HH:MM:SS--  http://www.google.com/
    Resolving www.google.com (www.google.com)... 142.251.36.36
    Connecting to www.google.com (www.google.com)|142.251.36.36|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: unspecified [text/html]
    Saving to: ‘index.html’

    index.html                   [ <=>                             ]  17.65K  --.-KB/s    in 0.005s  

    YYYY-MM-DD HH:MM:SS (3.53 MB/s) - ‘index.html’ saved [18072]

```

#### Recon Tools
<ins>NMAP</ins>

Nmap the “Network Mapper”, is an open source tool for network exploration and security auditing... more info on [the nmap website ](https://nmap.org)

```
    nmap -h

```

<ins>Enum4linux</ins>

With this tool enumeration of SMB shares, on both Windows and Linux systems, is made straigh forward. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB. It's installed by default on Parrot and Kali, however if you need to install it, you can do so from the official github.
The syntax of Enum4Linux is nice and simple: "enum4linux [options] ip"

<ins>FFUF</ins>

Short for Fuzz Faster U Fool. A Go Language user enumeration tool for web applications.

<ins>smbmap / smbclient</ins>

These tools are used for enumerating smbshares on default ports 139 and 445 from a targeted devices.

```
    smbmap -H [TARGET-IP]

    smbclient //[TARGET-IP/SHARE] -U %

```

Once you are connected to the SMB server, you can find all the commands that smbclient knows by typing ‘help’. Some commenly used are ls, cd, chmod, mget

<ins>wpscan</ins>

This tool is used to enumerate Wordpress websites.

```
    wpscan -h

```

### Weaponization
Weaponization, the second phase of the cyber kill chain following reconnaissance, involves creating malicious payloads designed to exploit identified vulnerabilities. Upon successful delivery and execution, these weaponized payloads deploy malware, leading to system compromise. Knowing file extentions and how files are recognized by anti virus software, this will help you in how to use file obfuscation to your benefits.

An example would be a Malicious Macro

A macro is an automated input sequence that imitates keystrokes or mouse actions. A macro is typically used to replace a repetitive series of keyboard and mouse actions and are common in spreadsheet and word processing applications like MS Excel and MS Word. A malicious macro, or macro virus is a computer virus that replaces a macro. When these actions and commands are replaced by a reverse shell, this can cause significant harm to a computer.

This reverse shell can simply be described as a piece of code or program which can be used to gain command execution on a device. A reverse shell is a type of shell in which the target machine communicates back to the attacking machine. Commonly a script is run on the target machine which tries to call back to the attachers machine. On the attakers machine a listening tool like netcat, is waiting for the incomming connection.

#### Weaponization Tools
With the help of tools like Metasploit Malicious Macros can be compiled.

### Delivery
The art of getting your weaponized file to your Target. this can happen by mail or through file sharing.

For example, if you need to setup a quick webserver you can use python, or if php is available, to do this.
```
    python3 -m http.server

    php -S 0.0.0.0:8000

```

### Exploitation
In the Exploitation phase, the malicious code is executed within the victim’s system.

#### Exploitation Tools
<ins>Metasploit</ins>

Installing metasploit:
```
    curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && chmod 755 msfinstall && ./msfinstall

```

<ins>netcat</ins>

netcat or nc is a network multipurpose network debugging tool, but mostly associated with exploitation, as it is capable to be used as a listening device for reverse connections.

```
    nc -vnlp 6969

```

### Brute force tools
These tools automate the work off enumaration by using a predefind set of characters or dictionary to go to every possible combination, feeding the tool to guess a possible success.
As this enumeration is done through CPU or GPU cycles and happens, depending on the machine, in mear seconds of what a human could input. These tools can be verry noisy, so they should be used carefully.

<ins>wordlist</ins>
To use dictionary attacks, a list of words is needed. One of the most famous used for passwords is probably RockYou, but there are many more for specific use cases. Some linux distro's come with their own wordlists and can be found in ``` /usr/share/wordlists ```

<ins>Dirbuster / Gobuster</ins>

These tools is used for website file and folder enumeration which can be usefull to find hidden folders on a webserver. It uses a wordlist to go through GET request.

Example:
```
    gobuster dir -u http://[TARGET_URL] -w wordlist/directory-list-2.3-medium.txt

```

<ins>Hydra</ins>

Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more. Hydra comes by default on both Parrot and Kali, however if you need it, you can find the GitHub here.
The syntax for the command we're going to use to find the passwords is this:

```
    hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV [TARGET IP] ftp

```

<ins>John</ins>

Also known as John the ripper is a multi brute force tool


### Command & Control and Post Exploitation
Once you have controll of a system and have an active shell, you should take a couple of steps back. Will persistance be needed or is it a quick in-out mission?
First lest go over root access before going of persistance.

If a linux shell is provide of for instance a webserver, you need to find out what permissions is granted to the user you are currently logged in with.


```
    whoami

```

Next you will need to find out if permissions are granted to elevate your current permissions.

[More to come]
...
