# Challenge Name: The Chamber of Secrets

## Challenge Overview

We are given a C program that asks for a password.
If the correct password is entered, the function `reveal_secret()` is called and prints the flag.

At first glance, it looks like we must solve the password logic:

```c
if (basilisk_fury == 631 && strlen(password) == 8 && password[0] == 'V')
{
    reveal_secret();
}
```

### Conditions:
- Must start with 'V'
- Must be 8 characters long
- ASCII sum must equal 631

This suggests solving a constraint problem.

## But in CTFs, always look at the functions that hint the flag

## Step-1: Look at reveal_secret()

```c
void reveal_secret()
{
    unsigned char part1[] = {69, 88, 67, 123, 0};
    unsigned char part2[] = {112, 52, 114, 115, 51, 108, 116, 48, 110, 103, 117, 51, 0};
    unsigned char part3[] = {95, 119, 52, 115, 95, 104, 51, 114, 51, 125, 0};
    
    printf("\n The basilisk's eyes open! The chamber unseals!\n");
    printf(" Flag: %s%s%s\n", part1, part2, part3);
    exit(0);
}
```

The flag is already stored inside the binary as **ASCII values**.

Just raw ASCII.

## Step 2 - Decode the ASCII

ASCII can be decoded manually or using online decoder like dcode or CyberChef

### ASCII Values:
- **part1**: 69, 88, 67, 123, 0
- **part2**: 112, 52, 114, 115, 51, 108, 116, 48, 110, 103, 117, 51, 0
- **part3**: 95, 119, 52, 115, 95, 104, 51, 114, 51, 125, 0

### Decoded:
- **part1**: EXC{
- **part2**: p4rs3lt0ngu3
- **part3**: _w4s_h3r3}

## Final Flag: `EXC{p4rs3lt0ngu3_w4s_h3r3}`