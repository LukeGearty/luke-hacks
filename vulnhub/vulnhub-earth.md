
![nasa-vhSz50AaFAs-unsplash](https://github.com/user-attachments/assets/0dc3b3b2-9b51-483d-ad11-988786bcbe3c)

# Introduction

Vulnhub has a series of vulnerable machines, called ‘The Planets’, where there are vulnerable machines named after the planets. A vulnerable machine is a virtual machine deliberately designed to be vulnerable, to teach hacking and cybersecurity techniques. ‘Earth’ is a machine, rated as Easy, and there are two goals: to make access and get a user flag, and to elevate privileges and get the root flag. 

This post will serve as a walkthrough of the tools and techniques used to hack the Earth vulnerable machine. It will go into key vulnerabilities and suggestions on remediating those vulnerabilities. 

# Methodology
Tools used: nmap, gobuster, dirb, python scripting, nc, ltrace

Nmap was used to get initial ports open on the target machine.

Gobuster and dirb were used to look for directories and pages on the web server.

I used python scripting to decode an XOR encrypted key, but this also could’ve easily been accomplished with something like cyberchef.

Netcat was used to create reverse shells and copy over a binary for analysis.

Ltrace was used to trace the library calls of said binary to finally get the root flag. 

# Walkthrough
The first step is to get the IP address for the vulnerable machine.
<img width="1020" height="289" alt="first_nmap" src="https://github.com/user-attachments/assets/fa90d877-c632-40a7-8619-4f341ffe72f9" />

There are three ports open on the vulnerable machine: port 22 for ssh, port 80 for http, and port 443 for https. Before scanning anything else, I went to the web page to take a look around.

<img width="1344" height="648" alt="first_web_visit" src="https://github.com/user-attachments/assets/ae7b4b9d-c287-4bd8-8d17-b952b77d7f95" />

Going to http://192.168.104.9, there is a simple page that says Bad Request. This gives nothing, so the next step is to go to the https page. 

<img width="1285" height="618" alt="https_visit" src="https://github.com/user-attachments/assets/c899d1d2-187e-4006-b77c-3d4f32bbf900" />


Taking a look, there is a security warning, but that is because there is a self-signed certificate. A self-signed certificate is a certificate that doesn’t come from a proper CA. These aren’t trusted by browsers because of this, so whenever you go to a website with a self-signed certificate, you get a warning.

<img width="787" height="273" alt="self_signed_certificate" src="https://github.com/user-attachments/assets/7244e876-5803-4910-a0cd-59d0fed43789" />


Looking into the certificate details, there is a domain name there, earth.local.
<img width="784" height="510" alt="certificate_details" src="https://github.com/user-attachments/assets/0bd7f0ea-7c8f-4c04-88a9-3cabf53224f7" />



There is also a subject alt name.
<img width="809" height="140" alt="subject_alt_names" src="https://github.com/user-attachments/assets/287b1e3b-ba62-4c24-95de-2695da8691e1" />


The next step is to put these into the /etc/hosts file. This file maps domains to IP addresses. When we do that, if we type in “http://earth.local”, we will go to the web page. 
<img width="674" height="198" alt="modify_etc_hosts" src="https://github.com/user-attachments/assets/a1281043-91cc-4503-a7a4-8b4780082f64" />

Going to that exact web page, there is ‘Earth Secure Messaging Service’. It allows a user to send a message to Earth, specify a key, and it seemingly encrypts it. 
<img width="1462" height="871" alt="earth_local_messaging" src="https://github.com/user-attachments/assets/dd202c4e-1039-4aef-a1a2-182a47f736cc" />

Those messages may have important information, and it would appear the next step would be to try to decrypt those messages. But we need to find the key before we can do that.

Terratest.earth.local has the same webpage. 
<img width="1457" height="859" alt="terratest_earth_local" src="https://github.com/user-attachments/assets/96d0c572-a6ac-43c7-a662-6083a304ede8" />

Not having anywhere else to go, it was time to look for web pages. This is looking for directories and pages that may be hidden, and those pages can have useful information. At first, I used gobuster to perform this web enumeration. I first did it on http://earth.local.
<img width="1494" height="838" alt="gobuster_scan" src="https://github.com/user-attachments/assets/6389ff4a-adec-4de9-bce6-416d47bf280f" />


There is an /admin page. Going to that page, there is a log in page. 
<img width="960" height="490" alt="admin_page_login" src="https://github.com/user-attachments/assets/e4778fdd-3763-436e-8e87-94bc0ac90cf1" />

So a new goal added will now be to find a username and password for this page. For further web enumeration, on https://terratest.earth.local, I used the ‘dirb’ tool. 
<img width="775" height="425" alt="dirb_https_terratest" src="https://github.com/user-attachments/assets/2f2ddb4d-0aab-4641-a0dc-ebb0d3226730" />


There is a robots.txt file. The robots.txt file tells (well-behaved) web crawlers which pages they can visit and which ones they should not access. Checking out the robots.txt file:

<img width="963" height="597" alt="robots txt" src="https://github.com/user-attachments/assets/064d6d1f-fe73-4c44-8d65-f6c1ee3feb73" />


To translate this image, it is essentially banning any file that may have the name ‘testingnotes’. The line at the bottom, ‘Disallow: /testingnotes.*’ has the asterisk wildcard which represents any number of characters. Going to testingnotes.txt (after checking a few more based on the above):

<img width="1463" height="878" alt="testingnotes txt" src="https://github.com/user-attachments/assets/8a96c4cf-1eaf-47c7-b084-3006e4347d49" />


We have a few things from this page: 
XOR Encryption is used as the algorithm
terra is the username for the admin portal
Testdata.txt was used to test encryption

Going to testdata.txt:
<img width="1440" height="298" alt="testdata txt" src="https://github.com/user-attachments/assets/ed339a7f-43a3-4b4b-9332-92a506392315" />

This looks like it was used as the key. The next step is decrypting the messages. There are many ways to do this, I wrote a python script to do it. 
<img width="1501" height="683" alt="python_unxored" src="https://github.com/user-attachments/assets/6ac1b8e9-31d2-44a3-8291-d6bebde383a0" />

Testing ‘earthclimatechangebad4humans’ as the password and ‘terra’ as the username, I was able to log in to that admin page.
<img width="1168" height="320" alt="access_to_admin" src="https://github.com/user-attachments/assets/0af9dcfd-b68a-4996-aecb-ce203077f007" />


There is a page that allows the admin to run CLI commands. I tried to run a few commands:

<img width="951" height="175" alt="ls_command" src="https://github.com/user-attachments/assets/0e0d7b41-6594-4f59-bcb4-39f0a9884bec" />
<img width="725" height="172" alt="whoami_command" src="https://github.com/user-attachments/assets/b8c4f199-12ad-4ed5-a248-61feeb061595" />
<img width="790" height="164" alt="pwd_command" src="https://github.com/user-attachments/assets/ddc4685b-244d-462c-9ffd-ff73f912c675" />




I also opened and got the /etc/passwd file to get the users. 

<img width="1453" height="407" alt="etc_passwd" src="https://github.com/user-attachments/assets/9ddad05c-c38c-46bf-aeac-b0e7bcd46184" />

So my next step was to try to set up a reverse shell. A reverse shell allows an attacker to gain remote access to a machine. It is a shell that is running on one machine, sending and accepting messages from another. It allows an attacker to run their own commands on a target system. 

<img width="640" height="158" alt="nc_listener" src="https://github.com/user-attachments/assets/dd5649a6-514f-48a9-8ef1-6dc1b22ebff4" />



Using netcat to set up a listener on my attacking machine, I attempted to set up the shell…
<img width="881" height="258" alt="nc_failed_remote-connections-forbidden" src="https://github.com/user-attachments/assets/db4edaf3-a82a-4c71-9a8e-4219defa9574" />

And discovered that remote connections were forbidden. Looking into it a little further, I discovered that the system is just looking for IP addresses in the CLI command, not actually checking if outbound connections are being made. 
<img width="829" height="240" alt="echo_failed" src="https://github.com/user-attachments/assets/1aea6b00-acd0-4625-965d-958f0f3da48a" />


Looking into it a little further, the program isn’t really checking for valid IP addresses either.
<img width="1045" height="256" alt="not_valid_ip_echo" src="https://github.com/user-attachments/assets/59a0846d-9691-4591-a04f-cbdb0fd5540b" />


So knowing that, there is a way to get around this program. The main idea behind it is to:
Set up the echo command that will print out the nc command.
Encode it in base64

Then on the CLI interface, echo that base64 encoded command, pipe it to decode it, and then use bash to execute the command. 

<img width="581" height="78" alt="base64_encoded_cmd" src="https://github.com/user-attachments/assets/b4c3886c-61fb-4e04-b437-333c46a0c575" />

<img width="1285" height="552" alt="initial_access" src="https://github.com/user-attachments/assets/1a8b1118-32af-4d56-9b60-7f86462d5c39" />

I’m in. The next step is to upgrade the shell a bit and get the user flag.
<img width="1150" height="63" alt="user_flag_found" src="https://github.com/user-attachments/assets/6aafd8f8-5144-4483-91f4-843e0df7a36b" />


So to now escalate privileges and get the root flag, I searched for binaries with the SUID bits set. SUID bits enable a user to execute a binary as the owner. Finding such binary that belongs to the root user is a common way of escalating privileges. 

<img width="706" height="327" alt="files_with_suid" src="https://github.com/user-attachments/assets/0b4b141c-7860-42cb-86af-4f447ae48d46" />

There is the reset_root binary, which looks to be promising. Running that just to see what it does:

<img width="467" height="71" alt="reset_root_output" src="https://github.com/user-attachments/assets/7ce716e5-87da-422a-9d19-ff0ba70e059e" />


So there needs to be triggers present, and the binary checks to see if these triggers are present. Looking into the binary with the strings command, which prints human readable characters from a file:
<img width="906" height="682" alt="strings_output" src="https://github.com/user-attachments/assets/7e9bd658-6300-4ee2-85b1-7277003371ae" />


If this binary successfully executes, then it resets the password. The next goal is to figure out those triggers. The next step was to get the binary over to my kali machine. First, set up another netcat listener on a different port.
<img width="671" height="87" alt="nc_1337_listener" src="https://github.com/user-attachments/assets/1cdf68ec-abd1-4de0-87a8-97428a421d24" />


Then, open the file and get that to the machine on that port. 
<img width="727" height="51" alt="get_reset_root-to_kali" src="https://github.com/user-attachments/assets/d6405b63-8bb7-44a1-ac81-f0a1136beb0f" />

Once the binary was on my Kali machine, I used the ‘ltrace’ command. That command is used to trace library calls. Library calls are a call to code in a prewritten library.
<img width="899" height="183" alt="ltrace_output" src="https://github.com/user-attachments/assets/8b385f4a-e6a0-43af-ba95-f311535d7f37" />


So those are the three triggers. After creating those files, I was able to run the binary and become root.

 <img width="560" height="227" alt="i_am_root" src="https://github.com/user-attachments/assets/98125095-503a-4d28-8311-16a1b0bd3beb" />
<img width="771" height="526" alt="root_flag" src="https://github.com/user-attachments/assets/0716bcc1-87aa-41b7-97dd-41a8e7b847f7" />


# Findings
Self-signed certificates are not trusted by browsers. 

Usernames and important information  in plaintext files on a web page. Encryption key exposed on a web page. 

Insufficient checking of outbound connections on /admin.

# Remediations

Replace self-signed certificate with one from a proper CA (Let’s Encrypt for example). 

Carefully look through files to make sure important documents are not exposed to the internet. 

Block outbound connections using a firewall, and on the CLI interface limit commands to only those that are necessary. 
