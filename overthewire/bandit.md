# Level 0
The primary goal is to log in to the game using ssh, on port 2220. The username is bandit0 and the password is bandit0.

```
ssh bandit0@bandit.labs.overthewire.org -p 2220

```

<img width="741" height="788" alt="bandit0" src="https://github.com/user-attachments/assets/c0966cd6-c15d-46be-b81f-33d0e64cb9ca" />


# Level 0 -> 1

The password for the next level is stored in a file called readme in the home directory.

I used the ls command to find the file, and then used the cat command to open and read the file. 

<img width="706" height="211" alt="bandit1" src="https://github.com/user-attachments/assets/dfcaff65-41b2-4d62-881a-1193bd89dc99" />

username: bandit1
password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
# Level 1->2
The password for the next level is stored in a file called '-' located in the home directory.  The hint for this level is to google 'dashed filename'. I found this stackoverflow:
https://stackoverflow.com/questions/42187323/how-to-open-a-dashed-filename-using-terminal
That explains what to do to open the file. Using - as an argument refers to stdin/stdout, so to open a file with that name, you need to specify the full path. 

<img width="624" height="69" alt="bandit2" src="https://github.com/user-attachments/assets/36c9ffb2-a7e7-40e3-87d9-a0e53255b208" />

username: bandit2
password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx
# Level 2->3
The name of the file containing the password is

```
--spaces in this filename--
```
The hint for this was to google spaces in filename.
To open a file/directory from the command line, you either need to quote the file name such as:
```
cat 'File Here'
```
Or use the escape character
```
cat File\ Here
```
Since the filename for this level uses dashes, we have to use the technique from the previous level as well to open the file.ls

username: bandit3
password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx

<img width="475" height="182" alt="bandit3" src="https://github.com/user-attachments/assets/6a85c9ee-5443-464a-9a61-409384e2c487" />

# Level 3->4
The password for the next level is stored in a hidden file in the 'inhere' directory. 

To find hidden directories, you would use the 'ls' command with the -a option. The man page (manual) for ls describes it as: 
```
-a, --all
              do not ignore entries starting with .
```
<img width="437" height="94" alt="bandit4" src="https://github.com/user-attachments/assets/a21f6323-2377-488f-96e9-e6bd175a2425" />

username: bandit4
password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

# Level 4->5
The password for the next level is stored in the only human-readable file in the inhere directory. 

There are many ways to do this, but the way I solved this was to use the following sequence of commands:

```
find ./inhere -type f | xargs file | grep text
```

To go through this, in the first part I am searching for files in the inhere directory. Xargs is a command that reads items from standard input and executes a command, in this case file, which checks the type of file. Grep text is just looking for the string text in the output.

<img width="607" height="107" alt="bandit5" src="https://github.com/user-attachments/assets/ffc3680f-e0e5-443f-be21-5e7caaf43112" />


username: bandit5
password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

# Level 5->6
The password is stored in a file in the inhere directory and has all the following properties:
1. human readable
2. 1033 bytes in size
3. not executable

Building on the command from the last level:

```
find ./inhere -type f -size 1033c \! -executable | xargs file | grep text
```

Modifying it a little bit, I added to the find command -size 1033c for 1033 bytes, and the \! -executable to find files that are not executable.

username: bandit6
Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

<img width="873" height="122" alt="bandit6" src="https://github.com/user-attachments/assets/cfe8a332-d68e-4aa1-94dc-cae1b1c9c477" />


# Level 6->7

The password for the next level is stored somewhere on the server and has the following properties:
1. Owned by user bandit7
2. Owned by group bandit6
3. 33 bytes in size

```
find / -user bandit7 -group bandit6 -size 33c -type f 2>/dev/null
```

Breaking down the command, I am searching from the root directory for files owned by user bandit7 and group bandit6, 33 bytes in size. I am using 2>/dev/null to redirect stderr to /dev/null, which is like a trash can for any errors that a command generates. Find will generate a lot of errors, especially if you are searching from root, so you will want to use that liberally.

