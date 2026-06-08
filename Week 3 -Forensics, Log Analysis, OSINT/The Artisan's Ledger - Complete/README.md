# CTF Write-up: The Artisan's Ledger

## Challenge Overview

* **Name:** The Artisan's Ledger
* **CTF:** OWASPKL
* **Category:** OSINT / Forensics
* **Objective:** Identify the real-world location of a provided image, extract and convert nearby Roman numeral years to the decimal system, determine the name of a specific referenced sculpture, and construct the flag.

---

## Tools Used

* Web Browser
* Reverse Image Search Engine (e.g., Google Lens, Yandex)
* Open-Source Intelligence (Wikipedia, historical architectural records)

---

## Step-by-Step Solution

### Step 1: Initial Analysis & Visual Search

The provided asset, `artisan.jpg`, displays a bronze relief portrait of a man with a mustache and beard, mounted on a light-colored stone obelisk. Beneath the portrait is a distinct banner bearing the quote from Shakespeare's *Hamlet*: **"TO THINE OWN SELF BE TRUE"**.

To start, we perform a reverse image search or a targeted keyword search querying the quote alongside "bronze relief monument".

### Step 2: Identifying the Monument

The visual and text-based searches immediately identify the structure as the **Monument to Edward Onslow Ford**, a renowned English sculptor. The monument is located at the intersection of Abbey Road and Grove End Road in St John's Wood, London.

### Step 3: Extracting and Converting the Years

The challenge instructions require us to locate two "Roman numeral year strings that appeared nearby" and convert them to the modern decimal system.

By reviewing wider photos and historical records of the Edward Onslow Ford monument, we find the inscriptions denoting his birth and death years:

1. **MDCCCLII** which converts to **1852** (the smaller number).
2. **MCMI** which converts to **1901** (the bigger number).

### Step 4: Identifying the Sculpture

The final clue tasks us to "find out the name of the sculpture that the nearby figure was based on."

Further research into the monument's architecture reveals that the obverse (back) side features a bronze figure of a mourning Muse. Digging into the history of this specific obelisk shows that this seated figure is a replica. It was cast from one of Edward Onslow Ford's own famous masterpieces: a statue named **The Muse of Poetry** (which is famously part of the Shelley Memorial at Oxford).

### Step 5: Assembling the Flag

The challenge specifies the flag format as `OWASPKL{smaller_bigger_the_name_of_sculpture}`. We assemble the pieces we have gathered:

* Smaller number: `1852`
* Bigger number: `1901`
* Sculpture name: `the_muse_of_poetry`

## Flag

**`OWASPKL{1852_1901_the_muse_of_poetry}`**
