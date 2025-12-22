Chronos is a vulnerable machine on Vulnhub, rated as ‘medium’. This post will be a walkthrough of the entire machine, from making initial access to escalating privileges to root. 

After discovering the IP address of the target machine with a ping scan of my internal network, I did a service version scan of all TCP ports to find what services were running on the target machine and what versions.

<img width="1221" height="386" alt="nmap_sv_version" src="https://github.com/user-attachments/assets/d43831c4-485c-4fd6-9e48-90e571b5d96d" />


Port 22 for SSH, port 80 for http, and port 8000 also for http. I went to the web page on port 80 to do some more enumeration.

<img width="1330" height="730" alt="web_page" src="https://github.com/user-attachments/assets/3df14466-5136-4131-9382-0c3900fa2a59" />


There was nothing on the web page itself, but I looked at the page source and found the javascript.
<img width="1330" height="742" alt="page_source" src="https://github.com/user-attachments/assets/403f092c-9e09-42f3-b21b-bf653337bc3c" />


The Javascript was deliberately obfuscated to make it difficult to see what was going on at first glance. Very roughly, this was setting a user agent as “Chronos” and making a request to the web page on port 8000, and getting a date as a response. 
<img width="1820" height="459" alt="obfuscated_js" src="https://github.com/user-attachments/assets/880089a1-a3c1-4997-8ff3-3148143a6d88" />


I changed the etc/hosts file to map the IP address, 192.168.104.17, to chronos.local. 

<img width="687" height="237" alt="etc-hosts" src="https://github.com/user-attachments/assets/6a8298e1-dd57-4fe8-8e98-d2aa7522877d" />



After I did that, when I went to the web page, it displayed the exact date.
<img width="1262" height="460" alt="web_page_after_etc_hosts_edit" src="https://github.com/user-attachments/assets/c9d0a333-ee41-4afd-874c-7b0e560027a3" />



I opened Burpsuite and intercepted a request made to the backend, but it didn’t return anything just yet.
<img width="799" height="298" alt="burp_request" src="https://github.com/user-attachments/assets/8d75d942-a584-4c1b-82e0-91961f62bf8c" />

<img width="630" height="367" alt="burp_response" src="https://github.com/user-attachments/assets/a9d50418-e011-4679-b931-b7b2cf11f001" />


I also went to the URL manually.

<img width="996" height="385" alt="chronos-local-port8000" src="https://github.com/user-attachments/assets/f3ae0b37-f671-4f5e-ac0b-3f7ffabc34a8" />


I went to the URL that was in the obfuscated Javascript, but received a permission denied error.

<img width="1141" height="306" alt="without-user-agent-chronos" src="https://github.com/user-attachments/assets/17680034-3ce0-40c4-9f7a-e954282a714f" />


From this behavior, it was clear that this would only respond if the User-Agent was set as “Chronos.” A User-Agent tells the website what software you are using to access it. I changed the User-Agnet to “Chronos” in Burpsuite, and was able to get the date.

<img width="1261" height="318" alt="with-user-agent-chronos" src="https://github.com/user-attachments/assets/0161f148-bb9b-410e-8a57-0638e28cf384" />


I originally thought the encoding in the URL was base64, but upon further testing I discovered that it was actually base58.

<img width="1259" height="365" alt="base58-encoding" src="https://github.com/user-attachments/assets/aa13f5df-a9dc-4af1-9b04-4d1f53c732cd" />


The error message also gave some information on the directory format, which would come in handy later. I then took the string into cyberchef and decoded it.

<img width="1532" height="716" alt="base58-translation" src="https://github.com/user-attachments/assets/65bb0dc8-0759-4840-93f2-afffc457834a" />



I had a theory that I would be able to use this to encode a reverse shell payload. I set up a netcat listener.

<img width="759" height="202" alt="nc_listener_setup" src="https://github.com/user-attachments/assets/65c6a081-6f13-45fd-a673-b14491283408" />


I encoded a reverse shell payload with cyberchef.

<img width="1539" height="682" alt="reverse_shell_payload" src="https://github.com/user-attachments/assets/38bad24c-3e71-4c5b-87cd-050f42123702" />


And I sent the request using Burpsuite.

<img width="1260" height="752" alt="send_request_burp" src="https://github.com/user-attachments/assets/12c18457-8c31-4a95-96b8-c4836ae82209" />


I was able to catch the reverse shell.

<img width="1313" height="494" alt="reverse_shell_caught" src="https://github.com/user-attachments/assets/de6f0e04-c9df-4744-98b9-e83435cda0f2" />


I looked around a little bit more and found, in the /opt/chronos-v2/backend directory, a package.json file that showed some dependencies. One of them was express-fileupload 1.1.7.

<img width="1197" height="793" alt="package-json in opt-chronos-v2-backend" src="https://github.com/user-attachments/assets/f44509a1-53aa-4dba-9170-7e38066a1506" />


This had a remote code execution vulnerability. Following the link at the bottom, I modified an executable to get a second shell.

<img width="1034" height="866" alt="express-fileupload-vuln" src="https://github.com/user-attachments/assets/e4531113-9846-433e-9a87-d97b91269625" />
<img width="1265" height="636" alt="exploit-py" src="https://github.com/user-attachments/assets/8425d452-5bce-4e19-a15b-a1d63cc851b0" />



I started up an http server to get that onto the victim machine.

<img width="1022" height="331" alt="wget" src="https://github.com/user-attachments/assets/b68c9928-e357-42ce-9281-232fbfe6945c" />


I started up the second reverse shell to listen on port 8888.

<img width="1137" height="286" alt="starting_second_reverse_shell" src="https://github.com/user-attachments/assets/386d4475-7b04-4202-9006-4d66faa01c17" />


I then executed the script on the victim machine.
<img width="926" height="151" alt="starting_exploit" src="https://github.com/user-attachments/assets/f9744278-81b2-4e58-94fa-4a2c6d076cca" />



And I was able to get the second reverse shell, this time as the ‘imera’ user.

<img width="984" height="195" alt="catching_second_reverse_shell" src="https://github.com/user-attachments/assets/fd49b186-3508-41d5-bf0c-c8db79eb6c74" />


I went to imera’s home directory to get the first flag.

<img width="921" height="235" alt="user_flag" src="https://github.com/user-attachments/assets/db19603c-dcb0-45a1-9dbb-80d480bcefef" />

I then ran sudo -l to see what imera could run as sudo, and found that imera could run npm and node as a superuser. 

<img width="1320" height="261" alt="sudo-l" src="https://github.com/user-attachments/assets/5461a66b-4d7e-4254-b888-06dd775c863a" />


According to gtfobin, there is a way to escalate privileges using the ‘node’ command. 

<img width="1180" height="240" alt="gtfo-bin-node" src="https://github.com/user-attachments/assets/228492b4-5d24-45b6-9df1-90fa13f09878" />


I copied that command and was able to escalate to the root user.

<img width="1195" height="425" alt="final-payload-root" src="https://github.com/user-attachments/assets/ae732ce5-786a-43e6-8b4f-b066b6cdf63c" />

