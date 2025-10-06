# Fiction Scraper (2024/2025)

Collection of contemporary fiction from literary magazines, with full text extraction. Each entry contains `title`, `author`, `date` (year), `url`, and complete story `text`.

## Main Dataset: `final.json`

**Total Entries: 612** (as of October 4, 2025)

### Breakdown by Source

| Source | Count | Percentage |
|--------|-------|------------|
| **Joyland** | 357 | 58.3% |
| **Granta** | 111 | 18.1% |
| **Other** | 101 | 16.5% |
| **The Paris Review** | 36 | 5.9% |
| **VQR (Virginia Quarterly Review)** | 7 | 1.1% |

### Breakdown by Year

| Year | Count | Percentage |
|------|-------|------------|
| **2024** | 308 | 50.3% |
| **2025** | 304 | 49.7% |

### Sources Scraped

- **Joyland**: https://joylandpublishing.com/fiction/
- **Granta**: https://granta.com/fiction/
- **The Paris Review**: https://www.theparisreview.org/fiction/
- **VQR**: https://www.vqronline.org/online/fiction
- **Other**: Various literary magazines

All entries are deduplicated by URL and title+author combinations.

## Data Format

Each entry in `tier2_clean.json` follows this structure:

```json
{
  "title": "Story Title",
  "author": "Author Name",
  "date": "2024",
  "url": "https://example.com/story-url",
  "text": "Full story text..."
}
```

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Available Scrapers

### Individual Magazine Scrapers

- **`scrape_joyland.py`** - Scrapes Joyland Magazine fiction
- **`paris_review_fiction_scrape.py`** - Scrapes The Paris Review fiction
- **`scrape_vqr.py`** - Scrapes VQR (Virginia Quarterly Review) fiction
- **`scrape_four_mags.py`** - Scrapes McSweeney's, American Short Fiction, The Southern Review, One Story
- **`scrape_additional_mags.py`** - Additional magazine scrapers

### Utility Scripts

- **`remove_duplicates_tier2.py`** - Removes duplicate entries from tier2_clean.json
- **`analyze_metadata.py`** - Analyzes and reports metadata across all JSON files
- **`deduplicate_entries.py`** - Advanced deduplication across multiple files

### Run a Scraper

```bash
# Example: Scrape VQR and append to tier2_clean.json
python3 scrape_vqr.py

# Example: Analyze all JSON files
python3 analyze_metadata.py

# Example: Remove duplicates from main file
python3 remove_duplicates_tier2.py
```

Each scraper will automatically:
1. Extract fiction from 2024-2025 only
2. Save to its own JSON file
3. Append new entries to `tier2_clean.json` (with duplicate checking)

## Notes

- The scraper looks for publication year in common places (time/meta/ld+json/byline). If a page does not advertise a year, it will be skipped.
- Joyland pagination is followed to discover story links; other sites are discovered from the provided seed pages.
- Running the script multiple times appends only new unique entries to `fiction_2024_2025.json`.
- A lightweight politeness delay is included.

## Granta premium access

Some Granta fiction is behind a paywall. If you have a premium subscription, you can provide your session cookies so the scraper can access premium stories you are entitled to read.

- Set the environment variable `GRANTA_COOKIE` to your full Cookie header value copied from your browser while logged into granta.com.
- Example (macOS/Linux, in the same shell where you run the script):

```bash
export GRANTA_COOKIE="_gid=...; granta_session=...; other_keys=..."
python scrape_joyland.py
```

Tips to obtain the cookie string:
- Log in to https://granta.com in your browser.
- Open DevTools → Network tab → click any request to https://granta.com → Headers → Request Headers → copy the entire `Cookie` header value.

Security note: this value grants access to your account. Do not commit it to source control. Prefer setting it per-shell as shown above.
