# Telegram Whitelist & Internet Shutdown in Iran (Jan 2026)

This repo contains the data and code behind a Telegram analysis used in Factnameh’s research on **who kept internet access during the January 2026 shutdown in Iran** and which channels were effectively “whitelisted” by the authorities.

The analysis is based on more than 50,000 posts from 48 highly followed political/news channels inside Iran between **2026-01-03 and 2026-01-21**, plus a comparison period during the **Iran–Israel 12-day war (June 2025)**.


## Channel selection

The 48 Telegram channels analyzed in this project were selected from a larger pool of more than 250 Iran-related news and political media channels identified via tgstat.com, based on subscriber count, estimated reach, and citation frequency.
Channels based outside Iran and those with fewer than 20,000 followers were excluded. After this filtering, 48 channels remained and were used for the analysis.



---

## Project structure

- `data/`
  - `fctgposts.zip`  
    Zipped archive containing Telegram posts from 48 selected channels around the January 2026 internet shutdown.

  - `fctgposts_12days_war.csv`  
    Telegram posts from the same set of channels during the June 2025 Iran–Israel war (12-day comparison window).

- `notebooks/`
  - `fc_tg_analysis.ipynb`  
    Main analysis notebook: data cleaning, descriptive stats, channel activity profiles, whitelist detection, and text similarity analysis.

- `README.md`  
  You are here.


---

## Data description

### `fctgposts.csv` (Jan 2026 shutdown window)

Posts from 48 “inside Iran” political/news channels used in the shutdown analysis.

Columns:

- `channel_username` – Telegram channel username (with `@` prefix)
- `post_link` – Direct link to the Telegram post
- `date_tehran` – Post date in Tehran local time (`YYYY-MM-DD`)
- `hour_tehran` – Post hour in Tehran local time (`HH:MM`)
- `text` – Post text (Persian, emojis, etc.); can be empty for pure media posts
- `views` – Approximate view count at the time of export
- `date_hr` – Combined datetime in Tehran time, used as the main time index

This file is used to:

- Count posts per channel and per day before/during/after the shutdown.
- Identify which channels stayed active in the first **3 days** and **7 days**.
- Build “whitelist” sets of channels that apparently preserved external internet access.
- Feed the content similarity / copy-paste analysis.

### `fctgposts_12days_war.csv` (June 2025 comparison)

Posts from the same channels during the 12-day Iran–Israel war period in June 2025.

Columns:

- `channel` – Channel name (no `@` prefix)
- `date` – Message timestamp (ISO datetime, includes timezone)
- `id` – Telegram message id
- `views` – View count
- `link` – Direct link to the Telegram post

This file is used to compare the **severity of the Jan 2026 shutdown** with a previous episode of heavy disruption during the war, by looking at total posts per day and per channel.

---

## Notebook overview

All analysis lives in `notebooks/fc_tg_analysis.ipynb`. The notebook is structured in clear sections:

0. **Setup & data loading**  
   - Install/import Python libraries (`pandas`, `numpy`, `matplotlib`, `seaborn`, `emoji`, etc.).  
   - Read the CSV files from the `data/` folder.

1. **Impact of the shutdown on total channel activity**  
   - Aggregate posts per day across all channels.  
   - Show how daily post counts drop around **8–9 January 2026** and slowly recover.

3. **Internet shutdown phases**  
   - Define periods like “first 3 days”, “first 7 days”, and “relative return”.  
   - Track how many channels are active in each phase and how their posting volume changes.

4. **Whitelist analysis (who stayed online?)**  
   - Identify channels that kept posting in the **first 3 days** of the shutdown.  
   - Extend to channels that were consistently active in the **first 7 days**.  
   - Produce lists and simple charts that highlight which official and unofficial channels had uninterrupted connectivity.

5. **Content analysis & copy-paste detection**  
   - Clean text and compute word-level similarity between posts.  
   - Flag pairs/groups of posts where word overlap exceeds a chosen threshold (e.g. 50%).  
   - Extract a “copy-paste list” of posts with high similarity to measure how coordinated messaging was across the whitelist network.  
   - Prepare simple word lists that can be used for word clouds or further text analysis.

6. **Comparison with the June 2025 Iran–Israel 12-day war**  
   - Load `fctgposts_12days_war.csv`.  
   - Compute daily post counts and per-channel activity during and around the 12-day war.  
   - Compare these patterns with January 2026 to show that the 2026 shutdown was **much more severe** in terms of media activity (e.g., 90% drop vs. ~40% drop).

The notebook is meant to be readable and reproducible, not a polished library.

---

## How to run the analysis

1. Clone this repo:

   ```bash
   git clone https://github.com/Factnameh/telegram-whitelist-shutdown.git
   cd telegram-whitelist-shutdown
