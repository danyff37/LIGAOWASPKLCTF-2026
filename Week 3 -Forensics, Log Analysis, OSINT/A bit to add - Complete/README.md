# CTF Write-up: A bit to add

## Challenge Overview

* **Name:** A bit to add
* **CTF:** OWASPKL
* **Category:** OSINT
* **Objective:** Extract the exact creation date of a provided Bitly shortlink by accessing its public metadata expansion page to construct the flag.

---

## Tools Used

* Web Browser
* Bitly Public Analytics Feature

---

## Step-by-Step Solution

### Step 1: Analyzing the Clues

The challenge provides a shortlink (`bit.ly/4a6EzHh`) and two major hints:

1. The title "**A bit to add**" references the Bitly service and implies appending a character to the URL.
2. The initial tip to explore "**public metadata expansion paths**" points toward a feature that reveals analytics or metadata about the link without actually visiting the destination.

### Step 2: The Bitly Metadata Trick

Bitly has a well-known public analytics feature. By appending a plus sign (`+`) to the very end of any valid Bitly shortlink, anyone can view its public information page. This page includes the destination URL, page title, and the exact creation date.

We take the provided link and add the `+` character to create our target URL:

```text
https://bit.ly/4a6EzHh+

```

### Step 3: Extracting the Data

Navigating to this modified URL in a web browser bypasses the redirect to the target site (`[https://john-doe-elite-hacker.carrd.co/](https://john-doe-elite-hacker.carrd.co/)`) and instead opens the Bitly metadata page.

The metadata page reveals the following information:

* **Title:** My E-Business Card
* **Creation Date:** May 27 2026 06:14 UTC

### Step 4: Formatting the Flag

To get the final flag, the creation date (May 27, 2026) needs to be converted into the challenge's requested format: `OWASPKL{mmm_dd_yyyy}`, ensuring the use of lowercase letters for the month.

* **Month:** may
* **Day:** 27
* **Year:** 2026

Finally, we wrap the resulting date string in the required flag format.

## Flag

**`OWASPKL{may_27_2026}`**
