# Digital World Snakeoil Walkthrough

This post is a walkthrough of ‘digitalworld.local:snakeoil’, a vulnerable machine found of Vulnhub. This post will walkthrough the entire process, from the enumeration to exploiting the vulnerability found to privilege escalation.

<img width="1046" height="381" alt="initial_nmap_scan" src="https://github.com/user-attachments/assets/2d3ab8d3-c1ad-4de2-8531-1d991b81844c" />

I first did a ping scan of my host network to find the IP address of the target machine, 192.168.104.23.

<img width="1076" height="416" alt="nmap_sv_scan" src="https://github.com/user-attachments/assets/2d789186-1ed1-49ae-9cf4-52b657d9183b" />

I then conducted a second nmap scan, looking for the services and their version on all TCP ports. I found three services: port 22 for SSH and ports 80 and 8080 for http.

<img width="1919" height="463" alt="first_web_page" src="https://github.com/user-attachments/assets/b15c25d4-fe3d-4a57-9dcd-82cc1fb0d119" />

I then went to the web page on port 80, which only had a link to Google.

<img width="1919" height="670" alt="second_web_page" src="https://github.com/user-attachments/assets/f5f0611f-af7e-4901-8c5f-4fe177338743" />

Port 8080 had a page for Good Tech Inc’s ‘Snake Oil Project’. There were a few posts on this page.

<img width="1919" height="518" alt="snakeoil_introduction" src="https://github.com/user-attachments/assets/28da0412-fc83-4f9c-93f8-12784d95683e" />
<img width="1919" height="541" alt="snakeoil_houserules" src="https://github.com/user-attachments/assets/340b6f60-62cb-4969-bcdb-f225b035528d" />


<img width="1919" height="513" alt="snakeoil_useful_links" src="https://github.com/user-attachments/assets/d8b423ce-acea-44a0-bacc-d714dfcca808" />


On the useful links page, there was a link to flask-jwt documentation, which would come in handy later.

I tried to test out post creation to see if anything would happen or if there was anything interesting happening, but alas this was a dead end.

<img width="1919" height="597" alt="create_post" src="https://github.com/user-attachments/assets/54335106-a7c5-4136-8597-93f3e99611ba" />

I conducted a gobuster scan to enumerate for directories. There were a few that were interesting.

<img width="897" height="543" alt="gobuster_scan" src="https://github.com/user-attachments/assets/ef7af11f-66f6-437b-9052-e5a71edcb1a4" />


The users directory had one user listed, Patrick, and his password hash. I did attempt to crack the password, but my attempts were unsuccessful.


<img width="1883" height="253" alt="users" src="https://github.com/user-attachments/assets/dbbaf9c8-13bc-4a1e-a138-dbf907bf84aa" />

I went to /registration to see what was there, but I received a JSON response telling me the method was incorrect.


<img width="913" height="263" alt="registration_page" src="https://github.com/user-attachments/assets/2aa10bf1-8f06-4eb7-9cc4-7e5811814254" />

I intercepted a request in Burp Suite and changed the method to POST, and discovered that I could create my own user with this method. I sent the request to the repeater so that I could experiment and play with the HTTP requests without having to intercept the request each time.

<img width="904" height="723" alt="burp_request" src="https://github.com/user-attachments/assets/70b36139-5b07-45af-96e8-d696260809de" />

I named my user and its password “hacker” and received an access token. I copied and saved the access token, as I had a feeling it would come in handy later.


<img width="1251" height="780" alt="hacker_created" src="https://github.com/user-attachments/assets/4ea1613d-86d9-4922-b58e-f45fb5270a1e" />

I then went to the login web page, which also needed a POST request. I used the hacker user I created earlier to login, and received an access token and a refresh token.

<img width="1247" height="696" alt="login_page" src="https://github.com/user-attachments/assets/748baf1e-f202-4f5e-91d5-93414bcb5558" />

I tried to go to /run to see what was there, and it accepted a URL as a parameter, but for it to work it required a secret. My next move was then to go to /secret.

After many attempts at using my username and password, I checked the JWT documentation and found this little tidbit:

<img width="944" height="209" alt="access_token_cookie_jwt" src="https://github.com/user-attachments/assets/5a0a756d-6c5a-41ef-bef6-d4c0a426ef57" />

I added these values in Burp Suite and received the secret.

<img width="1245" height="816" alt="secret" src="https://github.com/user-attachments/assets/c2eb2fba-bd6a-479c-9565-00206158239b" />

I plugged that secret into my request to /run and found that the command it was running was curl. I also found that it was vulnerable to command injection.

<img width="1247" height="822" alt="command_injection" src="https://github.com/user-attachments/assets/c9516594-bbbb-46e1-bf7d-18e117814d89" />

I used this to first look at the files in the current directory to see if there was anything interesting. There was app.py, the source code for the application. I used the cat command to take a look. After I opened up app.py, I found the password for patrick.

<img width="1247" height="814" alt="app_py_open" src="https://github.com/user-attachments/assets/28a79cc0-3c1e-46d7-875e-cc278ffbb768" />

I used that username and password to SSH into the target.

<img width="1082" height="342" alt="ssh_login" src="https://github.com/user-attachments/assets/d602f228-bc94-4663-a1fe-049b711b3342" />

I found that, through running the sudo -l command, Patrick could run anything with sudo privileges. I used that to switch to the root user and obtain the flag.

<img width="988" height="363" alt="i_am_root" src="https://github.com/user-attachments/assets/731292c8-89a7-43e6-b30d-f7ffae5ccb20" />

