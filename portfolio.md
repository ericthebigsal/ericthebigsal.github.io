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

### Writing Specs Engineering Could Build From

Engineering needed crisp, validated specs to build connectors
efficiently — without them, build cycles stalled on ambiguity and
rework. I wrote the full ladder of product artifacts — business
requirements documents, functional specs, and engineering hand-off
specifications — for destination connectors and platform capabilities,
and standardized them into repeatable templates covering problem
framing, MUST/SHOULD/MAY requirements, scope and out-of-scope,
dependencies, risks, and open questions, so every spec hit the same bar.
Before committing engineering time, I validated integration feasibility
myself: interrogating partner API docs, building Postman collections,
and standing up mock APIs to prove a spec was buildable before anyone
wrote production code. I also authored platform-level requirements for
several bigger bets — a self-serve connector capability, a connector
SDK, real-time connectors, and a destination testing/"revamp"
initiative. The result was a pipeline of specs that arrived scoped,
de-risked, and validated, letting engineering take on a much wider
catalog of integrations than they otherwise could.

### Building an AI-Assisted Integration System — and the Tooling Around It

Program ownership at this scale generated enormous amounts of repeatable
analytical and reporting work, and the most technical response I built
to it was a two-part AI-assisted integration system in Python and
Claude. The discovery half is a human-in-the-loop pipeline: it takes an
OpenAPI spec and sample data, uses web search to understand the
destination API, validates the auth model with a live credential ping,
and produces a confidence-scored mapping from source columns to
destination endpoints — which I could review and adjust — that gets
written out as a config file fully specifying the integration. The
execution half is a dockerized engine that exposes an endpoint, loads
that config, applies the required transformations, and delivers the
data to the destination, handling production concerns like rate
limiting, payload size limits, callbacks, and pagination, backed by a
custom-built credential vault and a SQL database for config versioning
and run history. I held to one architecture principle throughout:
deterministic Python everywhere the logic was unambiguous, and LLM calls
only where natural-language reasoning actually added value. I validated
the resulting config format against Amperity's existing production
integration library and found it 85% compatible out of the box, with the
remaining 15% blocked by unsupported auth mechanisms or SOAP-based APIs.
Along the way I evaluated and discarded both a RAG layer and a
multi-agent orchestration approach — neither solved the actual problem,
and a single focused pipeline did the job without the added complexity.

Beyond that system, I built more than 30 reusable automation tools and
workflows covering release status reporting, requirements enrichment,
blocked-item triage, documentation auditing, roadmap generation, and
analytical-narrative generation, plus internal MCP servers that
connected the team's tools into automated workflows, and six mock
destination APIs built with FastAPI so integrations to credential-gated
partners could be scoped and demoed without needing live access.

### Turning Program Data into Decisions

Product leadership, customer success, and customers all needed
visibility into integration adoption, activation and usage, and program
health, but the data was scattered across systems. I designed and
shipped multiple production dashboards and data apps — built in
Streamlit-in-Snowflake and in HTML — covering connector release status,
activation and usage trends, monthly-business-review metrics, and a
documentation/knowledge dashboard for the team. I applied real
analytical rigor to each: choosing the visual encoding that actually fit
the question, and using statistical and anomaly-detection thinking
rather than naive fixed thresholds for monitoring. I calibrated every
output to its audience, building executive-ready roadmap decks and MBR
references alongside more operational, technical views for the team
itself.

### Closing the Loop Between Customers and Engineering

Customers and field teams regularly hit friction operating integrations
and activation features, and sales needed fast, credible feasibility
answers. I triaged reported connector and capability gaps by verifying
the claim across source code, vendor docs, product docs, live tenant
configuration, and run history — distinguishing a genuine product gap
from a misconfiguration or a documentation gap before anyone escalated
it as a bug. I built journey and campaign automation tooling and
validated activation mechanics like templating, segmentation, and
scheduling against real and synthetic data, and produced demo and pilot
environments and synthetic datasets to prove out features and
integrations for prospects and customers. That work shortened the loop
between a customer reporting a problem and knowing exactly whether it
was a config fix, a docs fix, or a build — and gave the field answers it
could trust.

### Running the Operational Backbone

The integration program touched engineering, product, sales, customer
success, docs, and partners, and I was the hub connecting all of them —
gathering demand, aligning priorities, tracking delivery, and reporting
status on recurring cadences that included weekly status updates, a
monthly roadmap review, and MBRs. I managed a tracking-system migration
mid-program, consolidating the team onto a new project/board structure
and re-pointing all the automation that depended on the old one. I
defined a repeatable process running from intake through spec authoring,
engineering hand-off, release, and adoption reporting, and encoded it as
tooling rather than leaving it as tribal knowledge, so it kept working
as the team and the program grew.

