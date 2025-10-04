![constantinos-kollias-yqBvJJ8jGBQ-unsplash](https://github.com/user-attachments/assets/c62b0be4-7840-4458-a2fe-a897ea3161fe)
# Introduction
Thales of Miletus was an ancient Greek philosopher. Thales of Vulnhub is a vulnerable machine found on Vulnhub. The purpose of a vulnerable machine is to teach cybersecurity and ethical hacking. The goal is to get the user flag and the root flag. 

This post will be a walkthrough of the machine, detailing the steps and tools and techniques used to hack Thales.

# Methodology
Tools used: nmap, tcpdump, gobuster, metasploit, john the ripper

Nmap was used to enumerate open ports and services.

Tcpdump was used to capture traffic, to better understand the kind of noise that my scans would be doing. 

Gobuster was used to enumerate web directories and pages.

Metasploit was used to exploit vulnerabilities. Msfconsole, the command line interface for metasploit, and msfvenom, a command used to generate payloads, were both used.

John the ripper was used to crack passwords. 

# Walkthrough
The first thing I did was use nmap -sn to do a host discovery/ping scan of my internal network to find the target machine’s IP address.

<img width="692" height="263" alt="nmap_host_discovery" src="https://github.com/user-attachments/assets/9dd32ad3-0e6f-4b9e-8dba-3109f9392d69" />

After finding the IP address of 192.168.104.12, I did a port scan of the target. I used -sS, which is the SYN scan, which sends the first part of the 3-Way Handshake (the SYN packet), but does not complete the connection. I used -p- to scan all TCP ports, and I used -oN to output the result to a text file for further reference. 

<img width="754" height="211" alt="nmap_port_scan" src="https://github.com/user-attachments/assets/11c37573-9f50-4be7-a1ea-05aeab621907" />

Just to further understand what kind of traffic this generates, I used tcpdump to capture traffic.

<img width="746" height="335" alt="tcpdump_output" src="https://github.com/user-attachments/assets/5d244083-9893-405b-91e9-fef509763abb" />

Ports 22 for SSH and 8080 for an http server were open. I went to the web page to see what I could find, and found a default Apache Tomcat, version 9.0.52, page. 

<img width="992" height="808" alt="webpage" src="https://github.com/user-attachments/assets/4dc56126-c66e-498a-a77e-4a8e78d98ac5" />

Clicking on the buttons to the right, server status, manager app, or host manager, would get a prompt for a username and password. At this point I obviously didn’t have any, but that was what I would be looking for next. 

<img width="988" height="497" alt="ask_for_login" src="https://github.com/user-attachments/assets/e82dc810-bd6c-4b48-8a1b-179987221508" />

<img width="983" height="438" alt="error_page" src="https://github.com/user-attachments/assets/02532c82-24b6-42e0-b9f2-03a3e309b559" />

I looked into the page source to see if there was anything there, but I didn’t find anything. 

<img width="900" height="696" alt="page_source" src="https://github.com/user-attachments/assets/6a9ff68a-6c9d-4f24-84da-7bd61d3cf0d9" />

I knew the version of the web page, but I did want to get the service version for port 22, so I set up an nmap scan with -sV, to get service versions, and targeted it for ports 22 and 8080. 

<img width="777" height="241" alt="nmap_sV" src="https://github.com/user-attachments/assets/b0e90ce4-73b1-4f8b-bbf1-d6e8e0798405" />

I also set up a gobuster scan to look for directories, but that didn’t turn out anything that would help in finding the username and password. 

<img width="802" height="417" alt="gobuster_scan" src="https://github.com/user-attachments/assets/7ef60569-b6df-4042-8654-0cf3617dc7fb" />

Since I knew the Apache version, however, I could search for exploits related to that. I opened up metasploit using msfconsole to see if there was anything I could use.

<img width="883" height="634" alt="msfconsole_open" src="https://github.com/user-attachments/assets/cf3ab6d8-c7db-4556-be7a-45a1fddfd4e7" />

I searched for tomcat to find exploits related to that version.

<img width="1078" height="701" alt="search_tomcat" src="https://github.com/user-attachments/assets/91b1a8b8-1d90-4503-8273-f20bb9596de0" />

And I found one specific to finding manager login credentials. 

<img width="1076" height="705" alt="scanner_tomcat" src="https://github.com/user-attachments/assets/a133ef23-a1ac-4902-b701-61c580cdd456" />

Setting up the RHOST, which is the IP address for the target machine in this case, I ran the exploit and found the username and password for the admin.

<img width="831" height="220" alt="found_credentials" src="https://github.com/user-attachments/assets/7fcfe6b8-9c87-40ef-b760-6bd6d126519f" />

I then logged in to the web application manager page.

<img width="992" height="834" alt="webapp_manager" src="https://github.com/user-attachments/assets/99a2a287-98c2-4e82-b84c-86965918e3ce" />

Scrolling down a little bit, there was an upload section. A war file is like a zip file that bundles files needed to run a Java web application. They are uploaded and can be used to deploy web pages. This can be exploited manually by crafting a payload that would set up a reverse shell. 

<img width="983" height="331" alt="upload_war" src="https://github.com/user-attachments/assets/99906add-6d0c-4e97-bd89-99616fb5fa87" />

The below exploit was followed from this web page on Tomcat. First, I crafted the malicious payload using msfvenom. I set the local port to 1234.

<img width="888" height="95" alt="msfvenom_payload" src="https://github.com/user-attachments/assets/a1fbc80a-55c6-4f69-9dc4-6e50f6e9caa4" />

Then I uploaded this payload and deployed it.

<img width="990" height="129" alt="uploading_payload" src="https://github.com/user-attachments/assets/a636d74c-e538-414c-87c7-a099ccd6a4d1" />


After deploying, there was a new web app called shell.

<img width="979" height="457" alt="uploaded_payload" src="https://github.com/user-attachments/assets/371e5adc-7abe-4507-af23-05de15852635" />


I set up a reverse listener, and received a shell after clicking on /shell.

<img width="1050" height="580" alt="reverse_listener" src="https://github.com/user-attachments/assets/2a5242b7-6c81-4523-a3a4-75389929eb27" />

To get a little bit more functionality, I used python3 to create a better shell.

<img width="900" height="95" alt="spawn_shell" src="https://github.com/user-attachments/assets/deb7a33d-cfd9-48ca-8561-7bdf3afe3bae" />

After that, I explored around the system and found my way to the home directory of the ‘thales’ user. The user flag was in there, alongside a notes.txt file. I could open the notes file and it did say that there was a backup script in another directory. I however could not open the user flag file, and I wanted to get that one first. 

<img width="994" height="201" alt="home_directory" src="https://github.com/user-attachments/assets/05be02fa-b872-4c06-9da1-8a2b819d52dd" />

Looking around, I was able to enter and take a look around in the /.ssh directory. I found a password protected private key.

<img width="730" height="78" alt="private_key" src="https://github.com/user-attachments/assets/ada5e0e4-32ed-41b2-ae0d-0f76d9fe4a7c" />

I copied that over to my Kali shell, and began the process of brute forcing the password. 

<img width="702" height="574" alt="encrypted_key" src="https://github.com/user-attachments/assets/80261954-b60e-4915-9480-33571f9e8a90" />

To accomplish that, I first used ssh2john to get the key into the proper format. I then used john the ripper, the password cracking tool, to get the password. 

<img width="865" height="273" alt="cracking_password" src="https://github.com/user-attachments/assets/74160a2f-9e31-45d8-8116-03cfba91f754" />

Using the above as the password, I was able to switch to the Thales user and get the user flag.

<img width="1036" height="171" alt="user_flag" src="https://github.com/user-attachments/assets/8a00e0d8-5d8f-4863-bb0f-3d6955aafc56" />

I then took a look at the backup bash file mentioned in the notes.txt file. The script itself wasn’t the interesting part about it though. 

<img width="853" height="562" alt="backup sh" src="https://github.com/user-attachments/assets/827bae00-e654-41bb-998f-96cd7a25882e" />

The interesting part was the permissions on the file. It was owned by root, and it was world writable, readable, and executable. This is a pretty serious vulnerability, because anybody could write to this file and have it be executed as root.

<img width="629" height="74" alt="backup sh_perms" src="https://github.com/user-attachments/assets/7d6ea69c-67fd-426d-8063-76f15de536a9" />

To exploit this, I made a few modifications that would just start another reverse listener to my Kali machine, this time on port 4444. I tried to use the su root command to switch to the root user, but that didn’t work. 

<img width="685" height="597" alt="modifications_backup sh" src="https://github.com/user-attachments/assets/bb149e50-a88f-475c-8bba-dffc2fc1e9f8" />

Setting up the reverse listener, I was able to get a root shell and get the root flag. 

<img width="792" height="313" alt="i_am_root" src="https://github.com/user-attachments/assets/01a49b27-7468-4431-821d-09b9c4605947" />

# Findings
Simple credentials allowed for brute force. 

Misconfigurations allowed a malicious WAR file upload.

.ssh directory able to be seen by other users. 

Weak password on SSH key.

World writable script owned by root. 

# Remediations

Enforce a stricter password policy.

Proper security configurations for the file uploads.

Configured .ssh directory and scripts.
