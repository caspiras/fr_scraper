# FedRAMP Rev5 Documentation Query Specification

This project contains a YAML specification file for querying and extracting content from the FedRAMP Rev5 documentation website at https://www.fedramp.gov/docs/rev5/

## Overview

The specification file (`fedramp-rev5-spec.yaml`) provides a structured mapping of:
- Site navigation and structure
- Document categories and types
- URL patterns for different resources
- CSS selectors for web scraping
- Best practices for responsible querying

## File Structure

```
.
├── README.md                    # This file
└── fedramp-rev5-spec.yaml      # Main specification file
```

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

## Version

Specification created: 2026-03-23
Target site: https://www.fedramp.gov/docs/rev5/
Format version: Rev5

## License

This specification is provided for research and documentation purposes.
