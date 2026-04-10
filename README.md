# FedRAMP Rev5 Documentation Monitor

This project monitors the FedRAMP Rev5 documentation websites for changes, providing detailed tracking of documentation updates and public notices.

## 🚨 FOR AI AGENTS: READ THIS FIRST

**If you are an AI agent (Claude, Cursor, etc.) working on this project:**

1. **READ THESE FILES IMMEDIATELY BEFORE DOING ANYTHING:**
   - `CLAUDE.md` - Mandatory agent instructions and workflow
   - `spec.md` - Complete operational specification
   - `snapshots/latest.json` - Current baseline pointer

2. **CRITICAL: When user asks "What has changed?"**
   - You MUST do a complete fresh crawl of BOTH websites
   - You MUST create a NEW snapshot with current timestamp
   - You MUST update `snapshots/latest.json`
   - See CLAUDE.md for step-by-step requirements

## Overview

This tool monitors TWO FedRAMP websites:
- **https://www.fedramp.gov/docs/rev5/** (Rev5 Documentation - 40-50+ pages)
- **https://www.fedramp.gov/notices/** (Public Notices - single page)

The specification file (`spec.md`) provides:
- Complete monitoring workflow and requirements
- Snapshot creation process
- Change detection methodology
- Site navigation and structure
- Document categories and types
- URL patterns for different resources
- CSS selectors for web scraping
- Best practices for responsible querying

## File Structure

```
.
├── README.md                    # This file
├── CLAUDE.md                    # AI agent instructions (READ FIRST)
├── .cursorrules                 # Cursor-specific rules
├── spec.md                      # Complete specification and workflow
└── snapshots/                   # Snapshot storage
    ├── latest.json              # Pointer to current baseline
    └── YYYY-MM-DDTHHMMSSZ-*/    # Timestamped snapshots
```

## Key Files

- **CLAUDE.md** - Mandatory instructions for AI agents. Read this first in every new session.
- **spec.md** - Complete operational specification with detailed monitoring workflow.
- **snapshots/latest.json** - Points to the most recent snapshot (baseline for comparisons).
- **snapshots/[timestamp]-update/** - Timestamped snapshot directories containing fetched website content.

## Usage Examples

### Example 1: Understanding Document Organization

The spec organizes documents by audience:

**For Cloud Service Providers:**
- Getting Started: `/playbook/csp/authorization/overview/`
- System Security Plan: `/playbook/csp/authorization/ssp/`
- Continuous Monitoring: `/playbook/csp/continuous-monitoring/`

**For Agencies:**
- Marketplace Info: `/playbook/agency/marketplace/overview/`
- Sponsorship: `/playbook/agency/sponsorship/`
- Authorization: `/playbook/agency/authorization/`

### Example 2: Constructing URLs

Using the URL pattern from the spec:
```
Pattern: /playbook/{audience}/{process}/{topic}/

Examples:
- https://www.fedramp.gov/playbook/csp/authorization/ssp/
- https://www.fedramp.gov/playbook/agency/marketplace/overview/
- https://www.fedramp.gov/playbook/csp/continuous-monitoring/poam/
```

### Example 3: Document Types to Query

The spec identifies these key document types:
- **Playbooks** - Comprehensive guides (HTML/PDF)
- **SSP** - System Security Plan templates
- **SAP** - Security Assessment Plan templates
- **SAR** - Security Assessment Report templates
- **POA&M** - Plans of Action & Milestones

### Example 4: Using CSS Selectors for Scraping

From the spec, use these selectors:
```css
/* Navigation menu */
nav

/* Main content area */
main, article, .content

/* Document links */
a[href*='/playbook/']

/* PDF downloads */
a[href$='.pdf']

