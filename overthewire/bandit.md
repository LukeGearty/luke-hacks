# Level 0
The primary goal is to log in to the game using ssh, on port 2220. The username is bandit0 and the password is bandit0.

```
ssh bandit0@bandit.labs.overthewire.org -p 2220

```

![[bandit0.png]]

# Level 0 -> 1

The password for the next level is stored in a file called readme in the home directory.

I used the ls command to find the file, and then used the cat command to open and read the file. 

![[bandit1.png]]
username: bandit1
password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
# Level 1->2
The password for the next level is stored in a file called '-' located in the home directory.  The hint for this level is to google 'dashed filename'. I found this stackoverflow:
https://stackoverflow.com/questions/42187323/how-to-open-a-dashed-filename-using-terminal
That explains what to do to open the file. Using - as an argument refers to stdin/stdout, so to open a file with that name, you need to specify the full path. 

![[bandit2.png]]
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

![[bandit3.png]]

# Level 3->4
The password for the next level is stored in a hidden file in the 'inhere' directory. 

To find hidden directories, you would use the 'ls' command with the -a option. The man page (manual) for ls describes it as: 
```
-a, --all
              do not ignore entries starting with .
```

username: bandit4
password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

# Level 4->5
The password for the next level is stored in the only human-readable file in the inhere directory. 

There are many ways to do this, but the way I solved this was to use the following sequence of commands:

```
find ./inhere -type f | xargs file | grep text
```

To go through this, in the first part I am searching for files in the inhere directory. Xargs is a command that reads items from standard input and executes a command, in this case file, which checks the type of file. Grep text is just looking for the string text in the output.
![[bandit5.png]]

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
![[bandit6.png]]

# Level 6->7

The password for the next level is stored somewhere on the server and has the following properties:
1. Owned by user bandit7
2. Owned by group bandit6
3. 33 bytes in size

```
find / -user bandit7 -group bandit6 -size 33c -type f 2>/dev/null
```

Breaking down the command, I am searching from the root directory for files owned by user bandit7 and group bandit6, 33 bytes in size. I am using 2>/dev/null to redirect stderr to /dev/null, which is like a trash can for any errors that a command generates. Find will generate a lot of errors, especially if you are searching from root, so you will want to use that liberally.

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


![[bandit8.png]]

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

![[bandit9.png]]

username: bandit9
password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

# Level 9->10
The password is stored in data.txt in one of the few human-readable strings, preceeded by several '=' characters.

The strings command is used to extract human-readable strings (aka printable ASCII) from files. Utilizing that command, and using the grep command to search for the = character, you can get the flag.

![[bandit10.png]]

username: bandit10
password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

# Level 10->11
The password for the next level is in data.txt, and it contains base64 encoded data.

Base64 is a binary-to-text encoding scheme that transforms binary data to a sequence of printable characters. It is not encryption, although it does look like encryption at a first glance if you were unfamiliar with it. 

You can encode in base64 from the command line just utilizing the base64 command. base64 -d decodes.

```
cat data.txt | base64 -d
```


![[bandit11.png]]

username: bandit11
password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr

# Level 11->12

The password for the next level is stored in the file data.txt, where all lowercase and uppercase letters have been rotated by 13 positions.

This is a Rot13 substitution cipher, which replaces a letter with the 13th letter after it in the alphabet. A becomes N, B becomes O, etc. 

There are many tools to reverse this, but I used cyberchef. I opened the file using cat, copied the contents and pasted it into cyberchef.

![[bandit12_1.png]]

![[bandit12_2.png]]

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

![[bandit13_1.png]]

Since this is a hexdump, you can use the xxd -r command to reverse the hexdump. We can output that to a new file.

![[bandit13_1 1.png]]

The next few steps were long and tedious. It consisted of finding out what kind of compressed file the output was, renaming it to match the file extension, and using the corresponding command to unzip the file.

![[bandit13_2.png]]

![[bandit13_3.png]]

username: bandit13
password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

# Level 13->14

The password for the next level is stored in **/etc/bandit_pass/bandit14** and can only be read by user bandit14. You don't get the next password, but you get a private ssh key that be used to log into the next level.

Using private ssh keys are a way to log in to a system without a password. The way to do it is with the -i option.

```
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```
![[bandit14_1.png]]

![[bandit14_2.png]]

username: bandit14
password: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

