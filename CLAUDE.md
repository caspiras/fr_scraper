# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Architecture

This is an **AI-agent-driven monitoring system** with NO traditional code files (no Python, JavaScript, etc.). You ARE the implementation, using Claude Code's built-in tools to execute the monitoring workflow defined in `spec.md`.

**How it works:**
1. User asks "What has changed?"
2. You crawl both FedRAMP websites using **WebFetch**
3. You save snapshots using **Write** and **Bash**
4. You compare snapshots using **Read**
5. You report changes to the user

## Mandatory First Steps for Every Session

**Before doing ANYTHING else, you MUST:**

1. **Check the current date/time:**
   ```bash
   date -u +"%Y-%m-%dT%H%M%SZ"
   ```
   - Understand what TODAY is (e.g., if it's April 21st 2026, then today is April 21st)
   - DO NOT assume the date or act like it's a different day

2. **Read the specification file:**
   ```
   Read: spec.md
   ```
   - This contains ALL operational guidelines
   - Read it completely before answering user questions

3. **Read the latest snapshot baseline:**
   ```
   Read: snapshots/latest.json
   ```
   - This tells you the baseline directory to compare against
   - This tells you when the last check was performed

## What This Project Does

This project monitors TWO FedRAMP websites for changes:
1. **https://www.fedramp.gov/docs/rev5/** (Rev5 Documentation - 36-50+ pages)
2. **https://www.fedramp.gov/notices/** (Public Notices - single page)

## ⚠️ Common Pitfall: Missing New Pages

**THE PROBLEM:**
- You only fetch links from the root page
- New pages added deep in the site are never discovered
- User reports "tool doesn't find new pages"

**THE FIX:**
- Use **RECURSIVE crawling** - fetch each page and extract MORE links
- Continue until NO new `/docs/rev5/` URLs are found
- Compare URL lists (new vs baseline) to detect additions/removals
- See detailed algorithm in "How to Discover ALL Pages" section below

## When User Asks About Changes

When the user asks ANY variation of "what has changed?" or "check for changes":

**🚨 YOU MUST DO A COMPLETE SITE SCRAPE - NOT JUST COMPARE FILES**

### Step-by-Step Process:

1. **Get current date/time** (see step 1 above)

2. **Load the baseline** from `snapshots/latest.json`:
   - Get the `baseline_directory` field
   - This is the previous snapshot you'll compare against
   - Example: `2026-04-09T151318Z-update`

3. **Create a NEW snapshot** with CURRENT timestamp:
   - Directory: `snapshots/YYYY-MM-DDTHHMMSSZ-update/`
   - Use the timestamp from step 1

4. **Crawl BOTH websites COMPLETELY**:
   - **Rev5 Docs**: Start at https://www.fedramp.gov/docs/rev5/
     - **CRITICAL: Use RECURSIVE crawling to discover ALL pages**
     - Fetch root page, extract all links under `/docs/rev5/`
     - For EACH discovered page, fetch it AND extract more links
     - Continue until NO new `/docs/rev5/` URLs are found
     - Fetch EVERY discovered page (typically 36-50+ pages)
     - Save each page as `.html` file in snapshot directory
     - Track ALL discovered URLs in a list
   - **Notices**: Fetch https://www.fedramp.gov/notices/
     - Extract all notice entries
     - Include in `_meta.json`

5. **Compare** new snapshot vs baseline snapshot:
   - **Page count changes** - Compare current page_count vs baseline
   - **New pages added** - Compare URL lists:
     - URLs in NEW snapshot but NOT in baseline `urls` array = NEW PAGES
     - Fetch and save these new pages to new snapshot
     - Report each new URL to user
   - **Pages removed** - URLs in baseline but NOT in new snapshot
   - **Content changes** - For matching URLs, compare HTML file sizes/content
   - **New notices added** - Compare notices_snapshot arrays

6. **Update** `snapshots/latest.json`:
   - Set `baseline_directory` to your NEW snapshot
   - Set `checked_at` to current timestamp
   - Set page/notice counts
   - Set notes describing changes

7. **Report** findings to user:
   - "Changes since [baseline date] ([X days] ago)"
   - List specific pages that changed
   - List new notices
   - Be explicit about the temporal gap

## 🔍 How to Discover ALL Pages (Including New Ones)

**CRITICAL: Many pages are NOT linked from the main index. You MUST crawl recursively.**

### Recursive Crawling Algorithm

```
1. Start with seed URL: https://www.fedramp.gov/docs/rev5/
2. Create empty sets: discovered_urls = {}, fetched_urls = {}
3. Add seed URL to discovered_urls

4. WHILE discovered_urls has URLs not in fetched_urls:
   a. Pick next unfetched URL from discovered_urls
   b. WebFetch the URL
   c. Save HTML content to snapshot directory
   d. Extract ALL links from the page
   e. For each link:
      - If link starts with /docs/rev5/ (but NOT /docs/20x/)
      - Convert to full URL
      - Add to discovered_urls
   f. Mark URL as fetched
   
5. When loop ends, ALL pages under /docs/rev5/ have been found and saved
```

### Detecting New Pages

**When comparing to baseline:**

1. **Load baseline URLs:**
   ```
   Read: snapshots/{baseline_directory}/_meta.json
   Get: baseline_urls = _meta.json["urls"]
   ```

2. **Load new URLs:**
   ```
   new_urls = [all URLs discovered in current crawl]
   ```

3. **Find NEW pages:**
   ```
   new_pages = new_urls - baseline_urls  (set difference)
   ```

4. **Find REMOVED pages:**
   ```
   removed_pages = baseline_urls - new_urls
   ```

5. **Report to user:**
   ```
   "NEW PAGES FOUND (X):
   - https://www.fedramp.gov/docs/rev5/new/page/here/
   - https://www.fedramp.gov/docs/rev5/another/new/
   
   REMOVED PAGES (Y):
   - https://www.fedramp.gov/docs/rev5/old/page/"
   ```

### Example: How New Pages Are Missed

**❌ WRONG (will miss new pages):**
```
1. Fetch https://www.fedramp.gov/docs/rev5/
2. Extract links from that ONE page only
3. Fetch those links
4. Done
Problem: New pages not linked from root are NEVER discovered!
```

**✅ CORRECT (finds all pages):**
```
1. Fetch https://www.fedramp.gov/docs/rev5/
2. Extract all /docs/rev5/ links → add to discovered list
3. Fetch FIRST discovered page
4. Extract all /docs/rev5/ links from it → add to discovered list
5. Fetch SECOND discovered page
6. Extract all /docs/rev5/ links from it → add to discovered list
7. Continue until ALL discovered pages have been fetched
8. Result: ALL pages found, including new ones deep in the site!
```

## What NOT to Do

**❌ NEVER do these when asked about changes:**
- Run `git log` or `git status`
- Report on spec.md file changes
- Report on this repository's files
- Just compare files without crawling the website
- Use old snapshots without creating a new one
- Skip updating `latest.json`
- Act like today is a different date

**❌ NEVER just compare previously fetched files:**
- You MUST fetch fresh content from BOTH websites
- You MUST create a new timestamped snapshot
- You MUST do a complete crawl every time

## Critical Rules

### Rule 1: Check Current Date FIRST
Always run `date -u` before doing anything. Know what day TODAY is.

### Rule 2: Create NEW Snapshot Every Time
Never compare old snapshots without creating a new one with current data.

### Rule 3: Crawl BOTH Websites Completely Using Recursive Discovery
- Start at https://www.fedramp.gov/docs/rev5/
- Extract links, fetch each page, extract MORE links from each
- Continue until NO new /docs/rev5/ URLs are found
- Typical result: 36-50+ pages discovered
- Also fetch https://www.fedramp.gov/notices/

### Rule 4: Compare URL Lists to Detect New/Removed Pages
- Load baseline URLs from `{baseline}/_meta.json["urls"]`
- Compare with newly discovered URL list
- Report any NEW pages (in new but not baseline)
- Report any REMOVED pages (in baseline but not new)
- Save ALL discovered pages to snapshot, including new ones

### Rule 5: Update latest.json Every Time
After creating a new snapshot, ALWAYS update `snapshots/latest.json` to point to it.

### Rule 6: Report Temporal Context
Always say "Changes since [date] ([X days/hours] ago)" to make the time gap clear.

## Example Correct Workflow

```
User: "What has changed?"

Agent:
1. Runs: date -u → "2026-04-10T16:15:18Z" (understands today is April 10th)
2. Reads: snapshots/latest.json → baseline is "2026-04-09T151318Z-update" (April 9th)
3. Calculates: Today (April 10th) - Baseline (April 9th) = 1 day ago
4. Creates: snapshots/2026-04-10T161518Z-update/
5. Crawls: All pages from BOTH websites (fresh fetch from fedramp.gov)
6. Compares: New snapshot vs April 9th baseline
7. Updates: snapshots/latest.json → points to April 10th snapshot
8. Reports: "Changes since April 9th at 3:13 PM (1 day ago): [changes found]"
```

## Example WRONG Workflow (Don't Do This)

```
User: "What has changed?"

Agent:
❌ Doesn't check current date
❌ Assumes today is April 9th when it's actually April 10th
❌ Compares files already in snapshots/ directory without fetching new content
❌ Doesn't create a new snapshot
❌ Doesn't update latest.json
❌ Reports changes without temporal context
```

## File Structure

```
.
├── CLAUDE.md              # This file (agent instructions)
├── README.md              # Human-readable project overview
├── spec.md                # Complete operational specification
├── .cursorrules           # Cursor-specific agent rules
└── snapshots/
    ├── latest.json        # Pointer to current baseline snapshot
    └── YYYY-MM-DDTHHMMSSZ-update/
        ├── _meta.json     # Metadata for this snapshot
        ├── root.html      # Main Rev5 docs page
        └── root__*.html   # 40-50+ individual page snapshots
```

## Tools You'll Use

- **Bash** - Run `date -u` to get current timestamp
- **Read** - Read spec.md, latest.json, baseline snapshots, HTML files for comparison
- **Write** - Create snapshot directories, write HTML files, write _meta.json, update latest.json
- **WebFetch** - Fetch fresh content from both websites (40-50+ calls per check)

## URL to Filename Conversion

When saving fetched pages as HTML files:
```
https://www.fedramp.gov/docs/rev5/
  → root.html

https://www.fedramp.gov/docs/rev5/balance/
  → root__balance.html

https://www.fedramp.gov/docs/rev5/playbook/csp/authorization/ssp/
  → root__playbook__csp__authorization__ssp.html
```

Rules:
- Remove `https://www.fedramp.gov/docs/rev5/` prefix
- Replace `/` with `__`
- Use `root` for main index

## Snapshot Completeness

A valid snapshot MUST contain:
- `_meta.json` file with all required fields (checked_at, page_count, notices_count, urls, notices_snapshot, etc.)
- 40-50+ `.html` files (one per Rev5 doc page)
- All URLs listed in `_meta.json` urls array
- Complete notices_snapshot array in `_meta.json`

**A snapshot with ONLY `_meta.json` is INCOMPLETE and INVALID.**

## Temporal Context

Always include temporal context in your reports:
- ✅ "Changes since April 14th at 2:10 PM (7 days ago)"
- ✅ "Last checked 3 days ago on April 18th"
- ❌ NOT "Changes since yesterday" if last check was 2+ days ago

## Summary Checklist

**When user asks about changes:**
1. ✅ Check current date with `date -u`
2. ✅ Read spec.md and snapshots/latest.json
3. ✅ Create NEW snapshot with CURRENT timestamp
4. ✅ Fetch fresh content from BOTH websites using RECURSIVE crawling
5. ✅ Continue crawling until NO new /docs/rev5/ URLs are discovered
6. ✅ Save ALL fetched pages to new snapshot directory
7. ✅ Compare new snapshot vs baseline (including URL lists to find new/removed pages)
8. ✅ Create _meta.json with ALL discovered URLs in the urls array
9. ✅ UPDATE snapshots/latest.json to point to new snapshot
10. ✅ Report with temporal context, listing any NEW or REMOVED pages

**DO NOT:**
- ❌ Skip checking current date
- ❌ Compare old files without fetching fresh website content
- ❌ Only fetch links from the root page (you'll miss new pages!)
- ❌ Stop crawling before ALL /docs/rev5/ pages are discovered
- ❌ Skip comparing URL lists (you'll miss new/removed pages)
- ❌ Forget to save newly discovered pages to the snapshot
- ❌ Forget to update latest.json
- ❌ Report without temporal context
- ❌ Run git commands when asked about changes (you're monitoring fedramp.gov, not this repo)