/* Section headings */
h1, h2, h3
```

### Example 5: Building a Query Tool

Using the spec to build a simple query:

1. **Identify your target audience:**
   - Cloud Service Providers → `/playbook/csp/`
   - Agencies → `/playbook/agency/`

2. **Select the process:**
   - Authorization
   - Continuous Monitoring
   - Marketplace
   - Sponsorship

3. **Choose the topic:**
   - SSP, SAP, SAR, POA&M
   - Overview, procedures, etc.

4. **Construct the full URL:**
   ```
   https://www.fedramp.gov/playbook/{audience}/{process}/{topic}/
   ```

### Example 6: Extracting Specific Content

Based on extraction targets in the spec:

**Get all document titles:**
- Selector: `h1, h2`
- Extract: text content

**Get all document links:**
- Selector: `a[href$='.pdf'], a[href*='/playbook/']`
- Extract: `href` attribute

**Get navigation structure:**
- Selector: `nav a, .nav-link`
- Extract: `href` and text content

## Best Practices

### Rate Limiting
```yaml
Requests per second: 1
Delay between requests: 1000ms
```

### User Agent
Always identify your scraper:
```
User-Agent: FedRAMP-Documentation-Bot/1.0 (Research Purpose)
```

### Caching
Enable caching to minimize redundant requests:
- Cache duration: 15 minutes
- Reduces server load

### Respect robots.txt
Check before scraping: https://www.fedramp.gov/robots.txt

## Navigation Structure

The FedRAMP Rev5 documentation is organized into these main sections:

1. **General Info** - Changelog, logos, foundational documents
2. **Authority & Responsibility** - Legal framework and guidance
3. **FedRAMP 20x** - Next-generation process materials
4. **FedRAMP Rev5** - Current revision playbooks and guides

## Document Categories

### Cloud Service Providers
- Getting Started guides
- Authorization Package documentation
- Continuous Monitoring resources

### Agencies
- Marketplace information
- Liaison program details
- Sponsorship procedures
- Authorization processes

### Balance Improvement Releases
Optional and mandatory enhancements bringing FedRAMP 20x features to Rev5

## Output Formats

The spec supports these output formats:
- JSON
- CSV
- Markdown

Recommended fields to extract:
- title
- url
- category
- document_type
- audience
- last_modified
- content_summary

## Query Strategies

### Strategy 1: Navigate by Audience
Start with CSP or Agency section, then drill down by process and topic.

### Strategy 2: Document Type Search
Search for specific document types (SSP, SAP, SAR, POA&M).

### Strategy 3: Direct URL Construction
Build URLs using the pattern and known values.

## Notes

- The site uses tab-based navigation with local storage for user preferences
- Balance Improvement Releases bridge FedRAMP 20x and Rev5 processes
- Content is primarily organized by audience (CSP vs Agency)
- Playbooks are available in both HTML and PDF formats
- Site structure may change; verify selectors periodically

## How It Works

### For Users

When you ask "What has changed?" or "Check for changes":
1. The agent checks the current date/time
2. Loads the baseline from `snapshots/latest.json`
3. Crawls BOTH websites fresh from fedramp.gov
4. Creates a new timestamped snapshot
5. Compares new vs baseline snapshot
6. Reports changes detected
7. Updates `latest.json` to point to new snapshot

### For AI Agents

**Read CLAUDE.md first.** It contains the mandatory workflow you must follow.

Critical requirements:
- Always check current date with `date -u` before doing anything
- Always create a NEW snapshot when checking for changes
- Always fetch fresh content from websites (don't just compare old snapshots)
- Always update `snapshots/latest.json` after creating snapshot
- Always report temporal context ("Changes since [date] ([X days] ago)")

## Monitored Websites

1. **Rev5 Documentation** (40-50+ pages)
   - Base: https://www.fedramp.gov/docs/rev5/
   - All pages under `/docs/rev5/` path

2. **Public Notices** (single page)
   - URL: https://www.fedramp.gov/notices/

## Version

- Created: 2026-03-23
- Updated: 2026-04-10
- Target sites: 
  - https://www.fedramp.gov/docs/rev5/
  - https://www.fedramp.gov/notices/
- Format version: Rev5

## License

This specification is provided for research and documentation purposes.
