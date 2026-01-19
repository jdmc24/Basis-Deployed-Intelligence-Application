# Basis Target Intelligence Map

An interactive dashboard identifying CPA firms that would be ideal customers for [Basis](https://www.getbasis.ai/), an AI platform for accounting firms.

**[View Live Demo](https://jdmc24.github.io/Basis-Deployed-Intelligence-Application/)**

---

## Overview

This project was created as part of a job application for Basis. It demonstrates:

- **Market research** into the CPA firm landscape
- **Strategic thinking** about ideal customer profiles
- **Data visualization** of target opportunities

## What is Basis?

Basis provides AI agents that automate Client Accounting Services (CAS) work for CPA firms. Their flagship customer, Wiss & Company, has reported **30% time savings** using the platform.

**Key use cases:**
- Transaction entry into QuickBooks/Xero
- Bank reconciliations
- Journal entries with AI-generated explanations
- Data verification

## Dashboard Features

- **45 target firms** mapped across the US
- **Priority ratings** (High/Medium/Low) based on fit criteria
- **Filters** by region, opportunity size, and priority
- **Opportunity sizing** based on estimated CAS staff

## Methodology

### Ideal Target Profile (Based on Wiss)

| Criteria | Rationale |
|----------|-----------|
| 200-5,000 employees | Large enough for CAS volume, small enough to adopt startup software |
| Strong CAS practice | Basis's primary use case |
| Tech-forward culture | Willingness to adopt AI tools |
| Regional/National firm | Budget for enterprise software |

### Opportunity Calculation

```
Opportunity = Estimated CAS Staff × $5,000/seat/year

Where:
- Estimated CAS Staff = Total Employees × 18%
- $5,000/seat/year = Enterprise SaaS pricing estimate
```

*Note: Actual Basis pricing is not public. These are estimates for demonstration.*

### Priority Rating Logic

- **High**: Strong CAS focus + tech-forward culture + right size (300-5000 employees)
- **Medium**: Has CAS, good size, but may have longer sales cycles or competing priorities
- **Low**: Very large (Big 4, likely build internally) or unclear CAS focus

## Research Highlights

### What Accountants Do With Time Savings

| Before Basis | After Basis |
|--------------|-------------|
| Data entry | Client advisory |
| Reconciliations | Business development |
| Manual verification | Higher-margin services |

### Open Questions

- How will entry-level accountants learn fundamentals if AI handles basic tasks?
- Will AI commoditize CAS services and drive down prices?
- What's the change management challenge for adoption?

## Tech Stack

- **HTML/CSS/JavaScript** - No build step, runs in any browser
- **[Leaflet.js](https://leafletjs.com/)** - Interactive mapping
- **[CartoDB Dark Matter](https://carto.com/basemaps/)** - Map tiles
- **Basis brand fonts** - JetBrains Mono, Oxygen

## Sources

- [Wiss Partnership Announcement](https://wiss.com/wiss-announces-strategic-partnership-with-accounting-ai-platform-basis/)
- [Basis $34M Series A (Accounting Today)](https://www.accountingtoday.com/news/major-ai-players-back-basis-with-34-million-series-a)
- [OpenAI Case Study on Basis](https://openai.com/index/basis/)
- [Future Finance Podcast - Mitchell Troyanovsky](https://blog.qflow.ai/post/future-finance-podcast-mitchell-troyanovsky/)

*Full research notes available upon request.*

---

*Built by Jake McCorkle as part of a Basis job application, January 2025*
