## Description
A simple web challenge that returns a fake flag when you spell it directly. Try a different approach — the main page itself reveals the real flag.

## Solution

When we click on the button which reveals the fake flag and it also reveals a very important hint for solving the challenge.

<img width="699" height="401" alt="image" src="https://github.com/user-attachments/assets/76389f66-7c5d-422a-bfe3-601ceeed6965" />

**HEAD??**

HEAD is a HTTP Request Method were the flag is hidden, To retrieve the flag we need to send an HEAD HTTP request method to the site.

We can use both burpsuite and curl request for solving this challenge, for ease of access i am using curl.

Command: `curl -I http://34.93.58.153:7000/`

<img width="517" height="148" alt="image" src="https://github.com/user-attachments/assets/6d62b827-3c73-4918-b620-16a32c75e5ec" />

We can see that the flag is revealed in the response header

Flag: `EXC{h3ad_request_reveals_th3_FL4G}`

