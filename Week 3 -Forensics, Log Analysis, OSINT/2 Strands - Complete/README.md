# CTF Write-up: 2 Strands - OSINT

## Challenge Overview

* **Name:** 2 Strands
* **CTF:** OWASPKL
* **Category:** OSINT
* **Objective:** Track the digital footprint of a suspect (Alexei Pasler) on a Russian social media platform, locate a specific image, determine its physical location, and retrieve the Global Plus Code to construct the flag.

---

## Tools Used

* Web Browser (Google Search, Google Maps)
* Linux Terminal (Kali)
* `exiftool` - For metadata analysis
* Yandex Images - For regional reverse image searching

---

## Step-by-Step Solution

### Step 1: Initial Reconnaissance & Identifying the Platform

The challenge states the suspect, Alexei Pasler, was active on a popular Russian social media platform following a ransomware attack. The most widely used social network in Russia is **VKontakte (VK)**. We begin by searching for his profile using either his English name or the Cyrillic translation ("Алексей Паслер").

Using targeted search engine queries (e.g., `site:vk.com "Алексей Паслер"`), we successfully isolate his profile.

### Step 2: Extracting the Digital Trail

On his VK profile (`[vk.com/wall1110451979_2](https://vk.com/wall1110451979_2)`), we find a post dated April 15, 2026. The caption translates to "Just a quiet walk in a special place."

Crucially, the post contains two main artifacts:

1. An attached audio track titled **"Alley"**.
2. An image of a paved, tree-lined pathway flanked by two dark steles.

### Step 3: Technical Analysis

Before relying on visual clues, we download the image and check for any hidden metadata (like GPS coordinates) using `exiftool`.

```bash
$ exiftool esjbah2SQ4wu_XBYhTxFWo470VSWklDBbQHnpW6ITnQI3iqblZc1L4ZHjVlsBJJQJIlDl77feTPl1w7cJA9HSmjx.jpg
ExifTool Version Number         : 13.50
File Name                       : esjbah2SQ4wu...jpg
...
Image Width                     : 1138
Image Height                    : 640

```

The output reveals a completely clean file with no GPS data or original timestamps. Since social media platforms like VK automatically strip EXIF data for privacy, the technical route is a dead end. We must pivot to visual intelligence (IMINT).

### Step 4: Visual Intelligence (IMINT)

We carefully examine the visual features of the image:

* **The Pillars:** Zooming in on the right pillar reveals stylized carvings depicting circles dividing from one, to two, to four. This represents biological cell division (mitosis).

Synthesizing our gathered intelligence:

* **The Challenge Title:** "2 Strands"
* **The Audio Clue:** "Alley"
* **The Visual Clue:** Cell division

Combined, these clues point directly to a double helix, leading us to search for a "DNA Alley".

### Step 5: Geolocation & Flag Extraction

Searching for a DNA-themed alley or genetic monuments in Russia leads to the scientific town of **Akademgorodok in Novosibirsk**. The pathway depicted in the photograph is situated in a park in front of the Institute of Cytology and Genetics. This specific walkway leads directly to the famous **Monument to the Laboratory Mouse** (a bronze sculpture of an anthropomorphic mouse knitting a DNA double helix).

With the exact physical location identified, we retrieve the Global Plus Code:

1. Locate the "Monument to the Laboratory Mouse" in Novosibirsk on Google Maps.
2. Drop a pin precisely on the monument's coordinates (`54.849028, 83.106056`).
3. Extract the alphanumeric Global Plus Code from the information panel.

The Global Plus Code is `9M65R4X4+J9`. We wrap this code in the required challenge format.

## Flag

**`OWASPKL{9M65R4X4+J9}`**
