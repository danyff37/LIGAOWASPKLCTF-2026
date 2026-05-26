# CTF Write-up: unpackme0 - Easy

## Challenge Overview

* **Name:** unpackme0 - Easy
* **CTF:** OWASPKL
* **Category:** Reverse Engineering / Malware Analysis
* **Objective:** Identify the packer used on the provided binary, unpack it, and retrieve the MD5 hash of the unpacked file to construct the flag.

---

## Tools Used

* Linux Terminal (WSL/Kali)
* `cat` (or `strings` / `file`) - For initial static analysis
* `upx` - The Ultimate Packer for eXecutables
* `md5sum` - For hashing the final file

---

## Step-by-Step Solution

### Step 1: Initial Analysis & Identifying the Packer

The first step in dealing with an obfuscated binary is figuring out how it was packed. By printing the raw contents of the binary to the terminal using `cat unpackme0`, we can look for human-readable strings and magic bytes.

Scrolling through the garbled output, several clear indicators jump out:

1. The strings `UPX!` and `UPX 5.11`
2. An explicit info string: `$Info: This file is packed with the UPX executable packer [http://upx.sf.net](http://upx.sf.net) $`

This confirms the binary is packed using **UPX (Ultimate Packer for eXecutables)**, a very common, open-source executable packer.

### Step 2: Unpacking the Binary

Knowing it's a UPX-packed file, we can use the official UPX tool to decompress it. If UPX isn't installed, it can be easily grabbed from the Kali repositories:

```bash
sudo apt update && sudo apt install upx-ucl

```

With the tool installed, we use the `-d` (decompress) flag to unpack the binary. We also use the `-o` flag to write the output to a new file so we don't accidentally overwrite the original challenge binary.

```bash
$ upx -d unpackme0 -o unpackme0_unpacked
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2024
UPX 4.2.4        Markus Oberhumer, Laszlo Molnar & John Reiser     May 9th 2024

        File size         Ratio      Format      Name
   --------------------  ------   -----------   -----------
     24327 <-      6852   28.17%   linux/amd64   unpackme0_unpacked

Unpacked 1 file.

```

### Step 3: Hashing for the Flag

The challenge states that the final flag is the MD5 hash of the *unpacked* binary, formatted as `OWASPKL{<hash>}`. We generate the hash using the standard Linux `md5sum` utility on our newly created file:

```bash
$ md5sum unpackme0_unpacked
1cc6a3b62cac36ab18e0c4685a7f4bdf  unpackme0_unpacked

```

Finally, we wrap the resulting 32-character hash in the required flag format.

## Flag

**`OWASPKL{1cc6a3b62cac36ab18e0c4685a7f4bdf}`**