<img width="723" height="136" alt="bandit7" src="https://github.com/user-attachments/assets/98c0ffcc-04f9-431a-94e5-309ce132b31f" />

username: bandit7
password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj

# Level 7->8

The password for the next level is stored in the file data.txt next to the word millionth.

Using the wc -w command to see the word count, there are 197,133 words in this file. To search for a particular word, you can use the 'grep' command.

```
cat data.txt | grep "millionth"
```

The | symbol, also called a pipe, takes the output of one command and sends it to another. You can also just use:
```
grep "millionth" data.txt
```


<img width="640" height="796" alt="bandit8" src="https://github.com/user-attachments/assets/78a39106-cef6-4a11-8703-6742c1a2bfa6" />


username: bandit8
password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc

# Level 8->9

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once. 

The uniq command is meant for this kind of task. Using the -u option, you can print unique lines in a file. However, the entries in the file must be sorted before you do that. According to the man page:

```
uniq' does not detect repeated lines unless they are adjacent.  You  may  want to sort the input first, or use 'sort -u' without 'uniq
```

So the full command is:

```
sort data.txt | uniq -u
```
<img width="715" height="772" alt="bandit9" src="https://github.com/user-attachments/assets/5fb765c8-5d1a-45db-9a3c-5ac3db20a470" />

username: bandit9
password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

# Level 9->10
The password is stored in data.txt in one of the few human-readable strings, preceeded by several '=' characters.

The strings command is used to extract human-readable strings (aka printable ASCII) from files. Utilizing that command, and using the grep command to search for the = character, you can get the flag.


<img width="523" height="298" alt="bandit10" src="https://github.com/user-attachments/assets/8efdae3c-150c-4760-89de-d6e27bbf42ca" />


username: bandit10
password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

# Level 10->11
The password for the next level is in data.txt, and it contains base64 encoded data.

Base64 is a binary-to-text encoding scheme that transforms binary data to a sequence of printable characters. It is not encryption, although it does look like encryption at a first glance if you were unfamiliar with it. 

You can encode in base64 from the command line just utilizing the base64 command. base64 -d decodes.

```
cat data.txt | base64 -d
```


<img width="608" height="116" alt="bandit11" src="https://github.com/user-attachments/assets/700afbb7-9cca-44fd-ab15-a0fc2ca1c5af" />

username: bandit11
password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

# Level 11->12

The password for the next level is stored in the file data.txt, where all lowercase and uppercase letters have been rotated by 13 positions.

This is a Rot13 substitution cipher, which replaces a letter with the 13th letter after it in the alphabet. A becomes N, B becomes O, etc. 

There are many tools to reverse this, but I used cyberchef. I opened the file using cat, copied the contents and pasted it into cyberchef.

<img width="555" height="93" alt="bandit12_1" src="https://github.com/user-attachments/assets/53cf5e0c-6339-4958-8a68-d8e939d60f7d" />

<img width="564" height="526" alt="bandit12_2" src="https://github.com/user-attachments/assets/ac0f7281-174a-4337-97d9-47bb74cfa56c" />

username: bandit12
password: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

# Level 12->13

The password is in data.txt, which has a hexdump of a file that has been compressed. It is recommended to create a temporary directory, copy the datafile to that temporary directory and rename it.

To do as the instructions suggested, use the mktemp -d command to make a temporary directory. Use the cp command to copy data.txt to that directory, and use the mv command to rename a file. As an example:

```
mktemp -d
cp data.txt /tmp/tmp.veeHcJKbht
cd  /tmp/tmp.veeHcJKbht
mv data.txt hexdump
```

<img width="541" height="176" alt="bandit13_1" src="https://github.com/user-attachments/assets/5e126a44-c353-429d-95bc-407411841df0" />


Since this is a hexdump, you can use the xxd -r command to reverse the hexdump. We can output that to a new file.

