# Security Assessment: Vulnhub Napping Walkthrough
![jen-bonner-lEfGL-D39i8-unsplash](https://github.com/user-attachments/assets/5c23d69b-add7-4ea6-a0a0-5216a8ef0391)


# Introduction
Napping is a vulnerable machine found on Vulnhub, rated as easy. A vulnerable machine is a virtual machine that has deliberate security flaws, for the purpose of teaching ethical hacking and security testing. There are two goals for this particular machine: to get the user flag, and to escalate privileges and get the root flag. 

This post will serve as a walkthrough of the machine, detailing the steps and tools used to hack into the machine and get the two flags.

# Methodology
Tools Used: nmap, gobuster

Nmap was used to find the host IP address and enumerate open ports and services on the target machine.

Gobuster was used to enumerate subdirectories on the target’s web server. 


# Walkthrough

The first thing I did was to use nmap to figure out the IP address of the target machine. I used the -sn option, which tells nmap to not do any port scanning after host discovery. This is also called a ping scan.

<img width="734" height="259" alt="nmap_host_discovery" src="https://github.com/user-attachments/assets/361cf543-40a4-444a-959e-8102c32a80a1" />

After getting the IP address of 192.168.104.11, I did a second nmap scan of that specific address. I used the -sS option, which is a SYN scan. This looks for open ports by sending a SYN packet, but does not complete the TCP three-way handshake. If a SYN ACK is received, the port is open. If a RST packet is sent, the port is closed. This SYN scan option is available because on my Kali machine I am a privileged user. If I was not, I would have to use another scan.

The -p- tells nmap to scan all ports, which in TCP wouldn’t take that much longer than just scanning the top 1000 ports. The -oN option puts that output into a file, which is handy to use later on if I were to forget which ports were open.

<img width="739" height="213" alt="nmap_scan" src="https://github.com/user-attachments/assets/b2b0c56e-48b4-4ffc-925e-8a9621baad2f" />

There are two open ports on the target machine. Port 22 for SSH, and port 80 for HTTP. After this, I went to the web page to have a look around. 

<img width="987" height="811" alt="login_page" src="https://github.com/user-attachments/assets/2445d68f-fb54-4909-9986-ba697bf5ff75" />

A very simple login page with an option to create a user. To test what this page has to offer, I created a user, santa, with password santa1.

<img width="986" height="809" alt="created_user" src="https://github.com/user-attachments/assets/b2f65ec1-e5bc-4186-b280-1f90c536812e" />

While all of this was going on, I also used the gobuster tool to enumerate subdirectories on the web page. This however did not really give anything useful. 

<img width="806" height="626" alt="gobuster_scan" src="https://github.com/user-attachments/assets/afc4e6ad-ee3d-4f11-863f-c4b9c4601e7a" />

I turned my attention to the submit link form, just entering https://google.com and seeing what would happen. 

<img width="987" height="811" alt="testing_submit_1" src="https://github.com/user-attachments/assets/451296f0-5f7a-4c07-bd20-c2c3eedc1267" />

After entering that, I pressed submit and a link to Google appeared on the web page. 

<img width="522" height="254" alt="testing_submit2" src="https://github.com/user-attachments/assets/a10d8c2d-bcda-4d66-a023-04ce6b796eb6" />

After entering that, I pressed submit and a link to Google appeared on the web page. 

<img width="981" height="806" alt="testing_submit3" src="https://github.com/user-attachments/assets/888c5d83-24b0-4110-a207-e38970d601aa" />

The interesting thing about this is that the web page appears in a new tab. In HTML, this would mean the link was set with the html attribute `target=’_blank’`. This could just be a way to allow users to go to other pages without leaving your web page, there is actually a security vulnerability here.

As explained in this blog post, the page being linked to gains partial access to the source page via the window.opener object. This is a javascript property that refers to the current window. The linked blog post provides a way of exploiting this vulnerability. 

At a high level, you create two html pages. The first is an html page that looks exactly like the targeted website’s login page. The second is a web page that will use the window.opener to redirect them to the fake page. The user reenters their credentials, and you get their username and password.

To execute this vulnerability, first I created a fake login page.

<img width="874" height="612" alt="dup_html" src="https://github.com/user-attachments/assets/88db2d87-c587-4d2b-b9e7-2469affba1bc" />

Then I created the test html file that will redirect to the one above.

<img width="796" height="234" alt="payload_html" src="https://github.com/user-attachments/assets/9c8f2a3f-b5c5-4ada-8d5d-841b8c9982b5" />

I set up a netcat listener on port 4444 that will catch the credentials.

<img width="469" height="154" alt="listener_set_up" src="https://github.com/user-attachments/assets/4d7cd107-1917-4306-8405-acfc612f99a2" />

I then set up a python server for the redirecting html file. I didn’t specify a port, so it serves on port 8000.

<img width="553" height="124" alt="python_server" src="https://github.com/user-attachments/assets/9d5cc8e8-0cdd-4c9f-bbaf-4fa83e699ea4" />

Entered the link into the web page.

<img width="601" height="289" alt="submitting_screenshot" src="https://github.com/user-attachments/assets/a7bd34dd-2eb4-4dd5-afd2-82cb3db45bc8" />

Clicked on the page to make sure it worked.

<img width="664" height="347" alt="page_opened" src="https://github.com/user-attachments/assets/5edfdc37-0c55-4a56-b2c7-f69fd60483f3" />

And after a little bit of time, I received the credentials.

<img width="734" height="240" alt="credentials_received" src="https://github.com/user-attachments/assets/ff09ba51-008c-48ba-97cb-8f2b910cd30e" />

I used ssh to log in to the target machine.

<img width="873" height="618" alt="made_access_daniel" src="https://github.com/user-attachments/assets/e50aa017-c4ac-45e8-91c4-c71970981299" />

Looking around the server, I opened up the /etc/passwd file to see if there were any other users for some lateral movement. There was one other user, Adrian.

<img width="775" height="599" alt="etc-passwd" src="https://github.com/user-attachments/assets/9b1f9434-2062-4279-b2cf-8c92b314f5cb" />

Looking into Adrian’s home directory, there was a python script there that was writable by anyone in the administrators group. There was also the user flag, but as Daniel I was unable to open it.

<img width="724" height="141" alt="permissions_adrian_dir" src="https://github.com/user-attachments/assets/858659e3-b843-4f23-8147-9bab951f74a8" />

These permissions meant that I could write to query.py, execute the file, and having it run as if Adrian executed it. To exploit this, I modified the file to connect to another netcat listener on my Kali machine.

<img width="629" height="349" alt="query py" src="https://github.com/user-attachments/assets/3bc023bf-3935-4e91-b7b2-143af0ec360c" />

The original query.py, before my adjustments.

<img width="686" height="236" alt="getting_reverse_shell_setup" src="https://github.com/user-attachments/assets/f05ca82e-0eaa-44a8-b44e-2b7e9bef301e" />

And after my adjustments. I set up a reverse shell bash script in the /tmp directory, and the new query.py script just executes that script. I then set up a netcat listener on port 1337 to catch the connection.

<img width="779" height="227" alt="i_am_adrian" src="https://github.com/user-attachments/assets/d506a8f7-c596-4016-b9ca-2b05edf2c1ee" />

I was then able to get the user flag.

<img width="335" height="113" alt="user_flag" src="https://github.com/user-attachments/assets/7f64cc92-ad2d-49c8-82e3-0e75a84caba8" />

As Adrian, I ran sudo -l to see if there was anything I can run as root. Adrian had the permission to run ‘vim’ as root.

<img width="821" height="212" alt="can_run_vim" src="https://github.com/user-attachments/assets/0e33905c-6b26-4020-99e8-2578c2850f7e" />

According to GTFObin, if the vim binary is allowed to run as a superuser, it does not drop the elevated privileges and may be used for elevated privileges. Using that knowledge and the exploit listed, I was able to escalate privileges, become root and get the flag.
<img width="708" height="326" alt="iam_root" src="https://github.com/user-attachments/assets/08aedaa1-64c6-4730-b2e5-d7f528782d64" />

# Findings
Weak password policies on the web page. 

Target=”_blank” vulnerability allowed for leaked credentials.

Group writable script was able to be modified to make outbound connections. 

Vim being able to run as root allowed privilege escalation.

# Remediation
Enforce stricter password policies on the web page.

For any web page that opens in a new window, add rel="noopener noreferrer" (this is less of an issue nowadays, this is basically implied by default). 

Limit outgoing connections.

Limit binaries that can be run as root without a password. 
