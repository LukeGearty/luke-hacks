# Vulnhub Hackable 2
I first performed a ping scan of my virtual network to get the IP address of the target. I performed a ping scan with no port scanning because I wanted to do port scanning directly on the IP address.

<img width="1106" height="391" alt="nmap-sn-scan" src="https://github.com/user-attachments/assets/af9e84ed-9789-44f7-8cfd-57d52e9e4162" />

The IP address was 192.168.104.19. I then did another scan, this time on all ports, to find all the open ports, and used the -sV option to get the service versions running on those ports.

<img width="1185" height="396" alt="nmap-sv-scan" src="https://github.com/user-attachments/assets/afe287eb-2b24-4ca4-89eb-d390a6b66c0d" />

Port 21 for ftp, port 22 for SSH, and port 80 for http were running on the target. I used ftp to anonymously login to first see if that was enabled, and then to see if there were any interesting files. The only file there was CALL.html.


<img width="1064" height="429" alt="ftp-anonymous-login" src="https://github.com/user-attachments/assets/65a0bfd5-2e01-4b7a-ae81-9c1d23e8d122" />
<img width="771" height="372" alt="call-html" src="https://github.com/user-attachments/assets/1d275e6d-d888-4f96-81c8-a876fe4764df" />

I visited the web page to see what I could find.

<img width="1326" height="890" alt="web-page" src="https://github.com/user-attachments/assets/65f55f38-3a03-4498-a00a-476f8815cbe0" />


The only thing I could find was a comment in the page source, a hint that mentioned gobuster and dirb, which told me the next step was to scan for hidden directories.

<img width="1310" height="771" alt="do-you-like-gobuster-hint" src="https://github.com/user-attachments/assets/19b27c32-97ca-479e-8e2a-c7b452a0cf03" />


I used gobuster and found a directory called /files.

<img width="1272" height="563" alt="gobuster-output" src="https://github.com/user-attachments/assets/50cd23a1-6a52-4c3d-a5e6-e0c24a28a56e" />


Inside the files directory, I found the same CALL.html file from the ftp login. I guessed from here that I could use FTP to upload a script for a reverse shell on the target machine.

<img width="892" height="425" alt="files-directory" src="https://github.com/user-attachments/assets/f9b67b55-2b06-415c-b497-c2013badcfb9" />


I first got the php reverse shell from pentestmonkey.

<img width="1256" height="767" alt="php-reverse-shell" src="https://github.com/user-attachments/assets/813750c6-ea57-4056-bcb1-0713b3bf5da3" />

I then put that script on the target machine.

<img width="1258" height="488" alt="ftp-put-reverse-shell" src="https://github.com/user-attachments/assets/b4aa5415-108c-495a-8f14-483db1e42f1c" />

I set up my netcat listener on port 4444.

<img width="783" height="175" alt="nc-listener-setup" src="https://github.com/user-attachments/assets/633d90a2-f807-4191-92e0-08e3f1b0e0d9" />

When I refreshed the /files directory, the script was there.

<img width="785" height="297" alt="php-reverse-shell-in-directory" src="https://github.com/user-attachments/assets/5cd089da-88dd-4b3c-a825-7aaa83be403e" />


After I clicked on it, I had a reverse shell.

<img width="1257" height="775" alt="reverse-shell" src="https://github.com/user-attachments/assets/97983eb0-517a-4feb-826b-09a2e6d8fa9f" />


I explored the directory system for a bit and found a text file called important.txt. The file said to run a script to see data.

<img width="685" height="230" alt="important-txt" src="https://github.com/user-attachments/assets/4841a842-cfcb-47ab-bdb9-e63b3f76a4a8" />


I ran the script, and it provided me with some ASCII art for Shrek and an md5 hash of what I theorized was shrek’s password.

<img width="830" height="519" alt="runme-sh-output" src="https://github.com/user-attachments/assets/f204f994-eeaf-476d-9598-97e9327a8ff4" />


I used crackstation to check, and it turned out it matched ‘onion’. 

<img width="1460" height="489" alt="crackstation-md5-password" src="https://github.com/user-attachments/assets/dc755062-79c4-4ae4-8691-b79750fa7e24" />


I used that to login to the target as shrek via SSH, and I obtained the user flag after doing that.

<img width="632" height="571" alt="user-flag-ssh" src="https://github.com/user-attachments/assets/7b4df3ba-4675-4360-9cae-06063adfc5aa" />


I ran sudo -l to find what commands shrek could run as sudo, and found that he could run python3.5.

<img width="1217" height="211" alt="sudo-l" src="https://github.com/user-attachments/assets/4a2af945-ab9f-43a0-905e-712154bfac5f" />


I checked GTFObin to see if there was any privilege escalation available with python, and there was a way to do that.

<img width="1155" height="245" alt="gtfo-bin-python" src="https://github.com/user-attachments/assets/0664a7f7-b613-4cf5-b109-2faa8ba2828d" />


I ran the above and obtained root access and the root flag.

<img width="855" height="89" alt="exploit" src="https://github.com/user-attachments/assets/53f75fa5-486d-42f7-8e8a-38240808c071" />

<img width="805" height="742" alt="root" src="https://github.com/user-attachments/assets/c3fcfbcc-6c87-4408-b4b6-b5d7d90f324d" />



