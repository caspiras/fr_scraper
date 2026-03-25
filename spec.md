# FedRAMP Rev5 Documentation Monitoring Specification

**Target:** https://www.fedramp.gov/docs/rev5/
**Created:** 2026-03-23
**Version:** Rev5

---

## ⚠️ READ THIS FIRST - CHANGE DETECTION REQUIREMENTS

**WORKFLOW CLARIFICATION:**
- You do NOT monitor in the background continuously
- You DO NOT need a scheduled script or external monitoring
- **TRIGGER: When user asks about changes to the website in ANY variation**
  - "What has changed?"
  - "What has changed on the website?"
  - "Have there been any changes?"
  - "Any updates to the site?"
  - "Has anything changed?"
  - "What's new?"
  - "Any changes to the FedRAMP docs?"
  - **ANY question asking about changes/updates to the website**
- **ACTION: Immediately crawl all pages and compare to last stored snapshots**

**When user asks about changes (in ANY variation) you MUST NOT:**
- ❌ Say you can't detect changes automatically
- ❌ Say you need the user to provide a baseline
- ❌ Suggest setting up external monitoring
- ❌ Check only the changelog page
- ❌ Say "changelog shows no new entries"
- ❌ Rely on any single page to tell you what changed

**When user asks about changes (in ANY variation) you MUST:**
- ✅ Recognize ANY variation asking about changes as YOUR TRIGGER
- ✅ Start checking NOW - immediately upon seeing the question
- ✅ Crawl ALL 40-50+ individual pages under /docs/rev5/
- ✅ Compare each page's current content to stored snapshots from last check
- ✅ The stored snapshots ARE the baseline (in project directory)
- ✅ Report which specific pages changed and what changed in each
- ✅ YES, you WILL tell the user about per-page changes when they ask

**Trigger variations include:**
- Questions about "changes", "updates", "what's new", "modifications"
- Referencing "the website", "the site", "FedRAMP docs", "Rev5 docs"
- Past or present tense: "changed", "has changed", "did anything change"

**This is comprehensive page-by-page comparison, not a changelog lookup.**

---

## AGENT INSTRUCTIONS

**If you are an agent reading this specification:**

This file contains the **guidelines** for how you should operate when monitoring and querying FedRAMP Rev5 documentation.

**Your Role:**
- Read this spec to understand your capabilities and constraints
- Use the patterns, selectors, and strategies defined here as your operational framework

---

## 🚨 CRITICAL: CHANGE DETECTION IS NOT A CHANGELOG LOOKUP

**TRIGGER-BASED WORKFLOW:**
- User asking about changes (ANY variation) = Your trigger to check NOW
- Examples: "What changed?", "Any updates?", "What's new on the website?", "Has anything changed?"
- You fetch all pages at that moment and compare to stored snapshots
- You DO NOT need background monitoring or scheduled scripts
- You DO NOT need user to provide a baseline (snapshots are stored in project)
- You WILL detect and report per-page changes when user asks

**When user asks "What has changed?" you MUST:**
1. Recognize this as your TRIGGER to start checking
2. Crawl ALL 40-50+ pages individually under https://www.fedramp.gov/docs/rev5/
3. Fetch current content from EVERY page (not just one changelog page)
4. Load stored snapshots from last check (or note this is first check)
5. Compare each page's content vs. stored snapshots **from the last time you checked**
6. Report which pages changed since the last check and what changed in each
7. Store new snapshots with timestamp for next comparison

**TEMPORAL TRACKING:**
- **First time user asks:** Establish baseline by crawling all pages, store snapshots, report "First check - baseline established"
- **Subsequent times:** Compare current state vs. snapshots from last time you checked
- **Report format:** "Changes since last check on [date/time]"
- **You ARE capable of detecting per-page changes** - that's the whole purpose

**🚫 ABSOLUTELY FORBIDDEN - RESPONSES TO AVOID:**
- ❌ **DO NOT** fetch the changelog page and report what it says
- ❌ **DO NOT** rely on the changelog to tell you what changed
- ❌ **DO NOT** say "changelog shows no changes" or "no new changelog entries"
- ❌ **DO NOT** make excuses like "minor edits may happen without changelog updates"
- ❌ **DO NOT** say "I can't detect changes automatically"
- ❌ **DO NOT** say "I don't watch the site in the background"
- ❌ **DO NOT** say "I won't know until you ask me to check"
- ❌ **DO NOT** suggest external monitoring or scheduled scripts as an alternative
- ✅ **YOU** are responsible for detecting ALL changes by comparing page content
- ✅ **YOU** must crawl every page and diff the content yourself when user asks
- ✅ **YOU** WILL detect per-page changes when triggered by user asking
- ✅ The changelog is irrelevant - you detect changes by direct comparison
- ✅ User asking "What changed?" IS them asking you to check

