# Challenge Name: The Pensieve Memory Mystery

## Concept Of the Challenge

The challenge description gives us the encoding logic:

- Each memory fragment contains two characters combined into one value
- The first character is shifted left by 7 bits
- Then it is XORed with the second character

### Mathematically:

```
encoded = (char1 << 7) ^ char2
```

This immediately suggests:

- ASCII characters are being used (typically 7-bit values)
- Two 7-bit characters are packed into a single number
- Since XOR is reversible, decoding should be straightforward

## Reversing the Encoding

To retrieve the original characters:

```
char1 = encoded >> 7
char2 = encoded ^ (char1 << 7)
```

### Explanation:

- Right shift removes the lower 7 bits → recovers `char1`
- XOR again removes the shifted portion → recovers `char2`

Since XOR is its own inverse, this works cleanly.

## Exploitation Script

Assuming each encoded fragment is stored as 2 bytes (16-bit big-endian values):

```python
decoded = ""

data = "⋘⇻㩷ᡟ㥵㜳㧟ᡮ᧟㧰᧬㙟㚴㎱ㆴ㙟ㆰ㛢ᣮᩴᢰ㝽"

for ch in data:
    encoded = ord(ch)

    char1 = encoded >> 7
    char2 = encoded ^ (char1 << 7)

    decoded += chr(char1)
    decoded += chr(char2)

print(decoded)
```

## Result

Running the script reconstructs the full hidden memory text.

Inside the decoded output, we find the flag in the expected CTF format:

### Final Flag: `EXC{tw0_run3s_0n3_sp3ll_m4g1c4l_c0mb1n4t10n}`

```
EXC{tw0_run3s_0n3_sp3ll_m4g1c4l_c0mb1n4t10n}
```
