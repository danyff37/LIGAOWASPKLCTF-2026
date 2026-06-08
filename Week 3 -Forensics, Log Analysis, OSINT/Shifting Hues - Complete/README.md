# CTF Write-up: Shifting Hues - OSINT

## Challenge Overview

* **Name:** Shifting Hues
* **CTF:** OWASPKL
* **Category:** OSINT / Historical Geolocation
* **Objective:** Geolocate the landmark in the provided image and determine the historical color of a specific staircase step in the year 2009 to construct the flag.

---

## Tools Used

* Google / Google Lens - For initial landmark geolocation
* Historical Search / Wikimedia Commons - To find archival imagery of the location from the target year

---

## Step-by-Step Solution

### Step 1: Initial Analysis & Geolocation

The first step in this OSINT challenge is identifying the location shown in `macaque.jpg`. The image depicts a long-tailed macaque sitting on a golden, pumpkin-shaped railing ornament. In the blurred background, several key identifiers jump out:

1. A steep, brightly colored staircase (blue, yellow, and red steps).
2. Intricate, colorful Hindu temple architecture.
3. The entrance to a massive limestone cave.

Searching for these visual elements ("limestone cave Hindu temple steep colorful stairs Malaysia") quickly geolocates the landmark to **Batu Caves** in Gombak, Selangor, Malaysia.

### Step 2: Establishing the Timeline

The challenge description explicitly states that tourists photograph the stairs daily in their *current* state, but we need to find the color of the 67th step in the exact year **2009**.

Researching the history of the Batu Caves staircase reveals that the vibrant, rainbow gradient seen in the background of the provided image is a recent addition. The 272 steps were painted in these "shifting hues" in August 2018 as part of a beautification project for a temple consecration ceremony.

### Step 3: Historical OSINT

Knowing the current stairs are a red herring, we need to find out what they looked like in 2009. By utilizing Google Images with custom date ranges (e.g., `before:2010-01-01`) or searching Wikimedia Commons for archival photos of Batu Caves from the late 2000s, the historical footprint becomes clear.

Archival photos from 2009 show that the staircase did not have individual colored zones. Instead, the entire 272-step flight featured a uniform color scheme:

* The **treads** (the horizontal flat part where you step) were painted red.
* The **risers** (the vertical, forward-facing part of the step) were painted **white**.

### Step 4: Bypassing the Rabbit Hole

The prompt specifically asks for the color of the **67th step**. In many OSINT challenges, an exact number implies the need to find a highly specific, high-resolution photo and manually count steps.

However, because the pre-2018 color scheme was entirely uniform across all three flights of stairs, the "67th step" is a classic CTF distractor. Every single step looked exactly the same. When looking up the staircase—which is the dominant visual perspective in almost all historical photos and from the perspective of a climber—the primary visible color of the steps themselves is the white of the risers.

## Flag

**`OWASPKL{white}`**
