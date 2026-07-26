# Full Career Portfolio Page Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a `portfolio.html` (+ `portfolio.pdf` download) to `ericthebigsal.github.io` that Eric can drop into a cold LinkedIn "send me a resume" DM — a comprehensive-but-weighted, non-JD-specific career portfolio, distinct from the tighter homepage.

**Architecture:** Single Markdown source (`portfolio.md`) rendered via pandoc to both `portfolio.html` (styled with a new `portfolio-style.css` matching the site's existing look) and `portfolio.pdf` (via headless Chrome print), exactly mirroring the pipeline the `research-company` Claude skill already uses for resumes/cover letters (`career/.claude/skills/research-company/SKILL.md`). One new nav link from the homepage; the new page links back.

**Tech Stack:** Plain HTML/CSS (no framework, matches existing site), pandoc, headless Chrome (`--print-to-pdf`), git.

## Global Constraints

- Single source of truth is `portfolio.md` — never hand-edit `portfolio.html` or `portfolio.pdf` directly; always regenerate from `portfolio.md`.
- Every `##` heading used in the jump-nav gets an explicit pandoc header id via `{#id}` syntax (e.g. `## Amperity {#amperity}`) — do not rely on pandoc's auto-slugify, which is ambiguous with em dashes/parens in these headings.
- No internal scaffolding survives into `portfolio.md`/`.html`/`.pdf`: no `[verify]` tags, no `**Problem.**`/`**What you did.**`/`**Impact.**`/`**Skills:**` bolded labels (those are `ACCOMPLISHMENTS.md`'s internal drafting scaffold — convert to flowing first-person prose that covers the same ground), no "IN PROGRESS", no customer names/revenue/deal-size specifics (per `ACCOMPLISHMENTS.md`'s own confidentiality note at line 10-15 — company names are fine, everything else stays abstracted).
- Voice: first person ("I designed...", not "You did..." or "Eric did...").
- Tier 1 (full depth, all sub-themes): Amperity, Press Ganey.
- Tier 2 (one substantial paragraph, no sub-theme breakdown): Microsoft, Amazon, Accolade.
- Tier 3 (one line each): Appature, Concur Technologies, EDS/General Motors.
- Legacy Microsoft certifications (MCP/MCSD/MCT) are never listed anywhere on this page.
- Reuse `styles.css`'s existing design tokens (`--color-bg`, `--color-surface`, `--color-text`, `--color-muted`, `--color-border`, `--color-accent`, `--font-sans`, `--max-width: 760px`) in `portfolio-style.css` — same brand, don't invent a new palette.
- The 4 AI-portfolio project case studies already fully exist (`index.html`'s Featured Projects section + `agentic-qa-tool.html`) — summarize in 2-3 sentences and link out, never re-author the full case study here.

---

### Task 1: `portfolio-style.css`

**Files:**
- Create: `ericthebigsal.github.io/portfolio-style.css`

**Interfaces:**
- Produces: CSS classes `.back-link`, `.jump-nav`, `.role-section`, `.role-meta`, `.role-compact`, `.role-oneline`, `.project-summary-grid`, `.project-summary-card`, `.pdf-download`, plus `#title-block-header { display: none; }` (hides pandoc's auto title block, matching the `resume-style.css` convention noted in `career/.claude/skills/research-company/SKILL.md`). Later tasks' HTML (via `portfolio.md`) rely on exactly these class names.

- [ ] **Step 1: Read the existing site's design tokens**

Run: `sed -n '1,10p' ~/Documents/career/ericthebigsal.github.io/styles.css`
Confirm the `:root` block matches:
```css
:root {
  --max-width: 760px;
  --color-bg: #fafafa;
  --color-surface: #ffffff;
  --color-text: #1f2933;
  --color-muted: #52606d;
  --color-border: #e4e7eb;
  --color-accent: #1d4ed8;
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}
```
If it differs, use the actual values in Step 2 instead of what's written below.

- [ ] **Step 2: Write `portfolio-style.css`**

```css
:root {
  --max-width: 760px;
  --color-bg: #fafafa;
  --color-surface: #ffffff;
  --color-text: #1f2933;
  --color-muted: #52606d;
  --color-border: #e4e7eb;
  --color-accent: #1d4ed8;
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: var(--font-sans);
  background: var(--color-bg);
  color: var(--color-text);
  line-height: 1.65;
  max-width: var(--max-width);
  padding: 2rem 1.5rem 4rem;
  margin: 0 auto;
}

#title-block-header {
  display: none;
}

.back-link {
  display: inline-block;
  margin-bottom: 1.5rem;
  color: var(--color-accent);
  text-decoration: none;
  font-weight: 600;
}
.back-link:hover {
  text-decoration: underline;
}

h1 {
  font-size: 2rem;
  margin: 0 0 0.25rem;
}

h1 + p {
  color: var(--color-muted);
  font-size: 1.05rem;
  margin: 0 0 1.5rem;
}

.jump-nav {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem 1rem;
  padding: 1rem;
  margin: 0 0 2rem;
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  font-size: 0.9rem;
}
.jump-nav a {
  color: var(--color-accent);
  text-decoration: none;
}
.jump-nav a:hover {
  text-decoration: underline;
}

h2 {
  font-size: 1.4rem;
  margin: 2.5rem 0 0.25rem;
  padding-top: 1.5rem;
  border-top: 1px solid var(--color-border);
}

.role-meta {
  color: var(--color-muted);
  font-style: italic;
  margin: 0 0 1rem;
  font-size: 0.95rem;
}

.role-section h3 {
  font-size: 1.1rem;
  margin: 1.5rem 0 0.5rem;
}

.role-compact p {
  margin: 0 0 1rem;
}

.role-oneline {
  margin: 0.5rem 0;
}
.role-oneline strong {
  color: var(--color-text);
}

.project-summary-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1rem;
  margin: 1rem 0;
}
.project-summary-card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  padding: 1rem 1.25rem;
}
.project-summary-card h3 {
  margin: 0 0 0.5rem;
  font-size: 1.05rem;
}
.project-summary-card p {
  margin: 0 0 0.75rem;
  font-size: 0.95rem;
  color: var(--color-muted);
}

.pdf-download {
  display: inline-block;
  margin: 0.5rem 0 1rem;
  padding: 0.6rem 1.1rem;
  background: var(--color-accent);
  color: #fff;
  text-decoration: none;
  border-radius: 6px;
  font-weight: 600;
}
.pdf-download:hover {
  opacity: 0.9;
}

@media print {
  .jump-nav,
  .back-link,
  .pdf-download {
    display: none;
  }
  .role-section,
  .role-section h3 {
    page-break-inside: avoid;
  }
  a {
    color: inherit;
    text-decoration: underline;
  }
}
```

- [ ] **Step 3: Verify the file is valid CSS**

Run: `python3 -c "import re,sys; s=open('/Users/ericsalerno/Documents/career/ericthebigsal.github.io/portfolio-style.css').read(); assert s.count('{')==s.count('}'), 'unbalanced braces'; print('OK', s.count('{'), 'rules')"`
Expected: `OK <N> rules` — confirms no unclosed rule from a copy/paste error.

- [ ] **Step 4: Commit**

```bash
cd ~/Documents/career/ericthebigsal.github.io
git add portfolio-style.css
git commit -m "Add stylesheet for the full career portfolio page"
```

---

### Task 2: `portfolio.md` — header, About, jump-nav

**Files:**
- Create: `ericthebigsal.github.io/portfolio.md` (this task creates the file; later tasks append to it)

**Interfaces:**
- Consumes: `.back-link`, `.jump-nav`, `h1 + p` styles from Task 1.
- Produces: The file's opening block, plus these exact anchor ids for later tasks and the jump-nav to agree on: `#about`, `#amperity`, `#press-ganey`, `#microsoft`, `#amazon`, `#accolade`, `#earlier-roles`, `#ai-projects`, `#skills-credentials`, `#resume-contact`. Every later task that adds a `##` heading MUST use exactly one of these ids via `{#id}`, or the jump-nav links break.

- [ ] **Step 1: Write the opening of `portfolio.md`**

```markdown
[← Back to ericthebigsal.github.io](index.html){.back-link}

# Eric Salerno — Full Career Portfolio

A comprehensive look at my career, for anyone who wants more than a
resume — recruiters without a specific job description, hiring managers
doing early diligence, or anyone curious what "technical PM who builds"
actually means in practice. Skip to whatever's relevant, or read straight
through — the [homepage](index.html) has the short version, and the AI
project case studies are written up in full there too.

<nav class="jump-nav" aria-label="Jump to section">
<a href="#about">About</a>
<a href="#amperity">Amperity</a>
<a href="#press-ganey">Press Ganey</a>
<a href="#microsoft">Microsoft</a>
<a href="#amazon">Amazon</a>
<a href="#accolade">Accolade</a>
<a href="#earlier-roles">Earlier Roles</a>
<a href="#ai-projects">AI Portfolio Projects</a>
<a href="#skills-credentials">Skills &amp; Credentials</a>
<a href="#resume-contact">Resume &amp; Contact</a>
</nav>

## About {#about}

I'm a technical product manager who builds. I don't just own the backlog —
I design the architecture and ship the system. Most recently, at Amperity,
I designed and built a multi-agent RAG SDK that automates third-party API
integration and cut development time roughly 90%, from about four weeks
down to a couple of days.

Over nearly 30 years I've gone from senior full-stack developer to
technical product/program manager to Director and Senior Director of
Technical Program Management, and the through-line has stayed the same:
turn an idea — or a model, or a spec — into a shipped, integrated product.
I shipped the PlayReady DRM platform APIs at Microsoft, built
fraud-and-abuse systems for Amazon Coins, led a data-exchange and AI-NLP
partnership with Epic at Press Ganey, and spent the last year owning a
connector and activation program at Amperity — shipping 24 connectors and
the agentic tooling around them.

What I care about now: agentic workflows, LLM orchestration, and
autonomous systems that do real work in production — built fast, on small
teams, where the PM is close enough to the code to debate the architecture
and close enough to the customer to know it matters.
```

This reuses the homepage's `About` section verbatim (same three paragraphs, source: `index.html`'s `#about` section) — no need to rewrite what's already good.

- [ ] **Step 2: Verify pandoc accepts the header attribute syntax**

Run: `pandoc /Users/ericsalerno/Documents/career/ericthebigsal.github.io/portfolio.md -o /tmp/portfolio-task2-check.html --standalone && grep -o 'id="about"' /tmp/portfolio-task2-check.html`
Expected: `id="about"` printed — confirms the `{#about}` syntax produced the exact id the jump-nav links to (not a pandoc-slugified variant).

- [ ] **Step 3: Commit**

```bash
cd ~/Documents/career/ericthebigsal.github.io
git add portfolio.md
git commit -m "Start portfolio.md: header, About, jump-nav"
```

---

### Task 3: `portfolio.md` — Tier 1: Amperity (full depth)

**Files:**
- Modify: `ericthebigsal.github.io/portfolio.md` (append after the `#about` section)

**Interfaces:**
- Consumes: `.role-section`, `.role-meta` from Task 1; appends after Task 2's content.
- Produces: `## Amperity {#amperity}` section, consumed by Task 2's jump-nav link.

**Source:** `career/ACCOMPLISHMENTS.md` lines 74–247 (Themes A–F) for texture/detail, and `career/Eric_Salerno_General_Resume.md`'s "Amperity — Technical Product Manager" bullets (already-cleaned resume voice) as the primary fact source. Use the **confirmed numbers only** from `ACCOMPLISHMENTS.md` line 67 ("Captured (confirmed by Eric)") — do not use the "Still open" unconfirmed revenue-influence line, and do not invent any number not present in either source.

- [ ] **Step 1: Write the Amperity section**

Structure: one `<div class="role-section">` per employer, `## Employer {#id}` heading, then a `<p class="role-meta">` for the dated subtitle, then one `### Theme` (no explicit id needed — only `##` headings are in the jump-nav) per sub-theme, each as 1-2 flowing first-person paragraphs (not bulleted "Problem/What you did/Impact/Skills" labels — write the same information as prose).

Worked example for the first theme, to fix the voice/format pattern the rest of this task (and Task 4) follow:

```markdown
<div class="role-section" markdown="1">

## Amperity {#amperity}

<p class="role-meta">Technical Product Manager · Feb 2025 – Jun 2026 · Remote / US — Enterprise Customer Data Platform (CDP) &amp; Data Activation</p>

Amperity is an enterprise customer-data-platform and data-activation
company. I owned the connector and destination-activation program
end-to-end — from customer demand intake through requirements, spec
authoring, engineering hand-off, release tracking, and adoption
reporting — across a ~16-month tenure (Feb 2025 – Jun 2026) that also
included ~30 years of prior career: senior developer through
Director/Sr. Director of Technical Program Management.

### Owning the Connector &amp; Destination Integration Program

The company's value depends on activating customer data into a long tail
of third-party destinations — ad platforms, ESPs, marketing and loyalty
tools — but demand arrived from every direction (sales, customer success,
customers directly) with no consistent way to capture, judge, scope, or
report on it. I stood up and ran the end-to-end intake pipeline: captured
demand through the product feedback tool and direct customer signals,
translated it into structured, engineering-ready requests, and
prioritized against business context. I authored 59+ connector request
specs spanning CAPI/server-side event APIs, ESPs, ad networks, messaging,
loyalty, and real-time streaming destinations, enriched each with the
business context engineering needed to prioritize — which customers
wanted it, urgency, strategic fit — and maintained the release roadmap
(real-time / traditional / pilot swimlanes) presented at the recurring
product-monthly meeting. The result: a chaotic, multi-channel demand
stream became a single governed pipeline with clear status, ownership,
and a forward-looking roadmap — the connective tissue between customers,
sales/CS, and engineering. Over the tenure this shipped 24 connectors to
production (12 batch, 12 real-time).

</div>
```

Continue with the remaining 5 Amperity themes in the same voice/structure, inside the same `<div class="role-section">`, using this content (drawn from `ACCOMPLISHMENTS.md` lines 107-243, condensed to flowing prose, keeping every concrete fact listed — don't drop any bullet, just de-scaffold it):

- **Product Specs & Technical Requirements at Scale** (lines 107-131): wrote the full ladder of BRDs, FSDs, and engineering hand-off specs for destination connectors and platform capabilities; standardized them into repeatable templates (problem framing, MUST/SHOULD/MAY requirements, scope, dependencies, risks, open questions); validated integration feasibility before committing engineering time by interrogating partner API docs, building Postman collections, and standing up mock APIs; authored platform-level requirements for bigger bets — a self-serve connector capability, a connector SDK, real-time connectors, and a destination testing/"revamp" initiative. Result: specs that were scoped, de-risked, and validated before engineering touched them.
- **Hands-On Technical Tooling & Automation** (lines 135-171): designed and built the two-part AI-assisted integration system described in the confirmed-numbers callout — a HITL discovery pipeline (OpenAPI spec + sample data → web search → auth validation via live credential ping → confidence-scored column-to-endpoint mapping → config file) and a dockerized execution engine (endpoint → config load → transform → deliver, with production-grade rate limiting, payload management, callbacks, pagination, a custom credential vault, and a SQL database for config versioning/run history). Architecture principle: deterministic Python everywhere the logic is unambiguous, LLM only where natural-language reasoning adds value. Validated the config format against Amperity's existing production integration library at 85% compatible out of the box (remaining 15% blocked by unsupported auth mechanisms or SOAP-based APIs). Evaluated and discarded both RAG and multi-agent orchestration during development — a single focused pipeline solved the actual problem without the added complexity. Also built 30+ reusable automation tools/workflows (release status, requirements enrichment, blocked-item triage, docs auditing, roadmap generation, analytical-narrative generation), internal MCP servers connecting the team's tools into automated workflows, and 6 mock destination APIs (FastAPI) so integrations to credential-gated partners could be scoped and demoed without live access.
- **Data Products, Dashboards & Executive Reporting** (lines 174-196): designed and shipped multiple production dashboards/data apps (Streamlit-in-Snowflake and HTML) covering connector release status, activation/usage trends, MBR metrics, and a documentation/knowledge dashboard — applying real analytical rigor (right visual encoding for the question, statistical/anomaly-detection thinking rather than naive thresholds) and building executive-ready outputs calibrated to the audience.
- **Customer-Facing Enablement & Solution Engineering** (lines 200-216): triaged reported connector/capability gaps, verifying claims across source code, vendor docs, product docs, live tenant config, and run history to distinguish a genuine product gap from a misconfiguration or docs gap; built journey/campaign automation tooling and validated activation mechanics (templating, segmentation, scheduling); produced demo/pilot environments and synthetic datasets for prospects and customers. This shortened the loop between a customer reporting a problem and knowing exactly whether it was a config fix, a docs fix, or a build.
- **Cross-Functional Program Management & Process** (lines 223-242): acted as the hub across engineering, product, sales, CS, docs, and partners — gathering demand, aligning priorities, tracking delivery, and reporting status on recurring cadences (weekly status, monthly roadmap, MBRs); managed a tracking-system migration and re-pointed the automation depending on it; defined a repeatable intake → spec → hand-off → release → adoption-reporting process and encoded it as tooling rather than tribal knowledge.

Close with one summary sentence using only the confirmed numbers from `ACCOMPLISHMENTS.md` line 67: ~90% cut in integration dev time (4 weeks → ~2 days), 85% compatibility against the existing production integration library, 24 connectors shipped (12 batch + 12 real-time), intake cut from ~1 week to ~1 hour, and 3 hand-built pilot connectors that kept Amperity competitive in active sales deals.

- [ ] **Step 2: Verify no scaffolding labels leaked through**

Run: `grep -nE '\*\*Problem\.\*\*|\*\*What you did\.\*\*|\*\*Impact\.\*\*|\*\*Skills:\*\*|\[verify' /Users/ericsalerno/Documents/career/ericthebigsal.github.io/portfolio.md`
Expected: no output (no matches). If anything prints, rewrite that line as flowing prose per the worked example.

- [ ] **Step 3: Commit**

```bash
cd ~/Documents/career/ericthebigsal.github.io
git add portfolio.md
git commit -m "Add Amperity (Tier 1) to portfolio.md"
```

---

### Task 4: `portfolio.md` — Tier 1: Press Ganey (full depth)

**Files:**
- Modify: `ericthebigsal.github.io/portfolio.md` (append after the Amperity section)

**Interfaces:**
- Produces: `## Press Ganey {#press-ganey}` section, consumed by Task 2's jump-nav link.

**Source:** `career/ACCOMPLISHMENTS.md` lines 248–344 (Themes A–E), same prose-conversion rule and confidentiality rule as Task 3. Role/dates: "Senior Director, Technical Program Management · Mar 2022 – Aug 2024 · Greater Seattle — promoted from Distinguished TPM; reported directly to the CTO" (from `ACCOMPLISHMENTS.md` line 249 / general resume).

- [ ] **Step 1: Write the Press Ganey section**

Same structure as Task 3 (`<div class="role-section">`, `## Press Ganey {#press-ganey}`, `<p class="role-meta">`, one `###` per theme, flowing first-person prose). Content, condensed from lines 255-339 (keep every concrete fact, de-scaffold the labels):

- **Epic Partnership & Integration** (lines 255-270): Press Ganey's patient-experience data had significant untapped value inside the Epic EHR ecosystem — the dominant system in US healthcare — but no integration existed, and building one required a true strategic partnership across two organizations with different cultures, timelines, and architectures. I spearheaded the Press Ganey/Epic integration, delivering key milestones including MyChart (patient-facing portal) and Cheers CRM (Epic's relationship-management tool), bringing Press Ganey data directly into the systems clinicians and administrators actually use. I was the primary relationship owner across both organizations — managing senior leadership and key decision-makers at both companies simultaneously, coordinating product/engineering/sales/marketing at both to keep execution aligned with the partnership roadmap, gathering and analyzing market requirements to define scope, and presenting executive summaries and status reports to senior leadership at both companies. Result: a live Epic integration giving healthcare organizations real-time patient-experience insights inside the clinical workflow — a differentiated capability that expanded Press Ganey's EHR footprint.
- **M&A Technical Unification** (lines 274-287): Press Ganey had made multiple acquisitions, each with its own tech stack, data architecture, and engineering team, needing assessment, rationalization, and unification onto the Press Ganey platform without disrupting live products. I drove the technical unification across the acquired companies, working directly with the CTO on integration strategy; led technical due diligence for multiple acquisitions (feasibility, integration risk, synergy opportunities before commitments were made); developed and implemented migration strategies for acquired systems and data, coordinating engineering teams across multiple locations; and built and presented technical roadmaps and integration proposals to senior leadership and board members.
- **Building the TPM Career Path** (lines 291-305): Press Ganey had no defined TPM discipline — no career path, no role definitions, no performance expectations — making it impossible to hire, develop, or retain technical program management talent consistently. I built the first TPM career path at Press Ganey from scratch, working closely with the CTO and their direct reports to define roles, responsibilities, and career progression for ICs and managers; authored job descriptions, role definitions, and career progression models for every level; defined performance expectations, KPIs, and compensation guidelines; and delivered presentations and workshops to build organization-wide buy-in, including transitioning existing Business Analysts and Project Managers into TPM roles. Result: a structured TPM discipline across the engineering organization, and the foundation for consistent hiring and development going forward.
- **FedRAMP Feasibility Analysis** (lines 309-321): Press Ganey was considering FedRAMP authorization to expand into the federal healthcare market — a substantial, typically multi-year, multi-million-dollar investment that needed rigorous analysis before committing. I led the feasibility analysis (technical, operational, and financial requirements), evaluated the full compliance scope (infrastructure, process controls, staffing, tooling, third-party assessment costs, ongoing maintenance), and built an ROI-based no-go recommendation that I drove through to an actual decision rather than a report that sat on a shelf. Leadership accepted the no-go recommendation, avoiding a costly multi-year program that wouldn't have generated sufficient return — one of the harder and more valuable calls a TPM can make and defend.
- **SDLC Unification (Jira / Jira Align)** (lines 325-339): Engineering teams operated with inconsistent processes and tooling, making it impossible to get reliable visibility into project health, team performance, or roadmap progress at the executive level. I drove adoption of Jira and Jira Align across multiple engineering teams, collaborated with executive management and development leadership to define and prioritize KPIs, built custom reports and dashboards tracking progress/velocity/milestones, and presented data-driven insights to leadership on a recurring basis.

- [ ] **Step 2: Verify no scaffolding labels leaked through**

Run: `grep -nE '\*\*Problem\.\*\*|\*\*What you did\.\*\*|\*\*Impact\.\*\*|\*\*Skills:\*\*|\[verify' /Users/ericsalerno/Documents/career/ericthebigsal.github.io/portfolio.md`
Expected: no output.

- [ ] **Step 3: Commit**

```bash
cd ~/Documents/career/ericthebigsal.github.io
git add portfolio.md
git commit -m "Add Press Ganey (Tier 1) to portfolio.md"
```

---

### Task 5: `portfolio.md` — Tier 2: Microsoft, Amazon, Accolade

**Files:**
- Modify: `ericthebigsal.github.io/portfolio.md` (append after Press Ganey)

**Interfaces:**
- Produces: `## Microsoft {#microsoft}`, `## Amazon {#amazon}`, `## Accolade {#accolade}` sections, consumed by Task 2's jump-nav links.

**Source:** `ACCOMPLISHMENTS.md` lines 345-455 (Microsoft, Amazon) and 821-896 (Accolade). Medium depth = one substantial paragraph per employer, no `###` sub-theme breakdown, no bulleted lists. Strip the `[verify]` tags at lines 435, 447, 450 (Amazon) and 856 (Accolade) — do not include the unconfirmed metrics they're attached to; state the outcome qualitatively instead (e.g. "improved fraud detection coverage" rather than inventing a percentage).

- [ ] **Step 1: Write the three Tier-2 sections**

```markdown
<div class="role-section role-compact" markdown="1">

## Microsoft {#microsoft}

<p class="role-meta">Senior Program Manager · Jan 2004 – Oct 2014 (two stints, with Appature in between Nov 2012 – Sep 2013) · Redmond, WA</p>

I led the initial API design and shipped the PlayReady DRM platform — core
functionality and APIs for Windows 8 and Silverlight 2/3/4 — a true
zero-to-one platform build that needed Tier-1 content-partner adoption
against a contested standards landscape. I drove product strategy
end-to-end (scenarios, feature specs, Sprint-based Agile delivery),
fostered collaboration across Windows, IIS, A/V Services, and Online
Services, engaged directly with Tier-1 and third-party partners, and
represented PlayReady at industry standards bodies. Later, on Xbox One, I
led the security program hardening the console's offline licensing model
at the silicon and firmware level — security updates to the optical disc
drive DSP, coordinated across ODD teams, silicon vendors, and firmware
engineers, plus third-party penetration testing before launch — and led
the launch of a security-focused SaaS delivery model for critical
Microsoft technologies. Earlier in my Microsoft tenure, I was product
owner and PM for a highly customized SharePoint-based knowledge-sharing
platform for Microsoft Consulting Services, leading a 20+ person offshore
development and testing team in Bangalore and traveling to India twice for
major delivery milestones.

</div>

<div class="role-section role-compact" markdown="1">

## Amazon {#amazon}

<p class="role-meta">Senior Technical Program Manager · Oct 2014 – Jul 2015 · Seattle, WA — Amazon Coins team</p>

Amazon Coins is Amazon's virtual currency for purchasing apps, games, and
in-app content. I developed and implemented fraud-detection models using
data-mining techniques to catch abuse patterns specific to virtual
currency — patterns traditional retail fraud models weren't built to
catch — using a customer-experience-first approach that improved detection
coverage without adding friction for legitimate customers. I also built
customer-segmentation models to identify high-value customers ("whales")
within the user base, enabling marketing to target campaigns calibrated to
actual customer behavior and value.

</div>

<div class="role-section role-compact" markdown="1">

## Accolade {#accolade}

<p class="role-meta">Director of TPM / Technical Product Manager · Mar 2017 – May 2022 · Seattle, WA — promoted from IC TPM to Director, Mar 2021</p>

Accolade is a personalized health-navigation company serving large
employer clients. As an IC TPM I led the design and implementation of new
data-ingestion infrastructure for partner feeds — member claims,
employment data, enrollments — establishing the data-quality foundation
later projects depended on. I then owned a multi-year migration from a
heavily customized Microsoft Dynamics CRM to a custom in-house solution
for Accolade's 50+ Health Assistant workforce, using a phased rollout
(pilot → structured feedback → iterate → full scale) that improved user
satisfaction and data accuracy without disrupting Health Assistants
serving members day to day. When Accolade acquired 2nd.MD and PlushCare, I
drove the program to unify Accolade's core technology and infrastructure
with both acquired companies — data mapping, API design, HIPAA-compliant
data security, and integration across services/data tiers, client portals,
CRM portals, and member-facing portals. I was promoted to Director of TPM
and led a team of 4-5 Technical Program Managers responsible for all data
ingress and egress across the platform, building deep hands-on expertise
across data warehouses, data lakes, CRM systems, and partner feeds to
resolve client and partner data issues quickly.

</div>
```

- [ ] **Step 2: Verify no `[verify]` tags or scaffolding labels leaked through**

Run: `grep -nE '\*\*Problem\.\*\*|\*\*What you did\.\*\*|\*\*Impact\.\*\*|\*\*Skills:\*\*|\[verify' /Users/ericsalerno/Documents/career/ericthebigsal.github.io/portfolio.md`
Expected: no output.

- [ ] **Step 3: Commit**

```bash
cd ~/Documents/career/ericthebigsal.github.io
git add portfolio.md
git commit -m "Add Microsoft, Amazon, Accolade (Tier 2) to portfolio.md"
```

---

### Task 6: `portfolio.md` — Tier 3, AI Projects, Skills, Credentials, Resume & Contact

**Files:**
- Modify: `ericthebigsal.github.io/portfolio.md` (append after Accolade; this is the final section of the file)

**Interfaces:**
- Consumes: `.project-summary-grid`, `.project-summary-card`, `.role-oneline`, `.pdf-download` from Task 1.
- Produces: `## Earlier Roles {#earlier-roles}`, `## AI Portfolio Projects {#ai-projects}`, `## Skills &amp; Credentials {#skills-credentials}`, `## Resume &amp; Contact {#resume-contact}` — the last 4 jump-nav targets from Task 2.

**Sources:**
- Tier 3: `Eric_Salerno_General_Resume.md` lines 82-91 (Appature, Concur, EDS/GM).
- AI Projects: `index.html`'s Featured Projects section (5 cards) — reuse the existing blurbs, trimmed to 2-3 sentences where they're already close to that length, linking to the same URLs already used there.
- Skills: `ACCOMPLISHMENTS.md` lines 897-931 (condense the 4 categories, drop nothing load-bearing).
- Credentials: `Eric_Salerno_General_Resume.md` line 101, **with the legacy Microsoft certifications (MCP/MCSD/MCT) removed** — keep only "AI/ML Foundations (Neural Networks · Machine Learning · Thinking Machines) · Recommendation Systems with Python · Predictive Analytics & Data Mining".
- Resume & Contact: reuse `index.html`'s `#resume` and `#contact` sections verbatim (read them with `sed -n '/resume-heading/,/site-footer/p' index.html` before writing this step, since they weren't fully captured during planning).

- [ ] **Step 1: Write the Earlier Roles section**

```markdown
<div class="role-section" markdown="1">

## Earlier Roles {#earlier-roles}

<p class="role-oneline"><strong>Appature</strong> — Technical Program Manager, Nov 2012 – Sep 2013, Seattle, WA (martech startup, acquired by IMS Health). Owned the platform APIs for a Marketing Relationship Management SaaS on AWS, used by customers, internal developers, and Customer Success; drove integration of the platform's ETL/CRUD APIs into SnapLogic's visual designer to reduce customer adoption friction.</p>

<p class="role-oneline"><strong>Concur Technologies</strong> — Program Manager. Grew from individual-contributor developer to dev lead to PM; supported Fortune-500 customer engagements.</p>

<p class="role-oneline"><strong>EDS / General Motors</strong> — Product Manager / Developer / Test. Early ASP web-app adopter.</p>

</div>
```

- [ ] **Step 2: Write the AI Portfolio Projects section**

Read the 5 existing project cards first: `sed -n '/projects-heading/,/resume-heading/p' /Users/ericsalerno/Documents/career/ericthebigsal.github.io/index.html`. Then write:

```markdown
<div class="role-section" markdown="1">

## AI Portfolio Projects {#ai-projects}

Since mid-2026 I've been building and directing AI coding agents on a
portfolio of from-scratch technical projects — full write-ups and code are
linked below; this is the short version.

<div class="project-summary-grid">

<div class="project-summary-card" markdown="1">

### Agent-Reviewed Mock Interview Tool

A live interview-prep tool built with Claude Code: real answers saved
through the browser, reviewed and reacted to by the agent in real time —
draft/approve/feedback loop, live status updates, and a self-caught
reviewer bias that got fixed and turned into a reusable Claude Code skill.

[Case study →](agentic-qa-tool.html)

</div>

<div class="project-summary-card" markdown="1">

### AI-Assisted Martech Integration Builder

An agent ingests an OpenAPI spec, cross-references domain knowledge files
for gotchas the spec doesn't mention, gates risky operations behind human
review, then generates and verifies a working MCP tool server against a
live account (Klaviyo and HubSpot).

[GitHub →](https://github.com/ericthebigsal/agent-integration) ·
[Live demo →](https://ericthebigsal.github.io/agent-integration)

</div>

<div class="project-summary-card" markdown="1">

### RAG Pipeline, Built From Scratch

A retrieval-augmented generation system with no LangChain, no LlamaIndex,
no vector database. A custom parser chunks OpenAPI specs at the operation
level so schema and meaning stay together — every stage, from chunking to
prompt assembly, is visible and attributable to project code.

[GitHub →](https://github.com/ericthebigsal/rag-pipeline)

</div>

<div class="project-summary-card" markdown="1">

### judge-dread

A local-first regression-testing harness for AI prompts: deterministic
checks plus LLM-as-judge evaluation, built entirely on Gemini's free tier.

[GitHub →](https://github.com/ericthebigsal/judge-dread)

</div>

<div class="project-summary-card" markdown="1">

### nanoGPT, Trained From Scratch

A character-level GPT trained from scratch on public-domain science
fiction from Project Gutenberg, built with nanoGPT — hands-on with the
fundamentals of how these models actually learn.

[GitHub →](https://github.com/ericthebigsal/nanogpt-scifi)

</div>

</div>
</div>
```

- [ ] **Step 3: Write the Skills & Credentials section**

```markdown
<div class="role-section" markdown="1">

## Skills &amp; Credentials {#skills-credentials}

**Product / Program craft:** requirements (BRD/PRD/FSD), product discovery
and customer interviews, prioritization, roadmapping, release management,
stakeholder management, executive communication, process design, MBR/QBR
reporting.

**Technical:** Python, SQL (Snowflake), REST API integration and
analysis, FastAPI, Postman/Newman, data visualization (Streamlit,
HTML/D3), CI/CD pipelines, Git, LLM/AI tooling (MCP servers, prompt/skill
authoring), mock-service and test-harness design, data modeling for
analytics.

**Domain:** Customer Data Platforms (CDP), marketing/advertising
activation, data integration and connectors, identity/data onboarding,
ad-tech and martech ecosystems (CAPI/server-side events, ESPs, ad
networks, loyalty, messaging), healthcare data interoperability (Epic).

**AI / ML (agent-directed, from-scratch, ongoing portfolio):** transformer
and self-attention architecture, tokenization design, training loops and
optimization, PyTorch, reproducible ML pipeline engineering, RAG
architecture from first principles, LLM-as-judge evaluation design,
structured-output API usage, regression-testing harness design,
multi-agent development workflows, agentic tool design (MCP server
generation, HITL workflows).

**Education:** University of Michigan — B.S., Industrial &amp; Operations
Engineering. Michigan Technological University — Engineering.

**Certifications:** AI/ML Foundations (Neural Networks, Machine Learning,
Thinking Machines) · Recommendation Systems with Python · Predictive
Analytics &amp; Data Mining.

</div>
```

- [ ] **Step 4: Write the Resume & Contact section**

Reuse `index.html`'s existing `#resume`/`#contact` links verbatim, plus the PDF download link for this page itself:

```markdown
<div class="role-section" markdown="1">

## Resume &amp; Contact {#resume-contact}

<a class="pdf-download" href="portfolio.pdf">Download this page as a PDF</a>

**Resume:** [Download Resume (PDF)](assets/resume/Eric-Salerno-Resume.pdf)

**Contact:** [esalerno86@gmail.com](mailto:esalerno86@gmail.com) ·
[LinkedIn](https://www.linkedin.com/in/ersalerno) ·
[GitHub](https://github.com/ericthebigsal)

</div>
```

- [ ] **Step 5: Verify no `[verify]` tags, scaffolding labels, or legacy certs leaked through**

Run:
```bash
grep -nE '\*\*Problem\.\*\*|\*\*What you did\.\*\*|\*\*Impact\.\*\*|\*\*Skills:\*\*|\[verify|MCP / MCSD / MCT|Microsoft Certified Professional' /Users/ericsalerno/Documents/career/ericthebigsal.github.io/portfolio.md
```
Expected: no output. The `MCP / MCSD / MCT` / `Microsoft Certified Professional` check specifically confirms the legacy-certs-dropped constraint held (note: `MCP` also appears legitimately elsewhere in this file meaning "Model Context Protocol" — if this grep flags a false positive, confirm by reading the matched line, don't just delete every `MCP` occurrence).

- [ ] **Step 6: Commit**

```bash
cd ~/Documents/career/ericthebigsal.github.io
git add portfolio.md
git commit -m "Add Earlier Roles, AI Projects, Skills/Credentials, Resume/Contact to portfolio.md"
```

---

### Task 7: Render `portfolio.html` and `portfolio.pdf`

**Files:**
- Create: `ericthebigsal.github.io/portfolio.html` (generated)
- Create: `ericthebigsal.github.io/portfolio.pdf` (generated)

**Interfaces:**
- Consumes: complete `portfolio.md` (Tasks 2-6) and `portfolio-style.css` (Task 1).

- [ ] **Step 1: Render HTML**

```bash
cd ~/Documents/career/ericthebigsal.github.io
pandoc portfolio.md -o /tmp/portfolio.html --standalone \
  --metadata title="Eric Salerno - Full Career Portfolio" \
  --metadata lang=en \
  --include-in-header=portfolio-head.html \
  --css=portfolio-style.css --embed-resources
cp /tmp/portfolio.html portfolio.html
```

- [ ] **Step 2: Verify every jump-nav anchor resolves**

```bash
for id in about amperity press-ganey microsoft amazon accolade earlier-roles ai-projects skills-credentials resume-contact; do
  grep -q "id=\"$id\"" portfolio.html && echo "OK: $id" || echo "MISSING: $id"
done
```
Expected: `OK: <id>` for all 10 — any `MISSING` means that task's `{#id}` attribute didn't make it into the markdown; go back and fix it before continuing.

- [ ] **Step 3: Render PDF**

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless \
  --disable-gpu --no-pdf-header-footer --print-to-pdf=portfolio.pdf \
  --print-to-pdf-no-header "file:///tmp/portfolio.html"
```

- [ ] **Step 4: Verify the PDF exists and is non-trivial**

Run: `ls -la portfolio.pdf && file portfolio.pdf`
Expected: `portfolio.pdf: PDF document` and a file size in the hundreds of KB range (a near-empty PDF, e.g. under 20KB, means the print step failed silently — check `/tmp/portfolio.html` opens correctly in a browser first).

- [ ] **Step 5: Commit**

```bash
git add portfolio.html portfolio.pdf
git commit -m "Render portfolio.html and portfolio.pdf from portfolio.md"
```

---

### Task 8: Wire up navigation and final verification

**Files:**
- Modify: `ericthebigsal.github.io/index.html`

**Interfaces:**
- Consumes: `portfolio.html` (Task 7) as the link target.

- [ ] **Step 1: Add a nav link on the homepage**

Read the current header markup: `sed -n '20,25p' index.html`. Add a link to `portfolio.html` in the `site-header` — e.g. immediately after the `.tagline` paragraph:

```html
<p class="tagline">Agentic AI, multi-agent systems &amp; LLM orchestration — designed and shipped, not just roadmapped.</p>
<p><a href="portfolio.html">Full Career Portfolio →</a></p>
```

Use `Edit` to insert this line after the existing `.tagline` paragraph in `index.html` — don't restructure the surrounding markup.

- [ ] **Step 2: Verify the homepage link resolves**

Run: `grep -o 'href="portfolio.html"' index.html`
Expected: `href="portfolio.html"` printed once.

- [ ] **Step 3: Full-file final verification**

```bash
cd ~/Documents/career/ericthebigsal.github.io
echo "--- scaffolding/verify-tag check ---"
grep -nE '\*\*Problem\.\*\*|\*\*What you did\.\*\*|\*\*Impact\.\*\*|\*\*Skills:\*\*|\[verify|IN PROGRESS' portfolio.md portfolio.html
echo "--- legacy certs check ---"
grep -nE 'MCSD|MCT\b|Microsoft Certified' portfolio.md portfolio.html
echo "--- back-link check ---"
grep -o 'href="index.html"' portfolio.html
echo "--- word count (sanity: should be well beyond resume length) ---"
wc -w portfolio.md
```
Expected: both scaffolding/legacy-cert greps print nothing; the back-link grep prints at least one match; word count is at minimum ~1500 words (a resume-length page would be under 800 — this confirms "not 1-2 pages" was actually met).

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Link the homepage to the full career portfolio page"
```

- [ ] **Step 5: Push**

```bash
git push
```

Confirm the page is live at `https://ericthebigsal.github.io/portfolio.html` once GitHub Pages rebuilds (usually under a minute) before telling Eric it's ready to share.
