# CTF Write-up: On the run

## Challenge Overview

* **Name:** On the run
* **CTF:** OWASPKL
* **Category:** OSINT / Geolocation
* **Objective:** Identify the historical landmark shown in the provided image and determine the name of the nearest railway station to construct the flag.

---

## Tools Used

* Reverse Image Search Engine (Google Lens, Yandex Images, or TinEye)
* Google Maps - For geospatial reconnaissance and routing

---

## Step-by-Step Solution

### Step 1: Initial Analysis & Identifying the Landmark

The first step in geolocation is analyzing the provided image (`location.jpg`) for unique architectural features. Looking at the photo, several clear indicators jump out:

1. Distinctive exposed brickwork along a long corridor.
2. Moorish and Indo-Saracenic style arched doorways and windows.
3. An unfinished, ruined aesthetic overlooking a green landscape.

By running this image through a reverse image search tool like Google Lens, the results immediately point to **Kellie's Castle** (also known as Kellie's Folly). A quick verification confirms that this is a famous, unfinished ruined mansion located in Batu Gajah, Perak, Malaysia.

### Step 2: Locating the Nearest Train Station

The challenge brief states that the suspects fled via public transport and asks for the "Name_Of_Railway_Station" nearest to the location.

Knowing they are at Kellie's Castle, we open Google Maps to trace their escape route:

1. Search for "Kellie's Castle, Batu Gajah, Perak".
2. Use the "Search Nearby" function and look for "Train Station" or "Railway Station".
3. The map reveals that the closest major railway infrastructure is the **Batu Gajah Railway Station** (Stesen Keretapi Batu Gajah), which is part of the KTM ETS line. It is situated just a few kilometers away from the castle.

### Step 3: Constructing the Flag

The challenge specifies that the final flag is the name of the railway station, formatted as `OWASPKL{Name_Of_Railway_Station}`. Following standard CTF conventions for strings with spaces, we replace the space with an underscore.

* **Station Name:** Batu Gajah
* **Formatted:** Batu_Gajah

Finally, we wrap the formatted string in the required flag format. *(Note: Depending on the exact challenge parameters, variations like `Batu_Gajah_Railway_Station` might be tested, but the town/station name is the standard).*

## Flag

**`OWASPKL{Batu_Gajah}`**
