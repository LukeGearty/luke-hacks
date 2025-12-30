# Looz: 1

I first did a nmap scan of the network to find the IP address of the target machine. I did a ping scan using ‘nmap -sn’, which tells nmap to not do a port scan on the IP addresses.

<img width="1340" height="458" alt="nmap-pingscan" src="https://github.com/user-attachments/assets/b474beb9-78f7-4c0b-ab3b-b1e6bf7ca892" />


I then did a port scan for all ports, using -sV to find what services are running on what ports and what versions of the services were running. 

<img width="1494" height="532" alt="nmap-sv-output" src="https://github.com/user-attachments/assets/aeb42366-eba2-4795-8ca3-d9553c37fc91" />


Port 22 for SSH, 80 for http, 139 and 445 for SMB, 3306 for mysql, and port 8081 for another web page. I opened a web browser and went to the web page to see if there was anything I could find.

<img width="1335" height="885" alt="webpage_output" src="https://github.com/user-attachments/assets/d1b310bc-c115-4acc-9f36-dd15d220b74b" />


I looked through the page source and found that there was a username and password in there.

<img width="1201" height="288" alt="comment-username-password" src="https://github.com/user-attachments/assets/e2c04b88-8d1c-4067-a8a0-ba05e6aa707e" />


I now needed to find a wordpress login page. I went to the /wp-admin directory, typically the login page for a wordpress website, but it wasn’t there, at least for the web page running on port 80. I did a scan of directories, using dirb, and found no other interesting directories.

<img width="1158" height="585" alt="first_dirb_scan" src="https://github.com/user-attachments/assets/2a4216cc-1a29-4d71-b5b5-822b5aadaa57" />


I also did a dirb scan of the web server running on port 8081, and did find the /wp-admin directory.

<img width="1127" height="710" alt="second_dirb_scan" src="https://github.com/user-attachments/assets/32097f5c-4fa9-4f41-a6b2-8a8e10d459f1" />


When I went there, however, I only received a page that said the file was not found.

<img width="1320" height="548" alt="file_not_found_wp-admin" src="https://github.com/user-attachments/assets/d4b12b58-cfb0-457e-bc9f-743e37c2d8ea" />


I took a look at the URL, and found that I needed to edit my /etc/hosts file to map the target IP address to wp.looz.com and looz.com. Once I did that, and restarted my browser, I was able to get to the login page.

<img width="1316" height="879" alt="wp-login-page" src="https://github.com/user-attachments/assets/142d34cd-8d51-4d15-aee9-2d01bb5903be" />

I imputed the username ‘john’ and the password found in the comment, and was in the admin dashboard.

<img width="1328" height="884" alt="wp-admin" src="https://github.com/user-attachments/assets/383a8ace-6fe8-467e-ba56-9908be1ab24e" />


In the users section, I found there was another user that had administrator role, ‘gandalf’. 
<img width="1159" height="731" alt="list-of-users" src="https://github.com/user-attachments/assets/91ac46c9-c0a8-4412-b7be-283d3006bfff" />


I used hydra to brute force the SSH login. As a note, this took a long time (a little under an hour), so if you are doing  this, be patient and don’t freak out if it isn’t immediate. 
<img width="1822" height="556" alt="hydra-output" src="https://github.com/user-attachments/assets/362d492b-ab7e-4b84-bf8c-b14eb1094ce9" />


I used that username and password to login via SSH.
<img width="1315" height="800" alt="ssh-result" src="https://github.com/user-attachments/assets/ee3cc8e4-7d5a-43de-a4a8-b972ffd299d9" />



Gandalf couldn’t do much on the target machine, but there was another user, Alatar, found when I looked at the /etc/passwd file.

<img width="1390" height="765" alt="etc-passwd" src="https://github.com/user-attachments/assets/40879132-7865-4fc7-8f58-e57295743b18" />


Alatar had a directory where the user flag was located, and gandalf was able to open it.

<img width="669" height="156" alt="user flag" src="https://github.com/user-attachments/assets/2c66216f-5edf-417f-8fcb-51fac5070e68" />


There was also a directory titled Private, and in there was a binary titled shell_testv1.0. I looked at the binary and saw that it was owned by root, and it was world executable. 

<img width="1819" height="109" alt="shell-testv1" src="https://github.com/user-attachments/assets/ef9f07bf-240d-4e72-925c-2018afacdfd0" />


I executed it, just to see what it does, and I became the root user. From there, I went to the /root directory and got the root flag.

<img width="637" height="219" alt="i_am_root" src="https://github.com/user-attachments/assets/36c52008-228b-4d22-94bf-a20ba793b5ea" />