Across the tenure, the headline numbers: the AI-assisted integration
system cut connector development time roughly 90%, from about four
weeks down to about two days; its output was 85% compatible with
Amperity's existing production integration library out of the box; 24
connectors shipped to production (12 batch, 12 real-time); intake
automation cut the time from a customer request to a dev-ready spec and
ticket from about a week to about an hour; and three hand-built pilot
connectors kept Amperity competitive in active sales deals.

</div>

<div class="role-section" markdown="1">

## Press Ganey {#press-ganey}

<p class="role-meta">Senior Director, Technical Program Management · Mar 2022 – Aug 2024 · Greater Seattle — promoted from Distinguished TPM; reported directly to the CTO</p>

Press Ganey is a leading healthcare experience and performance
improvement company serving thousands of healthcare organizations. I
joined as a Distinguished TPM and was promoted to Senior Director,
owning a portfolio of strategic initiatives directly for the CTO.

### Leading the Epic Partnership &amp; Integration

Press Ganey's patient-experience data had significant untapped value
inside the Epic EHR ecosystem — the dominant system in US healthcare —
but no integration existed, and building one required a true strategic
partnership across two organizations with different cultures,
timelines, and architectures. I spearheaded the Press Ganey/Epic
integration, delivering key milestones including MyChart (Epic's
patient-facing portal) and Cheers CRM (Epic's relationship-management
tool), bringing Press Ganey data directly into the systems clinicians
and administrators actually use. I was the primary relationship owner
across both organizations — managing senior leadership and key
decision-makers at both companies simultaneously, coordinating
product, engineering, sales, and marketing at both to keep execution
aligned with the partnership roadmap, gathering and analyzing market
requirements to define scope, and presenting executive summaries and
status reports to senior leadership at both companies. The result was
a live Epic integration giving healthcare organizations real-time
patient-experience insights inside the clinical workflow — a
differentiated capability that expanded Press Ganey's EHR footprint.

### Unifying Technology Across Acquisitions

Press Ganey had made multiple acquisitions, each with its own tech
stack, data architecture, and engineering team, all needing assessment,
rationalization, and unification onto the Press Ganey platform without
disrupting live products. I drove the technical unification across the
acquired companies, working directly with the CTO on integration
strategy. I led technical due diligence for multiple acquisitions —
assessing feasibility, integration risk, and synergy opportunities
before commitments were made — developed and implemented migration
strategies for acquired systems and data, coordinating engineering
teams across multiple locations, and built and presented technical
roadmaps and integration proposals to senior leadership and board
members.

### Building the TPM Career Path from Scratch

Press Ganey had no defined TPM discipline — no career path, no role
definitions, no performance expectations — which made it impossible to
hire, develop, or retain technical program management talent
consistently. I built the first TPM career path at Press Ganey from
scratch, working closely with the CTO and their direct reports to
define roles, responsibilities, and career progression for ICs and
managers. I authored job descriptions, role definitions, and career
progression models for every level, defined performance expectations,
KPIs, and compensation guidelines, and delivered presentations and
workshops to build organization-wide buy-in, including transitioning
existing Business Analysts and Project Managers into TPM roles. The
result was a structured TPM discipline across the engineering
organization, and the foundation for consistent hiring and development
going forward.

### Making the Call Not to Pursue FedRAMP

Press Ganey was considering FedRAMP authorization to expand into the
federal healthcare market — a substantial, typically multi-year,
multi-million-dollar investment that needed rigorous analysis before
committing. I led the feasibility analysis across technical,
operational, and financial requirements, evaluated the full compliance
scope — infrastructure, process controls, staffing, tooling,
third-party assessment costs, and ongoing maintenance — and built an
ROI-based no-go recommendation that I drove through to an actual
decision rather than a report that sat on a shelf. Leadership accepted
the no-go recommendation, avoiding a costly multi-year program that
wouldn't have generated sufficient return — one of the harder and more
valuable calls a TPM can make and defend.

### Unifying the SDLC Around Jira and Jira Align

Engineering teams operated with inconsistent processes and tooling,
making it impossible to get reliable visibility into project health,
team performance, or roadmap progress at the executive level. I drove
adoption of Jira and Jira Align across multiple engineering teams,
collaborated with executive management and development leadership to
define and prioritize KPIs, built custom reports and dashboards
tracking progress, velocity, and milestones, and presented data-driven
insights to leadership on a recurring basis.

</div>

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
