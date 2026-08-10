# CosmoVerse 2026 Data Challenge

## Why this challenge exists

The **Hubble constant (H₀)** describes how fast the universe is expanding today. It is one of the most fundamental numbers in cosmology — and right now, two independent ways of measuring it give answers that disagree by roughly 5–10%, at a statistical significance of ~5σ. This disagreement is known as the **Hubble tension**, and it is one of the most prominent open problems in modern cosmology.

On one side: measurements of the early universe using the cosmic microwave background (CMB, e.g. the Planck satellite) give H₀ ≈ 67 km/s/Mpc (kilometres per second per megaparsec). On the other side: measurements of the local universe using the cosmic distance ladder give H₀ ≈ 73 km/s/Mpc. These two numbers should agree if our standard cosmological model is correct. They do not.

**This matters enormously.** If the tension is real — not a measurement error — it could be the first observational evidence of physics beyond the standard ΛCDM model (Lambda Cold Dark Matter — the current best-fit model of cosmology): new particles, new interactions, or a different history of cosmic expansion. Identifying whether the tension is a genuine signal or a systematic error in one or both measurement chains is therefore one of the highest-priority questions in cosmology today.

### What the local distance ladder measurement involves

The local H₀ measurement proceeds in steps (rungs):

1. **Rung 1 — Geometric anchor:** The distance to a nearby galaxy (NGC 4258) is measured directly from water maser orbits (radio emission from water molecules orbiting the galaxy's central black hole, whose geometry and velocity can be measured to give a purely geometric distance), with no assumptions about stellar physics.
2. **Rung 2 — TRGB distances:** That geometric distance calibrates a standard candle (an object whose intrinsic brightness is known, so its distance can be inferred from how bright it appears) called the **Tip of the Red Giant Branch (TRGB)**, which is then used to measure distances to galaxies that also host Type Ia supernovae (a class of stellar explosions with nearly uniform and standardizable intrinsic brightness, usable as distance indicators).
3. **Rung 3 — Supernovae to H₀:** The calibrated supernova brightness, combined with supernova observations of galaxies receding with the Hubble flow (the general expansion of the universe, observed as a redshift proportional to distance), yields H₀.

Errors anywhere in this chain can bias H₀. The key question this challenge asks is: **how much do choices made in the TRGB step (Rung 2) affect the final H₀?**

### What is the TRGB?

Every old galaxy contains millions of red giant stars. As these stars age, they grow steadily brighter — until they reach a critical luminosity at which helium ignition abruptly ends the red giant phase. This upper brightness limit is called the **Tip of the Red Giant Branch**. Because it occurs at nearly the same luminosity in all old stellar populations, it works like a cosmic ruler: measure how bright the tip *appears* in a galaxy, compare it to how bright it *intrinsically* is (calibrated from the geometric anchor), and you get the distance. The distance is reported as a **distance modulus** μ, defined as μ = 5 log₁₀(d / 10 pc), where d is the distance in parsecs — a larger μ means a more distant galaxy.

In practice, finding the tip requires analyzing a catalog of individual stars in a galaxy (the photometry), selecting the red giant population, building a brightness histogram (the luminosity function), and detecting the sharp drop at the bright end — the point where star counts abruptly fall off because no stars exist above the tip. The sharpness of this edge depends on analysis choices — which stars you include, how much you smooth the histogram — and those choices can shift the inferred distance and therefore H₀. That sensitivity is what this challenge is designed to measure.

### Why a community-wide data challenge?

Different research groups analyzing the same TRGB data have obtained H₀ values that differ from each other by more than their stated uncertainties. Some of this scatter comes from genuine methodological differences: which stars to include, how to detect the tip, how to correct for dust (interstellar dust dims and reddens stars, shifting their apparent brightness and therefore the inferred TRGB magnitude). But it has been difficult to disentangle the effects of those choices because each group uses different data, different code, and different pipelines.

This challenge fixes the dataset and the supernova analysis, and asks many participants to independently measure TRGB distances with their own methodological choices. By comparing results across participants, we can:

- **Quantify how much TRGB methodology drives scatter in H₀**, independent of supernova or calibration differences.
- **Make the measurement transparent and reproducible** — all data, code, and individual results will be publicly released so that anyone in the community can inspect, reproduce, and build on the work.
- **Democratize access** to a measurement that has historically been accessible only to a small number of groups with specialized pipelines.

The output of this challenge is a **public repository** containing the common dataset, all participant pipelines, and a compiled comparison of results. This is a contribution to the field, not just a training exercise.

Methodologies are introduced at the **CosmoVerse 2026 Summer School** (organized by the **CosmoVerse COST Action**, a European network of cosmologists working on the Hubble tension and related problems).

---

## The ground rules — what you can and cannot change

**Please read this before you start.**

### ✅ You are free to explore: the TRGB measurement

Everything in the TRGB notebook (`1_trgb/`, Part 1 of this guide) is yours to experiment with. The star selection cuts (which color and brightness range of stars to include), the smoothing bandwidth (how much to blur the brightness histogram before detecting the tip), and the spatial clipping region (whether to exclude the crowded central regions of the galaxy) — these are your scientific choices, and varying them is the entire point of the challenge. There is no single correct answer. Try different settings, look at the plots, and pick settings you can scientifically justify.

### 🚫 Do not change: the supernova analysis

The H₀ notebook (`2_hubble_constant/`, Part 2) uses a fixed dataset of Type Ia supernovae and a fixed statistical pipeline to convert your TRGB distances into H₀. **Do not modify the SN data files or the fitting code.** The supernova analysis is held constant across all participants so that differences in H₀ can be traced back to TRGB choices alone. Modifying the SN side makes your result incomparable to everyone else's and undermines the purpose of the challenge.

> **Note on future work:** How choices in the supernova analysis affect H₀ is an open question and a strong candidate for a future CosmoVerse data challenge. That effort is separate and TBD — it is not part of this challenge.

---

## Setup — do this once before anything else

### 1. Get the code

If you have not already, download this repository to your computer. On the [CosmoVerse Data Challenge GitHub page](https://github.com/cosmoversecost/Data-Challenge), click the green **Code** button and select **Download ZIP**. Unzip the downloaded file.

When downloaded from GitHub, the unzipped folder will typically be named `Data-Challenge-main`. You can rename it to anything you like or leave it as-is — the name does not matter, only its contents. The folder should contain this README and the subfolders `1_trgb/`, `2_hubble_constant/`, and `photometry/`.

(If you are familiar with git, you can also clone the repository instead.)

### 2. Install Python and required packages

You need Python 3.8 or later and Jupyter notebooks. If you do not already have these, install [Anaconda](https://www.anaconda.com/download) (recommended — it includes Python, Jupyter, and most scientific packages) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html).

Open a terminal (on Mac: Applications → Utilities → Terminal; on Windows: open the **Anaconda Prompt** from the Start menu — do not use the standard Windows Command Prompt) and install the required packages:

**If you installed Anaconda or Miniconda:**
```bash
conda install numpy pandas matplotlib astropy jupyter
```

**If you are using a standalone Python installation:**
```bash
pip install numpy pandas matplotlib astropy jupyter
```

### 3. Verify the folder structure

After downloading, your directory should look like this:

```
CosmoVerse Data Challenge/
├── README.md                     ← this file
├── photometry/
│   ├── NGC4258_xxxxx.csv         ← anchor galaxy (always needed)
│   └── NGC1365_xxxxx.csv         ← your target galaxy
├── 1_trgb/
│   └── TRGB_distance_measurement.ipynb
└── 2_hubble_constant/
    └── demo_mu_to_H0.ipynb
```

### 4. Launch Jupyter

Open a terminal and navigate to the `CosmoVerse Data Challenge/` folder, then launch Jupyter. For example, if the folder is on your Desktop:

**On Mac or Linux:**
```bash
cd ~/Desktop/Data-Challenge-main
jupyter notebook
```

**On Windows (in the Anaconda Prompt):**
```bash
cd "C:\Users\YourName\Desktop\Data-Challenge-main"
jupyter notebook
```

Replace `Data-Challenge-main` with the actual name of your folder, and replace the path with wherever you saved it (e.g. `~/Downloads/Data-Challenge-main` if you have not moved it yet). A browser window should open automatically showing the contents of the folder. If it does not open, look at the terminal output — Jupyter will print a line like:

```
http://localhost:8888/tree?token=abc123...
```

Copy that full URL and paste it into your browser manually.

**Leave this terminal window open for the entire session** — closing it will stop Jupyter. If you accidentally close it, just run `jupyter notebook` again from the same folder. You will open both notebooks (`1_trgb/` and `2_hubble_constant/`) from this same browser window, so always launch Jupyter from this top-level folder.

---

## How the pipeline works

The pipeline has two parts connected by a shared results file:

1. **Part 1 — TRGB notebook (`1_trgb/`):** You measure the apparent brightness of the red giant tip in each galaxy. Combined with the known distance to NGC 4258 (the geometric anchor), this gives the distance modulus μ to your target galaxy. You iterate on your parameter choices until you are satisfied, then export the result.

2. **Part 2 — H₀ notebook (`2_hubble_constant/`):** Your exported distance moduli are automatically loaded and fed into the fixed supernova pipeline, which computes H₀.

The file `trgb_results.csv` (created automatically in the `CosmoVerse Data Challenge/` folder the first time you export) is the handoff between the two parts. It accumulates your measurements one galaxy at a time (there is also a batch option to run through all galaxies with the same measurement parameters).

---

## Part 1: Measure the Distance to a Galaxy

In the Jupyter browser window, click on `1_trgb/` and then open `TRGB_Distance_Measurement_CosmoVerse_DataChallenge_2026.ipynb`.

---

### Step 1 — Pick your galaxy

Scroll down through the notebook until you see the heading **"2."** or **"Section 2"**. Find the line:

```python
TARGET_GALAXY = 'NGC1365'   # ← change this
```

Replace `'NGC1365'` with the name of your assigned galaxy (e.g. `'NGC4536'`). Use the exact same capitalization and format as the photometry filename — for most galaxies this is `'NGC'` followed by the number with no spaces (e.g. `'NGC1448'`). For Messier galaxies the prefix is `'M'` (e.g. `'M101'`). Do not change anything else in this cell.

---

### Step 2 — Run the notebook from the top

In the menu bar at the top of the notebook, click **Kernel**, then **Restart & Run All**. A dialog box will appear — click **Restart and Run All Cells** to confirm.

The notebook will run through all sections automatically. This may take a minute or two. **Do not proceed to Step 3 until the notebook finishes running.** Two ways to tell it has finished: (1) the spinning indicator in your browser tab returns to a static icon, and (2) every *code* cell in the notebook shows a number in square brackets on its left (e.g. `[12]`) rather than an asterisk `[*]` — an asterisk means that cell is still running. Text and heading cells do not show brackets at all; that is normal.

If a cell produces an error shown in red, stop and check the Troubleshooting section at the bottom of this guide. For errors not listed there, contact your session coordinator.

The notebook will:
- Load the star catalogs for NGC 4258 (anchor) and your target galaxy from the `photometry/` folder
- Display a color-magnitude diagram (a plot of each star's brightness vs. color)
- Detect the TRGB using a Sobel edge-detection filter (a mathematical operation that finds sharp edges in the brightness histogram)
- Run a bootstrap uncertainty estimate (repeated random resampling to quantify statistical noise)
- Compute the distance modulus μ

---

### Step 3 — Inspect the diagnostic plots and adjust parameters

This is the scientific heart of the challenge. Your goal is to make choices that produce a clean, stable TRGB detection and to document your reasoning.

Go to **Section 3** in the notebook (and also the quick-tune cell near the top of **Section 6**, which lets you adjust parameters without scrolling all the way back up). You will find parameter cells like:

```python
SELECTION['color_lo']  = 1.0   # blue edge of the RGB selection region
SELECTION['color_hi']  = 1.5   # red edge of the RGB selection region
SMOOTHING['tau']       = 0.10  # smoothing bandwidth in magnitudes
```

Change these values, then re-run from that cell downward: click the parameter cell, then in the menu click **Run → Run All Below**. You do not need to restart the whole notebook each time. This will re-run all cells below your parameter cell, including the bootstrap, which may take a minute — this is normal.

**What to look for in the three diagnostic panels:**

- **Color-magnitude diagram (left panel):** The red giant branch should appear as a roughly diagonal sequence of stars running from faint (bottom) to bright (top). Your selection region (shown as a box) should surround the upper part of this sequence. Avoid including stars that are clearly not on the red giant branch — for example, a sparse scatter of stars just above the RGB tip (these are AGB stars — helium shell-burning stars at a slightly later evolutionary stage than the RGB), a roughly horizontal sequence at bluer colors (the horizontal branch or red clump — core helium-burning stars), or a vertical blue sequence (young, massive stars unrelated to the red giant branch). The exact appearance varies by galaxy.
- **Luminosity function and Sobel response (middle panels):** The brightness histogram of selected stars should show a rising trend toward fainter magnitudes (in astronomy, fainter = larger magnitude number, so this means rising toward the right side of the histogram) with a drop at the bright end (left side) — the drop marks the TRGB. The Sobel filter response (the derivative of this histogram) should show a single clear peak at the TRGB location. Multiple peaks of similar height mean the detection is ambiguous; try increasing `tau` (more smoothing) or tightening the color selection.
- **Bootstrap distribution (right panel):** The distribution of TRGB estimates from repeated resampling should look like a single bell-shaped curve centered on your measurement. A bimodal (two-humped) distribution means the algorithm is jumping between two candidate edges; increase `tau` until the distribution becomes unimodal.

The notebook's **Section 9a** contains a formal stability checklist — try `tau` values of 0.05, 0.10, 0.15, and 0.20, and shift `color_lo` and `color_hi` each by ±0.2 mag, checking that the TRGB magnitude does not move by more than ~0.03 mag across these variations. You should run this checklist for each galaxy. A measurement that shifts significantly under small parameter changes should not be reported as your final result without flagging the instability.

There is no single correct parameter choice. Document your reasoning in the notebook by adding a text cell (click **Insert → Insert Cell Below**, then change the cell type to **Markdown** from the dropdown at the top of the notebook) and writing a brief note explaining your choices.

---

### Step 4 — Check the distance result

Scroll to **Section 8** in the notebook. The result is already computed from Step 2 — you are just reading the output. It prints a summary like:

```
μ(target) = 31.48 ± 0.09
d = 19.8 ± 0.8 Mpc
```

μ is the distance modulus; d is the physical distance in megaparsecs (Mpc), where 1 Mpc ≈ 3.26 million light-years. As a sanity check: the challenge galaxies are all in the range μ ≈ 30–34 mag (roughly 10–65 Mpc). A value far outside this range suggests something went wrong in the detection — go back to Step 3, adjust your parameters, re-run from that cell downward, and check Section 8 again.

You can compare your result to published measurements. Two useful public catalogs are:
- **EDD** (Extragalactic Distance Database): a compilation of distance measurements to nearby galaxies from many methods — [edd.ifa.hawaii.edu](http://edd.ifa.hawaii.edu)
- **CATs** (Comparative Analysis of TRGBs): TRGB-specific distances using a consistent pipeline and methodological choices across all galaxies — referenced in the notebook's citation list -[https://github.com/JiaxiWu1018/CATS-H0/tree/main]

---

### Step 5 — Export your measurement

When you are satisfied with your result, scroll to **Section 10** at the very bottom of the notebook. Run the single cell there (click on it and press **Shift+Enter**, or click **Run → Run Selected Cells**).

This saves your result to a file called `trgb_results.csv` in the top-level `CosmoVerse Data Challenge/` folder — one level above the `1_trgb/` folder where the notebook lives. You will see a confirmation:

```
Added new entry for NGC1365.
Saved to: /path/to/CosmoVerse Data Challenge/trgb_results.csv

All measurements so far:
    host       mu  sigma_mu galaxy_name
  n1365  31.4800    0.0900     NGC1365
```

Note: the `host` column shows `n1365` rather than `NGC1365`. This is an internal shorthand used by the H₀ notebook — it is automatically derived from your galaxy name and is correct.

If you later change your parameters and re-export, the cell will overwrite your previous entry for that galaxy rather than adding a duplicate.

---

### Repeat Steps 1–5 for each assigned galaxy

Go back to Step 1, change `TARGET_GALAXY` to your next assigned galaxy, and work through the steps again. Each galaxy gets its own row in `trgb_results.csv`. You do not need to finish all galaxies before moving to Part 2 — you can compute an interim H₀ after measuring just one.

---

## Part 2: Calculate the Hubble Constant

**Reminder: do not modify any files in `2_hubble_constant/data/` or the file `mu_to_H0.py`.** See the ground rules above.

Go back to the Jupyter file browser. To do this from inside the TRGB notebook, click the **Jupyter logo** in the top-left corner of the notebook — this returns you to the file listing. Alternatively, use the URL that Jupyter printed in your terminal when it started (the one beginning with `http://localhost:...`). From the file browser, click on `2_hubble_constant/` and open `demo_mu_to_H0.ipynb`.

---

### Step 6 — Run all cells from the top

Click **Kernel → Restart & Run All**, then confirm. The notebook will:

1. Load the fixed SN dataset from the `data/` folder.
2. Read your `trgb_results.csv` and print which galaxies were found.
3. Compute H₀ using your TRGB distances, automatically omitting galaxies you have not yet measured.
4. Print your result and display a Hubble diagram.

The output will look like:

```
Loaded 1 TRGB measurement(s):
    host      mu  sigma_mu galaxy_name
   n1365  31.480    0.090     NGC1365

1 host(s) with measurements: ['n1365']
36 host(s) dropped (no measurement yet).

H0 = 72.3 ± 1.5 km/s/Mpc
```

The more galaxies you measure, the more calibrators enter the fit and the tighter the H₀ uncertainty. Having only one or a few galaxies measured is expected at this stage — the result is still valid.

---

### Step 7 — Explore and compare

The rest of the notebook shows diagnostic plots — a Hubble diagram of your calibrator galaxies and Hubble-flow supernovae, and a plot showing how H₀ shifts with a uniform offset in your distance scale. These are already computed and just need to be read.

If you want to try different TRGB parameter choices and see the effect on H₀: click the Jupyter logo to go back to the file browser and navigate into `1_trgb/` — or, if you left the TRGB notebook open, simply switch to that browser tab. Adjust parameters and re-export (Step 5), then return to the H₀ notebook tab and click **Kernel → Restart & Run All** to pick up the updated results. Do not manually edit `trgb_results.csv` directly — always update it by re-running Section 10 of the TRGB notebook.

---

## What to submit

Participants are expected to submit:

Run **Section 9** of the TRGB notebook when you are satisfied with your measurement. This writes (or updates) your entry in `trgb_results.csv`. Submit that file. All required information is automatically computed and saved by the notebook — no manual data entry is needed.

Each row in the CSV records everything needed to reproduce your H₀ measurement from the shared photometry files:

| Column | Description |
|---|---|
| `galaxy_name` | Target galaxy you measured |
| `phot_source` | Photometry source (`EDD` or `CATS`) |
| `mu`, `sigma_mu` | Distance modulus and total uncertainty |
| `trgb_app` | Target apparent TRGB magnitude (extinction-corrected) |
| `trgb_app_anchor` | Anchor (NGC 4258) apparent TRGB magnitude (extinction-corrected) |
| `tip_contrast` | Tip contrast quality diagnostic |
| `n_stars`, `n_faint` | RGB star counts (total selected; stars within 1 mag below tip) |
| `color_lo_f606w`, `color_hi_f606w` | F606W−F814W color selection window |
| `color_lo_f555w`, `color_hi_f555w` | F555W−F814W color selection window |
| `tau` | GLOESS smoothing bandwidth (mag) |
| `bin_width` | Luminosity function bin size (mag) |
| `edge_weighting` | Sobel edge weighting scheme (`simple`, `poisson`, or `hatt`) |
| `poisson_floor` | Poisson floor used in edge detection |
| `scale_factor` | Spatial clipping ellipse scale factor |
| `mag_bright`, `mag_faint` | Magnitude range limits (`None` = automatic) |
| `n_boot` | Number of bootstrap iterations |

The organizers can verify your distance modulus as:
$$\mu = \texttt{trgb\_app} - \texttt{trgb\_app\_anchor} + 29.397$$
and reproduce your target TRGB measurement by re-running the notebook with the stated parameters. These parameters are saved and outputted by the notebook.


All submitted results will be compiled into the public repository alongside the common dataset and analysis code.

---

## Quick Reference

| Step | Action | Where |
|------|--------|-------|
| Setup | Install packages; download photometry into `photometry/`; launch Jupyter from top-level folder | Terminal |
| 1 | Set `TARGET_GALAXY` | TRGB notebook, Section 2 |
| 2 | Kernel → Restart & Run All | TRGB notebook |
| 3 | Adjust parameters; inspect plots; run Section 9a stability checks | TRGB notebook, Sections 3, 6, 9a |
| 4 | Check distance result against sanity range (μ ≈ 30–34) | TRGB notebook, Section 8 |
| 5 | Run Section 10 export cell | TRGB notebook, Section 10 |
| Repeat 1–5 | For each assigned galaxy | — |
| 6–7 | Kernel → Restart & Run All | H₀ notebook (`2_hubble_constant/`) |

---

## Troubleshooting

**Error on first run: "No photometry file found"**
→ The CSV for your galaxy is not in the `photometry/` folder, or the filename does not contain the galaxy name. Download the correct file from the GitHub repository and confirm it is placed in `photometry/`.

**"No results file found"** in the H₀ notebook
→ You have not yet run the export cell in Section 10 of the TRGB notebook. Do that first, then re-run the H₀ notebook.

**"host not in calibrator list"** warning
→ The galaxy name in `TARGET_GALAXY` does not match any entry in the challenge's calibrator list. Check spelling and capitalization — use exactly the format in the photometry filename (e.g. `'NGC1365'`, not `'ngc1365'` or `'NGC 1365'`).

**The Sobel filter shows multiple peaks of similar height**
→ The TRGB detection is ambiguous. Try increasing `tau` (more smoothing) or narrowing the color selection range. If the problem persists, the number of stars in your selection may be too low — try widening `color_hi` slightly.

**The bootstrap distribution is bimodal (two humps)**
→ The algorithm is jumping between two candidate features. You will need to decide whether to adopt a larger uncertainty to account for this or adjust the smoothing.

**Error: "ModuleNotFoundError: No module named '...'"**
→ A required package is missing. In your terminal (or Anaconda Prompt), run `conda install <package-name>` (Anaconda users) or `pip install <package-name>` (standalone Python), then restart Jupyter and try again.

**The notebook appears frozen — a cell shows `[*]` and nothing happens for several minutes**
→ Click **Kernel → Interrupt** to stop the running cell. If that does not work, click **Kernel → Restart**, then re-run with **Kernel → Restart & Run All**.

**The browser shows "Connection failed" or "No connection to server"**
→ The Jupyter server stopped, most likely because the terminal window was closed. Re-open your terminal, navigate to the folder, and run `jupyter notebook` again. Then refresh the browser.

**An error appears that is not listed here**
→ Note the full error message (the red text below the cell) and contact your session coordinator.

---

*Questions? Contact the CosmoVerse Data Challenge organizers: Siyang Li, Eleonora Di Valentino, Jackson Said*

---

## Acknowledgements

### Data Challenge Organizers
Siyang Li, Eleonora Di Valentino, Jackson Said 

### Challenge preparation and materials
Siyang Li, Yukei Murakami, Kayla Owens (team leads)

### Notebooks
- **TRGB distance measurement notebook** (`1_TRGB/`): Siyang Li (coordinator); algorithm contributors: Chandra Shekhar Saraf, Swayamtrupta Panda, Mahdi Najafi, Rahul Shah, Luis Escamilla, Payam Ghafari, Masoume Reyhani
- **Hubble constant notebook** (`2_hubble_constant/`): Yukei Murakami

### Data curation
Kayla Owens (coordinator); Masroor C. Pookkillath, Ayush Hazarika, Antonio Quintana, Trisha Khan, Jess Worsley, Vasiliki Karanasou, Yoelsy Leyva

---

