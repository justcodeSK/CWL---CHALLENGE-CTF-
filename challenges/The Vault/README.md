# Reverse Engineering Report – The Vault

## Table of Contents
1. Scope & Objective  
2. Reconnaissance  
3. Static Analysis  
4. Disassembly Analysis  
5. Reconstruction  
6. Verification  
7. Conclusion  

---

# 1. Scope & Objective

This challenge is a **Linux Binary Reverse Engineering Assessment**.  
The objective was to analyze a provided binary file, understand its internal logic, reconstruct hidden data, and recover the final flag.

The challenge file was provided as:

```
THE VAULT.zip
```

The investigation followed standard reverse engineering procedures including reconnaissance, static analysis, disassembly inspection, and runtime verification.

---

# 2. Reconnaissance

## File Enumeration

Initial inspection of the provided archive.

```
ls
file "The Vault.zip"
```

Output:

```
The Vault.zip: Zip archive data, made by v2.0, extract using at least v2.0,
last modified Jan 01 1980 00:00:00,
uncompressed size 15924,
method=deflate
```

### Findings

- Confirmed that the file is a **ZIP archive**
- Timestamp shows **January 1, 1980**, which is suspicious
- This often indicates **metadata stripping or intentional manipulation**
- The archive was **not encrypted**

---

## Filename Issue

Running the command incorrectly caused an error:

```
file The Vault
```

The command fails because Linux interprets the input as:

```
The
Vault
```

Two separate filenames.

### Correct Command

```
file "The Vault.zip"
```

---

# 3. Archive Inspection

The archive contents were inspected **without extracting**.

```
unzip -l "The Vault.zip"
```

### Findings

The archive contained a directory:

```
The Vault/vault
```

Inside the directory was a **single file named `vault` with no extension**.

---

# 4. Binary Identification

The extracted file was analyzed using the `file` command.

```
file vault
```

Output analysis revealed:

- **ELF Linux Executable**
- **32-bit binary**
- Compiled for **i386 architecture**
- **PIE (Position Independent Executable)**
- **Dynamically linked**
- Uses **system libraries**
- **NOT stripped** (debug symbols exist)

### Findings

Because the binary is **not stripped**, function names remain visible, which simplifies reverse engineering.

---

# 5. Strings Analysis

## Objective

Extract readable strings from the binary to identify:

- URLs
- Hidden text
- Commands
- Program logic clues

Command used:

```
strings vault | less
```

### Observed String Categories

The output strings were grouped into four sections:

1. Normal system strings
2. Program logic strings
3. Suspicious fragments
4. Function names

### Important Strings Found

```
Usage: ./vault [OPTIONS]
--flag
--intro
--help
Enter the password to access the Flag:
Better Luck Next Time!
Too many arguments, try again!!!
```

### Findings

- The program accepts command-line options.
- Access to the flag requires a **password**.
- Several strings ended with **letter 'f'**, indicating fragmented storage.
- Function names revealed:

```
concatpasswd
concatflag
```

This indicates that the **password and flag are constructed dynamically during execution**.

---

# 6. Static Analysis Using objdump

The binary was disassembled using:

```
objdump -d vault | grep concat
```

### Findings

Both functions were referenced inside the **main function**.

This confirmed:

- The **password and flag are assembled during runtime**
- String fragments are pushed to the stack and concatenated

The investigation then focused on where **concatflag** was called.

---

# 7. Disassembly Analysis

The `main` function was analyzed to reconstruct the stack behavior.

Key observations included:

- Function arguments pushed onto the stack
- Little-endian hexadecimal constants
- Reverse order argument passing

Relevant instructions:

```
144c: lea -0x68(%ebp),%eax → push
1450: lea -0x62(%ebp),%eax → push
1454: lea -0x6e(%ebp),%eax → push
1458: lea -0x50(%ebp),%eax → push
```

### Key Observations

- Some arguments are pushed in **reverse order**
- Hexadecimal fragments represent ASCII characters
- Values are stored using **little-endian format**

---

# 8. Fragment Decoding

The flag was stored as **hexadecimal fragments**.

Each fragment was decoded and combined.

### Fragment 1

```
0x67616c46 + 0x7b
```

Decoded:

```
Flag{
```

---

### Fragment 2

```
0x41753059 + 0x72
```

Decoded:

```
Y0uAr
```

---

### Fragment 3

```
0x72434133 + 0x61
```

Decoded:

```
3ACra
```

---

### Fragment 4

```
0x72336b63 + 0x7d
```

Decoded:

```
ck3r}
```

---

# 9. Reconstructed Flag

Combining all fragments resulted in the final flag:

```
Flag{Y0uAr3ACrack3r}
```

---

# 10. Password Reconstruction

The same method was applied to analyze the **concatpasswd** function.

Fragments used for password construction were decoded and combined.

### Final Password

```
S7p3rS3ct3rOrgan1z2t10n
```

---

# 11. Verification

The binary was executed with the recovered password and flag option.

Command:

```
./vault --flag
```

Output:

```
Flag{Y0uAr3ACrack3r}
```

This confirmed that the reconstructed flag was correct.

---

# 12. Conclusion

This reverse engineering challenge demonstrated several key binary analysis techniques:

- Reconnaissance of suspicious archives
- Binary identification and inspection
- Static analysis using **strings** and **objdump**
- Understanding **stack-based string construction**
- Analyzing **function calls and calling conventions**
- Decoding **little-endian hexadecimal data**
- Reconstructing hidden runtime values

Through careful analysis of the binary's internal logic, both the **hidden password** and the **final flag** were successfully recovered.

Final Flag:

```
Flag{Y0uAr3ACrack3r}
```
