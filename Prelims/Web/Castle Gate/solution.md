## Description
Hogwarts’ enchanted login gate protects access to the Great Hall. Unfortunately, the Ministry trusted the job to a sleepy intern who mixed ancient magic with unsafe database spells.

Your task is to authenticate without knowing any valid wizard credentials. The login scroll checks your input against the castle’s registry, but the spell casting is… flawed.

Can you manipulate the charm, bypass the wards, and prove you belong inside Hogwarts?

http://34.93.58.153:5000/

## Solution

<img width="1250" height="594" alt="image" src="https://github.com/user-attachments/assets/0aae8ac9-173f-4a9c-aac3-515f0e08be5a" />


We can see a login page where we need to enter credentials to access the flag.

The credentials are not stored in any of the endpoints.

Usually, login pages may have SQL injection vulnerabilities, which can allow us to bypass authentication and gain access to the flag.

Learn SQL Injection by watching my video: https://www.youtube.com/watch?v=IAZhLJpmUQE

**SQL Injection Payload** 

Username: `' or 1=1--`

Password: `' or 1=1--`

Which bypasses the login page and reveals our flag:

Flag: `EXC{pwn1n3d_l0g!n_f4r!y_s1mpl3}`


<img width="658" height="464" alt="image" src="https://github.com/user-attachments/assets/f6481307-0ac7-476e-98cb-4b59f70b71f8" />


<img width="1327" height="540" alt="image" src="https://github.com/user-attachments/assets/10703915-4abe-4e79-afed-2153c12e2c66" />