<img width="702" height="124" alt="bandit_13_2" src="https://github.com/user-attachments/assets/c56fa636-de8c-4363-b6e3-3611c8e3ace3" />

The next few steps were long and tedious. It consisted of finding out what kind of compressed file the output was, renaming it to match the file extension, and using the corresponding command to unzip the file.

<img width="704" height="724" alt="bandit13_2" src="https://github.com/user-attachments/assets/52a9dc0d-e077-48c2-9f51-218ecaad943d" />

<img width="711" height="258" alt="bandit13_3" src="https://github.com/user-attachments/assets/5c0e32ed-a3e4-436c-abe9-53ae4e48de69" />

username: bandit13
password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

# Level 13->14

The password for the next level is stored in **/etc/bandit_pass/bandit14** and can only be read by user bandit14. You don't get the next password, but you get a private ssh key that be used to log into the next level.

Using private ssh keys are a way to log in to a system without a password. The way to do it is with the -i option.

```
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```
<img width="719" height="767" alt="bandit14_1" src="https://github.com/user-attachments/assets/5fa89786-a6ce-4d6f-96e5-84a70a588155" />


<img width="446" height="81" alt="bandit14_2" src="https://github.com/user-attachments/assets/f64e7fd9-0a63-46a5-911c-b5bb0c7d42d1" />


username: bandit14
password: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

# Level 14 ->15
The password for the next level can be retrieved by submitting the password of the current level to port **30000** on localhost.

You can use the netcat to connect to a machine on a specific port. It is used to write and read data between two computers.

```
nc localhost 30000
```

<img width="690" height="121" alt="bandit15" src="https://github.com/user-attachments/assets/64ddd7b7-e50a-4f1b-99e3-e42f65763e92" />


username: bandit15
password: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

# Level 15->16

The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using ssl/tls encryption.

The plain nc command does not support encryption, but a reimplementation of the nc tool, ncat, has additional features that support encryption. You can just specify the --ssl option to enable encryption.

```
ncat --ssl localhost 30001
```


<img width="502" height="138" alt="bandit16_1" src="https://github.com/user-attachments/assets/f8e8bd3a-b5e8-4397-8c69-65f54317cdc4" />


As another method of solving this, you can also use the openssl command. 

<img width="707" height="703" alt="bandit16_2" src="https://github.com/user-attachments/assets/da7d84a8-f3b1-41a8-aa8e-ff59da51e4f7" />


username: bandit16
Password: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

# Level 16->17

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. Only one of these ports has a SSL/TLS server listening on them, others will just send back what you send to it.

To see the ports and services running on a machine, nmap, the network mapper, can be used to scan ports and services. We can use the -p option to specify ports, and use the service version scan to see what those ports are running. Typically nmap will just make assumptions on what is running, so -sV is needed.

```
nmap -sV -p 31000-32000 localhost
```
<img width="652" height="188" alt="bandit17" src="https://github.com/user-attachments/assets/b677ac70-d261-4f13-b551-18043c453100" />

From the above, there are 5 ports open. Of the 5, 2 are running ssl servers. Of those 2, one is running an ssl/echo, port 31790, so that might be our port.

The clue gives this as a hint:
**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

If you attempt to connect via openssl, you will get a keyupdate. According to this stackoverflow:
https://stackoverflow.com/questions/58547963/openssl-keyupdate-and-ssl-key-updatewrong-ssl-version

This is triggered via K/k, and since the password starts with k it will triggert the keyupdate. To mitigate this, use this command:
```
openssl s_client -ign_eof localhost:31790
```


<img width="715" height="557" alt="bandit17_1" src="https://github.com/user-attachments/assets/03b1fe47-bd32-4f6f-a7dd-c642056c3e9f" />


You get a private ssh key. Copy that over to your home terminal, paste it into a text file, change permissions to 600 (owner of the file can read and write) and you can use it to log in.

