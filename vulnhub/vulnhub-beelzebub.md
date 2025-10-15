# Security Assessment: Vulnhub Beelzebub Walkthrough

![alessio-zaccaria-po1ffK4lLMw-unsplash](https://github.com/user-attachments/assets/cb5c9b8d-2ff9-484e-a10c-5f61f028e8af)


## Introduction
Beelzebub is a vulnerable machine found on Vulnhub. A vulnerable machine is a virtual machine deliberately designed to be vulnerable, to teach hacking and cybersecurity techniques. For this vulnerable machine, there are two goals: to get the user flag and to get the root flag.

This post will be a walkthrough of the tools and techniques used to complete this vulnerable machine. 

## Methodology

The steps I took for this machine were as follows:
Reconnaissance: Scanning for open ports, looking for vulnerabilities, trying to find holes in the system that could be exploited. This was mostly web scanning and enumeration, since one of the only open ports found was for an open port.
Discovery: This step was all about finding secrets, a username and password for this particular engagement. 
Vulnerability Assessment: Once vulnerabilities were found, research into the vulnerability was done to see what it could do. 
Exploitation: Once the vulnerability was confirmed, exploitation of the vulnerability was done to escalate privileges.

Tools used: nmap, dirb, gobuster, burp suite, wpscan

Nmap was used for scanning of open ports and services.

Dirb and gobuster were used to scan for hidden web directories.

Burp Suite was used to analyze http requests and responses.

Wpscan was used to scan for usernames on a wordpress website.

## Walkthrough

The first step was to do a ping scan of my network to find the target’s IP address. A ping scan is a form of host discovery where active IPs are identified by ICMP echo requests, and a port scan is not done after host discovery. 

<img width="694" height="261" alt="nmap_host_discovery" src="https://github.com/user-attachments/assets/cad6493a-103e-49fd-8ab8-4a030b97a6c8" />

After finding the IP address of 192.168.104.14, I did a SYN scan of the target. The SYN scan sends the first step of the TCP three-way handshake,  a packet with the SYN flag set, to a target but does not complete the handshake. If a target replies with a SYN ACK, then the port is open. 

<img width="687" height="216" alt="nmap_first_scan" src="https://github.com/user-attachments/assets/53e95a02-23ed-41f4-b4da-405f068cc92c" />

Ports 22 for ssh, and port 80 for http were open on the target. I next went to the web page to see if there was anything interesting there. 

<img width="988" height="814" alt="webpage" src="https://github.com/user-attachments/assets/86e334bc-5c38-42f9-a9c0-bc6396e430dc" />

The web page however was just a default Apache2 web page, so nothing interesting. Viewing the page source also didn’t have anything. 

The next step was to search for hidden directories and files. To do this, I first used the dirb tool.

<img width="716" height="790" alt="dirb_scan" src="https://github.com/user-attachments/assets/1cb99960-1a78-4e08-b534-a758968d1eef" />

Some of these directories looked interesting, but a lot of them were forbidden to enter. For example, the below javascript directory:

<img width="990" height="816" alt="javascript_directory" src="https://github.com/user-attachments/assets/48a41745-8566-41d2-9c8f-e9a21bf7c19f" />

And a php info directory:

<img width="945" height="696" alt="php_info" src="https://github.com/user-attachments/assets/ff8f7a9f-0c91-4c05-b945-9130e09c1163" />

There was however an index.php page that at first glance had the not found page, but it looked slightly different from the others.

<img width="725" height="283" alt="index_php" src="https://github.com/user-attachments/assets/b91dcd7a-0ce3-4a19-8cf9-5a21c57fcddb" />

Viewing the page source, there was an interesting comment in the html that said “beelzebub” hacked into the web page. 

<img width="676" height="272" alt="index_php_pagesource" src="https://github.com/user-attachments/assets/2370da38-11c9-4639-a1e5-94173b6da9f6" />

From the above hint, especially about md5 hashing, there might be something else  hidden on the web server. Taking a hash of “beelzebub” and using that as a directory in a web enumeration scan, I was able to find more directories. 

<img width="706" height="575" alt="gobuster_scan" src="https://github.com/user-attachments/assets/c126067b-b40c-418e-bc69-ed9bbbaa2413" />

I found even further directories scanning through the /wp-content directory. 

<img width="705" height="470" alt="gobuster2_scan" src="https://github.com/user-attachments/assets/5a3052ec-1a5a-4669-893a-fe32ff181c67" />

There was one, the /uploads directory, that had a page called Talk to Valak. 

<img width="971" height="492" alt="wp-content_uploads" src="https://github.com/user-attachments/assets/abe3f607-8c4e-4884-bdb2-6351b8c13ce9" />

Going on to that page, it was just a page where you could say “Hi” to Valak. You type a name, and you get a response. 

<img width="990" height="353" alt="talk_to_valak" src="https://github.com/user-attachments/assets/890972fb-83c6-495e-8222-8672d86267e4" />

I opened up the page in Burp Suite to see if there was anything going on with the HTTP request or response that I couldn’t easily see. There wasn’t anything weird going on with the HTTP request:

<img width="1263" height="412" alt="burpsuite_request" src="https://github.com/user-attachments/assets/1c6d5f09-86fb-46b4-b2f8-0d016a00db64" />

However, there was something that was interesting happening with the response, in that a password came with it.

<img width="631" height="576" alt="burpsuite_response" src="https://github.com/user-attachments/assets/41b7c63f-355d-41e8-82c6-80c04ce2a646" />

The next step was to get the username. Technically, since this was all virtualized I already knew the username when I booted up the VM, but this was practice just in case I didn’t have access to that. The web page uses wordpress, so I used a wordpress vulnerability scanner, wpscan. 

<img width="713" height="601" alt="wpscan" src="https://github.com/user-attachments/assets/dcb29e3a-15c8-40d8-8962-0b51ebc4b368" />

And after that I had the username, krampus.

<img width="721" height="739" alt="usernames_found" src="https://github.com/user-attachments/assets/d611b5a8-38f5-432e-b8a8-f0d7fcb6c851" />

I used the username and password to ssh login to the target, and I got the user flag.

<img width="703" height="613" alt="krampus_user" src="https://github.com/user-attachments/assets/8d2ad9ef-8903-4829-9a20-3d2717ac8615" />

The next step was privilege escalation. I tried to see if there was anything that krampus could run as sudo, but krampus was not allowed to use sudo.

<img width="559" height="201" alt="no_sudo" src="https://github.com/user-attachments/assets/fd23a436-1572-40df-ad56-51f7869f46e6" />

I took a look at the .bash_history file, which keeps track of the user’s last 500 commands, and noticed that it looked like a hacker ran commands here. Remember from one of the above screenshots, “beelzebub” hacked into this target. 

<img width="710" height="748" alt="bash_history" src="https://github.com/user-attachments/assets/2a9ac7c6-dea5-468a-800f-51d902169eb0" />

<img width="710" height="763" alt="bash_history2" src="https://github.com/user-attachments/assets/9e2a8137-3690-4d68-a702-e95ac93bd9d2" />

From the above, it is clear that “beelzebub” got some kind of exploit onto the target and ran it. I did a little bit of research into the exploit, 47009 from exploit-db.

<img width="705" height="447" alt="exploit" src="https://github.com/user-attachments/assets/ca46c432-5ddc-4146-8889-83e60ae33487" />

The exploit takes advantage of CVE-2019-12181, which is a privilege escalation vulnerability. 

<img width="832" height="691" alt="cve-2019-12181" src="https://github.com/user-attachments/assets/11afa168-7b03-4329-941a-981d209fe33c" />

Following in the footsteps of “beelzebub”, I got my own exploit onto the target and used it for privilege escalation. The vulnerable VM wasn’t connected to the internet, so I had to set up a web server on my Kali Linux machine. 

<img width="595" height="135" alt="python_server" src="https://github.com/user-attachments/assets/cd2a74bb-630a-4f11-af09-f56a4492a60d" />

<img width="664" height="178" alt="getting_exploit" src="https://github.com/user-attachments/assets/9b0b6003-3335-46a2-b268-200fdf63c0d7" />

<img width="708" height="289" alt="iam_root" src="https://github.com/user-attachments/assets/8ae44ff0-0aba-4a04-860b-534bdbb49a03" />

## Findings
Password sent in plain text over an HTTP request.

Vulnerability with CVSS score of 8.8 found on the machine, able to be utilized for privilege escalation.

## Remediation
Either encrypt the password or don’t send it over plain text.

Update the system to remediate the vulnerability.

