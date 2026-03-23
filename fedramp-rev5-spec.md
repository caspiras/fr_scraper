# FedRAMP Rev5 Documentation Query Specification

**Target:** https://www.fedramp.gov/docs/rev5/
**Created:** 2026-03-23
**Version:** Rev5

## Metadata

| Property | Value |
|----------|-------|
| Name | FedRAMP Rev5 Documentation |
| Base URL | https://www.fedramp.gov |
| Docs Path | /docs/rev5/ |
| Full URL | https://www.fedramp.gov/docs/rev5/ |
| Version | Rev5 |
| Description | Query specification for FedRAMP Revision 5 documentation and resources |

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

- The site uses tab-based navigation with local storage for preferences
- Balance Improvement Releases bridge FedRAMP 20x and Rev5
- Content is organized by audience (CSP vs Agency)
- Playbooks are the primary document format
- Check for PDF downloads in addition to HTML content
- Site structure may change; verify selectors periodically