<img width="650" height="764" alt="bandit17_3" src="https://github.com/user-attachments/assets/76503d6c-76ab-4ba5-bc16-524fb11ee7fd" />



username: bandit17
password: EReVavePLFHtFlFsjn3hyzMlvSuSAcRD

# Level 17 -> 18
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the new level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new.

To check the contents of two files to see what is different between them, you would use the diff command. 

<img width="532" height="211" alt="bandit18" src="https://github.com/user-attachments/assets/a351f19e-6f08-4724-a384-748c126c2793" />

username: bandit18
password: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

# Level 18 -> 19

The password is in a file called readme in the home directory. Unfortunately, someone has modified .bashrc to log you out when you log in.

When you use the password from the last level, you do indeed get logged out immediately.

<img width="698" height="335" alt="bandit19_1" src="https://github.com/user-attachments/assets/f35a4a39-8ff6-4e26-831a-7a56cd9383be" />



You can however execute commands after attempting an ssh login. To get the password, I used this command:

```
ssh bandit18@bandit.labs.overthewire.org -p 2220 'command; cat readme'
```

<img width="675" height="260" alt="bandit19_2" src="https://github.com/user-attachments/assets/4b4c79dc-1435-4831-ae96-f2b62dc43690" />


username: bandit19
password: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

# Level 19->20
To gain access to the next level, there is a setuid binary in the home directory that must be used.

A setuid binary is an executable file that runs with the permissions of the owner, not the user who executed it. The binary in the home directory is called bandit20-do. 

Taking a look at it:

<img width="552" height="183" alt="bandit20_1" src="https://github.com/user-attachments/assets/0725a619-6926-4c4d-8fc2-5b1a53bccd76" />


It allows the user executing it to run a command as the owner, in this case bandit20. Recall from earlier levels that there is a directory that stores all the passwords for all the users. 


<img width="563" height="138" alt="bandit20_2" src="https://github.com/user-attachments/assets/339ff90a-09be-438b-9b26-c5e1ff033e7f" />


username: bandit20
password: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

# Level 20->21

There is a setuid binary in the home directory that makes a connection to localhost on the port you specify as a commandline argument, then reads a line of text from the conenction and compares it to the password in the previous level. 

This level requires that you ssh into bandit20 on two separate tabs. On one tab, you set up a netcat listener on a port of your choice, and on the other tab you use the binary to connect to the listener. Then on the tab with your netcat listener, send the old password and get the new password.


<img width="489" height="105" alt="bandit21_1" src="https://github.com/user-attachments/assets/e2df4553-5592-417f-89fa-ee4720a4727b" />

<img width="538" height="58" alt="bandit21_2" src="https://github.com/user-attachments/assets/e196c577-2bb1-4752-9b5a-18537114b196" />


username: bandit21
password: EeoULMCra2q0dSkYj561DX7s1CpBuOBt

# Level 21 -> 22

A program is running at regular intervals from cron. To get the password, we need to look into /etc/cron.d for the configuration and see what command is being executed. 

Cron is a scheduling command that runs a job, a computational task, at specified times. It is used if you need something run at regular intervals. Looking into the directory, there is a cron job running, /usr/bin/cronjob_bandit22.ssh. 

<img width="708" height="226" alt="bandit22" src="https://github.com/user-attachments/assets/6552838a-0cd9-4156-a723-f48186c7bc9b" />

username: bandit22
password: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

# Level 22 -> 23
Like the previous level, a program is running at regular intervals and the key to the password of the next level is with that password. The hint talks about looking at shell scripts written by other people. 

The script looks like this:

<img width="620" height="330" alt="bandit23_1" src="https://github.com/user-attachments/assets/acf563ee-fe71-498a-a231-5032abdef0fe" />


Going through it:
First it sets a variable to be the name of the user who calls it, myname.
Then it takes a md5 hash of the string (I am user $myname).
Then it copies the password of the user calling it to a file with the name of that hash in the /tmp directory.

