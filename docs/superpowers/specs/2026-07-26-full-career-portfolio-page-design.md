# Full Career Portfolio Page — Design

## Problem

Recruiters frequently reach out on LinkedIn with a vague "I saw your profile,
can you send me a resume?" and no job description. A resume is the wrong
artifact to send cold — it's tuned for a specific role, and going
back-and-forth asking for a JD rarely gets a reply. Eric needs a single link
he can drop into that DM: not tailored to any job, not resume-length, that
gives a recruiter with zero context a real sense of his full career and
what he's capable of.

## Approach

Add a new page, `portfolio.html`, to the existing `ericthebigsal.github.io`
repo — not a new repo/site. Considered and rejected: expanding the homepage
itself (it needs to stay a sharp first impression for people who already
have context, not grow to "not 1-2 pages" length); a fully separate
repo/site (doubles maintenance — two `About`/contact sections, two design
systems, and creates the exact confusion this is meant to avoid — which
link do you send?).

Eric also wants a PDF download of this page, so authoring follows the
pattern already established everywhere else in this project (resumes,
cover letters, company writeups): a single Markdown source rendered to
both HTML and PDF, rather than two independently-maintained copies that
can drift apart. `portfolio.md` is the source of truth; it renders to
`portfolio.html` (pandoc + a new `portfolio-style.css`, visually matching
this site rather than the plainer stylesheet used for company research
docs) and to `portfolio.pdf` (same headless-Chrome print step already used
for resume/cover-letter PDFs — see Rendering below). Both deploy via the
repo's existing GitHub Pages publish — no new infrastructure. The homepage
gets one new nav link ("Full Portfolio") pointing to `portfolio.html`;
`portfolio.html` links back to the homepage and offers "Download PDF"
near its own Resume/Contact section.

## Content Source & Cleanup

Source material is `career/ACCOMPLISHMENTS.md` (the private master brag doc)
and `career/Eric_Salerno_General_Resume.md`. Both are written for Eric's own
resume-bullet-drafting use, not for external readers, so nothing gets
copy-pasted as-is:

- Rewrite from ACCOMPLISHMENTS.md's internal second-person "you did" /
  prompt-style bullets into first-person professional prose, matching the
  voice already established in the homepage's `About` section.
- Strip anything internal-only: `[verify]` tags, "IN PROGRESS" labels,
  reflective asides, open-questions notes. Per existing project convention
  (no internal notes in third-party-visible artifacts), this page gets the
  same treatment as a resume or cover letter, not the raw doc.
- Company names are used freely — the homepage already names Amperity,
  Microsoft, Amazon, and Press Ganey, so there's no new confidentiality
  step here.

## Structure & Weighting

Comprehensive but weighted — not every role gets equal depth. In page
order:

1. **Header / orientation** — name, one-line positioning, and a short note
   that this is the full career portfolio (since a cold reader has no other
   context), with a jump-nav to the sections below (this page is long by
   design; skimming should be easy).
2. **About** — same positioning as the homepage, reused rather than
   rewritten.
3. **Tier 1 — full depth: Amperity, Press Ganey.** Full Problem → What you
   did → Impact treatment, covering all the sub-themes already written up
   in `ACCOMPLISHMENTS.md` for these two (Amperity: connector/integration
   program, specs at scale, tooling/automation, data products/reporting,
   customer enablement, cross-functional process; Press Ganey: Epic
   partnership, M&A unification, building the TPM career path, FedRAMP
   feasibility, SDLC unification). These are the most recent and most
   relevant to what Eric is targeting now.
4. **Tier 2 — medium depth: Microsoft, Amazon, Accolade.** One solid
   paragraph each capturing the real substance (Microsoft: PlayReady DRM
   platform APIs, Xbox One hardware security; Amazon: fraud detection,
   customer segmentation; Accolade: data ingestion infrastructure, HCRM
   migration, M&A unification, building out the Director of TPM function) —
   no full Problem/Impact/Skills breakdown, just enough to establish real
   credibility. These already get a one-line callout on the homepage; this
   page is where that gets substantiated.
5. **Tier 3 — one line each: Concur, EDS/GM.** Career-length signal only
   ("individual contributor developer → dev lead → PM" / "early ASP web-app
   adopter"), no further detail.
6. **AI Portfolio Projects** — 2–3 sentence summary of each of the 4
   existing project case studies (Agent-Reviewed Mock Interview Tool,
   AI-Assisted Martech Integration Builder, RAG Pipeline, judge-dread,
   nanoGPT-scifi) with a "Read the full case study →" link back to the
   homepage/`agentic-qa-tool.html`. Not duplicated in full — those write-ups
   already exist and are already good; this page shouldn't re-author them.
7. **Skills inventory** — condensed from `ACCOMPLISHMENTS.md` section 4.
8. **Credentials** — modern AI/ML coursework only (Neural Networks/ML
   Foundations, Recommendation Systems, Predictive Analytics & Data
   Mining). Legacy Microsoft certifications (MCP/MCSD/MCT) are dropped
   entirely, not listed — Eric's explicit call that they're no longer
   relevant signal.
9. **Resume + Contact** — reused from the homepage (same PDF link, same
   contact section).

## Rendering

Matches the pipeline already used by the `research-company` skill for
resumes/cover letters:

```bash
pandoc portfolio.md -o /tmp/portfolio.html --standalone \
  --metadata title="Eric Salerno - Full Career Portfolio" \
  --metadata lang=en \
  --include-in-header=portfolio-head.html \
  --css=portfolio-style.css --embed-resources
cp /tmp/portfolio.html portfolio.html   # repo root

"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless \
  --disable-gpu --no-pdf-header-footer --print-to-pdf=portfolio.pdf \
  --print-to-pdf-no-header "file:///tmp/portfolio.html"
```

`portfolio-style.css` is a new stylesheet (lives alongside `styles.css` in
this repo) built to match the site's existing look — fonts, colors, the
same card treatment used for the homepage's project case studies — rather
than reusing the plainer `writeup-style.css` from the `research-company`
skill, which is tuned for internal research docs, not a public-facing page.

## Styling & Length

New component styles (in `portfolio-style.css`) as each tier needs: a full
Problem/Impact card style for Tier 1 and the project case studies, a more
compact block style for Tier 2/3 entries. No new CSS framework or build
tooling beyond pandoc (already used elsewhere in the project) and the
existing headless-Chrome PDF step. The page will be long (several thousand
words) — that's intentional per Eric's brief — so the top jump-nav is
load-bearing, not decorative, in both the HTML and as PDF bookmarks/anchors
where pandoc supports it.

## Out of scope

- No changes to `agentic-qa-tool.html` or the existing case-study content
  beyond linking to it.
- No changes to `career/ACCOMPLISHMENTS.md` itself — it stays the internal
  source doc; `portfolio.md` is a derived, cleaned artifact.
