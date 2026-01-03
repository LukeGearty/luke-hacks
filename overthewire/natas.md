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

# Level 6
I found a web page that has an input form that says “input secret”. 

<img width="1330" height="517" alt="natas6-pt1" src="https://github.com/user-attachments/assets/e10e004b-5fb0-4e43-89c2-3a6303f94ee8" />


Inputting anything gets a message that states “wrong secret”.

<img width="601" height="203" alt="natas6-pt2" src="https://github.com/user-attachments/assets/0b96d1c9-dbf8-4c99-ac81-269dcf85dda4" />

There is a button to look at the source code. The code is simply checking if the user input secret matches a specific string, and if it does then access is granted. That string, called secret, is in a ‘includes/secret.inc’ file, not in the source code itself. 

<img width="655" height="373" alt="natas6-pt3" src="https://github.com/user-attachments/assets/a0a32d46-1f29-48ed-8b21-5433c644592e" />

I went to that directory, which was just a blank page. I checked the source code and found the secret.

<img width="994" height="362" alt="natas6-pt4" src="https://github.com/user-attachments/assets/cb657286-a53e-46d9-a004-4ad9195ba99e" />

<img width="915" height="392" alt="natas6-pt5" src="https://github.com/user-attachments/assets/274f0125-ec91-4df0-9272-9cd4350340c9" />


I input that string and received the password for the next level. 

<img width="600" height="220" alt="natas6-pt6" src="https://github.com/user-attachments/assets/ac90da0f-873d-4c1e-922e-f5cb6d1a2ea5" />

Natas7
bmg8SvU1LizuWjx3y7xkNERkHxGre0GS


# Natas7

I went to the web page for this level, and there were two links, one to a home page and one to an about page.

<img width="1329" height="422" alt="natas7-pt1" src="https://github.com/user-attachments/assets/9994018f-7cf3-4f80-b3ef-15f46cb3beb4" />


I clicked on the home link, and the web page changed to say ‘this is the front page’. The URL changed to include ‘index.php?page=home’. 

<img width="1333" height="424" alt="natas7-pt2" src="https://github.com/user-attachments/assets/a72ba21d-f791-49f4-b143-ecc72b4c8479" />

I looked at the page source and saw this comment.

<img width="773" height="153" alt="natas7-pt3" src="https://github.com/user-attachments/assets/6e0a2353-11d4-473d-b504-cb67b269bc9f" />


Local file inclusion is a vulnerability where an attacker can make a web page display files from the server filesystem that are not intended to be displayed. In the URL, I changed the ‘?page=home’ to ‘?page=/etc/natas_webpass/natas8’ and received the password for the next level.

<img width="1325" height="425" alt="natas7-pt4" src="https://github.com/user-attachments/assets/46c4bd7d-de1a-42a4-b467-0f77fb38a27f" />

Natas8
xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q 

# Level 8

I logged in to the web page and found the input secret form.

<img width="1329" height="495" alt="natas8-pt1" src="https://github.com/user-attachments/assets/3ca037c7-9b09-4e60-9c90-dc5ba4e10e59" />


I checked the source code and found that if the input secret matches the secret in the code, then the user will be granted access.

<img width="643" height="256" alt="natas8-pt2" src="https://github.com/user-attachments/assets/7445d58d-1526-45fb-88c5-2858d7b34bb2" />


The trick was that the user secret goes through a function to encode it, and the encoded secrets are compared. The encodeSecret function encodes a string with base64, then reverses it, then encodes that reversed string to hex.

I took the encodedSecret string and used cyberchef to reverse that process to get the correct secret.

<img width="1538" height="597" alt="natas8-pt3" src="https://github.com/user-attachments/assets/4276ac61-07e7-4c08-9d5d-6537c03fec9a" />

<img width="603" height="221" alt="natas8-pt4" src="https://github.com/user-attachments/assets/e4fdb4c6-ba26-422a-bf92-a1041ed7de33" />


Natas9

ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t

# Level 9

I logged in to the level and found an input form that searched for words containing whatever input I gave it.

<img width="1332" height="560" alt="natas9-pt1" src="https://github.com/user-attachments/assets/c5a1d227-eb93-4d72-a6bd-4d1b71c4c20f" />


I tested this out just searching for words containing ‘a’.

<img width="1324" height="897" alt="natas9-pt2" src="https://github.com/user-attachments/assets/d253f3d7-6544-4efb-9490-9278d7748446" />

I looked at the source code and found that the code was taking the input, called $key in the code, and executing a command, ‘grep -i $key dictionary.txt’.

<img width="891" height="340" alt="natas9-pt3" src="https://github.com/user-attachments/assets/b019c86d-4d40-461d-ae4c-892f826b6adb" />

Passing user input directly to a command without sanitization is dangerous, because the user can execute their own commands on the system. I passed this payload, ‘; cat /etc/natas_webpass/natas10;’ to the user input and received the password for the next level. On the Linux command line, the semicolon is a command separator that allows the user to execute multiple commands sequentially. 

