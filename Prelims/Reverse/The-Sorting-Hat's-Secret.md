# The Sorting Hat's Secret

## Initial Analysis

We're given a 64-bit ELF executable called `th3_s3cr3t`. Let's start by examining it.

### File Information

```bash
$ file th3_s3cr3t
th3_s3cr3t: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV),
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2,
for GNU/Linux 3.2.0, not stripped
```

The binary is not stripped, which means debugging symbols are present - this will make our analysis easier.

### Running the Binary

```bash
$ ./th3_s3cr3t
 Enter the magical incantation :>: hello

 The magic fizzles. You are not worthy.! :d~
```

The program prompts for a "magical incantation" (password) and rejects our input.

## Static Analysis with Strings

Using the `strings` command reveals some interesting information:

```bash
$ strings th3_s3cr3t
```

Key findings:

- `EXC{th3_H`
- `p4$$w0rdH`
- `rd_w4$_`
- `ALOHOMORH`
- `but_1n_rH`
- `3v3rs3}`
- `alohomora`
- Function names: `reverse`, `reveal_flag`, `main`

The strings suggest the flag is split across multiple segments, and there's a reference to "alohomora" - the Harry Potter unlocking charm (fitting with the Sorting Hat theme!).

## Dynamic Analysis with ltrace

Let's trace the library calls to understand what the program is doing:

### Attempt 1: Random Input

```bash
$ ltrace ./th3_s3cr3t
printf(" Enter the magical incantation :"...)  = 35
__isoc99_scanf(...)                            = 1
strlen("hello")                                = 5
strcmp("olleh", "alohomora")                   = 14
puts("\n The magic fizzles...")                = 46
```

**Key observation:** The input "hello" is being reversed to "olleh" before comparison!

### Attempt 2: Using "alohomora"

```bash
$ ltrace ./th3_s3cr3t
printf(" Enter the magical incantation :"...)  = 35
__isoc99_scanf(...)                            = 1
strlen("alohomora")                            = 9
strcmp("aromohola", "alohomora")               = 6
puts("\n The magic fizzles...")                = 46
```

The program reversed "alohomora" to "aromohola" and compared it against "alohomora" - they don't match!

## Solution

The program:

1. Takes user input
2. Reverses the input string
3. Compares the reversed string with "alohomora"

To pass the check, we need to input the reverse of "alohomora", which is **"aromohola"**.

### Attempt 3: The Correct Incantation

```bash
$ ltrace ./th3_s3cr3t
printf(" Enter the magical incantation :"...)  = 35
__isoc99_scanf(...)                            = 1
strlen("aromohola")                            = 9
strcmp("alohomora", "alohomora")               = 0
puts("\n✨ The Sorting Hat speaks...")         = 31
printf("%s", "EXC{th3_p4$$w0rd_w4$_")          = 21
printf("%s", "ALOHOMORA_")                     = 10
puts("but_1n_r3v3rs3}")                        = 16
```

Success! The flag is revealed:

```bash
$ ./th3_s3cr3t
 Enter the magical incantation :>: aromohola

✨ The Sorting Hat speaks...
EXC{th3_p4$$w0rd_w4$_ALOHOMORA_but_1n_r3v3rs3}
```

## Flag

```
EXC{th3_p4$$w0rd_w4$_ALOHOMORA_but_1n_r3v3rs3}
```
