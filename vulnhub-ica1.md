![alexandre-lallemand-gZpM8PjMdBo-unsplash](https://github.com/user-attachments/assets/4a222cb2-ccef-47f8-864c-662a35f2a2b4)

# Introduction
ICA:1 is a vulnerable machine found on Vulnhub. A vulnerable machine is a virtual machine deliberately designed to be vulnerable, to teach hacking and cybersecurity techniques. For this vulnerable machine, there are two goals: to get the user flag and to get the root flag.

This post will be a walkthrough of the tools and techniques used to complete this vulnerable machine. 

# Tools and Methodology
At a high level, this engagement went through the following steps. Reconnaissance to find open ports and services and vulnerabilities. Then, after finding the vulnerabilities, exploiting the vulnerabilities to get into the system and capture the user flag. Then, after getting into the system, figure out ways to escalate privileges to get the root flag.

Tools Used: nmap

Nmap was used for scanning and reconnaissance on the target machine, discovering open ports and services and their versions.

# Walkthrough
I first did a ping scan of the internal network to find the target’s IP address. The -sn option tells nmap to not run any port scans after host discovery.

<img width="694" height="264" alt="nmap_host_discovery" src="https://github.com/user-attachments/assets/03bb6651-1f83-4beb-9652-ecd8c0387e89" />

After discovering the IP address of 192.168.104.13, I did a SYN scan of the target. The SYN scan sends SYN packets, the first step in the TCP three way handshake. If the host replies with a SYN-ACK, the port is open. If the host replies with a RST, the port is closed. If it does not reply, or an ICMP unreachable error, the assigned state is ‘filtered’. 

<img width="712" height="283" alt="syn_scan" src="https://github.com/user-attachments/assets/4ae75535-3d3d-4fc6-945c-a0db7bda3396" />

Ports 22 for SSH, 80 for http, 3306 and 33060 for mysql were open on the target machine. I went to the web page to see what was there.

<img width="988" height="810" alt="web_page" src="https://github.com/user-attachments/assets/a797be5e-73f1-4deb-9112-0f11daad5770" />

The web page was running qdPM 9.2. Through research on that version, there was a password exposure vulnerability, where the database password was stored in an accessible YAML file. YAML is a human-readable data serialization format. 

<img width="1708" height="341" alt="exploit_db_qdpm" src="https://github.com/user-attachments/assets/f7af2be7-0fcb-43ab-bf7a-101b173d201a" />

To access the file, I went to http://192.168.104.13/core/config/databases.yml. I used curl to accomplish this and saved the output to a file for future reference.

<img width="574" height="240" alt="databases yml_pass" src="https://github.com/user-attachments/assets/f37c27e6-d9fd-4d33-bc08-41fbf07b9917" />

To see if there were any other version vulnerabilities, I used nmap again and used -sV  to find the versions of each service, to see if there were any other vulnerabilities I could find. 

<img width="741" height="321" alt="nmap_sV" src="https://github.com/user-attachments/assets/57b9d7c1-d146-4c5d-9b08-f4041b068796" />

I then used the username and password from the yml file and logged into the sql server. I had to use mysql, and use the –skip-ssl option because there was an untrusted certificate. 

<img width="711" height="244" alt="logging_in_sql" src="https://github.com/user-attachments/assets/c6adfa86-c931-4db9-bc7c-3bdfdaf9d72d" />

I then took a look at all the databases.

<img width="760" height="234" alt="show_databases" src="https://github.com/user-attachments/assets/695d49ba-3a4d-411a-b885-8dae04c19668" />

From the above list, staff was the most intriguing one. I selected that one and got a list of the staff names, alongside their role and their id. This told me that these were ssh usernames.

<img width="716" height="699" alt="users_sql" src="https://github.com/user-attachments/assets/63a2234c-a5ce-4355-88cc-ad981f32e6ed" />

Taking a look at the login information, I found their encoded (not encrypted) passwords. 

<img width="591" height="393" alt="users_passwords" src="https://github.com/user-attachments/assets/df6ee54e-f058-44f8-b320-3e630dccb388" />

I did have to decode the passwords, using the base64 -d command, and match each password with the user id, but I was able to use Travis’ password to log in to the system. 

<img width="1769" height="837" alt="getting_in_to_system" src="https://github.com/user-attachments/assets/36710766-2292-464c-83c3-557f8148b59a" />

I tested what Travis was allowed to do on the system. First, I ran sudo -l to see if there was anything he could run as sudo, but Travis was not allowed to run sudo.

<img width="633" height="145" alt="no_sudo" src="https://github.com/user-attachments/assets/2fe800fb-5dac-41d4-a249-aa78504ce161" />

I then used the find command to look for binaries with the SUID bits set. Files with the SUID bits set execute as the file owner, not as the user who executed them. For certain binaries, that can lead to privilege escalation if the binary is owned by root. Getting the list of binaries, there was one that stuck out, called /opt/get_access.

<img width="1266" height="590" alt="opt-access" src="https://github.com/user-attachments/assets/a80c40ea-5a84-4976-9777-65280145bc19" />

Taking a look at the file, it seems to give access to the ICA system, but only within certain hours. I utilized the strings command to find human-readable characters in the binary.

<img width="818" height="771" alt="strings_opt_access" src="https://github.com/user-attachments/assets/9b3a92e4-c43d-497e-834b-859d54a5ee63" />

This binary uses the cat command to open a file, but it does not use an absolute path. When a command is called, the system searches through the $PATH, a list of directories, for the executable of the same name. If a binary calls a command, like cat, without an absolute path, the system will search through that list of directories for the cat command. This can be exploited by manipulating the path variable to include a directory of an attacker’s choice. Inside that directory would be an executable file of the same name being called, and it could do anything that the attacker wants it to do. 

I changed directories to the /tmp directory, created a file called cat which will just change to a root shell and made my /tmp/cat executable.

<img width="436" height="162" alt="getting_payload_ready" src="https://github.com/user-attachments/assets/6634cb29-1109-4fc9-9484-253abc8acb27" />

I then changed the PATH variable so that /tmp was added to it. That way, when I called /opt/get_access, it would search through /tmp first.

<img width="618" height="136" alt="changing_path_variable" src="https://github.com/user-attachments/assets/75ef7350-a7ed-47c3-acce-1ed22306dacc" />

I then called /opt/get_access, and switched over to the root user and got the flag.

<img width="679" height="241" alt="i_am_root" src="https://github.com/user-attachments/assets/26fe4d4c-4312-4b8e-9d80-823ae910496a" />

# Findings

Password exposure vulnerability

Poor encryption/storage of passwords, instead using encoding.
Calling binaries without the absolute path.

# Remediations
Update to newer version to avoid password exposure vulnerability.

Properly encrypt stored passwords.

Utilize absolute path when writing executables and scripts.
