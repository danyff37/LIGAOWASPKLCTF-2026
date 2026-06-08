# CTF Write-up: Data Guy

## Challenge Overview

* **Name:** Data Guy
* **CTF:** LIGA OWASP KL CTF 2026
* **Category:** Forensics
* **Objective:** Analyze the embedded structural tags and metadata of a provided MP4 video file to extract hidden information and recover the flag.

---

## Tools Used

* Linux Terminal (WSL/Kali)
* `exiftool` - For parsing and extracting metadata from the media container
* `strings` & `grep` - For extracting and filtering raw ASCII/text strings from the file

---

## Step-by-Step Solution

### Step 1: Initial Analysis & Metadata Extraction

The challenge description includes a very strong hint: *"Begin by analyzing the embedded structural tags and properties of the media file."* This points us directly away from visual/audio steganography and straight toward file metadata.

We can use `exiftool`, the standard utility for reading, writing, and editing meta information, to dump all the properties of the provided `secretive_guy.mp4` file.

```bash
$ exiftool secretive_guy.mp4
ExifTool Version Number         : 13.50
File Name                       : secretive_guy.mp4
... [snip] ...
Video Frame Rate                : 30
Handler Type                    : Metadata
Handler Vendor ID               : Apple
Encoder                         : Lavf61.7.100
Ads Created                     : 2026-05-27
Ads Ext Id                      : 4909ce4c-8444-4a0f-a34a-d6e322f45482
Ads Fb Id                       : 525265914179580
Ads Touch Type                  : 2
User Comment                    : The secret is OWASPKL{pl4y_7h3_vid}
Creator Tool                    : Canva brand=HiyuHiyu engine=wlc platform=web
Media Data Size                 : 451383
Media Data Offset               : 8246
... [snip] ...

```

Scrolling through the output, we notice a custom metadata tag inserted by the video editing software (`Canva`). The `User Comment` field clearly contains our flag in plain text!

### Step 2: Alternative Extraction using Strings

Alternatively, since we know the standard flag format begins with `OWASPKL{`, we can bypass metadata parsers entirely. By using the `strings` command, we can search the raw binary data of the MP4 file for readable text, and pipe it into `grep` to quickly filter for the flag.

```bash
$ strings secretive_guy.mp4 | grep -i "OWASPKL"
    <rdf:li xml:lang='x-default'>The secret is OWASPKL{pl4y_7h3_vid}</rdf:li>

```

This confirms our finding. The string extraction reveals that the comment was actually stored inside an XMP/RDF metadata wrapper embedded within the MP4 container.

### Step 3: Formatting the Flag

Both methods lead us directly to the hidden secret. We simply extract the flag format exactly as it was found in the user comments.

## Flag

**`OWASPKL{pl4y_7h3_vid}`**
