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
## Tools Used (Reverse Engineering)

The following tools were used during the reverse engineering analysis of the binary:

- **file** – Identified the binary format and architecture.
- **unzip** – Inspected and extracted the archive contents.
- **strings** – Extracted readable text from the binary to locate clues.
- **objdump** – Disassembled the binary to analyze program structure and function calls.
- **grep** – Filtered relevant functions and strings during analysis.
- **less** – Used for navigating large outputs from analysis tools.

These tools enabled static analysis of the binary and helped reconstruct hidden program data such as the password and flag.
---
# 1. Scope & Objective

This challenge involves **reverse engineering a Linux binary** provided in a compressed archive named **"The Vault.zip"**.

The goal of the investigation is to analyze the binary, understand its internal logic, reconstruct hidden program data, and extract the **password and flag** embedded within the executable.

The assessment demonstrates practical skills in:

- Linux binary analysis
- Static analysis
- Disassembly inspection
- Stack analysis
- Little-endian decoding
- Runtime reconstruction of hidden strings

All analysis was performed using **standard Linux reverse engineering tools**.

---

# 2. Reconnaissance

## File Enumeration

Initial directory inspection:

```
ls
```

The downloaded file was:

```
The Vault.zip
```

To determine the file type:

```
file "The Vault.zip"
```

Output:

```
The Vault.zip: Zip archive data, made by v2.0,
extract using at least v2.0,
last modified Jan 01 1980 00:00:00,
uncompressed size 15924,
method=deflate
```

### Observations

- The file is a **ZIP archive**
- Timestamp shows **Jan 01 1980**
- This timestamp is commonly used when **metadata is intentionally removed**
- The archive **is not encrypted**

---

## Filename Issue

Running:

```
file The Vault
```

Resulted in an error.

This occurred because Linux interpreted the command as two files:

```
The
Vault
```

Correct usage requires quotation marks:

```
file "The Vault.zip"
```

---

## Archive Inspection

The archive contents were inspected without extraction:

```
unzip -l "The Vault.zip"
```

### Findings

The archive contains:

```
The Vault/vault
```

- Only **one file**
- No file extension

---

# 3. Static Analysis

## Binary Identification

After extracting the archive, the file type was analyzed:

```
file vault
```

Output indicated:

```
ELF 32-bit executable
Intel 80386 architecture
Dynamically linked
PIE (Position Independent Executable)
Not stripped
```

### Key Observations

| Property | Meaning |
|--------|--------|
| ELF | Linux executable format |
| 32-bit | Compiled for i386 architecture |
| PIE | Position Independent Executable |
| Dynamically Linked | Uses system libraries |
| Not Stripped | Debug symbols and function names remain |

The fact that the binary is **not stripped** makes reverse engineering significantly easier.

---

# 4. Strings Analysis

## Objective

Extract readable strings from the binary to identify:

- Commands
- URLs
- Messages
- Clues related to program logic

Command used:

```
strings vault | less
```

### Observed String Categories

The extracted strings were grouped into four main categories:

1. **Normal System Strings**  
   Standard library and system references.

2. **Program Logic Strings**  
   Messages indicating program behavior.

3. **Suspicious Fragments**  
   Possible pieces of hidden data.

4. **Function Names**

---

### Important Program Strings

The following strings were discovered:

```
Usage: ./vault [OPTIONS]

--flag
--intro
--help

Enter the password to access the Flag:
Better Luck Next Time!
Too many arguments, try again!!!
```

### Key Findings

- The program contains a **password-protected flag**
- Flag retrieval requires **correct password input**
- Certain strings appear to end with **character fragments**
- Flag components may be **constructed dynamically**

---

## Function Discovery

Important functions identified:

```
concatpasswd
concatflag
```

These names suggest that:

- The **password is assembled dynamically**
- The **flag is also constructed at runtime**

---

# 5. Disassembly Analysis

To analyze the binary structure, **objdump** was used.

```
objdump -d vault | grep concat
```

### Findings

The functions:

```
concatpasswd
concatflag
```

are **called inside the main function**.

This indicates:

- The password and flag are **not stored directly**
- They are **built during execution**

---

## Inspecting Function Calls

The flag fragments are passed into the **concatflag** function from `main`.

Relevant assembly instructions:

```
144c: lea -0x68(%ebp),%eax
1450: lea -0x62(%ebp),%eax
1454: lea -0x6e(%ebp),%eax
1458: lea -0x50(%ebp),%eax
```

These instructions push memory references onto the stack.

### Important Observation

Arguments are pushed in **reverse order** due to the **x86 calling convention**.

---

# 6. Reconstruction

## Flag Fragment Analysis

The flag fragments were stored as **hexadecimal constants** in **little-endian format**.

Each fragment needed to be:

1. Converted from hex
2. Reversed (little-endian correction)
3. Translated to ASCII
4. Combined in the correct order

---

### Fragment Decoding

Fragment 1

```
0x67616c46 + 0x7b
```

Decoded:

```
Flag{
```

---

Fragment 2

```
0x41753059 + 0x72
```

Decoded:

```
Y0uAr
```

---

Fragment 3

```
0x72434133 + 0x61
```

Decoded:

```
3ACra
```

---

Fragment 4

```
0x72336b63 + 0x7d
```

Decoded:

```
ck3r}
```

---

## Reconstructed Flag

Combining all fragments:

```
Flag{Y0uAr3ACrack3r}
```

---

## Password Reconstruction

The **concatpasswd** function was analyzed using the same process.

After decoding and assembling the fragments, the reconstructed password was:

```
S7p3rS3ct3rOrgan1z2t10n
```

---

# 7. Verification

To confirm the findings, the program was executed with the correct option:

```
./vault --flag
```

After entering the reconstructed password, the program returned:

```
Flag{Y0uAr3ACrack3r}
```

This confirms that both the **password and flag reconstruction were correct**.

---

# 8. Conclusion

This challenge required detailed **static and disassembly analysis** of a Linux executable.

Key reverse engineering techniques used:

- Binary identification
- Strings extraction
- Static disassembly
- Stack argument reconstruction
- Little-endian decoding
- Manual reconstruction of runtime strings

Through these techniques, the hidden program logic was understood, and both the **password and flag were successfully reconstructed**.

The challenge demonstrated practical skills in:

- Linux binary reverse engineering
- Assembly code interpretation
- Program flow analysis
- Data reconstruction from memory fragments