Reversing this a bit to take a md5sum where the name of the user is bandit23:

<img width="614" height="138" alt="bandit23_2" src="https://github.com/user-attachments/assets/c17f10b0-4d17-47ba-8ea2-3841ddb3a15b" />


username: bandit23
password: 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

# Level 23->24
This level is much like the last one where there is a cron job running at regular intervals. The hint for this level requires the creation of our own shell script. 

Taking a look at the program running: 

<img width="739" height="406" alt="bandit24_1" src="https://github.com/user-attachments/assets/4b800b95-1ea1-4bfb-8a18-031f1ee2f80b" />

At a high level, this script will execute and then delete all the scripts in the foo directory belonging to the executor of the script. Since the executor is bandit24, the full directory would be /var/spool/bandit24/foo.

We can create a script that will open up the password file for bandit24, place it into the foo directory, and wait for it to be executed. 

To solve this, I created a temporary directory and and password file, then created a shell script with this in it:

```
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/tmp.directory/password
```

This will open the bandit24 password and redirects the output to the new password file. I then changed everything to be world writeable (I was lazy here and just gave everything every position). I copied it into the /foo directory and waited a minute and had the password.

<img width="701" height="272" alt="bandit24_2" src="https://github.com/user-attachments/assets/c5b6e1d2-2910-4dd2-ab82-d7a58a364a03" />

username: bandit24
password: gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

# Level 24 -> 25
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. The only way to get the pincode is to brute-force the pincode. 

This will require writing another script, but first I wanted to see what happens when you connect on the port.

<img width="1202" height="134" alt="bandit25_1" src="https://github.com/user-attachments/assets/90e60ff5-0efa-4e0e-b3da-4be7cb477a53" />

The wrong answer will have 'Wrong!' and there needs to be a space between the password and the number.

```
#!/bin/bash

for i in {0000.9999}
do
	echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i" >> wordlist.txt
done
```

This will  just go through the entire process of writing the password and the pin number from 0000 to 9999, and get that into a wordlist text file. 

<img width="1200" height="524" alt="bandit25_2" src="https://github.com/user-attachments/assets/b1470393-c7d4-40fc-a79f-af7db3106358" />


After the wordlist was generated, used that to generate the responses, and looked the results for anything that didn't have "Wrong!".

username: bandit25
Password: iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

# Level 25->26
The hint for this level states that logging in to bandit26 from bandit25 should be easy, and the shell for bandit26 is not /bin/bash.

Looking at the /etc/passwd file will reveal the shell for bandit26 is /usr/bin/showtext.

<img width="610" height="74" alt="bandit26_1" src="https://github.com/user-attachments/assets/388abaa0-158a-4356-b33d-ab0d5a45d809" />

There is also a ssh key for bandit26 in the bandit25 home directory. However if we use that, we get immediately logged out.

<img width="962" height="487" alt="bandit26_2" src="https://github.com/user-attachments/assets/036b9ae0-c1c2-451b-b170-6bc2beb9da16" />


Taking a look at /usr/bin/showtext, it is executing the more command on a text file. When the more command is used, it displays just enough information to fit the screen and then goes into an interactive mode. 

<img width="487" height="149" alt="bandit26_3" src="https://github.com/user-attachments/assets/29f4e6d3-315f-44b2-b613-d2f96dd69425" />


If we make the terminal screen smaller, we can spawn a shell. 

<img width="862" height="247" alt="bandit26_4" src="https://github.com/user-attachments/assets/f4cc47b4-a88f-4a7c-b7d0-256f4ed31f19" />


Then press v to enter vim, enter :set shell=/bin/bash, and then enter :shell to spawn a shell.

<img width="641" height="711" alt="bandit26_5" src="https://github.com/user-attachments/assets/41047cf1-74ba-4893-8b89-22fe885d2105" />


# Level 26->27

The hint for this level tells to hurry up and grab the password. 

