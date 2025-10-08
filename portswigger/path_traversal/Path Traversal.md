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
![[Screenshot 2025-10-04 123445.png]]

![[Screenshot 2025-10-04 123515.png]]

![[Screenshot 2025-10-04 123535.png]]

![[Screenshot 2025-10-04 123656.png]]


# Common Obstacles

## Absolute File path blocked

If the application strips or block directory traversal sequences ../../, it may be possible to use the absolute file path.
### Lab

![[Screenshot 2025-10-04 123933.png]]
![[Screenshot 2025-10-04 123953.png]]
![[Screenshot 2025-10-04 124002.png]]

## Stripping Traversal Sequences

Nested sequences, such as ....// or ....\/ may be needed. When the inner sequence is stripped, they revert to simple traversal sequences.
### Lab

![[Screenshot 2025-10-08 132341.png]]

![[Screenshot 2025-10-08 132506.png]]

![[Screenshot 2025-10-08 132555.png]]

## Superfluous URL-Decode

Some services may strip any traversal sequences before passing input to the application. This can be bypassed by URL encoding. The ../../../ sequence becomes  %2E%2E%2F%2E%2E%2F%2E%2E%2F. You can also double URL encode, so that sequence becomes %252E%252E%252F%252E%252E%252F%252E%252E%252F.

### Lab

![[Screenshot 2025-10-08 132741.png]]
![[Screenshot 2025-10-08 132900.png]]
![[Screenshot 2025-10-08 133127.png]]![[Screenshot 2025-10-08 133135.png]]

## Validation of start Path

Some applications may need the user supplied filename to begin with the expected base folder, for example /var/www/images.

### Lab
![[Screenshot 2025-10-08 133623.png]]

![[Screenshot 2025-10-08 133713.png]]

## Validation of File Extension with null byte bypass

There may be a need to supply the expected file extension, like .jpg or .png for images. In this case, it might be possible to terminate a file path with a null byte, %$00.

### Lab
![[Screenshot 2025-10-08 133304.png]]

![[Screenshot 2025-10-08 133357.png]]

# Prevent Path Traversal

Avoid passing user-supplied input to the filesystem APIs. 

If that is not possible, validate user input before processing. Compare input with a whitelist of permitted values.

If that is not possible, validate that it only contains permitted content. - Alphanumeric characters only for example. 

After validating supplied input, append the input to the base directory and use a platform filesystem API to canonicalize the path. 