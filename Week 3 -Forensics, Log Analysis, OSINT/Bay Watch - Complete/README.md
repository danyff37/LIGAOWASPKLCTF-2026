# CTF Write-up: Bay Watch

## Challenge Overview

* **Name:** Bay Watch
* **CTF:** OWASPKL
* **Category:** OSINT (Open Source Intelligence)
* **Objective:** Identify the exact location of a turtle sanctuary shown in a provided image (`object.jpg`) to verify an environmental researcher's field deployment area, excluding words like "Turtle Sanctuary".

---

## Tools Used

* Image Viewer - For initial visual inspection and clue extraction
* Web Browser / Search Engine - For advanced dorking and data correlation

---

## Step-by-Step Solution

### Step 1: Initial Analysis & Clue Extraction

The provided image features a large wooden billboard set up outdoors in a tropical, coastal environment. The billboard displays a painted mural of a sea turtle on the sand and an LNG (Liquefied Natural Gas) carrier ship in the water.

Following the challenge's initial tip to look for "distinct and unique features," the most critical OSINT goldmines are the four logos painted in the top right corner of the canvas:

1. **UMT:** The official logo for *Universiti Malaysia Terengganu*.
2. **misc:** The corporate logo for *MISC Berhad*, a Malaysian shipping line (which perfectly explains the LNG vessel in the painting).
3. **heart OF THE OCEAN:** A stylized logo featuring a sea turtle, indicating a specific environmental campaign or initiative.
4. **PeTer:** Accompanied by the text *Persatuan Pelukis Terengganu* (Terengganu Artists Association), confirming the local artists who created the mural.

### Step 2: Open Source Research (Dorking)

With these specific entities identified, we can pivot to a search engine. We need to construct a query that ties all these unique elements together.

Executing a targeted search query like:

```text
"MISC" "UMT" "Heart of the Ocean" turtle

```

This immediately brings up relevant news articles, corporate sustainability reports, and university press releases.

### Step 3: Correlating the Data

The search results reveal that MISC Group and Universiti Malaysia Terengganu (UMT) have a joint marine biodiversity and conservation initiative operating under the banner **"Heart of the Ocean."**

By reading through the public details of the *UMT-MISC Sea Turtle Conservation Initiative*, we learn about their field deployment areas. Their flagship location—which features hatcheries, volunteer programs, and an "outdoor classroom" where such murals are erected—is located on Redang Island (Pulau Redang), Terengganu.

Specifically, the sanctuary is officially named the **Chagar Hutang Turtle Sanctuary**.

### Step 4: Constructing the Flag

The challenge explicitly states to exclude the words "Turtle Sanctuary" from the actual flag and to use the format `OWASPKL{Location_Name}`.

Taking the specific location name, **Chagar Hutang**, we replace the space with an underscore (standard for most rigid CTF flag formats) and wrap it in the required flag wrapper. *(Note: If the CTF platform does not strictly enforce underscores for spaces, `OWASPKL{Chagar Hutang}` is the direct alternative).*

---

## Flag

**`OWASPKL{Chagar_Hutang}`**
