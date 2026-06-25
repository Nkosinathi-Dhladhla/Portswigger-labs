# Path_Traversal 

## What is Path Traversal?
Path traversal is a vulnerability that enables an attacker to read arbitrary files on the server running
on the application. In simpler terms, path traversal allows an attacker to access files 
and directories outside the application's intended folder. 

## Example:
Imagine a website that displays:
https://example.com/image?file=cat.jpg

The server might load the file from:
/var/www/images/cat.jpg

If the application does not properly validate the filename, an attacker might send: 
https://example.com/image?file=../../../etc/passwd

The ../ means "go up one directory." By chaining multiple ../ sequences, the attacker can navigate outside the images folder and access sensitive files. 

## How this is performed in the lab
Goal - Retrieve the contents of the etc/passwd file
This is done using Burp Suite. Turn the intercept on, view images in http history, click one of links to the images, send it to a repeater, and perform the path traversal by editing the file path of the image in the repeater. 

