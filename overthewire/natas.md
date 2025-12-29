# Level 0

I logged in to the web page using the password provided on the website.

<img width="1329" height="622" alt="natas0-pt1" src="https://github.com/user-attachments/assets/46dc0d4a-ec0f-44a9-a8f5-1a28fdb980d7" />

The hint was given on the page: "You can find the password for the next level on this page."

<img width="1327" height="429" alt="natas0-pt2" src="https://github.com/user-attachments/assets/1aadb5e5-10f0-4217-ac95-c311d3a7893e" />

I right clicked and viewed the page source and the password was right there in a comment.  HTML comments have this format: <!-- This is a comment-->. Comments are text in code that are ignored when the code is executed. 

<img width="1325" height="541" alt="natas0-pt3" src="https://github.com/user-attachments/assets/dcb231a1-22c0-44b1-8cee-4f9c5461d6d5" />

natas1
0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq

# Level 1

I logged in to the web page using the password from the previous level. 


The hint said that right clicking was blocked on this page.

<img width="1333" height="584" alt="natas1-pt1" src="https://github.com/user-attachments/assets/a8a3f6be-a19f-423f-86fa-41bd93230c22" />

Another way to view page source is to use ctrl+U (or cmd+U). 

<img width="1325" height="532" alt="natas1-pt2" src="https://github.com/user-attachments/assets/47aede94-019f-4f9d-a4dd-8e32644c76c2" />

natas2
TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI

# Level 2
I logged in to the web page using the password from the previous level, and the hint on the page stated that there was nothing on this page. 

<img width="1332" height="487" alt="natas2-pt1" src="https://github.com/user-attachments/assets/e719579e-1cec-4755-8adb-787d7a81cbe1" />

The page source also had nothing in it. 

<img width="1323" height="334" alt="natas2-pt2" src="https://github.com/user-attachments/assets/e4c8aae1-000a-4450-8840-96ad595bacef" />

There was however, a small image, in a directory called files.

<img width="511" height="55" alt="natas2-pt3" src="https://github.com/user-attachments/assets/269d14bc-e51a-456f-9fe9-16a271b22de3" />

I went to that directory and found the image displayed, along with a file called users.txt

<img width="860" height="330" alt="natas2-pt4" src="https://github.com/user-attachments/assets/8aafeec5-b2ce-4715-a694-2694a3316fd2" />

Users.txt was a file that contained usernames and passwords, including the password for natas3.

natas3
3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH

# Level 3
I logged in to the web page using the password from the previous level, and found that it contained the same hint as level 2. "There is nothing on this page."

<img width="1328" height="557" alt="natas3-pt1" src="https://github.com/user-attachments/assets/f5e205d2-3db6-4aff-985f-3f1ab775d227" />

I checked out the page source and there was a comment that said "not even google will find it this time..."

<img width="1327" height="479" alt="natas3-pt2" src="https://github.com/user-attachments/assets/b30b8c85-094b-4a6a-89ac-7aad9feb89fc" />

The robots.txt file is a text file that tells search engine crawlers which directories to ignore. The hint is stating that 'not even google will find it', meaning that there is a directory that will not show up in google searches.

I checked out the robots.txt file and saw that there was a secret directory called s3cr3t.

<img width="1330" height="375" alt="natas3-pt3" src="https://github.com/user-attachments/assets/deec004b-fdb9-4831-8b6e-4320dc232ade" />

In that s3cr3t directory was a users.txt file that contained the password for the next level.

<img width="1335" height="371" alt="natas3-pt4" src="https://github.com/user-attachments/assets/e84a8d1a-dc09-438c-81bd-9e917dfe5bda" />

natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ

# Level 4
After I logged in, the web page stated 
"Access disallowed. You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/""

<img width="1317" height="488" alt="natas4-pt1" src="https://github.com/user-attachments/assets/2c140ee8-77ca-41b8-b858-99e86e83e9bc" />

I clicked on the refresh page button, and the message changed to the below.
<img width="1329" height="516" alt="natas4-pt2" src="https://github.com/user-attachments/assets/75bd2b2a-ed88-4df8-af35-7bea90ec76b8" />


I opened up Burpsuite and intercepted a request after clicking the refresh button. In the referer, I changed it to the correct URL. 
<img width="1263" height="396" alt="natas4-pt3" src="https://github.com/user-attachments/assets/622c19fc-21c4-4244-b0c1-43d12bbd6a01" />
<img width="1321" height="539" alt="natas4-pt4" src="https://github.com/user-attachments/assets/601380cb-2682-4b75-84ee-d2c38c44865a" />

natas5
0n35PkggAPm2zbEpOU802c0x0Msn1ToK

# Level 5
After I logged in to the web page, it said "Access disallowed. You are not logged in".

<img width="1329" height="530" alt="natas5-pt1" src="https://github.com/user-attachments/assets/d32ab5e3-ed31-405d-b8ae-3c4d5152f56e" />

I utilized Burpsuite to intercept a request, and found that there was a Cookie sent where the value was 'loggedin=0'.

<img width="1260" height="316" alt="natas5-pt2" src="https://github.com/user-attachments/assets/e28758d5-7225-474b-a16f-b48f39ec597d" />

I changed that to 1 to get the password for the next level.

<img width="1328" height="602" alt="natas5-pt3" src="https://github.com/user-attachments/assets/32917bc9-5ab9-45fa-b0f3-7acf7bc95ba7" />

natas6
0RoJwHdSKWFTYR5WuiAewauSuNaBXned


