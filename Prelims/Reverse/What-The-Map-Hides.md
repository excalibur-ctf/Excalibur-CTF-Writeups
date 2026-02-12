# What The Map Hides - Reverse Engineering Write-Up

**File Type:** 64-bit ELF (PIE, dynamically linked, not stripped)

## Challenge Overview

Upon execution, the binary displays the following prompt:

```
What The Map Hides

The blank parchment lies before you...
The Weasley twins whisper: 'Tap it with your wand and speak the secret phrase!'
Enter the secret phrase:
```

The program accepts two specific phrases:

- `mischief managed`
- `I solemnly swear that I am up to no good`

Despite entering the correct phrases, no flag is revealed during runtime. This indicates that static analysis is required to locate the flag.

## Step 1: Initial Analysis

```bash
file the_map
```

Output:

```
ELF 64-bit LSB pie executable, x86-64, dynamically linked, not stripped
```

The binary is not stripped, indicating that symbol information is available for analysis.

### String Analysis

```bash
strings the_map
```

Notable strings identified:

```
mischief managed
I solemnly swear that I am up to no good
Try examining the parchment more carefully...
```

The hint "Try examining the parchment more carefully..." suggests that static analysis is the intended approach.

## Step 2: Runtime Analysis

Using `ltrace` to trace library calls:

```bash
ltrace ./the_map
```

Observations:

- `fgets()` is used to read user input
- `strcmp()` compares input against the two known phrases
- No flag output occurs regardless of correct input

This confirms that the flag is not printed during normal program execution.

## Step 3: Static Analysis

Disassembling the main function:

```bash
objdump -d the_map
```

While analyzing the assembly code of the binary, we notice a series of ASCII value
i,e. Near the end of the main function, a series of `movb` instructions are observed:

```asm
c6 85 50 ff ff ff 52    movb   $0x52,-0xb0(%rbp)
c6 85 51 ff ff ff 56    movb   $0x56,-0xaf(%rbp)
c6 85 52 ff ff ff 68    movb   $0x68,-0xae(%rbp)
...
c6 45 86 3d             movb   $0x3d,-0x7a(%rbp)
c6 45 87 3d             movb   $0x3d,-0x79(%rbp)
c6 45 88 00             movb   $0x0,-0x78(%rbp)
```

The program constructs a string byte-by-byte on the stack but never prints it.

## Step 4: Byte Extraction

Extracting all hexadecimal values from the `movb` instructions:

```
52 56 68 44 65 7a 46 66 63 7a 42 73 4d 32 31 75
62 48 6c 66 4a 48 63 7a 59 58 4a 66 4d 56 38 30
62 56 39 31 63 46 39 30 62 31 39 75 4d 46 39 6e
4d 44 42 6b 66 51 3d 3d
```

Converting to ASCII:
can be done manually or using cyberchef/dcode.fr

```
RVhDezFfczBsM21ubHlfJHczYXRfMV80bV91cF90b19uMF9nMDBkfQ==
```

## Step 5: Encoding Recognition

The string terminates with `==`, which is characteristic of Base64 padding. This indicates that the string is Base64 encoded.

## Step 6: Base64 Decoding

Decoding the Base64 string using online tools like cyberchef/dcode.fr:

```bash
echo "RVhDezFfczBsM21ubHlfJHczYXRfMV80bV91cF90b19uMF9nMDBkfQ==" | base64 -d
```

Result:

```
EXC{1_s0l3mnly_$w3ar_1_4m_up_t0_n0_g00d}
```

**Flag:** `EXC{1_s0l3mnly_$w3ar_1_4m_up_t0_n0_g00d}`