# Level 14 ->15
The password for the next level can be retrieved by submitting the password of the current level to port **30000** on localhost.

You can use the netcat to connect to a machine on a specific port. It is used to write and read data between two computers.

```
nc localhost 30000
```

![[bandit15.png]]

username: bandit15
password: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

# Level 15->16

The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using ssl/tls encryption.

The plain nc command does not support encryption, but a reimplementation of the nc tool, ncat, has additional features that support encryption. You can just specify the --ssl option to enable encryption.

```
ncat --ssl localhost 30001
```


![[bandit16_1.png]]

As another method of solving this, you can also use the openssl command. 

![[bandit16_2.png]]

username: bandit16
Password: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

# Level 16->17

The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. Only one of these ports has a SSL/TLS server listening on them, others will just send back what you send to it.

To see the ports and services running on a machine, nmap, the network mapper, can be used to scan ports and services. We can use the -p option to specify ports, and use the service version scan to see what those ports are running. Typically nmap will just make assumptions on what is running, so -sV is needed.

```
nmap -sV -p 31000-32000 localhost
```

![[bandit17.png]]

From the above, there are 5 ports open. Of the 5, 2 are running ssl servers. Of those 2, one is running an ssl/echo, port 31790, so that might be our port.

The clue gives this as a hint:
**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

If you attempt to connect via openssl, you will get a keyupdate. According to this stackoverflow:
https://stackoverflow.com/questions/58547963/openssl-keyupdate-and-ssl-key-updatewrong-ssl-version

This is triggered via K/k, and since the password starts with k it will triggert the keyupdate. To mitigate this, use this command:
```
openssl s_client -ign_eof localhost:31790
```


![[bandit17_1.png]]

You get a private ssh key. Copy that over to your home terminal, paste it into a text file, change permissions to 600 (owner of the file can read and write) and you can use it to log in.

![[bandit17_3.png]]

username: bandit17
password: EReVavePLFHtFlFsjn3hyzMlvSuSAcRD

# Level 17 -> 18
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the new level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new.

To check the contents of two files to see what is different between them, you would use the diff command. 

![[bandit18.png]]

username: bandit18
password: x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO

# Level 18 -> 19

The password is in a file called readme in the home directory. Unfortunately, someone has modified .bashrc to log you out when you log in.

When you use the password from the last level, you do indeed get logged out immediately.

![[bandit19_1.png]]

You can however execute commands after attempting an ssh login. To get the password, I used this command:

```
ssh bandit18@bandit.labs.overthewire.org -p 2220 'command; cat readme'
```

![[bandit19_2.png]]

username: bandit19
password: cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

# Level 19->20
To gain access to the next level, there is a setuid binary in the home directory that must be used.

A setuid binary is an executable file that runs with the permissions of the owner, not the user who executed it. The binary in the home directory is called bandit20-do. 

Taking a look at it:

![[bandit20_1.png]]

It allows the user executing it to run a command as the owner, in this case bandit20. Recall from earlier levels that there is a directory that stores all the passwords for all the users. 

![[bandit20_2.png]]

username: bandit20
password: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO

# Level 20->21

There is a setuid binary in the home directory that makes a connection to localhost on the port you specify as a commandline argument, then reads a line of text from the conenction and compares it to the password in the previous level. 

This level requires that you ssh into bandit20 on two separate tabs. On one tab, you set up a netcat listener on a port of your choice, and on the other tab you use the binary to connect to the listener. Then on the tab with your netcat listener, send the old password and get the new password.

![[bandit21_1.png]]

![[bandit21_2.png]]

username: bandit21
password: EeoULMCra2q0dSkYj561DX7s1CpBuOBt

# Level 21 -> 22

A program is running at regular intervals from cron. To get the password, we need to look into /etc/cron.d for the configuration and see what command is being executed. 

Cron is a scheduling command that runs a job, a computational task, at specified times. It is used if you need something run at regular intervals. Looking into the directory, there is a cron job running, /usr/bin/cronjob_bandit22.ssh. 

![[bandit22.png]]

username: bandit22
password: tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q

# Level 22 -> 23
Like the previous level, a program is running at regular intervals and the key to the password of the next level is with that password. The hint talks about looking at shell scripts written by other people. 

The script looks like this:
![[bandit23_1.png]]

Going through it:
First it sets a variable to be the name of the user who calls it, myname.
Then it takes a md5 hash of the string (I am user $myname).
Then it copies the password of the user calling it to a file with the name of that hash in the /tmp directory.

