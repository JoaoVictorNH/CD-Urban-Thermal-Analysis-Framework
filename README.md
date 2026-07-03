# Urban Thermal Analysis Framework

A ready-to-run toolkit that uses free satellite images to show **how hot a city area is, and how it has changed over time** — including where new buildings appeared and whether those areas got warmer.

**You do not need to know how to code to use this.** Everything runs inside **Google Colab**, a free website where you click "Run" on one step at a time, fill in a few simple boxes (a project name, a date range, a location), and get maps and images back. This guide walks you through the whole thing, click by click.

> 💡 **First time here?** Read [How the Notebooks Work](#how-the-notebooks-work) first — it explains the button-and-form style used everywhere, and it will make every other step much easier to follow.

---

## Table of Contents

1. [What You Get](#what-you-get)
2. [Words You'll See (Quick Glossary)](#words-youll-see-quick-glossary)
3. [What You Need Before Starting](#what-you-need-before-starting)
4. [One-Time Setup](#one-time-setup)
5. [How the Notebooks Work](#how-the-notebooks-work)
6. [Which Notebook Should I Open?](#which-notebook-should-i-open)
7. [Running a Notebook, Step by Step](#running-a-notebook-step-by-step)
8. [Finding and Viewing Your Results](#finding-and-viewing-your-results)
9. [The Change-Detection Checking Pipeline (Optional)](#the-change-detection-checking-pipeline-optional)
10. [Troubleshooting](#troubleshooting)
11. [Project Folder Reference (For the Curious)](#project-folder-reference-for-the-curious)
12. [Credits](#credits)

---

## What You Get

This project compares **two dates** of the same area — an earlier one ("T1") and a more recent one ("T2") — using free satellite pictures, and answers questions like:

- 🌡️ **How hot is the surface of the city**, and how has that changed?
- 🏝️ **Where are the "heat islands"** — spots noticeably hotter than the surrounding area?
- 🏗️ **Where did the city grow** (new buildings, new paved areas) between the two dates?
- 🔥 **Did the areas that grew get hotter** than the areas that stayed the same?

Everything is calculated automatically. At the end, you get map images (a format called **GeoTIFF**, opened with free map software) plus charts and simple tables inside the notebook itself, summarizing what was found.

Three ready-made study areas are included — you don't need to set anything up to try them:

| Folder | What it covers |
|---|---|
| `A_Zone/` | Study area "A" |
| `B_Zone/` | Study area "B" |
| `Sobral_Perimeter/` | The full city of Sobral, Ceará, Brazil |

---

## Words You'll See (Quick Glossary)

A handful of terms come up repeatedly. Here's what they mean in plain language:

| Term | In plain words |
|---|---|
| **Notebook** | The document you run — a mix of instructions and clickable steps, ending in `.ipynb`. Think of it as an interactive recipe. |
| **Google Colab** | The free website where notebooks run. No installation needed — just a browser and a Google account. |
| **Cell / Form / Step** | One block inside a notebook. Most look like a small form: a title, a short explanation, a few fill-in boxes, and a ▶ (play) button. Running it performs that one step. |
| **Google Earth Engine (GEE)** | Google's free satellite-image library and computer, used behind the scenes to fetch and process the images. You need a free account for it (details below). |
| **ROI** (Region of Interest) | The boundary of the area you want to study — already provided for the three zones above. |
| **T1 / T2** | The two dates being compared: T1 is the earlier one, T2 is the more recent one. |
| **LST** | *Land Surface Temperature* — how hot the ground/rooftops/vegetation actually is, measured from the satellite (not the air temperature from a weather forecast). |
| **UHI** | *Urban Heat Island* — how much hotter (or cooler) a spot is compared to the average of the whole study area. |
| **GeoTIFF (.tif)** | The map-image file format this project produces. It looks like a picture, but it also carries real-world location data, so it opens correctly in mapping programs like **QGIS** (free) or **Google Earth Pro** (free). |
| **KML / KMZ** | A file format that stores the outline (boundary) of an area, used here to define the ROI. |

---

## What You Need Before Starting

You only need two things, both free:

### 1. A Google Account
Any regular Gmail-based account works. It gives you:
- **Google Drive** — where this project's folder will live.
- **Google Colab** — where the notebooks run.

Don't have one? Create it at [accounts.google.com](https://accounts.google.com) — takes about two minutes.

### 2. A Google Earth Engine Account (free, for research/academic use)

This is what actually supplies the satellite images. Setting it up takes a few minutes, but approval can take anywhere from a few minutes to a day.

1. Go to [earthengine.google.com](https://earthengine.google.com) and click **Get Started**.
2. Sign in with the same Google account from step 1.
3. Fill in the short registration form. When asked for the purpose, choose something like **"Academic / Research"**.
4. Once approved, go to [code.earthengine.google.com](https://code.earthengine.google.com) and note the **project name** shown in the top-left corner (something like `ee-yourname`). You will type this exact name into the notebook later — no need to write it down now, the notebook itself explains where to find it again.

That's it — no downloads, no payment, no coding knowledge needed for either account.

---

## One-Time Setup

You only do this once, the first time you use the project.

### Step 1 — Put the project folder in the right place in Google Drive

The notebooks expect to find the project folder at the very top level of your Google Drive — not tucked inside another folder.

1. Go to [drive.google.com](https://drive.google.com) and make sure you're at the top level of **"My Drive"** (not inside any other folder).
2. Drag the whole `Framework` folder (the one containing `A_Zone`, `B_Zone`, `Sobral_Perimeter`, etc.) into the browser window to upload it.
3. Wait for the upload to finish — this can take a while depending on your internet connection, since it includes satellite image files.

When done, your Drive should look like this:

```
My Drive
 └── Framework
      ├── A_Zone
      ├── B_Zone
      ├── Sobral_Perimeter
      ├── Change_Detection_Validation
      └── Heigths_Neural_Network_Change_Detection
```

> ⚠️ **One file is missing on purpose:** the AI model file (`mmcr_train100.pt`, about 460 MB) is too large to include automatically. You only need it if you plan to run **Section 6 — Urban Change Detection with AI**. Download it from the link in `Heigths_Neural_Network_Change_Detection/Link_Acess.txt` and place it inside that same folder. You can skip this for now and come back to it later.

### Step 2 — Open a notebook

1. In Google Drive, go into the zone folder you want to explore (e.g. `A_Zone`).
2. Double-click the file ending in `.ipynb` (e.g. `Pipeline_A_Zone.ipynb`).
3. It should open automatically in **Google Colaboratory**. If it opens as a preview instead, right-click the file → **Open with** → **Google Colaboratory**.

You're now ready to run it — see [Running a Notebook, Step by Step](#running-a-notebook-step-by-step).

---

## How the Notebooks Work

Before you run anything, it helps to know what you're looking at.

Every notebook is a **top-to-bottom list of steps**. Each step is a boxed panel with:
- A **title**, like `2.1 — Define ROI`.
- A short **plain-language explanation** of what it does.
- A few **fill-in fields** — text boxes, dropdown menus, sliders, or date pickers — pre-filled with sensible defaults.
- A **▶ play button** on the left edge (appears when you hover over the panel, or click on it).

**You never need to read or edit any programming code.** The code that does the actual work is hidden behind each panel by design — what you see is a friendly form. If you're curious and want to peek at it anyway, there's a small `⋮` menu or arrow that reveals it, but it's entirely optional.

To run the notebook:
1. Click the ▶ button on the **first** panel.
2. Wait for it to finish (a spinning icon turns into a ✓ or prints a message when done).
3. Adjust the fields on the **next** panel if needed, then click its ▶ button.
4. Repeat, working **top to bottom, in order** — later steps depend on earlier ones having already run.

If a step is still running, the play button shows a spinning circle. Some steps (like satellite image searches or AI processing) can take a few minutes — that's normal.

---

## Which Notebook Should I Open?

You only need to run **one** notebook to get a full result for one area — you don't need to run all of them.

| I want to... | Open this |
|---|---|
| Analyze study area A | `A_Zone/Pipeline_A_Zone.ipynb` |
| Analyze study area B | `B_Zone/Pipeline_B_Zone.ipynb` |
| Analyze the whole city of Sobral | `Sobral_Perimeter/Pipeline_Sobral_Perimeter.ipynb` |
| Check how accurate the AI change-detection model is | See [The Change-Detection Checking Pipeline](#the-change-detection-checking-pipeline-optional) (optional, more advanced) |

All three main notebooks work the same way and follow the same steps described below — only the pre-filled area boundary differs.

---

## Running a Notebook, Step by Step

The notebook is organized into numbered **Sections**, grouped into three big parts. Run them top to bottom. You can stop after any section and pick up again later — Colab keeps nothing running in the background once you close the tab, so when you come back you'll need to re-run from the top (see [Troubleshooting](#troubleshooting)).

### Part 1 — Input Data (Sections 1–4)

Getting everything connected and choosing what to study.

| Section | What happens | What you might change |
|---|---|---|
| **1 — Setup** | Installs the technical tools this notebook needs and connects to your Google Drive. A pop-up will ask permission to access your Drive — click **Allow**/**Connect**. | Nothing, just run it. |
| Then | A step asks for your **Google Earth Engine project name** (see [What You Need](#what-you-need-before-starting)). The first time, a link opens asking you to log in and paste back a code — this proves it's really you. | Type in your own project name. |
| **2 — Define the Study Area** | Loads the boundary of the area you're studying and shows it on a satellite map. | Already pre-filled correctly for A_Zone/B_Zone/Sobral — you can just run it. Only change this if you want to study a different, custom area. |
| **3 — Choose Satellite Images** | Searches for available satellite pictures within a date range you choose (one search for the "before" period, one for the "after" period), and shows you a list to pick from. | Adjust the date ranges and the maximum cloud cover if you want a different time window. Pick two image IDs from the list shown (one Landsat, one Sentinel-2) and paste them into the next box. |
| **4 — Confirm Configuration** | Locks in your chosen name, map projection, and export folder name. | Usually just run it — defaults are sensible. |

> 💡 Tip from the notebook itself: pick images with **low cloud cover** (under ~10% works well for dry regions) and dates that are close together in the calendar year, to make a fair "before vs. after" comparison.

### Part 2 — Processing Tasks (Sections 5–7)

Where the actual analysis happens.

| Section | What happens |
|---|---|
| **5 — Temperature (LST & Heat Islands)** | Calculates how hot the ground is at each date, and highlights the "heat island" hot spots. This is required for everything after it. |
| **6 — AI Change Detection** *(optional but powerful)* | Uses an AI model to spot **new buildings** between the two dates. This section needs a **GPU** — in Colab's menu, go to `Runtime → Change runtime type` and pick a GPU option before running it. It has several small steps: export images, wait a few minutes for them to process on Google's servers, then run the AI model. The notebook explains each wait. |
| **7 — Vegetation & Built-up Indices** *(optional exploration)* | Shows a catalog of extra measurements you can compute (e.g. how much vegetation, how much bare/paved soil). You can skip this section if you're happy with the defaults already used later on. |

### Part 3 — Outputs Data (Sections 8–10)

Turning the numbers into answers and saving your results.

| Section | What happens |
|---|---|
| **8 — What Changed, and Did It Get Hotter?** | Compares vegetation/built-up change against temperature change across the whole area, with charts and an interactive map. |
| **9 — New Buildings vs. the Rest** | The key comparison: is the area that gained new buildings (from Section 6) significantly hotter than the area that didn't change? Shows a clear summary table with the answer. |
| **10 — Save to Google Drive** | Lets you choose which maps to save, then exports them as files to your Google Drive, inside a `Data_*` folder next to the notebook. |

Exports can take a few minutes for a small area, or longer for a large one. You can check progress at [code.earthengine.google.com/tasks](https://code.earthengine.google.com/tasks) — page will show each export as "Running" and then "Completed".

---

## Finding and Viewing Your Results

After Section 10, your results appear in Google Drive, inside:

```
Framework / <zone name> / Data_<zone name> /
```

All files end in `.tif` (GeoTIFF) — they are map images with location built in. To view them properly (with correct colors and geographic position), open them in free software such as:

- **[QGIS](https://qgis.org)** (recommended, free, works on Windows/Mac/Linux) — drag and drop the `.tif` file onto the map.
- **Google Earth Pro** (free) — also supports GeoTIFF layers.

Simply double-clicking a `.tif` file on your computer usually won't show it correctly, since normal photo viewers don't understand the geographic information inside it.

The notebook itself also shows interactive maps and charts as you go — no extra software needed to see those, they appear right below each step you run.

---

## The Change-Detection Checking Pipeline (Optional)

This is a separate, more advanced set of three notebooks in `Change_Detection_Validation/`, used to double-check how accurate the AI model (used in Section 6 above) really is, by comparing its output against a manually verified reference. You don't need it to get results for a study area — it's a quality-check tool for the AI model itself.

Run the three notebooks **in this exact order**, waiting for each one to fully finish before opening the next:

| Order | Notebook | What it does |
|---|---|---|
| 1 | `01.Search_and_Export_Sentinel_Data.ipynb` | Finds and exports the satellite images used for the check. |
| 2 | `02.U-net_extraction_Change_Detection_Map_for_Validation.ipynb` | Runs the AI model on those images and saves its answer. |
| 3 | `03.Validation_CD_Model.ipynb` | Compares the AI's answer to a known-correct reference and reports how accurate it was. |

> Wait for the export tasks from step 1 to show "Completed" at [code.earthengine.google.com/tasks](https://code.earthengine.google.com/tasks) before opening step 2.

---

## Troubleshooting

**A pop-up asks for permission to my Google account — is this safe?**
Yes. Colab needs permission to read/write files in your own Drive, and Earth Engine needs permission to confirm it's really you. Both are official Google sign-in screens.

**"Module not found" or a red error mentioning `modulos`**
This usually means Google Drive isn't connected yet, or the `Framework` folder isn't at the top level of your Drive. Re-run the very first step (Section 1) and make sure you allowed the Drive connection pop-up.

**Earth Engine keeps asking me to log in / authentication fails**
Just run that step again. If it keeps failing, visit [earthengine.google.com](https://earthengine.google.com), confirm you're signed in, and check that your project still appears at [code.earthengine.google.com](https://code.earthengine.google.com).

**No satellite images show up when I search**
Try a wider date range or a higher "maximum cloud cover" value in Section 3 — clear, cloud-free images aren't available every day everywhere.

**My exported files aren't showing up in Drive**
Exports run on Google's servers in the background, not instantly. Check [code.earthengine.google.com/tasks](https://code.earthengine.google.com/tasks) — large areas can take 10–30 minutes.

**Colab disconnected / says my session ended**
Free Colab sessions time out after a period of inactivity or after a few hours. This is normal — nothing is lost from your Drive, but you'll need to run the notebook again from the top (Section 1). Anything already exported to Drive is safe.

**Section 6 (AI Change Detection) says no GPU is available**
In the Colab menu, go to `Runtime → Change runtime type`, choose a GPU option (e.g. "T4 GPU"), save, and run Section 6 again.

**The model file `mmcr_train100.pt` can't be found**
It isn't included automatically because it's too large (~460 MB). Download it using the link in `Heigths_Neural_Network_Change_Detection/Link_Acess.txt` and place it in that same folder on your Drive, keeping the exact file name.

---

## Project Folder Reference (For the Curious)

You don't need any of this to run the notebooks — it's here for reference, or for anyone later extending the project with code.

```
Framework/
│
├── A_Zone/
│   ├── Pipeline_A_Zone.ipynb     ← the notebook you open
│   ├── A_Zone.kmz                ← saved boundary of the study area
│   ├── Data_A_Zone/              ← your results appear here after Section 10
│   └── modulos/                  ← the hidden code behind every step (no need to open)
│
├── B_Zone/                       ← same structure as A_Zone
├── Sobral_Perimeter/             ← same structure, covers the full city
│
├── Change_Detection_Validation/  ← the optional AI accuracy-check pipeline
│
└── Heigths_Neural_Network_Change_Detection/
     ├── mmcr_train100.pt         ← AI model file (download separately, see setup)
     └── Link_Acess.txt           ← download link and citation
```

**Main result files you'll find in each `Data_*` folder**, in plain terms:

| File pattern | What it shows |
|---|---|
| `LST_30m_*` | Surface temperature map |
| `UHI_30m_*` / `UHI_Intensity_30m_*` | Heat island map / heat island severity (5 levels) |
| `Sentinel2_SAVI_*` | Vegetation cover |
| `Sentinel2_DBSI_*` | Bare/dry soil |
| `Sentinel2_NBAI_*` | Built-up (constructed) area |
| `Delta_*` | The change between the two dates for any of the above |
| `changemask_*` | Areas flagged by the AI as newly built between the two dates |

All map files use coordinate system **EPSG:32724** (UTM Zone 24S), which matches the Ceará/Northeast Brazil region used in the default study areas.

---

## Credits

**AI model used for change detection:**

> Hafner, S., Ban, Y. and Nascetti, A., 2023. *Semi-Supervised Urban Change Detection Using Multi-Modal Sentinel-1 SAR and Sentinel-2 MSI Data.* Remote Sensing, 15(21), p.5135.

Original repository: [github.com/SebastianHafner/SemiSupervisedMultiModalCD](https://github.com/SebastianHafner/SemiSupervisedMultiModalCD)

**Satellite data:** Copernicus Sentinel-1 and Sentinel-2 (European Space Agency) | Landsat (USGS/NASA) — accessed through Google Earth Engine.
