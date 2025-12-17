“Deathnote: 1” is a vulnerable machine on Vulnhub, rated as easy. This post will be a walkthrough of hacking the machine and escalating privileges to root. I have never watched Death Note, so the references here all went over my head.

First I found the IP address of the target machine. I did this using nmap, with -sn for a ping scan of my host network. The -sn also tells nmap not to do a full port scan, as that would come later. 

<img width="1130" height="398" alt="nmap_ping_scan" src="https://github.com/user-attachments/assets/ca5488c5-7543-4550-9794-e70fb58480e4" />


I then perform a service version scan of all ports on the target IP address, although there were only two open: 22 for ssh, and 80 for http.

<img width="1321" height="395" alt="nmap-sv_scan" src="https://github.com/user-attachments/assets/d3369f19-68f4-4c90-ba51-e62eb473dcf7" />


I went to the web page, and it immediately redirects to ‘deathnote.vuln/wordpress’. 


I needed to modify the /etc/hosts file for the redirect to work. That file manually maps IP addresses to domain names.

<img width="734" height="237" alt="etc-hosts-file" src="https://github.com/user-attachments/assets/d3d81e82-6234-4469-a046-410bf58ac9a1" />


After doing that, the redirect worked and I was redirected to this website.

<img width="1912" height="883" alt="deathnote-vuln" src="https://github.com/user-attachments/assets/3b550ceb-4489-443e-9235-b6d4fe0b3830" />


On this web page, there was a link to a page rather subtly called HINT. This told me I needed to find a notes.txt file on the server, or see the L comment.

<img width="1917" height="804" alt="notes-txt-hint" src="https://github.com/user-attachments/assets/551f458c-9bd7-42cf-a86b-7de957b54ef5" />


The L comment said ‘my fav line is iamjustic3’. 

<img width="362" height="228" alt="iamjustic3" src="https://github.com/user-attachments/assets/d8009c82-e491-4b9c-b1e7-04d8a9257374" />


That looked like a password to me. Wordpress websites typically have a ‘wp-admin’ directory for site admins. I ran gobuster just to be sure, but I did manually check and found the login page. I used ‘kira’ as the username and ‘iamjustic3’ as the password and was able to log in. 

<img width="1917" height="885" alt="wordpress-admin" src="https://github.com/user-attachments/assets/de55c7ba-d06e-4196-99d4-df0b5b0d2fce" />


I looked at ‘media’ and found the notes.txt uploaded, and found the directory it was in, ‘wp-content’, another common wordpress directory.

<img width="1842" height="705" alt="notes-txt-admin-page" src="https://github.com/user-attachments/assets/42bafd11-9e69-4312-ba64-708f855cab4f" />


Going to that directory, I found the notes.txt file.

<img width="1286" height="835" alt="notes-txt" src="https://github.com/user-attachments/assets/eeb7f083-8967-4bb3-8add-d778f2c19a2a" />


This looked like a password list. I took a look in the /uploads directory and also found a user text file.

<img width="988" height="544" alt="users-txt" src="https://github.com/user-attachments/assets/5ec74203-f71a-4b8b-95ab-91ed8527bbad" />


I used these two wordlists to brute force the ssh login, using hydra.

<img width="1717" height="364" alt="hydra" src="https://github.com/user-attachments/assets/b2c171aa-32a9-4356-afdd-f1fe077bdf8d" />


I used l's password and I was able to login and get the first flag.

<img width="1704" height="689" alt="l-ssh-login" src="https://github.com/user-attachments/assets/b8b6c8f7-c75f-476a-9d44-36725fbe589b" />


The flag was in ‘brainfuck’, an esoteric programming language designed to be weird. Taking that into a brainfuck interpreter got me the below message.

<img width="745" height="569" alt="user-txt-brainfuck" src="https://github.com/user-attachments/assets/ab62792a-ce77-4756-b77b-b129c7e5ae52" />


I checked the /etc/passwd file and found that ‘kira’ also had a bash shell. 

<img width="1000" height="109" alt="etc-passwd-output" src="https://github.com/user-attachments/assets/1e2cdd33-6c6f-4fcd-aee2-a849bef41464" />


I ran the ‘sudo -l’ command and found that as l I could not run sudo, so trying to find a way to pivot to kira seemed like the best bet. 

<img width="768" height="85" alt="sudo-l output" src="https://github.com/user-attachments/assets/69da9066-1953-4db3-8a82-396627c32d6a" />

In the /opt directory, I found some interesting files. ‘/opt is reserved for the installation of add-on application software packages’ according to the Linux reference documents.

<img width="893" height="443" alt="case-file-txt" src="https://github.com/user-attachments/assets/7a103e49-7b28-4cf9-9827-c1aa42b7a0c0" />


In the fake-notebook-rule folder, I found a ‘case.wav’ file which was an ASCII text document. The text content was in hex. There was also another hint that told me to use cyberchef.

<img width="1238" height="213" alt="fake-notebook-rule" src="https://github.com/user-attachments/assets/ad3b1464-3fb5-48b6-9ce3-cd147be0b7ba" />


I put that into cyberchef and came away with another password.

<img width="1438" height="579" alt="cyberchef" src="https://github.com/user-attachments/assets/e7d2b848-8d37-48b7-b878-1607ea97b4b2" />



I used ‘kiraisevil’ to switch to the kira user.


<img width="1157" height="225" alt="su kira" src="https://github.com/user-attachments/assets/b2e87bac-a6bd-4c17-a8eb-28656da8251a" />


That output at the end was in base64, and I used the command line to decode it.

<img width="1514" height="133" alt="kira-txt" src="https://github.com/user-attachments/assets/1aadb36d-8863-4476-a9b3-08960484ed8b" />



I checked the /var directory for the misa file, and found a text file. 

<img width="1026" height="203" alt="misa" src="https://github.com/user-attachments/assets/2ad13320-0ca9-4ec2-aabe-95a9303e3778" />


I ran sudo -l to see what I could run as sudo, and it turns out I could run anything as sudo. Including ‘sudo su root’. 

<img width="1375" height="211" alt="sudo-l kira" src="https://github.com/user-attachments/assets/8a41df3f-8bd1-4370-8878-743035b35912" />



