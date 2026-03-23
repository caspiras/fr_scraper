# FedRAMP Rev5 Documentation Monitoring Specification

**Target:** https://www.fedramp.gov/docs/rev5/
**Created:** 2026-03-23
**Version:** Rev5

---

## AGENT INSTRUCTIONS

**If you are an agent reading this specification:**

This file contains the **guidelines** for how you should operate when monitoring and querying FedRAMP Rev5 documentation.

**Your Role:**
- Read this spec to understand your capabilities and constraints
- Use the patterns, selectors, and strategies defined here as your operational framework

**Data Source for User Queries:**
- When users ask about FedRAMP documentation content (e.g., "Where does it talk about Collaborative Continuous Monitoring?")
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

When the user asks "What has changed?" or "What has changed on the website?" you MUST:

1. **Understand "the website" = https://www.fedramp.gov/docs/rev5/ and ALL associated pages**
2. **Use WebFetch** to retrieve current content from the FedRAMP website (base + all subpages)
3. **Compare** current FedRAMP website content to stored website snapshot from last check
4. **Report ONLY** changes to the FedRAMP website content (pages added/modified/deleted)
5. **Never check** this git repository, spec.md, or any local files

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
- Use WebFetch to query https://www.fedramp.gov/docs/rev5/
- Compare current website to stored website snapshot
- Report ONLY FedRAMP website changes

When a user asks "Have there been any changes?" or "What's new?", the agent must:

1. **Fetch Current Website State:** Use WebFetch to retrieve current content from https://www.fedramp.gov/docs/rev5/
2. **Retrieve Last Snapshot:** Load the stored snapshot of the website from the last time the user asked
3. **Compare Website States:** Compare the current website content to the stored website snapshot
4. **Report Website Changes Only:** Show only what changed on the FedRAMP website since last check
5. **First-Time Check:** If this is the first check, state: "This is the first check of the FedRAMP website. Establishing baseline." then fetch and store current state
6. **Time Reference:** Always include the timeframe in change reports
   - Example: "Changes to the FedRAMP website since last check on [timestamp]"
   - Example: "No changes to the FedRAMP website since [timestamp]"

**Storage Requirements:**
- Persist last check timestamp across sessions
- **Store snapshot of WEBSITE CONTENT at each check** (fetched from https://www.fedramp.gov/docs/rev5/)
- Track per-user or per-session to avoid mixing different users' check histories

**Example Interaction:**
```
User: "Have there been any changes?"
Agent:
- Retrieves last check timestamp: 2026-03-20 14:30:00
- Retrieves stored website snapshot from 2026-03-20
- Fetches current website state from https://www.fedramp.gov/docs/rev5/
- Compares current website vs. stored website snapshot
- Reports: "Changes to the FedRAMP website since last check on March 20, 2026 at 2:30 PM:
  - Playbook 'Authorization Process' was updated
  - New document 'Security Assessment Guide' was added
  - Balance Improvement Program section was modified"
```

**What to Monitor for Changes:**
- **All pages** under /docs/rev5/ (see "Monitoring Targets" section for complete scope)
- Document content (text, sections, headings) on every page
- New or removed pages/documents at any depth
- Updated PDFs or templates
- Navigation structure modifications
- Metadata changes (titles, dates)
- Changes at any level of the site hierarchy

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
   - Retrieve stored page inventory from last check
   - Re-crawl starting from https://www.fedramp.gov/docs/rev5/
   - Discover current page inventory
   - **Compare inventories:**
     - Identify new pages (in current, not in stored)
     - Identify removed pages (in stored, not in current)
     - Identify existing pages to check for content changes
   - **For each existing page:**
     - Fetch current content
     - Compare to stored snapshot
     - Record any differences
   - Report all changes found across all pages

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
- Foundation Materials
- Processes
- Archived Phase 1 Content

### FedRAMP Rev5
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