<img width="599" height="239" alt="natas9-pt4" src="https://github.com/user-attachments/assets/51c8c593-a8e8-4f33-8d2b-ff1f89658290" />


Natas10
t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu

# Level 10
This level was similar to the last level, but now there was some filtering being done.

<img width="602" height="247" alt="natas10_pt1" src="https://github.com/user-attachments/assets/961fbffe-27fb-4bc0-9557-9cf8bd5f0048" />

I looked at the source code, and it filtered on characters like a semicolon or ampersand, so using the same trick as the last level would not work.

<img width="587" height="229" alt="natas10_pt2" src="https://github.com/user-attachments/assets/ed7b2972-0960-4419-a58b-0834d9d16cc0" />



However, it is possible to pass multiple files to grep, and it is known that the password for the next level was in the /etc/natas_webpass directory. I passed this payload: `a /etc/natas_webpass/natas11` to receive the password for the next level.

<img width="1325" height="889" alt="natas10_pt3" src="https://github.com/user-attachments/assets/196253e6-5120-440a-b25b-29127c380e79" />


Natas11
UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk

# Level 11

This level had a form where the user could type in a hex code and change the background color. It also had a message that said “cookies are protected with XOR encryption”. 

<img width="626" height="213" alt="natas11_pt1" src="https://github.com/user-attachments/assets/3e03ef20-d6e0-4d11-a73a-1b641255fba0" />


I looked at the source code, and to summarize, a json with a ‘showpassword’ value of ‘no’ along with the hexcode of the current background color, is XORed and base64 encoded, then set as the cookie. If the value of showpassword is yes, then the password will (naturally) be shown. 

<img width="1056" height="666" alt="natas11_pt2" src="https://github.com/user-attachments/assets/f3915702-135c-433e-819f-166945d3990c" />

I got the text of the cookie. Since I had the plaintext and the ciphertext, I wrote a short python script to get the key used to encrypt the plaintext.
<img width="1036" height="277" alt="natas11_pt3" src="https://github.com/user-attachments/assets/979ccce0-5406-42cb-8ed2-9b39aea49eb1" />
<img width="1025" height="697" alt="natas11_pt4" src="https://github.com/user-attachments/assets/9280c134-a6cb-4099-8d59-5a5366e7e60d" />



Since I had the key, I could encrypt a new cookie that would show the password. The plaintext of the cookie that would show the password would have “showpassword”: “yes” along with the background color. I modified the above script to create the new cookie.
<img width="957" height="469" alt="natas11_pt5" src="https://github.com/user-attachments/assets/358dbd7d-68f6-4450-b244-3f6571526dd6" />

<img width="912" height="103" alt="natas11_pt6" src="https://github.com/user-attachments/assets/557e352f-a306-4f4d-bfe2-76bccea22cdf" />



I then plugged the above value into the browser and received the password for the next level.

<img width="599" height="236" alt="natas11_pt7" src="https://github.com/user-attachments/assets/37236806-a576-458b-887d-0daf3649e178" />


Natas12
yZdkjAYZRd3R7tq7T5kXMjMJlOIkzDeB

# Level 12

The web page for this level was a form to upload a jpeg file. 

<img width="663" height="234" alt="natas12-pt1" src="https://github.com/user-attachments/assets/9c41e265-ce34-4ac6-b9fd-144781351e23" />

I looked at the source code, and a random path is generated and becomes the path to the jpeg when it is uploaded.

<img width="887" height="634" alt="natas12-pt2" src="https://github.com/user-attachments/assets/3cbaed9f-8019-4fc4-a8ba-62d311344981" />

From the source code, there was no kind of validation, meaning that I could upload any kind of file and it would be accepted.

<img width="598" height="136" alt="natas12-pt3" src="https://github.com/user-attachments/assets/ffe6dbe1-618d-4287-974e-8b5b585f3175" />

I looked at the page source and saw the random name, and this was being done client side. I could upload any kind of file and change the name/file extension in the developer tools.

<img width="690" height="215" alt="natas12-pt4" src="https://github.com/user-attachments/assets/5462e167-1899-4a4a-b9a9-2a080a6e9e4d" />

I wrote a short php file that would accept a command as an argument in the URL, for the variable ‘x’. 

<img width="727" height="96" alt="natas12-pt5" src="https://github.com/user-attachments/assets/45636697-9002-4b52-9e73-c8a4dc6c00f0" />

I uploaded the file.

<img width="599" height="145" alt="natas12-pt6" src="https://github.com/user-attachments/assets/ff43bf62-4a42-41ff-ab10-1fc48798b91b" />

And then used this argument `x=cat /etc/natas_webpass/natas13` to get the password for the next level.

<img width="1139" height="172" alt="natas12-pt7" src="https://github.com/user-attachments/assets/447a6ae2-5b7e-4280-9654-c53e764f60c1" />

Natas13
trbs5pCjCrkuSknBBKHhaBxq6Wm1j3LC

# Level 13
This level was similar to the previous level, except now there was some validation of the kind of file, meaning that I couldn't just upload a text file and have it be accepted.

