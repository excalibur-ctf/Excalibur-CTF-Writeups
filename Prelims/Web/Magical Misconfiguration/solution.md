## Description
The Excalibur flag is locked behind ancient Hogwarts magic. The portal claims only the Headmaster (admin) can access the Chamber of Secrets—but magic often fails in the details. Your role shows as Student. Find a way to obtain administrative access and reveal the hidden flag. No brute force. Think like a wizard… and a hacker.

http://34.93.58.153:7000/

## Solution

When we click on the access now button in the website which returns an Access Denied message.

<img width="1382" height="616" alt="image" src="https://github.com/user-attachments/assets/6d18a4df-3b35-44b9-9bc6-10f4d61eef4b" />

So, inorder to retrieve the flag we need to bypass the authorization from student to admin.

Mostly the authentication stuffs are stored in the cookie headers of the http request , so analysing there can be helpful.

There is a browser extension called Cookie Editor which makes our life easy for editing cookies.

Analysing the cookie we can see a parameter called isAdmin which is set to False

Changing the isAdmin from false -> true reveals the flag:

<img width="519" height="325" alt="image" src="https://github.com/user-attachments/assets/5743789d-4a1c-4c78-9a59-d67c82cb92aa" />


<img width="1357" height="668" alt="image" src="https://github.com/user-attachments/assets/25042e6a-275e-4a85-85ec-09cb50b4032e" />


Flag: `EXC{c00ki3_m4n1pu141!0n_byp4ss3d_XDXD!}`
