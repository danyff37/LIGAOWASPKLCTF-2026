## Challenge Overview

**Name:** Lockbox
**Points:** 997
**Objective:** Extract a hidden secret message from a Linux ELF binary.

The challenge provides two files: a compiled binary (lockbox) and a text file (friend_note.txt). According to the note, the binary requires a 64-character HMAC key passed via an --unlock argument to decrypt a secret message. The author also brags about using three layers of protection:

1. Scattering the data across memory.
2. Reversing the string.
3. Applying ROT13 encoding.

Since we do not have the 64-character HMAC key, dynamic analysis or brute-forcing is impractical. Instead, we can bypass the intended --unlock mechanism entirely using static analysis.

---

## Step 1: Static Analysis

The author claims that running strings on the binary yields "nothing useful." However, looking directly at the raw ASCII output of the binary reveals artifacts of the program's hardcoded execution instructions.

In C/C++, when a string is hardcoded and constructed at runtime, the compiler often breaks it into 8-byte (64-bit) chunks and moves them directly onto the stack using mov instructions. By inspecting the raw dump of lockbox, we can spot these exact stack operations mixed with ASCII characters:

```text
H}LM33HD5HD$0H_A0Z3Y_3HD$8H1G0E_mCmHD$@H3{YXCFNJHD$HD$PB

```

This sequence translates to standard x86_64 assembly instructions moving 8-byte chunks of our obfuscated string into adjacent memory addresses.

## Step 2: Extracting the Scattered Pieces

By stripping away the assembly opcodes (like H and HD$0), we can isolate the underlying string fragments in the order they are pushed to the stack:

1. }LM33HD5
2. _A0Z3Y_3
3. 1G0E_mCm
4. 3{YXCFNJ
5. B

Concatenating these scattered chunks gives us our fully assembled, but still encrypted, payload:
**}LM33HD5_A0Z3Y_31G0E_mCm3{YXCFNJB**

## Step 3: Reversing the Encryption Layers

With the full string recovered, we simply need to reverse the remaining two layers of "protection" mentioned in the note.

**1. Reversing the ROT13 cipher**
ROT13 is a simple letter substitution cipher that replaces a letter with the 13th letter after it in the alphabet. Because the alphabet has 26 letters, applying ROT13 to an already ROT13-encoded string decrypts it.

* **Input:** }LM33HD5_A0Z3Y_31G0E_mCm3{YXCFNJB
* **Output:** }YZ33UQ5_N0M3L_31T0R_zPz3{LKPSAWO

**2. Reversing the string**
The final step is to flip the decoded string backwards to restore the original formatting.

* **Input:** }YZ33UQ5_N0M3L_31T0R_zPz3{LKPSAWO
* **Output:** OWASPKL{3zPz_R0T13_L3M0N_5QU33ZY}

---

## Conclusion

By reading the compiled binary directly, we bypassed the --unlock parameter entirely. The "unbreakable" three-layer protection was easily defeated by reassembling the stack strings and reversing the basic encoding.

**Flag:** OWASPKL{3zPz_R0T13_L3M0N_5QU33ZY}
