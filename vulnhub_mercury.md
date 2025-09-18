# Security Assessment: Vulnhub Mercury Walkthrough

![nasa-71W3CWeZF7A-unsplash](https://github.com/user-attachments/assets/de776349-649d-4ebc-b8af-12110fb9e32f)

## Introduction

Vulnhub is a platform that hosts vulnerable machines that allow people to learn security and hacking techniques. These machines are an excellent hands-on way to learn cybersecurity. ‘Mercury’ is a machine part of a series called ‘The Planets’, and it is rated as ‘Easy.’ This machine has two goals: the first is to find weaknesses and vulnerabilities and make initial access to get the user flag, and the second is to elevate privileges to get the root flag. 

This post will serve as a walkthrough and recap of my attempt to hack ‘Mercury’ and to get the flags. This post will go over the tools used, the thinking behind certain decisions, and will go step by step on every step of the way. It will then go into the key vulnerabilities found and suggest some remediations based on how I got the root flag. For those who just want to see the walkthrough, scroll down to the section titled “walkthrough”. 

## Methodology

Tools Used: nmap, sqlmap

Nmap is an open source tool used for network scanning and security auditing. In a Nmap was only used at the beginning of the engagement, and was used only to discover the IP address of the target machine, as well as the open ports. After the initial nmap scan, there was no need for any further scanning. 

<img width="984" height="629" alt="nmap_output" src="https://github.com/user-attachments/assets/55f75d64-f3a7-4f8f-9c87-f00e20df7585" />

The other tool used in this engagement was sqlmap, another open source tool to automate exploiting SQL injection flaws. The key flaw found during the engagement was a SQL injection vulnerability. Sqlmap was used to exploit this to gain access to usernames and passwords.
<img width="984" height="631" alt="first_sqlmap_output" src="https://github.com/user-attachments/assets/ed33f6e3-7475-45c3-87ac-789a673773fe" />


For privilege escalation, I took advantage of a weakness with a script that had the SUID bit set. SUID allows an executable to be run as the owner of the executable. My solution took advantage of the PATH environment variable, to take advantage of the script. The PATH variable is the list of directories the linux shell searches for when it runs a command. If a user types in whoami, the shell looks for the whoami binary in the PATH variable. This can be changed and manipulated.

## Walkthrough

After getting the IP address of the target, I saw there were two ports open on the machine: port 22 for ssh, and port 8080 for an http-proxy website. Going to that website, there was simply a message that stated the website was still in development. 
<img width="1362" height="637" alt="website" src="https://github.com/user-attachments/assets/a7f3f54c-f5ec-4508-b073-0d0c7d0eb202" />

Just to see if I could find some low-hanging fruit, I did check out if there was a directory called /admin. There is not, but there was a very interesting error page.

<img width="1358" height="649" alt="django_error_page" src="https://github.com/user-attachments/assets/2f3545a8-731e-443b-bfde-1a18c0f8f900" />

The framework is Django, and it gave me other pages for the website. Robots.txt and mercuryfacts. Going to robots.txt, there wasn’t anything really important there.

<img width="1356" height="639" alt="robots txt" src="https://github.com/user-attachments/assets/fa6a0a43-82fb-45b7-8e8f-cf0efa6c0493" />

Mercuryfacts did however have some very interesting pages.
<img width="1358" height="636" alt="mercury-facts-inital-site" src="https://github.com/user-attachments/assets/a9d4b936-466e-4cda-b8e2-e527955075be" />

The first was the todo list:
<img width="1359" height="638" alt="todolist" src="https://github.com/user-attachments/assets/0c668852-d6c4-4a44-b56b-c1ae23b5868a" />

The first interesting discovery was the name of a table, users. This table would, presumably, have passwords. The second discovery was that this website doesn’t yet use models, and instead has a direct sql call.

Going back to mercuryfacts, you can click on the mercury fact to get a fun fact.
<img width="1370" height="644" alt="mercury_fact" src="https://github.com/user-attachments/assets/01cd3c73-ec03-4598-9a10-7efff1f29226" />

The Fact id part indicates to me that this page is vulnerable to SQL injection. To test this, in the URl, I put a simple OR 1=1–, and got every fact.
<img width="1362" height="643" alt="mercury_facts_sql_injection" src="https://github.com/user-attachments/assets/c4242aaa-2310-426b-98eb-1edf9c65b9e5" />

Taking advantage of this, I used sqlmap to try to get the users and passwords.

<img width="1380" height="554" alt="sqlmap_passwords_exposed" src="https://github.com/user-attachments/assets/40c14f72-035b-4a05-9f5b-f1cbec1c1b51" />

Of these, webmaster looks to be the most promising. Using ssh, I made the initial access and got the user flag.
<img width="1506" height="733" alt="initial_access" src="https://github.com/user-attachments/assets/a8f77c16-5cc7-4c75-8d77-54a11bf7ca68" />

Checking out what I can do as webmaster, there wasn’t much. Webmaster wasn’t enabled to use sudo.
<img width="1504" height="838" alt="webmaster_sudo_output" src="https://github.com/user-attachments/assets/14056788-7a08-45f9-b2e5-981fa13101a4" />
Opening up the /etc/passwd file, there was another user on the machine, linuxmaster.

<img width="1492" height="832" alt="etc_passwd_output" src="https://github.com/user-attachments/assets/164c5f7e-718c-40da-967b-48f4b338c0b5" />

Going into a directory in webmaster’s home directory, mercury_proj, there was a file called ‘notes.txt’ which had the accounts for webmaster and linuxmaster, and their passwords. The passwords were encoded in base64, not hashed. After decoding linuxmaster’s password, I switched over to that user. 

<img width="1503" height="838" alt="linuxmaster_password" src="https://github.com/user-attachments/assets/f46a9f18-e928-4414-b689-da7cf91a7c80" />
<img width="1509" height="833" alt="whoami_linuxmaster" src="https://github.com/user-attachments/assets/2b27923a-d7ca-44de-b304-0221cf95b144" />

Running sudo -l to figure out if there were any commands that linxumaster could run as root, there was one in particular.

<img width="1508" height="836" alt="sudo-l_linuxmaster" src="https://github.com/user-attachments/assets/69695b0f-677b-426d-a6c1-4085ebfdae7a" />

This check_syslog.sh was essentially just using the tail command on syslog file. The problem is that it just calls the command tail. The shell will look in the PATH variable for where that command is, and if it finds something with the name of tail, it doesn’t matter if the executable is actually tail or something just named that.
First, create a file in the /tmp directory, named tail. Make this /tmp/tail a script that switches to the root user.

Then, change this so it is executable.

Edit the path variable so it looks in the /tmp directory first.

And then execute the check_syslog.sh file. 

<img width="1508" height="838" alt="root_pass" src="https://github.com/user-attachments/assets/dea216ac-b9e3-4039-86e9-28554d76de03" />


## Findings
In Django, DEBUG=True’ will mean an error screen will show up when an error is made. According to the docs themselves, “Never deploy a site into production with DEBUG turned on.”

Using a direct SQL call makes it vulnerable to SQL injection attacks.

Having passwords in a text file, where the only protection is through base64 decoding, is a security flaw. 

Calling binaries without the absolute path.
## Remediations
Switch DEBUG to False. 

Utilize Django models instead of the direct sql call. 

User training to ensure better security practices in storing passwords. 

Using absolute paths when writing bash scripts. 