If you use the ls command, you see an executable bandit27-do. Running it allows you to run a command as another user. Using that, I opened the /etc/bandit_pass/bandit27 file.

<img width="572" height="143" alt="bandit27_1" src="https://github.com/user-attachments/assets/fa16f8ee-5a21-4f69-96bd-def72ed6c6a8" />


Username: bandit27
Password: upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB

# Level 27->28
There is a git repository at `ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo` via the port `2220`
The password for the user bandit27-git is the same as bandit27. 

To do this, I actually did it on my own machine instead of on the terminal I was using ssh. I used 

```
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

and entered the password to get the password.

<img width="671" height="703" alt="bandit28_1" src="https://github.com/user-attachments/assets/3c9b17d2-6a58-4c8c-9ed1-be970d01c151" />


Username: bandit28
Password: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

# Level 28->29
Similar to the last level, there is a git repository at `ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo` via the port `2220`. The password is the same as for the user bandit28.

If you run a similar command as previous level, you will get a very similar repo up until opening the README.md. 


<img width="650" height="169" alt="bandit29_1" src="https://github.com/user-attachments/assets/e0137bc2-fd63-44e6-af0c-75ca2c2463d0" />


As a version control system, git would have previous versions of the repo. Using the git log command, we can see the previous versions and the changes made to them. 

<img width="670" height="654" alt="bandit29_2" src="https://github.com/user-attachments/assets/f7b776fa-b273-4a3e-923e-513aec1c308e" />


username: bandit29
Password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7

# Level 29->30
Much of the same as the previous level, there is a git repository at `ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo` via the port `2220` and the password is the same as bandit29.

Cloning the repo and opening the README file:


<img width="605" height="178" alt="bandit30_1" src="https://github.com/user-attachments/assets/0766188a-5c70-489e-bd06-ea4b43240369" />


In addition to having previous versions, git also has branches, a moving point for a commit. This essentially means a diverging point from the main branch, a separate workspace you can do work in, commit and not have it affect the main code.


<img width="649" height="690" alt="bandit30_2" src="https://github.com/user-attachments/assets/f8082e17-95d1-4759-860f-6fab6ba828b3" />


username: bandit30
password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

# Level 30 -> 31
Just like the previous levels, we need to connect to `ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo` via the port `2220`
and the password is the same as bandit30.

Opening the README for this one just gets an allegedly empty file. Git has the ability to tag specific points in a repository's history, which is used to mark release points like v1.0, v2.0. You can use git tag to list existing tags.


<img width="568" height="280" alt="bandit31" src="https://github.com/user-attachments/assets/00c9322c-4409-4c6a-966c-eae6cdf6ae23" />



username: bandit31
Password: fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy

# Level 31->32
Just like the previous levels, there is a git repository at `ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo` via the port `2220` and the password is the same for bandit31.

This time we need to push a file to the remote repository with a specific name and content, specified in the README file. 

The thing about this is that we need to push a file called key.txt, and there is a gitignore file that is untracking files with the txt extension. We need to remove the gitignore file to push it to the repository. 

Then follow these steps:

```
git add key.txt
git commit -m "upload file or any message you want in here"
git push origin master
```

Make sure to configure your email and name using:
```
git config --global user.email "example@yoursite.com"
git config --global user.name "Brock Hackman"
```


<img width="667" height="548" alt="bandit32" src="https://github.com/user-attachments/assets/b446d8f0-a120-4874-8a0e-187ee8b6dca6" />


username: bandit32
password: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K

# Level 32 -> 33
The hint reads "After all this git stuff, it's time for another escape. Good luck!"

If you log in to the bandit32 user, you get a shell where you can't run any commands because they are converted to uppercase. If you type $0, which the name of the shell being executed, you can get a shell that actually works and get the password.


<img width="662" height="204" alt="bandit33" src="https://github.com/user-attachments/assets/3663962e-c659-4e5a-a938-6da642b2a61b" />

username: bandit33
password: tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0

