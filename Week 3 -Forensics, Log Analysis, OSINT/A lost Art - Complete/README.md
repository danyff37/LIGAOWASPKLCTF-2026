# CTF Write-up: A lost art

## Challenge Overview

* **Name:** A lost art
* **CTF:** OWASPKL
* **Category:** OSINT
* **Objective:** Decode a three-word coordinate string, locate a historical mural that no longer exists, and analyze historical imagery to find the color of a specific character's shirt.

---

## Tools Used

* Web Browser
* What3Words (Geocoding platform)
* Google Maps / Open-source image search (Google Images)

---

## Step-by-Step Solution

### Step 1: Decoding the Coordinates

The first step in tracking down the "asset drop" is understanding the cryptic clue: *"global three-word mapping string"* alongside the words *"Soil. Chucks. Promises."* This is a direct reference to **What3Words (W3W)**, a geocode system that assigns a unique three-word address to every 3-meter square in the world.

By navigating to what3words.com and searching for `///soil.chucks.promises`, the map drops a pin exactly at **Chew Jetty in George Town, Penang**. This confirms the "coastal settlement" clue mentioned in the prompt.

### Step 2: Historical OSINT Research

The challenge mentions a "foreign man" and a "lost art tragedy." George Town is famous for its street art, primarily spearheaded by Lithuanian artist **Ernest Zacharevic** in 2012.

Because the prompt specifies this is *lost* art, looking at the current Google Street View will only show the modern replacement mural ("Folklore by the Sea"). We need to find what *used* to be there.

A targeted Google search combining these data points is required:
`"Ernest Zacharevic" "Chew Jetty" mural 2012`

### Step 3: Analyzing the Original Artwork

The search results reveal photographs of Zacharevic's original 2012 mural at that exact location, titled **"Children in a Boat."** Because it was painted on wooden stilts so close to the water, the harsh sea elements caused it to peel and wash away completely within a year—perfectly explaining the "lost art tragedy."

The original artwork depicts two children playing inside a wooden sampan (boat) with a pet cat.

### Step 4: Extracting the Flag

The final instruction is to look for the **main colour of the shirt worn by the child closest to the cat**.

By closely examining the historical 2012 photographs of "Children in a Boat," we can observe the layout:

1. The cat is located on the far left side of the boat.
2. The child sitting on the left, immediately next to the cat, is a young boy.
3. The main colour of his shirt is **blue**.

Finally, we wrap that colour in the required flag format.

## Flag

**`OWASPKL{blue}`**
