## Description

A spell has been cast on this page, hiding something valuable in plain sight. The flag isn’t revealed by clicks or inputs—it lies silently within the page itself. Only those who know where to look will uncover the truth. Mischief managed.

## Solution

As the Description says the flag is stored in plain text somewhere in the website.

So lets analyse the source code of the website, by clicking Ctrl + U gives us the source code of the website.

When we click on the js file which is script.js

We can see the flag is splitted apart

<img width="616" height="317" alt="image" src="https://github.com/user-attachments/assets/9f2d6585-2790-4fdb-9b82-c3cafcd9a818" />


Part 1: `EXC{excalibur_`

Part 2: `ctf_magic_`

Part 3: `unlocked}` 

By concatenating the words we get the 3rd part ['u', 'n', 'l', 'o', 'c', 'k', 'e', 'd', '}']

Final Flag: `EXC{excalibur_ctf_magic_unlocked}`
