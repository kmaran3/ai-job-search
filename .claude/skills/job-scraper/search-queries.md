# Search Queries for Job Scraper

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; any skill you add with `/add-portal` is included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

**Language scope:** All queries in English only (candidate works exclusively in English, US market).

## Search Sites

Primary (US job boards):
- **linkedin.com/jobs** - LinkedIn job listings (filter: United States / San Francisco Bay Area); also covered by `linkedin-search` CLI
- **greenhouse.io** - Greenhouse ATS (many startups)
- **lever.co** - Lever ATS (many startups)
- **indeed.com** - Indeed job listings (filter: San Francisco Bay Area)
- **boards.greenhouse.io** and **jobs.lever.co** - Direct ATS board searches

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies

## Query Categories

Queries are grouped by priority. Combine each query with Bay Area location terms where the site supports it.

### Priority 1: Data Analyst / Data Scientist (Core Direction)

These match the strongest and most desired career direction.

```
site:linkedin.com/jobs "data analyst" "San Francisco" OR "Bay Area"
site:linkedin.com/jobs "data scientist" "San Francisco" OR "Bay Area"
site:linkedin.com/jobs "data analyst" Python BigQuery "San Francisco"
site:indeed.com "data analyst" "San Francisco, CA"
site:indeed.com "data scientist" "San Francisco, CA"
site:boards.greenhouse.io "data analyst" OR "data scientist"
site:jobs.lever.co "data analyst" OR "data scientist"
```

### Priority 2: AI Engineer / ML Engineer (AI Experience)

These match the AI agent and LLM experience from NTT Data and personal projects.

```
site:linkedin.com/jobs "AI engineer" "San Francisco" OR "Bay Area"
site:linkedin.com/jobs "machine learning engineer" "San Francisco" OR "Bay Area"
site:linkedin.com/jobs "LLM" OR "RAG" engineer "San Francisco"
site:indeed.com "AI engineer" "San Francisco, CA"
site:boards.greenhouse.io "AI engineer" OR "ML engineer"
site:jobs.lever.co "AI engineer" OR "machine learning"
```

### Priority 3: Analytics Engineer / BI Engineer (Adjacent Roles)

Adjacent roles that leverage dbt, LookML, BigQuery, and pipeline experience.

```
site:linkedin.com/jobs "analytics engineer" "San Francisco" OR "Bay Area"
site:linkedin.com/jobs "BI engineer" OR "business intelligence" Python "San Francisco"
site:linkedin.com/jobs "analytics engineer" dbt BigQuery "San Francisco"
site:indeed.com "analytics engineer" "San Francisco, CA"
site:boards.greenhouse.io "analytics engineer"
site:jobs.lever.co "analytics engineer"
```

### Priority 4: Sports Analytics (Passion Roles)

Sports team and sports tech roles. Wider geographic search since these are rare. Note: salary expectations lower for this category.

```
site:linkedin.com/jobs "sports analytics" OR "sports data" United States
site:linkedin.com/jobs "data analyst" OR "data scientist" NBA OR NFL OR MLB OR MLS
site:indeed.com "sports analyst" OR "sports analytics"
site:boards.greenhouse.io "sports" data OR analytics
site:jobs.lever.co "sports" data OR analytics
```

### Priority 5: Broader Data / Remote Roles (Wider Net)

Wider net for remote-friendly data roles or roles in other cities.

```
site:linkedin.com/jobs "data analyst" Python remote United States
site:linkedin.com/jobs "data scientist" remote GCP OR BigQuery United States
site:linkedin.com/jobs "AI engineer" remote United States
site:indeed.com "data analyst" remote
```

## Location Filter

When evaluating results, verify the job location against these tiers:

**Ideal:**
- San Francisco, CA and surrounding Bay Area (San Jose, Oakland, Palo Alto, Mountain View, Sunnyvale, Redwood City, South San Francisco, etc.)
- Remote (US-based)

**Acceptable:**
- Hybrid in Bay Area (2-3 days/week in office)
- Other major tech cities if remote-first: Seattle, New York, Austin, Los Angeles

**Borderline:**
- Hybrid requiring 4-5 days/week in a non-Bay Area city (would need to relocate there instead)

**Too far / Deal-breaker:**
- Consulting roles with heavy travel
- Roles requiring international relocation
- Charlotte-only roles (wants to leave)

## Language Filter

Working language: English (Native). Only English-language roles in the US market. A posting requiring any non-English language as a job condition is excluded per the Language Gate in `04-job-evaluation.md`.

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape AI" -> Priority 2 queries + custom LLM/agent-specific queries
- "/scrape sports" -> Priority 4 queries + team-specific career page searches
- "/scrape startups" -> Focus on greenhouse.io and lever.co boards