Reversing this a bit to take a md5sum where the name of the user is bandit23:

![[bandit23_2.png]]

username: bandit23
password: 0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

# Level 23->24
This level is much like the last one where there is a cron job running at regular intervals. The hint for this level requires the creation of our own shell script. 

Taking a look at the program running: 
![[bandit24_1.png]]

At a high level, this script will execute and then delete all the scripts in the foo directory belonging to the executor of the script. Since the executor is bandit24, the full directory would be /var/spool/bandit24/foo.

We can create a script that will open up the password file for bandit24, place it into the foo directory, and wait for it to be executed. 

To solve this, I created a temporary directory and and password file, then created a shell script with this in it:

```
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/tmp.directory/password
```

This will open the bandit24 password and redirects the output to the new password file. I then changed everything to be world writeable (I was lazy here and just gave everything every position). I copied it into the /foo directory and waited a minute and had the password.

![[Pasted image 20251013223857.png]]
username: bandit24
password: gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8

# Level 24 -> 25
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. The only way to get the pincode is to brute-force the pincode. 

This will require writing another script, but first I wanted to see what happens when you connect on the port.

![[bandit25_1.png]]

The wrong answer will have 'Wrong!' and there needs to be a space between the password and the number.

```
#!/bin/bash

for i in {0000.9999}
do
	echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i" >> wordlist.txt
done
```

This will  just go through the entire process of writing the password and the pin number from 0000 to 9999, and get that into a wordlist text file. 

![[bandit25_2.png]]

After the wordlist was generated, used that to generate the responses, and looked the results for anything that didn't have "Wrong!".

username: bandit25
Password: iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

# Level 25->26
The hint for this level states that logging in to bandit26 from bandit25 should be easy, and the shell for bandit26 is not /bin/bash.

Looking at the /etc/passwd file will reveal the shell for bandit26 is /usr/bin/showtext.


![[bandit26_1.png]]
There is also a ssh key for bandit26 in the bandit25 home directory. However if we use that, we get immediately logged out.
![[bandit26_2.png]]

Taking a look at /usr/bin/showtext, it is executing the more command on a text file. When the more command is used, it displays just enough information to fit the screen and then goes into an interactive mode. 

![[bandit26_3.png]]

If we make the terminal screen smaller, we can spawn a shell. 

![[bandit26_4.png]]

Then press v to enter vim, enter :set shell=/bin/bash, and then enter :shell to spawn a shell.
![[bandit26_5.png]]

# Level 26->27

The hint for this level tells to hurry up and grab the password. 

If you use the ls command, you see an executable bandit27-do. Running it allows you to run a command as another user. Using that, I opened the /etc/bandit_pass/bandit27 file.

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
![[bandit28_1.png]]

Username: bandit28
Password: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

# Level 28->29
Similar to the last level, there is a git repository at `ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo` via the port `2220`. The password is the same as for the user bandit28.

If you run a similar command as previous level, you will get a very similar repo up until opening the README.md. 

![[bandit29_1.png]]

As a version control system, git would have previous versions of the repo. Using the git log command, we can see the previous versions and the changes made to them. 

![[bandit29_2.png]]

username: bandit29
Password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7

# Level 29->30
Much of the same as the previous level, there is a git repository at `ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo` via the port `2220` and the password is the same as bandit29.

Cloning the repo and opening the README file:
![[bandit30_1.png]]

In addition to having previous versions, git also has branches, a moving point for a commit. This essentially means a diverging point from the main branch, a separate workspace you can do work in, commit and not have it affect the main code.
![[bandit30_2.png]]

username: bandit30
password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

# Level 30 -> 31
Just like the previous levels, we need to connect to `ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo` via the port `2220`
and the password is the same as bandit30.

Opening the README for this one just gets an allegedly empty file. Git has the ability to tag specific points in a repository's history, which is used to mark release points like v1.0, v2.0. You can use git tag to list existing tags.


![[bandit31.png]]

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

![[bandit32.png]]
username: bandit32
password: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K

# Level 32 -> 33
The hint reads "After all this git stuff, it's time for another escape. Good luck!"

If you log in to the bandit32 user, you get a shell where you can't run any commands because they are converted to uppercase. If you type $0, which the name of the shell being executed, you can get a shell that actually works and get the password.

![[bandit33.png]]

username: bandit33
password: tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0