<img width="611" height="244" alt="natas13-pt1" src="https://github.com/user-attachments/assets/3e0c0b17-3238-495a-96ae-b6bf3de5a8fe" />

I looked at the source code and the code was using exif_imagetype to check the type of file. Exif_imagetype checks the first few bytes of a file to determine the file signature and file type.

<img width="773" height="739" alt="natas13-pt2" src="https://github.com/user-attachments/assets/e376b39d-74fc-420a-85a9-f16c5a776c52" />


The below screenshot was the response when I uploaded a text file.

<img width="608" height="242" alt="natas13-pt3" src="https://github.com/user-attachments/assets/6d56205b-8a72-4878-bbab-a48fceba62c8" />

I changed the first few bytes of the previous level's exploit in order to trick the program into thinking a jpeg had just been uploaded.

<img width="719" height="114" alt="natas13-pt4" src="https://github.com/user-attachments/assets/42505a80-a6dd-40a6-b558-121585698f8a" />


I changed the form in the developer tools to accept a .php file extension.

<img width="975" height="842" alt="natas13-pt5" src="https://github.com/user-attachments/assets/83938c64-e78a-47be-a612-9cf0244c21e2" />


I then performed the same technique as the last level to get the password.

<img width="1101" height="440" alt="natas13-pt6" src="https://github.com/user-attachments/assets/2a9be6a2-2509-4934-8e18-ccaa241575dd" />


Natas14
z3UYcr4v4uBpeX8f7EZbMHlzK4UR2XtQ

# Level 14
The web page contained a username and password login form. 

<img width="652" height="233" alt="natas14-pt1" src="https://github.com/user-attachments/assets/ddbdf248-af26-4c69-bcc3-c4aec5a9677c" />

I looked at the source code and saw that there was a SQL injection vulnerability.The code was directly inserting user input into the query string without sanitizing it.

<img width="1029" height="283" alt="natas14-pt2" src="https://github.com/user-attachments/assets/13ec0f86-3df2-4ab0-9de8-dce6f4bbf826" />


An unsuccessful login looked like the below screenshot.

<img width="646" height="188" alt="natas14-pt3" src="https://github.com/user-attachments/assets/5a22ea43-e43f-435c-9b19-3e0ec9d038be" />

But a user could add ‘debug’ to the request parameters to see the SQL query.

<img width="1328" height="400" alt="natas14-pt4" src="https://github.com/user-attachments/assets/95b47429-2e74-4acc-905a-94ccf95f0351" />

I modified the request URL to evaluate to a condition that is always true in order to receive the password for the next level.

<img width="1327" height="463" alt="natas15-pt5" src="https://github.com/user-attachments/assets/b6e97823-84ab-4487-9e6f-b635227cdf82" />

Natas15
SdqIqBsFcz3yotlNYErZSZwblkm0lrvx

# Level 15

Level 15

The web page had a form to check whether a username exists. 

<img width="665" height="241" alt="natas15-pt1" src="https://github.com/user-attachments/assets/fb263fe5-accd-4e97-b91d-13a7306fbb77" />

<img width="709" height="198" alt="natas15-pt2" src="https://github.com/user-attachments/assets/1082bbe5-8633-4186-a6d0-79f591961894" />

In the source code, the user input was once again being put into the SQL query. However, the only thing that would be returned to the user was a string that said whether the user existed or not. 

<img width="696" height="372" alt="natas15-pt3" src="https://github.com/user-attachments/assets/6c695610-4a4e-4c72-b0fc-5ed84ebc8e0c" />


The web page was vulnerable to SQL injection, and I manually tested it to ensure it was, but the only thing returned was confirmation of whether the user exists (True) or not (False).

<img width="602" height="185" alt="natas15-pt4" src="https://github.com/user-attachments/assets/9101b25f-8ada-44ff-9852-6fa5191ede3e" />

<img width="628" height="166" alt="natas15-pt5" src="https://github.com/user-attachments/assets/5c28c4b4-b6a3-486a-bb4b-31d94e313c7d" />

This was vulnerable to a Blind SQL injection, where an attacker infers information from behavior of the application. For example, an attacker can use a Blind SQL injection to guess the characters in a password. 

This would take a very long time manually, but using sqlmap the process can be automated. I utilizing this command:

`sqlmap -u http://natas15.natas.labs.overthewire.org/index.php?debug --string="This user exists" --auth-type=Basic --auth-cred=natas15:SdqIqBsFcz3yotlNYErZSZwblkm0lrvx --data "username=natas16" --level=5 --risk=3 -T users -C username,password --dump`

To get the password for the next level.

<img width="1081" height="618" alt="natas15-pt6" src="https://github.com/user-attachments/assets/8c4e7110-fe35-4fb9-a09b-32c37eef1a37" />

<img width="1088" height="387" alt="natas15-pt7" src="https://github.com/user-attachments/assets/b3eddc92-51f1-46ea-b49f-6caaf931e996" />


Natas16
hPkjKYviLQctEW33QmuXL6eDVfMW4sGo

