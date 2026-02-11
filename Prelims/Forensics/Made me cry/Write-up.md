Description:
I remember this scene very well. It's like locked within my heart. A good memory of finishing the first movie of my favourite franchise. Look into my heart well enough to find what you have come for.

File: https://drive.google.com/file/d/1LlyepNtEbNuwakw8ANwboEzjptmWmA3I/view?usp=sharing

The description says about "look into my heart" which means we must look at the insides of the file. For which we must use "binwalk"

Command : binwalk Happy_ending.mp4
<img width="1464" height="427" alt="image" src="https://github.com/user-attachments/assets/416f7494-7207-49ca-97f5-a66dce0b445e" />

Binwalk reveals the existence of a zip archive. Now we need to extract it.

Command : binwalk -e Happy_ending.mp4 --run-as=root

<img width="1465" height="458" alt="image" src="https://github.com/user-attachments/assets/5ca5c836-22f1-4a4a-ad6d-4cc673dccd10" />

Inside we have a file called flag.txt. Let's read it using cat command.

Command :  cat flag.txt
<img width="1365" height="86" alt="image" src="https://github.com/user-attachments/assets/d41cb0e4-59a3-4cfd-b987-c173c5df74b8" />

Contents : RKP{05_JU56R05Z8_9JJJJJJ!!!!!}
Use rot13.5 in dcode.fr website to decode this.
<img width="1211" height="911" alt="image" src="https://github.com/user-attachments/assets/33c1431a-b325-445b-97fe-5f896e1befb4" />

Flag : EXC{50_WH01E50M3_4WWWWWW!!!!!}

