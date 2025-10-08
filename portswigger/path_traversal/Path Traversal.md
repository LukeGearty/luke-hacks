This is a writeup and notes taken of PortSwigger's learning path on the Path Traversal vulnerability. This will also include screenshots of the labs.

# What is Path Traversal?

Directory Traversal - on a web server, accessing files and directories stored outside the web root folder. It manipulates paths with dot-dot-slash "../" and variations to access arbitrary files. These files may be application source code, credentials for back end systems, and sensitive operating system files. 

Attackers can sometimes use a path traversal to modify application data or take full control of a server.

# Reading Arbitrary Files via path traversal
Example: 
An application displays images using this HTML:
`<img src="/loadImage?filename=218.png">`
load image reads from the /var/www/images directory to.
If there no defense, an attacker can modify the above to request a URL to retrieve sensitive files, like the /etc/passwd file.
`https://insecure-website.com/loadImage?filename=../../../etc/passwd`
An equivalent attack on a windows server:
`https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`

## Lab
<img width="1407" height="892" alt="Screenshot 2025-10-04 123445" src="https://github.com/user-attachments/assets/c338b02a-82c8-4be1-b89c-875b6786b637" />

<img width="1265" height="366" alt="Screenshot 2025-10-04 123515" src="https://github.com/user-attachments/assets/aa4c6727-2740-4819-9413-fc64bb4012c6" />

<img width="1256" height="375" alt="Screenshot 2025-10-04 123535" src="https://github.com/user-attachments/assets/1113181a-7873-4888-b35c-fb1cb1212dc8" />

<img width="1600" height="407" alt="Screenshot 2025-10-04 123656" src="https://github.com/user-attachments/assets/824847ee-88e1-4fa2-afbc-54817860ae21" />


# Common Obstacles

## Absolute File path blocked

If the application strips or block directory traversal sequences ../../, it may be possible to use the absolute file path.
### Lab

<img width="1409" height="804" alt="Screenshot 2025-10-04 123933" src="https://github.com/user-attachments/assets/35db0b15-41ed-4271-83b4-2512ef74a3b3" />

<img width="1261" height="415" alt="Screenshot 2025-10-04 123953" src="https://github.com/user-attachments/assets/a27f6e3d-621c-4ffa-9fdb-aba230a7530f" />

<img width="1257" height="411" alt="Screenshot 2025-10-04 124002" src="https://github.com/user-attachments/assets/03117a02-215e-4b9e-9173-f7a9394c4c12" />

## Stripping Traversal Sequences

Nested sequences, such as ....// or ....\/ may be needed. When the inner sequence is stripped, they revert to simple traversal sequences.
### Lab

<img width="1415" height="888" alt="Screenshot 2025-10-08 132341" src="https://github.com/user-attachments/assets/8dbd31a9-f24b-4929-a2e7-e00b2e48471f" />

<img width="1253" height="411" alt="Screenshot 2025-10-08 132506" src="https://github.com/user-attachments/assets/58f15d2e-7e2a-477b-9992-16441a47862d" />

<img width="1259" height="386" alt="Screenshot 2025-10-08 132555" src="https://github.com/user-attachments/assets/06e216b4-0b80-4dd1-a841-431a727bb711" />


## Superfluous URL-Decode

Some services may strip any traversal sequences before passing input to the application. This can be bypassed by URL encoding. The ../../../ sequence becomes  %2E%2E%2F%2E%2E%2F%2E%2E%2F. You can also double URL encode, so that sequence becomes %252E%252E%252F%252E%252E%252F%252E%252E%252F.

### Lab

<img width="1418" height="899" alt="Screenshot 2025-10-08 132741" src="https://github.com/user-attachments/assets/6247e8b0-3fef-4aa2-bb17-aa7e81c94781" />

<img width="1263" height="342" alt="Screenshot 2025-10-08 132900" src="https://github.com/user-attachments/assets/45f390ac-09f4-41d5-92b9-1d1a20b0e647" />

<img width="1259" height="408" alt="Screenshot 2025-10-08 133127" src="https://github.com/user-attachments/assets/19f0e8db-ffeb-4573-a647-775e3d565d84" />

<img width="1259" height="408" alt="Screenshot 2025-10-08 133127" src="https://github.com/user-attachments/assets/b196a634-6ec5-473a-afe5-980ad277a8f5" />

<img width="1526" height="616" alt="Screenshot 2025-10-08 133135" src="https://github.com/user-attachments/assets/2d441cf2-037a-4ae5-a8d5-5c8ca0370b77" />


## Validation of start Path

Some applications may need the user supplied filename to begin with the expected base folder, for example /var/www/images.

### Lab

<img width="1386" height="803" alt="Screenshot 2025-10-08 133623" src="https://github.com/user-attachments/assets/9deb51c9-b82b-4ceb-a0fb-248a3a5e08bc" />

<img width="1257" height="409" alt="Screenshot 2025-10-08 133713" src="https://github.com/user-attachments/assets/10295b8b-d97c-4465-9d0c-f3c70022a2a4" />

## Validation of File Extension with null byte bypass

There may be a need to supply the expected file extension, like .jpg or .png for images. In this case, it might be possible to terminate a file path with a null byte, %$00.

### Lab

<img width="1396" height="798" alt="Screenshot 2025-10-08 133304" src="https://github.com/user-attachments/assets/e4b7eeb9-edce-4c3f-8a31-743000948af6" />

<img width="1260" height="405" alt="Screenshot 2025-10-08 133357" src="https://github.com/user-attachments/assets/4c8f019d-730c-4073-a41c-dcc455e95ec5" />

# Prevent Path Traversal

Avoid passing user-supplied input to the filesystem APIs. 

If that is not possible, validate user input before processing. Compare input with a whitelist of permitted values.

If that is not possible, validate that it only contains permitted content. - Alphanumeric characters only for example. 

After validating supplied input, append the input to the base directory and use a platform filesystem API to canonicalize the path. 