**Example of WRONG behavior:**
```
❌ "Checked changelog: no new entries"
❌ "Changelog still shows v0.9.0-beta"
❌ "Minor edits may happen without changelog updates"
```

**Example of CORRECT behavior:**
```
✅ "Crawling all 47 pages under /docs/rev5/..."
✅ "Comparing current content to snapshot from March 20, 2026..."
✅ "Changes detected:
    - /playbook/csp/authorization/ssp/ - paragraph added to section 3
    - /playbook/agency/sponsorship/ - updated contact info
    Total: 2 pages changed, 45 pages unchanged"
```

**STEP-BY-STEP PROCESS YOU MUST FOLLOW:**
1. Start at https://www.fedramp.gov/docs/rev5/
2. Extract all links to pages under /docs/rev5/
3. For EACH link discovered:
   - Fetch the page content with WebFetch
   - Extract any additional links to other /docs/rev5/ pages
   - Continue until all pages discovered
4. Load stored snapshots from last check (or note this is first check)
5. For EACH page in your inventory:
   - Compare current content to stored snapshot
   - Note if content differs
6. Report which specific pages changed and how
7. Store new snapshots with timestamp for next check

**This is a comprehensive site crawl (40-50+ WebFetch calls), not a single page lookup.**

---

**⚡ AUTOMATIC QUERY BEHAVIOR:**

**When a user asks ANY question about FedRAMP content, you MUST automatically:**
1. Use WebFetch to query https://www.fedramp.gov/docs/rev5/
2. Extract the answer directly from the live website
3. Do NOT ask permission - just fetch and answer
4. Use the patterns/selectors from this spec to construct your queries

**Examples of questions that trigger automatic fetching:**
- "Where does it talk about Collaborative Continuous Monitoring?"
- "What are the SSP requirements?"
- "How do agencies sponsor CSPs?"
- "What is the Balance Improvement Program?"

**For ALL such questions:**
- ✅ Immediately use WebFetch to retrieve from https://www.fedramp.gov/docs/rev5/
- ✅ Use navigation structure and URL patterns from this spec to find content
- ✅ Extract verbatim information from the website
- ✅ Provide source attribution (section, URL, timestamp)
- ❌ Do NOT ask "should I fetch this?" - just do it

**🚫 EXCLUSIONS - DO NOT MONITOR OR QUERY:**

**FedRAMP 20x Content is EXCLUDED:**
- ❌ Do NOT monitor or fetch content from FedRAMP 20x sections
- ❌ Do NOT include FedRAMP 20x pages in monitoring scope
- ❌ Do NOT report changes to FedRAMP 20x content
- ❌ Exclude any URLs containing "20x" or related to FedRAMP 20x processes
- ✅ ONLY monitor and query FedRAMP Rev5 content

**Data Source for User Queries:**
- When users ask about FedRAMP documentation content
- **You MUST fetch that information from: https://www.fedramp.gov/docs/rev5/**
- Use WebFetch or equivalent tools to retrieve content from the live website
- This spec file contains guidelines for HOW you work, not the FedRAMP content itself

**What This Spec Provides:**
- ✅ Navigation structure to guide your queries
- ✅ CSS selectors for extracting content
- ✅ URL patterns for constructing requests
- ✅ Rules for presenting results (factual data only, attribution, etc.)

**What This Spec Does NOT Provide:**
- ❌ The actual FedRAMP documentation content
- ❌ Answers to user questions about FedRAMP policies/procedures

---

## ⚠️ CRITICAL: WHEN USER ASKS "WHAT HAS CHANGED?"

### 📍 DEFINITION: "THE WEBSITE" MEANS

When the user asks about "changes" or "the website", they mean **EXCLUSIVELY**:

**THE WEBSITE =**
- https://www.fedramp.gov/docs/rev5/ (base page)
- **AND ALL associated pages under /docs/rev5/**
  - https://www.fedramp.gov/docs/rev5/playbook/csp/authorization/getting-started/
  - https://www.fedramp.gov/docs/rev5/playbook/csp/authorization/ssp/
  - https://www.fedramp.gov/docs/rev5/playbook/agency/marketplace/overview/
  - https://www.fedramp.gov/docs/rev5/balance-improvement/
  - **Every page at any depth under /docs/rev5/**

**THE WEBSITE DOES NOT MEAN:**
- ❌ This git repository
- ❌ The spec.md file
- ❌ Local project files
- ❌ Any other website
- ❌ This frdocs-project

**When user says: "What has changed on the website?"**
- They mean: "What changed on https://www.fedramp.gov/docs/rev5/ and all its pages?"
- They DO NOT mean: "What changed in this git repo?"

### ❌ ABSOLUTELY FORBIDDEN - NEVER DO THIS:

When the user asks "What has changed?" or "Have there been any changes?" or "What has changed on the website?":

**NEVER run these commands:**
- `git log`
- `git status`
- `git diff`
- Any git command whatsoever

**NEVER show:**
- Git commit history (commit hashes, commit messages)
- Information about spec.md file changes
- Information about this project's repository
- Local file modifications
- Working tree status
- Branch information
- .DS_Store or other local files

**NEVER say things like:**
- "Latest commit: 94cd22a..."
- "This repo (frdocs-project)..."
- "Working tree: main is clean..."
- "Earlier commits..."
- Any reference to git activity in this project

### ✅ REQUIRED BEHAVIOR:

**🚨 CRITICAL: THIS IS NOT A CHANGELOG LOOKUP - THIS IS COMPREHENSIVE SITE CRAWLING**

When the user asks "What has changed?" or "Have there been any changes?" you MUST:

1. **Understand "the website" = https://www.fedramp.gov/docs/rev5/ and ALL associated pages**
2. **Fetch current state of ALL monitored pages:**
   - Use WebFetch to retrieve current content from EVERY page under /docs/rev5/
   - Crawl ALL pages (playbooks, guides, all subpages at any depth)
   - **❌ DO NOT just fetch a "changelog" page or "what's new" page**
   - **❌ DO NOT assume there's a single page that lists all changes**
   - **✅ DO crawl the ENTIRE website and check EVERY page's actual content**
   - This means fetching 40-50+ pages individually to check each one
3. **Compare against snapshot from the LAST TIME you checked:**
   - Load the stored snapshot from the last time you crawled the website
   - If this is the first check, there's no previous snapshot - establish baseline and store it
   - Compare page-by-page: current content vs. stored content from last check
   - Detect: new pages, modified pages, deleted pages, content changes on any page
   - **This is a temporal content diff operation (now vs. last check), not reading a changelog**
4. **Report ALL detected changes since the last time you checked:**
   - If first check: "First check - baseline established. No previous snapshot to compare."
   - If subsequent check: "Changes since last check on [date/time from last snapshot]:"
   - List every page that changed (content modifications) since last check
   - List every new page added since last check
   - List every page removed since last check
   - Provide specific details about what changed in each affected page
   - Store new snapshot with current timestamp for next comparison
5. **Never check** this git repository, spec.md, or any local files

**⚠️ WRONG APPROACH (DO NOT DO THIS):**
```
❌ Fetch https://www.fedramp.gov/docs/rev5/changelog/
❌ Look for a "What's New" page
❌ Check only one page for change information
```

**✅ CORRECT APPROACH (DO THIS):**
```
✅ Crawl starting from https://www.fedramp.gov/docs/rev5/
✅ Follow ALL links to discover all pages under /docs/rev5/
✅ Fetch content from EACH discovered page (40-50+ pages)
✅ Compare each page's current content vs. stored snapshot
✅ Report which specific pages changed and what changed in each
```

**Example correct response:**
```
User: "What has changed on the website?"

Agent: Checking the FedRAMP website (https://www.fedramp.gov/docs/rev5/ and all associated pages) for changes since last check on March 20, 2026 at 2:30 PM...

[Fetches current state from https://www.fedramp.gov/docs/rev5/ and all subpages]

Changes detected on the FedRAMP website:
- New page added: https://www.fedramp.gov/docs/rev5/playbook/csp/new-guide/
- Modified: https://www.fedramp.gov/docs/rev5/balance-improvement/ (section "Requirements" updated)
- Modified: https://www.fedramp.gov/docs/rev5/playbook/csp/authorization/getting-started/ (new paragraph added)
- No other changes detected across 47 monitored pages on the FedRAMP website
```

**Example WRONG response (NEVER DO THIS):**
```
This repo (frdocs-project)
Latest commit: 94cd22a...
Earlier commits...
```

---

## Purpose

A lightweight tool that monitors the FedRAMP REV 5 websites for documentation-focused projects, providing detailed tracking of file changes, history and change activity.

**🚨 THIS TOOL MONITORS: https://www.fedramp.gov/docs/rev5/ (the FedRAMP website)**

**🚫 THIS TOOL DOES NOT MONITOR: This git repository, spec.md, local files, or project history**

## Summary

The **FedRAMP Rev5 Documentation Monitor** is a monitoring specification designed to track FedRAMP Revision 5 documentation changes and updates. This tool targets the FedRAMP Rev5 documentation site at https://www.fedramp.gov/docs/rev5/, providing comprehensive monitoring of the `/docs/rev5/` path. The current specification version is Rev5 and serves as the operational framework for tracking file changes, history, and update activity across the entire FedRAMP documentation ecosystem.

## Monitoring Capabilities

### File Change Tracking
- **Document Updates:** Monitor when playbooks, templates, and guides are modified
- **New Content:** Detect when new documents are added to the site
- **Deletions:** Track when documents are removed or deprecated
- **Content Diff:** Compare document versions to identify specific changes

### History Tracking
- **Version History:** Maintain historical snapshots of document states
- **Timestamp Recording:** Track when changes occur with precise timestamps
- **Change Attribution:** Record what changed, when, and where on the site
- **Audit Trail:** Comprehensive log of all monitored changes

### Temporal Change Tracking

**Agent Requirement: Track Website Changes Since Last Check**

**🚨 CRITICAL: "Changes" means changes to the FedRAMP website content at https://www.fedramp.gov/docs/rev5/, NOT changes to this spec file or git repository.**

**🚫 FORBIDDEN ACTIONS WHEN USER ASKS ABOUT CHANGES:**
- DO NOT run `git log`, `git status`, or any git commands
- DO NOT check this project's repository
- DO NOT report on spec.md, README.md, or any local files
- DO NOT show commit history or git information

**✅ REQUIRED ACTIONS WHEN USER ASKS ABOUT CHANGES:**
- Use WebFetch to crawl ALL pages under https://www.fedramp.gov/docs/rev5/
- Compare current state of ALL pages to stored snapshots from last check
- Report ONLY FedRAMP website changes (actual page content changes)
- **❌ DO NOT just check a changelog page - crawl every single page**
- **✅ DO perform comprehensive page-by-page comparison across entire site**

When a user asks "Have there been any changes?" or "What's new?", the agent must:

1. **Fetch Current State of ALL Pages:**
   - Use WebFetch to retrieve current content from EVERY page under https://www.fedramp.gov/docs/rev5/
   - Crawl recursively through all playbooks, guides, and subpages
   - This is NOT just checking a "changelog" - you're checking EVERY page's actual content
2. **Retrieve Last Snapshot:** Load the stored snapshot of ALL pages from the last time the user asked
3. **Compare Page-by-Page:** Compare current content vs. stored content for EVERY monitored page
4. **Report ALL Changes Detected:**
   - Show what changed on EACH page that was modified
   - List new pages that were added
   - List pages that were removed
   - Provide details about content modifications on each affected page
5. **First-Time Check:** If this is the first check, state: "This is the first check of the FedRAMP website. Establishing baseline by crawling all pages." then fetch and store current state
6. **Time Reference:** Always include the timeframe in change reports
   - Example: "Changes to the FedRAMP website since last check on [timestamp]"
   - Example: "No changes detected across all monitored pages since [timestamp]"

**Storage Requirements:**
- Persist last check timestamp across sessions
- **Store snapshot of WEBSITE CONTENT at each check** (fetched from https://www.fedramp.gov/docs/rev5/)
- Store snapshots in the project directory (frdocs-project)
- Track per-user or per-session to avoid mixing different users' check histories

**IMPORTANT CLARIFICATION:**
- You do NOT monitor in the background between user questions
- You DO check for changes when user asks "What has changed?"
- User asking = your trigger to fetch all pages and compare to stored snapshots
- The stored snapshots ARE the baseline you compare against
- You WILL detect and report per-page changes when triggered by user question

**Example Interaction:**
```
User: "Have there been any changes?"

Agent Actions (CORRECT):
- Retrieves last check timestamp: 2026-03-20 14:30:00
- Retrieves stored snapshots of all 47 pages from last check
- Crawls current state starting from /docs/rev5/
- Makes 47+ WebFetch calls to fetch current content from EVERY page
- Compares page-by-page: current vs. stored snapshots
- Detects changes on 3 pages

Agent Reports (CORRECT):
"Crawling all pages under /docs/rev5/ to check for changes...

Changes to the FedRAMP website since last check on March 20, 2026 at 2:30 PM:

Modified Pages (3):
1. /playbook/csp/authorization/getting-started/
   - Added new paragraph in 'Prerequisites' section
   - Modified text in 'Timeline' section

2. /playbook/agency/sponsorship/
   - Updated contact information
   - Changed sponsorship process step 3 description

3. /balance-improvement/
   - Added new requirement to mandatory enhancements
   - Updated effective date

New Pages: None
Removed Pages: None
Total pages monitored: 47
Total changes detected: 3 pages modified"

---

Agent Actions (WRONG - DO NOT DO THIS):
- Fetches only the changelog page
- Reads what the changelog says

Agent Reports (WRONG - DO NOT DO THIS):
"Checked FedRAMP's documentation changelog: it still lists only v0.9.0-beta.
Minor edits may still happen on individual doc pages."
```

**What to Monitor for Changes:**
- **🔄 COMPREHENSIVE PAGE-BY-PAGE COMPARISON:**
  - Compare ALL pages under /docs/rev5/ (not just a changelog)
  - Check EVERY playbook, guide, and documentation page for content changes
  - Monitor actual page content (paragraphs, sections, headings) on each page
  - Detect when ANY page's content is modified
- **New or removed pages/documents at any depth**
- **Updated PDFs or templates**
- **Navigation structure modifications**
- **Metadata changes (titles, dates)**
- **Changes at any level of the site hierarchy**

**🚨 THIS IS NOT A CHANGELOG LOOKUP - READ THIS CAREFULLY:**
- ❌ **WRONG:** Fetch https://www.fedramp.gov/docs/rev5/changelog/ and read what changed
- ❌ **WRONG:** Look for a single "changelog" or "what's new" page
- ❌ **WRONG:** Assume the website has a page that lists all changes
- ✅ **CORRECT:** Crawl ALL 40-50+ pages under /docs/rev5/ individually
- ✅ **CORRECT:** Fetch current content from EVERY single page
- ✅ **CORRECT:** Compare each page's content against stored snapshot from last check
- ✅ **CORRECT:** Detect changes by doing page-by-page content diffing

**This means you will make 40-50+ WebFetch calls to check every page, not just 1 call to a changelog.**

**What NOT to Monitor or Report:**
- ❌ This specification file (spec.md)
- ❌ Local project files (README.md, etc.)
- ❌ Git history or commits in this project
- ❌ Changes to the spec file itself
- ❌ Agent code or configuration
- ❌ Any local repository activity

**NEVER Report:**
When the user asks "What has changed?" or "Have there been any changes?":
- DO NOT show git log output
- DO NOT show commits to this project
- DO NOT show spec.md file changes
- DO NOT show local file modifications

**ALWAYS Report:**
When the user asks "What has changed?" or "Have there been any changes?":
- ✅ ONLY show changes to https://www.fedramp.gov/docs/rev5/ website content
- ✅ ONLY compare website snapshots (current vs. last check)
- ✅ ONLY report additions/modifications/deletions on the FedRAMP website

### Change Activity Metrics
- **Update Frequency:** Track how often specific documents are updated
- **Change Volume:** Measure the extent of changes (additions, deletions, modifications)
- **Hot Spots:** Identify frequently updated sections or document types
- **Trend Analysis:** Patterns in documentation updates over time

### Monitoring Targets

**Scope: ALL pages under https://www.fedramp.gov/docs/rev5/**

The agent must monitor ALL pages within the /docs/rev5/ path, including:

**Main Index:**
- https://www.fedramp.gov/docs/rev5/ (landing page)

**All Subpages and Nested Paths:**
- All playbook pages: `/docs/rev5/playbook/**/*`
  - Example: `/docs/rev5/playbook/csp/authorization/getting-started/`
  - Example: `/docs/rev5/playbook/csp/authorization/ssp/`
  - Example: `/docs/rev5/playbook/agency/marketplace/overview/`
- All documentation sections under /docs/rev5/
- All nested subdirectories and their contents

**Discovery Strategy:**
1. **Start at Base URL:** https://www.fedramp.gov/docs/rev5/
2. **Follow All Links:** Recursively follow all internal links that stay within /docs/rev5/
3. **Build Page Inventory:** Maintain a list of all discovered pages
4. **Monitor Each Page:** Check every discovered page for changes on each monitoring run
5. **Detect New Pages:** Identify when new pages are added to the site
6. **Detect Removed Pages:** Identify when pages are deleted or return 404

**Content Types to Monitor:**
- HTML pages (all playbooks, guides, documentation)
- PDF document downloads
- Navigation structure changes
- Document metadata (titles, descriptions, dates)
- Site architecture modifications

**Monitoring Depth:**
- **Unlimited depth** - follow links recursively through all levels
- Monitor pages at any depth under /docs/rev5/
- Example depths:
  - Level 1: /docs/rev5/
  - Level 2: /docs/rev5/playbook/
  - Level 3: /docs/rev5/playbook/csp/
  - Level 4: /docs/rev5/playbook/csp/authorization/
  - Level 5: /docs/rev5/playbook/csp/authorization/getting-started/
  - And deeper as needed

**Exclusions:**
- External links (outside fedramp.gov domain)
- Links outside /docs/rev5/ path
- Static assets (images, CSS, JS) - unless specifically relevant to content
- **FedRAMP 20x content** - any pages related to FedRAMP 20x processes or materials
- Any URLs containing "20x" or referencing FedRAMP 20x documentation

### Monitoring Workflow

**Step-by-Step Process for Comprehensive Change Detection:**

1. **Initial Crawl (First Check):**
   - Start at https://www.fedramp.gov/docs/rev5/
   - Extract all links pointing to /docs/rev5/* pages
   - Follow each link and extract more links
   - Continue recursively until all pages discovered
   - Store complete inventory of all pages found
   - Save snapshot of each page's content

2. **Subsequent Checks (When User Asks About Changes):**
   - **DO NOT check the changelog page and report what it says**
   - **DO crawl all individual documentation pages:**
     - Retrieve stored page inventory from last check
     - Re-crawl starting from https://www.fedramp.gov/docs/rev5/
     - Discover current page inventory
   - **Compare inventories:**
     - Identify new pages (in current, not in stored)
     - Identify removed pages (in stored, not in current)
     - Identify existing pages to check for content changes
   - **For each existing page:**
     - Fetch current content with WebFetch
     - Compare to stored snapshot from last check
     - Record any differences
   - **Report all changes found:**
     - List specific pages that changed
     - Show what changed in each page
     - Total: X pages changed, Y unchanged
     - NOT just "changelog shows nothing new"

3. **Example of Comprehensive Detection:**
   ```
   User: "Have there been any changes?"

   Agent actions:
   - Crawls /docs/rev5/ and discovers 47 pages total
   - Compares to 45 pages from last check
   - Finds: 2 new pages added
   - Checks all 45 existing pages for content changes
   - Detects:
     - /playbook/csp/authorization/getting-started/ - content updated
     - /playbook/agency/sponsorship/ - section added
     - /balance-improvement/ - new page (1 of 2 new)
     - /continuous-monitoring-guide/ - new page (2 of 2 new)

   Agent reports:
   "Changes to FedRAMP website since [last check time]:
   - 2 new pages added
   - 2 existing pages modified
   - 0 pages removed
   [Details of each change...]"
   ```

**This ensures NO changes are missed, regardless of page depth or location.**

## Tracking Features

**Tracks:**
- New, modified, and deleted documentation files
- Line-by-line changes in specific files
- File reorganizations and structure changes
- Change history and timestamps
- Version updates and releases
- Configuration and build file changes

**Optimized For:**
- Documentation-heavy websites (Markdown, MDX, etc.)
- Identifying new content additions
- Understanding documentation evolution
- Tracking update activity

## Change Presentation

When changes are detected (comparing current state to last check), users should be presented with comprehensive delta comparisons:

**Change Reports Include:**
- **Side-by-side Comparison:** Previous version vs. current version
- **Line-level Diffs:** Highlighted additions (green), deletions (red), and modifications (yellow)
- **Content Summary:** High-level description of what changed
- **Metadata Changes:** Updated titles, dates, authors, or document properties
- **Structural Changes:** File moves, renames, or reorganizations
- **Context:** Surrounding unchanged content for reference

**Presentation Format:**
- Visual diffs with syntax highlighting
- Unified diff format for text-based review
- Summary statistics (lines added/removed, files affected)
- Links to full previous and current versions
- Timestamp and change attribution
- **Time Range:** Explicitly state "Changes since [last check timestamp]" or "No changes since [last check timestamp]"

This ensures users can quickly understand what was modified, added, or removed since their last check.

## Factual Data Only

**Primary Rule: Factual Reporting**
- The tool MUST ALWAYS provide factual information derived directly from the website
- Observable facts are the foundation: changes, diffs, file updates, timestamps, versions
- If information is not available or unclear, explicitly state "Information not available"

**What to Report:**
- Change records, timestamps, and version information
- Exact file additions, deletions, and modifications
- Line-by-line diffs showing actual content changes
- Version identifiers and update information
- File sizes, paths, and reorganizations

**What NOT to Report:**
- Assumptions about intent or purpose of changes
- Interpretations of why changes were made
- Predictions about future changes
- Subjective characterizations beyond observable data

**Principle:** Report what IS, not what might be or what we think it means.

## Query and Search Capabilities

**For Agents: When a user asks about FedRAMP documentation, you must use WebFetch (or equivalent) to query https://www.fedramp.gov/docs/rev5/ and extract the information. This spec tells you HOW to query and present results, not WHAT the FedRAMP content says.**

The agent provides query and search functionality to retrieve factual information from the website:

**Search Functions:**
- **Document Search:** Find documents by title, type, or content keywords
- **Content Search:** Locate specific text, sections, or terminology within documents
- **Metadata Search:** Query by document type, audience, category, or date
- **URL Search:** Find documents by path patterns or specific URLs
- **Version Search:** Locate specific versions or historical snapshots

**Query Types:**
- Natural language queries about document content
- Structured queries using filters (type, audience, date range)
- Boolean searches (AND, OR, NOT operations)
- Exact phrase matching
- Pattern-based searches using wildcards

**Search Results:**
- Direct excerpts from matched documents (verbatim text)
- Document metadata (title, URL, type, last modified date)
- Location information (section, heading, line number)
- Context surrounding matched content
- Links to full document sources
- **Section identification:** Specify which section/category contains the information
- **Embedded links:** Include any hyperlinks found within the content
  - Extract link text and URL
  - Preserve link context (what the link is about)
  - Format: "Link text" -> URL

**Result Presentation:**
- All results must be factual extracts from the website
- Quote marks around verbatim text excerpts
- Clear attribution to source document and location
- **Section context:** Include the section name where information was found
  - Example: "Found in Balance Improvement Program (BIP)"
  - Example: "Found in Cloud Service Providers > Authorization Package"
  - Example: "Found in Agencies > Sponsorship"
- **Embedded links:** If the retrieved content contains hyperlinks, include them
  - Show the link text as it appears in the original
  - Provide the full URL the link points to
  - Make it clear these are links from the source document
- Timestamps showing when data was retrieved
- "No results found" when queries return nothing

**Location Attribution Format:**
```
Section: [Section Name]
Document: [Document Title]
URL: [Full URL]
Retrieved: [Timestamp]
```

**Embedded Links Format:**
When content includes hyperlinks, present them as:
```
Content excerpt: "For more information, see the Authorization Process guide"
Embedded link: "Authorization Process guide" -> https://www.fedramp.gov/playbook/csp/authorization/
```

Or inline:
```
"For more information, see the [Authorization Process guide](https://www.fedramp.gov/playbook/csp/authorization/)"
```

**Query Constraints (Agent Requirements):**
- **Data Source:** Always fetch FedRAMP content from https://www.fedramp.gov/docs/rev5/ using WebFetch
- **This Spec's Role:** Use the navigation structure, URL patterns, and selectors from this spec to construct your queries
- Only return information that exists on the website
- Never synthesize or paraphrase content
- State clearly when information is not available
- Provide source URLs for all returned information (must be fedramp.gov URLs)
- Apply the "Factual Data Only" principles defined in this spec

This ensures users receive accurate, verifiable information directly from the FedRAMP Rev5 documentation website.

## Navigation Structure

### General Info
- Documentation Changelog
- FedRAMP Logos

### Authority & Responsibility
- Foundational Authority
- FedRAMP Authorization Act
- M-24-15 Memorandum
- Related Guidance

### FedRAMP 20x
**⚠️ EXCLUDED FROM MONITORING - DO NOT QUERY**
- Foundation Materials (excluded)
- Processes (excluded)
- Archived Phase 1 Content (excluded)

### FedRAMP Rev5
**✅ PRIMARY MONITORING SCOPE**
- Cloud Service Providers Playbooks
- Agencies Playbooks
- Balance Improvement Releases

## Document Categories

### Cloud Service Providers

**Path:** `/playbook/csp/`

#### Getting Started
- Overview
- Authorization Process

#### Authorization Package
- System Security Plan
- Security Assessment Plan
- Security Assessment Report

#### Continuous Monitoring
- Ongoing Authorization
- Plans of Action & Milestones
- Configuration Management

### Agencies

**Path:** `/playbook/agency/`

#### Marketplace
- Marketplace Information

#### Liaison Program
- Agency Liaison Details

#### Sponsorship
- Sponsorship Procedures

#### Authorization
- Authorization Procedures

### Balance Improvement Releases

Optional and mandatory enhancements bringing modern requirements from FedRAMP 20x to Rev5

**Types:**
- Optional Enhancements
- Mandatory Requirements

## Document Types

| Document | Abbreviation | Description | Formats |
|----------|-------------|-------------|---------|
| Playbooks | - | Comprehensive guides for authorization and continuous monitoring | HTML, PDF |
| Security Assessment Plan | SAP | Templates and guidance for security assessments | - |
| Security Assessment Report | SAR | Templates for documenting assessment results | - |
| System Security Plan | SSP | Templates for system security documentation | - |
| Plans of Action & Milestones | POA&M | Templates for tracking remediation efforts | - |
| Configuration Guides | - | Technical configuration documentation | - |

## URL Patterns

**Pattern:** `/playbook/{audience}/{process}/{topic}/`

**Examples:**
- `/playbook/csp/authorization/ssp/`
- `/playbook/csp/continuous-monitoring/poam/`
- `/playbook/agency/marketplace/overview/`

## Site Features

### Search
- **Enabled:** Yes
- **Description:** Site-wide search functionality

### Navigation
- **Type:** Tab-based
- **Persistent Preferences:** Yes
- **Storage:** Local

### Filters
- **Available:** No
- **Description:** No explicit filter functionality observed

## Query Strategies

### Strategy 1: Navigate by Audience
- **Description:** Start with CSP or Agency section, then drill down by process
- **Path:** `/{audience}/{process}/{topic}`
- **Method:** Navigation

### Strategy 2: Document Type Search
- **Description:** Use search to find specific document types (SSP, SAP, SAR, POA&M)
- **Method:** Search

### Strategy 3: Direct URL Construction
- **Description:** Build URLs using the pattern `/playbook/{audience}/{process}/{topic}/`
- **Method:** URL construction

## Web Scraping Selectors

### CSS Selectors

| Target | Selector | Description |
|--------|----------|-------------|
| Navigation Menu | `nav` | Main navigation container |
| Content Area | `main, article, .content` | Primary content area |
| Document Links | `a[href*='/playbook/']` | Links to playbook documents |
| Headings | `h1, h2, h3` | Section headings |

## Data Extraction Targets

| Target | Selector | Attribute |
|--------|----------|-----------|
| Document titles | `h1, h2` | text |
| Document links | `a[href$='.pdf'], a[href*='/playbook/']` | href |
| Navigation items | `nav a, .nav-link` | href, text |
| Content sections | `section, article` | text |

## Output Configuration

### Supported Formats
- JSON
- CSV
- Markdown

### Recommended Fields
- title
- url
- category
- document_type
- audience
- last_modified
- content_summary

## Best Practices

### Rate Limiting
- **Requests per second:** 1
- **Delay between requests:** 1000ms
- **Reason:** Be respectful of government servers

### User Agent
- **Required:** Yes
- **Example:** `FedRAMP-Documentation-Bot/1.0 (Research Purpose)`

### Caching
- **Enabled:** Yes
- **Duration:** 15 minutes
- **Purpose:** Cache responses to minimize repeat requests

### Robots.txt
- **Check before scraping:** Yes
- **URL:** https://www.fedramp.gov/robots.txt

## Important Notes

### Site Characteristics
- The site uses tab-based navigation with local storage for preferences
- Balance Improvement Releases bridge FedRAMP 20x and Rev5
- Content is organized by audience (CSP vs Agency)
- Playbooks are the primary document format
- Check for PDF downloads in addition to HTML content
- Site structure may change; verify selectors periodically
